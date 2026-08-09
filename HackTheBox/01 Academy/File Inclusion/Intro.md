- Many modern back-end languages, such as `PHP`, `Javascript`, or `Java`, use HTTP parameters to specify what is shown on the web page, which allows for building dynamic web pages, reduces the script's overall size, and simplifies the code. In such cases, parameters are used to specify which resource is shown on the page
- most common place we usually find LFI within is templating engines
- templating engine displays a page that shows the common static parts, such as the `header`, `navigation bar`, and `footer`, and then dynamically loads other content that changes between pages.
- LFI vulnerabilities can lead to source code disclosure, sensitive data exposure, and even remote code execution under certain conditions
- file Inclusion vulnerabilities can occur in many of the most popular web servers and development frameworks, like `PHP`, `NodeJS`, `Java`, `.Net`, and many others. Each of them has a slightly different approach to including local files, but they all share one common thing: loading a file from a specified path.

#### PHP
- `include()` function to load a local or a remote file as we load a page. If the `path` passed to the `include()` is taken from a user-controlled parameter, like a `GET` parameter, and `the code does not explicitly filter and sanitize the user input`, then the code becomes vulnerable to File Inclusion.
- E.g. ```
```
  if (isset($_GET['language'])) {
    include($_GET['language']);
}
```
- there are many other PHP functions that would lead to the same vulnerability if we had control over the path passed into them, e.g. `include_once()`, `require()`, `require_once()`, `file_get_contents()`, etc.

#### NodeJS
- Just as the case with PHP, NodeJS web servers may also load content based on an HTTP parameters. The following is a basic example of how a GET parameter `language` is used to control what data is written to a page:
```
if(req.query.language) {
    fs.readFile(path.join(__dirname, req.query.language), function (err, data) {
        res.write(data);
    });
}
```
- whatever parameter passed from the URL gets used by the `readfile` function, which then writes the file content in the HTTP response
- Another example is the `render()` function in the `Express.js` framework. The following example shows how the `language` parameter is used to determine which directory to pull the `about.html` page from:
```
app.get("/about/:language", function(req, res) { res.render(`/${req.params.language}/about.html`); });
```

#### Java
- same concept applies to many other web servers. The following examples show how web applications for a Java web server may include local files based on the specified parameter, using the `include` function:
```
<c:if test="${not empty param.language}">
    <jsp:include file="<%= request.getParameter('language') %>" />
</c:if>
```
- The `include` function may take a file or a page URL as its argument and then renders the object into the front-end template
- The `import` function may also be used to render a local file or a URL, such as the following example:
`<c:import url= "<%= request.getParameter('language') %>"/>`

#### .NET
- File Inclusion vulnerabilities may occur in .NET web applications. The `Response.WriteFile` function takes a file path for its input and writes its content to the response. The path may be retrieved from a GET parameter for dynamic content loading, as follows:
```
@if (!string.IsNullOrEmpty(HttpContext.Request.Query['language'])) {
    <% Response.WriteFile("<% HttpContext.Request.Query['language'] %>"); %> 
}
```
- Furthermore, the `@Html.Partial()` function may also be used to render the specified file as part of the front-end template:
```
@Html.Partial(HttpContext.Request.Query['language'])
```
- the `include` function may be used to render local files or remote URLs, and may also execute the specified files as well:
```
<!--#include file="<% HttpContext.Request.Query['language'] %>"-->
```

#### Read vs Execute
- File Inclusion vulnerabilities may occur in any web server and any development frameworks, as all of them provide functionalities for loading dynamic content and handling front-end templates.
- `some of the above functions only read the content of the specified files, while others also execute the specified files`.
- some of them allow specifying remote URLs, while others only work with files local to the back-end server.

| Function                     | Read Content | Execute | Remote URL |
| ---------------------------- | :----------: | :-----: | :--------: |
| **PHP**                      |              |         |            |
| `include()`/`include_once()` |      ✅       |    ✅    |     ✅      |
| `require()`/`require_once()` |      ✅       |    ✅    |     ❌      |
| file_get_contents()          |      ✅       |    ❌    |     ✅      |
| `fopen()`/`file()`           |      ✅       |    ❌    |     ❌      |
| **NodejS**                   |              |         |            |
| fs.readFile()                |      ✅       |    ❌    |     ❌      |
| fs.sendFile()                |      ✅       |    ❌    |     ❌      |
| res.render()                 |      ✅       |    ✅    |     ❌      |
| **Java**                     |              |         |            |
| include                      |      ✅       |    ❌    |     ❌      |
| import                       |      ✅       |    ✅    |     ✅      |
| .**NET**                     |              |         |            |
| @Html.Partial()              |      ✅       |    ❌    |     ❌      |
| @Html.RemotePartial()        |      ✅       |    ❌    |     ✅      |
| Response.WriteFile()         |      ✅       |    ❌    |     ❌      |
| include                      |      ✅       |    ✅    |     ✅      |

