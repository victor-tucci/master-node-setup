# Full master node setup guide


This guide will walk you through the complete process of setting up, staking, and running a Beldex Master Node.

## Beldex

- Port 29089 (storage server to storage server)
- Port 29090 (client to storage server)
- Port 19090 (blockchain syncing)
- Port 19095 (master Node to master Node)
- Port 1090 (UDP, not TCP, unlike all of the above; Belnet router data)

## LINUX
- get your public ip
```sh
curl -s ifconfig.me
```
## Daemon setup
 - Downlode zip package
 ```sh
sudo apt install wget zip -y
```
- Downlode beldex binaries
```sh
wget https://deb.beldex.io/Beldex-projects/Beldex/deps/v4.0.0/Linux/beldex-linux-x86_64-v4.0.0.zip
```
- Extract the file
```sh
unzip beldex-linux-x86_64-v4.0.0.zip
cd beldex-linux-x86_64-v4.0.0
```
- open tmux 
```sh
tmux new -s node
```
- Run beldexd
```sh
./beldexd --master-node --master-node-public-ip <ip> --storage-server-port 29090
```
- Once run the daemon 
```sh
CTRL+b and d
```
## beldex-storage-server setup
- Downlode beldex-storage-server binaries
```sh
wget https://deb.beldex.io/Beldex-projects/Beldex-storage-server/deps/v2.2.0/beldex-storage-linux-x86_64-v2.2.0.zip
```
- Extract the file
```sh
unzip beldex-storage-linux-x86_64-v2.2.0.zip
cd beldex-storage-linux-x86_64-v2.2.0
```
- open tmux 
```sh
tmux new -s storage-server
```
- Run beldex-storage
```sh
./beldex-storage 0.0.0.0 29090 --omq-port 29089
```
- Once run the storage-server
```sh
CTRL+b and d
```


## Belnet setup

- Downlode beldex-storage-server binaries
```sh
wget https://deb.beldex.io/Beldex-projects/Belnet/deps/v0.9.5/Belnet-linux-x86_64-v0.9.5.zip
```
- Extract the file
```sh
unzip Belnet-linux-x86_64-v0.9.5
cd Belnet-linux-x86_64-v0.9.5
```
- open tmux 
```sh
tmux new -s belnet
```
- Run belnet-bootstrap
```sh
./belnet-bootstrap
./belnet -r -g
```
- configure the belnet.ini
```sh
sudo vim /var/lib/belnet.ini
PRESS <insert> button
```
- add your public-ip and enable port in belnet.ini
 ```sh
 #data-dir=/var/lib/belnet -> data-dir=/var/lib/belnet
 #public-ip -> public-ip <your ip>
 #public-port -> public-port=1090
 ```
 - configure your beldexd.sock in belnet.ini
 verify .beldex directory in /root/.beldex or /home/USER/.beldex
  USER -> your user
 ```sh
  rpc=ipc:///root/.beldex/beldexd.sock 
  or
  rpc=ipc:///home/USER/.beldex/beldexd.sock
```
  enable which is suitable to you
  - save the belnet.ini file
  ```sh
  PRESS Esc and Shift + : then type wq then Enter
  ```
- Run the belnet in router mode
```sh
./belnet -r
CTRL+b and d
```
## prepare_registration
- open node tmux
```sh
tmux attach -t node
```
- give prepare registration command in daemon
```sh
prepare_registration
```
Current staking requirement: 10000.000000000 beldex
Will the operator contribute the entire stake? (Y/Yes/N/No/C/Cancel)
```sh
Type Y and click enter
```
Enter the beldex address for the solo staker (B/Back/C/Cancel):
```sh
paste the beldex address into the terminal and press Enter
```
Summary:
Operating costs as % of reward: 100%
Contributor     Address  Contribution       Contribution(%)
___________     _______  ____________       _______________
Operator        bxdsoF   10000.000000000    100.000000000

Because the actual requirement will depend on the time that you register, the
amounts shown here are used as a guide only, and the percentages will remain
the same.

Do you confirm the information above is correct? (Y/Yes/N/No/B/Back/C/Cancel):
```sh
type Y and click enter
```
The daemon will output a command which looks similar to:
```sh
register_master_node 18446744073709551612 bxfsoFt5jATGti4mSSR2GbBarRrorBb3rJznj7grJjmBNHVXCRuGNSmAw9ogGFgPWhi8Lf5qMjMcWAThdw2eUWRP2Q3rbgRY7 18426744073709551612 1638876090 dee3484363f443ecfcc73770beb370841710bfb89157b0691809f2d3c186e8e0 f368785551675d4568f09f94a98f810a9541a5811d09c8c8245c59ad0d8e1e07ba7192db7967299cfe183d68dcc0290c54e4facfe9155a483fec6a7378194c09
```
copy the command and close the tmux by
```sh
CTRL+b and d
```
Paste the whole text in wallet
```sh
Paste this text in electron wallet
```
## status check
- open node tmux 
```sh
tmux attach -t node
```

Then check the status of your Master node by type the status command .It shows the status of your Master Node.
The daemon will output a command which looks similar to:
```sh
status
```
Height: 729942, net hash 1.92 kH/s, v4.0.0-0ffabefca(net v12), 7(out)+29(in) connections, uptime 0d0h37m5s
MN: dee3484363f423ecfcc73770beb370841710bfb89157b0691809f2d3c186e8e0 active, proof: 20.4 minutes ago, last pings: 18sec (storage), 19sec (belnet)
