In this section, we explain:

- What SQL injection (SQLi) is.
- How to find and exploit different types of SQLi vulnerabilities.
- How to prevent SQLi.
## What is SQL injection (SQLi)?

SQL injection (SQLi) is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. This can allow an attacker to view data that they are not normally able to retrieve. This might include data that belongs to other users, or any other data that the application can access. In many cases, an attacker can modify or delete this data, causing persistent changes to the application's content or behavior.

In some situations, an attacker can escalate a SQL injection attack to compromise the underlying server or other back-end infrastructure. It can also enable them to perform denial-of-service attacks.


## SQL injection examples

There are lots of SQL injection vulnerabilities, attacks, and techniques, that occur in different situations. Some common SQL injection examples include:

- [[Web Pen Testing/Burp Suite + Portswigger Academy/SQL Injection/04 How to exploit#Retrieving hidden data|Retrieving hidden data]], where you can modify a SQL query to return additional results.
- [[Web Pen Testing/Burp Suite + Portswigger Academy/SQL Injection/04 How to exploit#Subverting application logic|Subverting application logic]], where you can change a query to interfere with the application's logic.
- [[Web Pen Testing/Burp Suite + Portswigger Academy/SQL Injection/04 How to exploit#SQL injection UNION attacks|UNION attacks]], where you can retrieve data from different database tables.
- [[Web Pen Testing/Burp Suite + Portswigger Academy/SQL Injection/04 How to exploit#Blind SQL injection vulnerabilities|Blind SQL injection]], where the results of a query you control are not returned in the application's responses.


