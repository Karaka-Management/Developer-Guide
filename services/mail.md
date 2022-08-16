# Mail

Sending and receiving emails is possible with the included mail classes. The mailing is implemented in such a way that you can create emails which you then can pass to a mail handler which sends the mails.

## Sending

For email sending external mail servers are recommended, especially for advertisements, mailing lists etc. since a self hosted mailing server is very likely to get blacklisted by email services.

### Mail handler

#### Handler

Available mail handlers are:

* IMAP
* POP3

##### IMAP Connection

```php
// Define mailer
$handler = new Imap('testuser', 'testuser', 143);
$handler->host  = '127.0.0.1';
$handler->flags = '/imap/notls/norsh/novalidate-cert';

// ...

// Send email
$handler->send($mail);
```

##### POP3 Connection

```php
// Define mailer
$handler = new Pop3('testuser', 'testuser', 110);
$handler->host  = '127.0.0.1';
$handler->flags = '/pop3/notls';

// ...

// Send email
$handler->send($mail);
```

#### Software

For sending emails you can chose between the following mailing programs depending on your server setup:

* Sendmail
* Mail
* Smtp


```php
// Mail
$handler->setMailer(SubmitType::MAIL);

// SMTP
$handler->setMailer(SubmitType::SMTP);
$handler->useAutoTLS = false;

// Sendmail
$handler->setMailer(SubmitType::SENDMAIL);
```

### Confirmation Address

```php
$mail                      = new Email();
$mail->confirmationAddress = 'test1@karaka.email';
```

### From, To, CC & BCC

```php
$mail = new Email();
$mail->setFrom('test1@karaka.email', 'Dennis Eichhorn');
$mail->addTo('test@karaka.email', 'Dennis Eichhorn');
$mail->addCC('test2@karaka.email', 'Dennis Eichhorn');
$mail->addBCC('test3@karaka.email', 'Dennis Eichhorn');
```

### Reply

```php
$mail = new Email();
$mail->addReplyTo('test4@karaka.email', 'Dennis Eichhorn');
```

### Subject

```php
$mail          = new Email();
$mail->subject = 'Test subject';
```
### Body

#### Plain

```php
$mail       = new Email();
$mail->body = 'Body';
```

#### Html

```php
$mail          = new Email();
$message       = \file_get_contents(__DIR__ . '/files/utf8.html');
$mail->charset = CharsetType::UTF_8;
$mail->body    = '';
$mail->bodyAlt = '';

$mail->setHtml(true);
$mail->msgHTML($message, __DIR__ . '/files');
```

#### Calendar

```php
$mail          = new Email();
$mail->body    = 'Ical test';
$mail->altBody = 'Ical test';
$mail->ical    = 'BEGIN:VCALENDAR'
    . "\r\nVERSION:2.0"
    . "\r\nPRODID:-//phpOMS//Karaka Calendar//EN"
    . "CONTENT"
    . "\r\nCALSCALE:GREGORIAN"
    . "\r\nX-MICROSOFT-CALSCALE:GREGORIAN"
    . "\r\nBEGIN:VEVENT"
    . "\r\nUID:201909250755-42825@test"
    . "\r\nDTSTART;20190930T080000Z"
    . "\r\nSEQUENCE:2"
    . "\r\nTRANSP:OPAQUE"
    . "\r\nSTATUS:CONFIRMED"
    . "\r\nDTEND:20190930T084500Z"
    . "\r\nLOCATION:[London] London Eye"
    . "\r\nSUMMARY:Test ICal method"
    . "\r\nATTENDEE;CN=Attendee, Test;ROLE=OPT-PARTICIPANT;PARTSTAT=NEEDS-ACTION;RSVP="
    . "\r\n TRUE:MAILTO:attendee-test@example.com"
    . "\r\nCLASS:PUBLIC"
    . "\r\nDESCRIPTION:Some plain text"
    . "\r\nORGANIZER;CN=\"Example, Test\":MAILTO:test@example.com"
    . "\r\nDTSTAMP:20190925T075546Z"
    . "\r\nCREATED:20190925T075709Z"
    . "\r\nLAST-MODIFIED:20190925T075546Z"
    . "\r\nEND:VEVENT"
    . "\r\nEND:VCALENDAR";

```

### Attachments

#### External file

```php
$mail = new Email();
$mail->addAttachment(__DIR__ . '/files/logo.png', 'logo');
```
#### Inline

```php
$mail = new Email();
$mail->setHtml(true);
$mail->msgHTML("<img alt=\"image\" src=\"cid:cid1\">");
$mail->addEmbeddedImage(__DIR__ . '/files/logo.png', 'cid1');
```

#### String files

```php
$mail = new Email();
$mail->addStringAttachment('String content', 'string_content_file.txt');
$mail->addStringEmbeddedImage(\file_get_contents(__DIR__ . '/files/logo.png'), 'cid2');
```

## Receiving

### Handler

#### IMAP Connection

```php
// Define mailer
$handler = new Imap('testuser', 'testuser', 143);
$handler->host  = '127.0.0.1';
$handler->flags = '/imap/notls/norsh/novalidate-cert';
```

#### POP3 Connection

```php
// Define mailer
$handler = new Pop3('testuser', 'testuser', 110);
$handler->host  = '127.0.0.1';
$handler->flags = '/pop3/notls';
```

### Mailboxes

#### List, Create, Rename, Delete

```php
$handler->getBoxes();
$handler->createBox('TestBox');
$handler->renameBox('TestBox', 'NewTestBox');
$handler->deleteBox('NewTestBox');
```

#### Info

```php
$handler->getMailboxInfo('INBOX.TestBox');

$handler->countMail('INBOX.TestBox');
$handler->handler->countRecent('INBOX.TestBox');
$handler->handler->countUnseen('INBOX.TestBox');
```

#### Mail List

```php
$handler->getHeaders('INBOX')
```