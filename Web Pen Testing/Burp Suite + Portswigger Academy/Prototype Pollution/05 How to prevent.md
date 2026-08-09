## Preventing GraphQL attacks

To prevent many common GraphQL attacks, take the following steps when you deploy your API to production:

- If your API is not intended for use by the general public, disable introspection on it. This makes it harder for an attacker to gain information about how the API works, and reduces the risk of unwanted information disclosure.
    
    For information on how to disable introspection in the Apollo GraphQL platform, see [this blog post](https://www.apollographql.com/blog/graphql/security/why-you-should-disable-graphql-introspection-in-production/#turning-off-introspection-in-production).
    
- If your API is intended for use by the general public then you will likely need to leave introspection enabled. However, you should review the API's schema to make sure that it does not expose unintended fields to the public.
    
- Make sure that suggestions are disabled. This prevents attackers from being able to use Clairvoyance or similar tools to glean information about the underlying schema.
    
    You cannot disable suggestions directly in Apollo. See [this GitHub thread](https://github.com/apollographql/apollo-server/issues/3919#issuecomment-836503305) for a workaround.
    
- Make sure that your API's schema does not expose any private user fields, such as email addresses or user IDs.
    

### Preventing GraphQL brute force attacks

It is sometimes possible to bypass standard rate limiting when using GraphQL APIs. For an example of this, see the [Bypassing rate limiting using aliases](https://portswigger.net/web-security/graphql#bypassing-rate-limiting-using-aliases) section.

With this in mind, there are design steps that you can take to defend your API against brute force attacks. This generally involves restricting the complexity of queries accepted by the API, and reducing the opportunity for attackers to execute denial-of-service (DoS) attacks.

To defend against brute force attacks:

- Limit the query depth of your API's queries. The term "query depth" refers to the number of levels of nesting within a query. Heavily-nested queries can have significant performance implications, and can potentially provide an opportunity for DoS attacks if they are accepted. By limiting the query depth your API accepts, you can reduce the chances of this happening.
    
- Configure operation limits. Operation limits enable you to configure the maximum number of unique fields, aliases, and root fields that your API can accept.
    
- Configure the maximum amount of bytes a query can contain.
    
- Consider implementing cost analysis on your API. Cost analysis is a process whereby a library application identifies the resource cost associated with running queries as they are received. If a query would be too computationally complex to run, the API drops it.
    

#### More information

For information on how to implement these features in Apollo, see [this blog post](https://www.apollographql.com/blog/graphql/security/securing-your-graphql-api-from-malicious-queries/).

### Preventing CSRF over GraphQL

To defend against GraphQL CSRF vulnerabilities specifically, make sure of the following when designing your API:

- Your API only accepts queries over JSON-encoded POST.
    
- The API validates that content provided matches the supplied content type.
    
- The API has a secure CSRF token mechanism.