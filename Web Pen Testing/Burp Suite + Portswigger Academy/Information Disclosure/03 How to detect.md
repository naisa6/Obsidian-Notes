## How to test for information disclosure vulnerabilities

Generally speaking, it is important not to develop "tunnel vision" during testing. In other words, you should avoid focussing too narrowly on a particular vulnerability. Sensitive data can be leaked in all kinds of places, so it is important not to miss anything that could be useful later. You will often find sensitive data while testing for something else. A key skill is being able to recognize interesting information whenever and wherever you do come across it.

The following are some examples of high-level techniques and tools that you can use to help identify information disclosure vulnerabilities during testing.

### Fuzzing

If you identify interesting parameters, you can try submitting unexpected data types and specially crafted fuzz strings to see what effect this has. Pay close attention; although responses sometimes explicitly disclose interesting information, they can also hint at the application's behavior more subtly. For example, this could be a slight difference in the time taken to process the request. Even if the content of an error message doesn't disclose anything, sometimes the fact that one error case was encountered instead of another one is useful information in itself.

You can automate much of this process using tools such as Burp Intruder. This provides several benefits. Most notably, you can:

- Add payload positions to parameters and use pre-built wordlists of fuzz strings to test a high volume of different inputs in quick succession.
- Easily identify differences in responses by comparing HTTP status codes, response times, lengths, and so on.
- Use grep matching rules to quickly identify occurrences of keywords, such as `error`, `invalid`, `SELECT`, `SQL`, and so on.
- Apply grep extraction rules to extract and compare the content of interesting items within responses.

You can also use the [Logger++](https://portswigger.net/bappstore/470b7057b86f41c396a97903377f3d81) extension, available from the BApp store. In addition to logging requests and responses from all of Burp's tools, it allows you to define advanced filters for highlighting interesting entries. This is just one of the many Burp extensions that can help you find any sensitive data that is leaked by the website.

### Using Burp Scanner

Burp Suite Professional users have the benefit of Burp Scanner. This provides live scanning features for auditing items while you browse, or you can schedule automated scans to crawl and audit the target site on your behalf. Both approaches will automatically flag many information disclosure vulnerabilities for you. For example, Burp Scanner will alert you if it finds sensitive information such as private keys, email addresses, and credit card numbers in a response. It will also identify any backup files, directory listings, and so on.

### Using Burp's engagement tools

Burp provides several engagement tools that you can use to find interesting information in the target website more easily. You can access the engagement tools from the context menu - just right-click on any HTTP message, Burp Proxy entry, or item in the site map and go to "Engagement tools".

The following tools are particularly useful in this context.

#### Search

You can use this tool to look for any expression within the selected item. You can fine-tune the results using various advanced search options, such as regex search or negative search. This is useful for quickly finding occurrences (or absences) of specific keywords of interest.

#### Find comments

You can use this tool to quickly extract any developer comments found in the selected item. It also provides tabs to instantly access the HTTP request/response cycle in which each comment was found.

#### Discover content

You can use this tool to identify additional content and functionality that is not linked from the website's visible content. This can be useful for finding additional directories and files that won't necessarily appear in the site map automatically.

### Engineering informative responses

Verbose error messages can sometimes disclose interesting information while you go about your normal testing workflow. However, by studying the way error messages change according to your input, you can take this one step further. In some cases, you will be able to manipulate the website in order to extract arbitrary data via an error message.

There are numerous methods for doing this depending on the particular scenario you encounter. One common example is to make the application logic attempt an invalid action on a specific item of data. For example, submitting an invalid parameter value might lead to a stack trace or debug response that contains interesting details. You can sometimes cause error messages to disclose the value of your desired data in the response.

• Review error handling mechanisms and test for potential information disclosure in error messages.

• Check for sensitive information exposed in server responses, headers, or log files.

• Assess directory listing vulnerabilities that may reveal directory or file structures.

• Verify if secure configuration settings are in place to prevent unnecessary information disclosure.

• Test for potential data leakage through insecure data transmission channels.

Find:

• Exif

• sensitive info in source code

• API keys

• Private content

• Tokens

• Base64 blobs

• Access token leak

• Sensitive info in invites

• Add headers and parameters found elsewhere in the app

• Hidden fields