# Codes

Supported codes are:

* C25
* C39
* C128 (C128a and C128b)
* Codebar
* Datamatrix
* Aztec
* QR

All codes expect the string to encode as parameter, the basic dimensions (width, height) and optionally the orientation for barcodes. The horizontal barcode image size will be adjusted automatically based on its length.

```php
$c128b = new C128b('ABcdeFG0123+-!@?', 200, 50);
$c128b->saveToPngFile('path/code.png');
```

The barcodes can be either saved as `png` and `jpeg` or outputted directly.

![C128b Code](Developer-Guide/services/codes1.png)

```php
$c128b = new C128b('ABcdeFG0123+-!@?', 200, 50);
$c128b = $C128a->get();
header ('Content-type: image/png');
imagepng($c128b);
imagedestroy($c128b);
```
