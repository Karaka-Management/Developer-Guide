```c++
/**
 * Jingga
 *
 * @package   App
 * @copyright Dennis Eichhorn
 * @license   OMS License 1.0
 * @version   1.0.0
 * @link      https://jingga.app
 */
#include <stdio.h>
#include <stdlib.h>
#include <string>

#include "cOMS/Utils/ApplicationUtils.h"
#include "cOMS/DataStorage/Database/Connection/ConnectionAbstract.h"
#include "cOMS/Utils/Parser/Json.h"
#include "cOMS/Utils/OSWrapper.h"
#include "cOMS/Router/Router.h"
#include "cOMS/Threads/Thread.h"
#include "cOMS/Application/ApplicationAbstract.h"
#include "Models/ResourceMapper.h"

#include "Routes.h"

#ifndef OMS_DEMO
    #define OMS_DEMO false
#endif

Application::ApplicationAbstract app = {0};

int main(int argc, char **argv)
{
    /* --------------- Basic setup --------------- */

    const char *arg = Utils::ApplicationUtils::compile_arg_line(argc, argv);

    // Set program path as cwd
    char *cwd = Utils::ApplicationUtils::cwd();
    if (cwd == NULL) {
        printf("Couldn't get the CWD\n");

        return -1;
    }

    char *cwdT = Utils::StringUtils::search_replace(cwd, "\\", "/");
    free(cwd);
    cwd = cwdT;

    Utils::ApplicationUtils::chdir_application(cwd, argv[0]);

    // Check config
    if (!Utils::FileUtils::file_exists("config.json")) {
        Controller::ApiController::notInstalled(argc, argv);

        return -1;
    }

    /* --------------- App setup --------------- */

    // Load config
    FILE *in = fopen("config.json", "r");
    if (in == NULL) {
        printf("Couldn't open config.json\n");

        return -1;
    }

    app.config = nlohmann::json::parse(in);

    fclose(in);

    // Setup db connection
    DataStorage::Database::DbConnectionConfig dbdata = {
        DataStorage::Database::database_type_from_str(app.config["db"]["core"]["masters"]["admin"]["db"].get_ref<const std::string&>().c_str()),
        app.config["db"]["core"]["masters"]["admin"]["database"].get_ref<const std::string&>().c_str(),
        app.config["db"]["core"]["masters"]["admin"]["host"].get_ref<const std::string&>().c_str(),
        atoi(app.config["db"]["core"]["masters"]["admin"]["port"].get_ref<const std::string&>().c_str()),
        app.config["db"]["core"]["masters"]["admin"]["login"].get_ref<const std::string&>().c_str(),
        app.config["db"]["core"]["masters"]["admin"]["password"].get_ref<const std::string&>().c_str(),
    };

    app.pool = Threads::pool_create(app.config["app"]["threads"]["count"].get<int>());

    app.db = DataStorage::Database::create_connection(dbdata);
    app.db->connect();

    /* --------------- Handle request --------------- */

    // Handle routes
    Router::Router router  = generate_routes(&app);
    Router::RouterFunc ptr = Router::match_route(&router, arg);

    // No endpoint found
    if (ptr == NULL) {
        ptr = &Controller::ApiController::printHelp;
    }

    // Dispatch found endpoint
    (*ptr)(argc, argv);

    /* --------------- Cleanup --------------- */

    Threads::pool_destroy(app.pool);

    DataStorage::Database::free_MapperData((DataStorage::Database::MapperData *) &Models::ResourceMapper);

    app.db->close();
    app.db = NULL;

    Router::free_router(&router);

    free((char *) arg);
    arg = NULL;

    // Reset CWD (don't know if this is necessary)
    chdir(cwd);

    free(cwd);
}
```

```c++
/**
 * Jingga
 *
 * @package   Models
 * @copyright Dennis Eichhorn
 * @license   OMS License 1.0
 * @version   1.0.0
 * @link      https://jingga.app
 */
#ifndef ROUTES_H
#define ROUTES_H

#include <stdio.h>
#include <stdlib.h>

#include "Controller/ApiController.h"
#include "Controller/InstallController.h"
#include "cOMS/Router/Router.h"
#include "cOMS/Application/ApplicationAbstract.h"

Router::Router generate_routes(Application::ApplicationAbstract *app)
{
    Controller::ApiController::app = app;

    Router::Router router = Router::create_router(4);

    Router::set(&router, "^.*?\\-h *.*$", (void *) &Controller::ApiController::printHelp);
    Router::set(&router, "^.*?\\-v *.*$", (void *) &Controller::ApiController::printVersion);
    Router::set(&router, "^.*?\\-r *.*$", (void *) &Controller::ApiController::checkResources);
    Router::set(&router, "^.*?\\-\\-install *.*$", (void *) &Controller::InstallController::installApplication);

    return router;
}

#endif

namespace Controller {
    namespace ApiController {
        static Application::ApplicationAbstract *app = NULL;

        void printHelp(int argc, char **argv)
        {
            printf("    The Online Resource Watcher app developed by jingga checks online or local resources\n");
            printf("    for changes and informs the user about them.\n\n");
            printf("    Run: ./App ....\n\n");
            printf("    -h: Prints the help output\n");
            printf("    -v: Prints the version\n");
            printf("\n");
            printf("    Website: https://jingga.app\n");
            printf("    Copyright: jingga (c) Dennis Eichhorn\n");
        }
    }
}

#endif
```