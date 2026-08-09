sudo nmap -sV -p- \[host]
sudo apt install redis-tools
[[redis-cli]] -h \[host]
info
select 0 
keys *
get flag
