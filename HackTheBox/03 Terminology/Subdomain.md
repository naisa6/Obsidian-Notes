What is a subdomain?
A subdomain name is a piece of additional information added to the beginning of a website’s domain name.
It allows websites to separate and organize content for a specific function — such as a blog or an online store — from the rest of your website.
For example, if we visit hackthebox.com we can access the main website. Or, we can visit
ctf.hackthebox.com to access the section of the website that is used for CTFs. In this case, ctf is the
subdomain, hackthebox is the primary domain and com is the top-level domain (TLD). Although the URL changes slightly, you’re still on HTB's website, under HTB's domain.
Often, different subdomains will have different IP addresses, so when our system goes to look up the subdomain, it gets the address of the server that handles that application. It is also possible to have one server handle multiple subdomains. This is accomplished via "host-based routing", or "virtual host routing", where the server uses the Host header in the HTTP request to determine which application is meant to handle the request.