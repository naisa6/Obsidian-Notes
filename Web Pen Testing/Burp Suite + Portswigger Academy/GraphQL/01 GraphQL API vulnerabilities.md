# GraphQL API vulnerabilities

GraphQL vulnerabilities generally arise due to implementation and design flaws. For example, the introspection feature may be left active, enabling attackers to query the API in order to glean information about its schema.

GraphQL attacks usually take the form of malicious requests that can enable an attacker to obtain data or perform unauthorized actions. These attacks can have a severe impact, especially if the user is able to gain admin privileges by manipulating queries or executing a CSRF exploit. Vulnerable GraphQL APIs can also lead to information disclosure issues.

In this section we'll look at how to test GraphQL APIs. Don't worry if you're not familiar with GraphQL - we'll cover the relevant details as we go. We've also provided some labs so you can practice what you've learned.