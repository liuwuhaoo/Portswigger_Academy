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


## Lab: Single-endpoint race conditions
Benchmark: Group in sequence, different emails, received emails in sequence `send 10 reset requests with different emails in sequence`;  
Probe: Group in parallel, different emails, received emails seems random timing, this is a collision; `send 10 reset requests with different emails in parallel`;   
Prove: Send two reset requests with two different emails in parallel, could receive emails targeted to both emials, one of them is the victim email.


## Lab: Exploiting time-sensitive vulnerabilities
Defense:  
1. Single thread processing for a single session, `session id` and `csrf id`;
2. Password reset token using timestamp.

Bypass:
, one of them is the victim email.


## 3f1403f9bc062876ba322de9bd500d8a13f2efDefense:  
1. Single thread processing for a single session, `session id` and `csrf id`;
2. Password reset token using timestamp.

Bypass:  
1. Start a new session to get two valid sessions;  
2. Send two password reset requests(one for victim) **at the same time** and get the valid reset token.
