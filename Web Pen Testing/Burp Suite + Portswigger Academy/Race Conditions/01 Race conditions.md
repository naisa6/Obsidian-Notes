Race conditions are a common type of vulnerability closely related to business logic flaws. They occur when websites process requests concurrently without adequate safeguards. This can lead to multiple distinct threads interacting with the same data at the same time, resulting in a "collision" that causes unintended behavior in the application. A race condition attack uses carefully timed requests to cause intentional collisions and exploit this unintended behavior for malicious purposes.

![[race-conditions-main-graphic.png]]


The period of time during which a collision is possible is known as the "race window". This could be the fraction of a second between two interactions with the database, for example.

Like other logic flaws, the impact of a race condition is heavily dependent on the application and the specific functionality in which it occurs.

In this section, you'll learn how to identify and exploit different types of race condition. We'll teach you how Burp Suite's built-in tooling can help you to overcome the challenges of performing classic attacks, plus a tried and tested methodology that enables you to detect novel classes of race condition in hidden multi-step processes. These go far beyond the limit overruns that you may be familiar with already.

## Limit overrun race conditions

The most well-known type of race condition enables you to exceed some kind of limit imposed by the business logic of the application.

For example, consider an online store that lets you enter a promotional code during checkout to get a one-time discount on your order. To apply this discount, the application may perform the following high-level steps:

1. Check that you haven't already used this code.
2. Apply the discount to the order total.
3. Update the record in the database to reflect the fact that you've now used this code.

If you later attempt to reuse this code, the initial checks performed at the start of the process should prevent you from doing this:

![[race-conditions-discount-code-normal.png]]

Now consider what would happen if a user who has never applied this discount code before tried to apply it twice at almost exactly the same time:

![[race-conditions-discount-code-race.png]]

As you can see, the application transitions through a temporary sub-state; that is, a state that it enters and then exits again before request processing is complete. In this case, the sub-state begins when the server starts processing the first request, and ends when it updates the database to indicate that you've already used this code. This introduces a small race window during which you can repeatedly claim the discount as many times as you like.

There are many variations of this kind of attack, including:

- Redeeming a gift card multiple times
- Rating a product multiple times
- Withdrawing or transferring cash in excess of your account balance
- Reusing a single CAPTCHA solution
- Bypassing an anti-brute-force rate limit

Limit overruns are a subtype of so-called "time-of-check to time-of-use" (TOCTOU) flaws. Later in this topic, we'll look at some examples of race condition vulnerabilities that don't fall into either of these categories.



