# Race Conditions

## Lab: Limit overrun race conditions

Burp Suite, Group Repeater, Send in Sequence, Parallel. `/cart/coupon`


## Lab: Bypassing rate limits via race conditions

[Turbo Intruder](https://portswigger.net/research/turbo-intruder-embracing-the-billion-request-attack)


## Lab: Multi-endpoint race conditions

1. Find collision requests, both or more only depends on one cookie, e.g. Seesion.  
2. Benchmark request behaviors, like reponse timeing, connection warming. 
3. Prove/Implement collision, which may need several tries.  

In this lab, checkout request and cart-adding request are collision requests.  

