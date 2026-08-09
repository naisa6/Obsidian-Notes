**

- Check source dev Tools:  
```
<script>
    

var isAdmin = false;

if (isAdmin) {

...

var adminPanelTag = document.createElement('a');

adminPanelTag.setAttribute('href', 'https://insecure-website.com/administrator-panel-yb556');

adminPanelTag.innerText = 'Admin panel';

...

}

</script>
```

- Some application frameworks support various non-standard HTTP headers that can be used to override the URL in the original request, such as X-Original-URL and X-Rewrite-URL
    
- Find a blog post by carlos. Click on carlos and observe that the URL contains his user ID. Make a note of this ID.
    
- ATTENTION: If you find roleid on your exam and numbers from 0 to 10 are not working, brute it from 0 to 100 via Intruder.
    

**