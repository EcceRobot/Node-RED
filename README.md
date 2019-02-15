# Node-RED


## Set up Node-RED

First of all, if starting from a minimal Debian configuration, install buidl-essential
```
sudo apt install build-essential
```

Node-RED requires **Node.js LTS 8.x**, so check the current version:
```
node -v
```
If the version is not correct, send the commands as **root**, not with sudo
```
# curl -sL https://deb.nodesource.com/setup_8.x | bash -
# apt install -y nodejs
```

Then, you can use **npm** - **n**ode **p**ackage **m**anager.
Check if it's present (should have been installed with nodejs)
```
npm -v
```

Now you can install Node-RED
```
sudo npm install -g --unsafe-perm node-red
```

Then you can start Node-RED with the commmand
```
$ node-red
```
and you will get
```
Welcome to Node-RED

25 Mar 22:51:09 - [info] Node-RED version: v0.18.4
25 Mar 22:51:09 - [info] Node.js  version: v8.11.1
25 Mar 22:51:09 - [info] Loading palette nodes
25 Mar 22:51:10 - [warn] ------------------------------------------
25 Mar 22:51:10 - [warn] [rpi-gpio] Info : Ignoring Raspberry Pi specific node
25 Mar 22:51:10 - [warn] ------------------------------------------
25 Mar 22:51:10 - [info] Settings file  : /home/nol/.node-red/settings.js
25 Mar 22:51:10 - [info] User Directory : /home/nol/.node-red
25 Mar 22:51:10 - [info] Server now running at http://127.0.0.1:1880/
25 Mar 22:51:10 - [info] Creating new flows file : flows_noltop.json
25 Mar 22:51:10 - [info] Starting flows
25 Mar 22:51:10 - [info] Started flows
```

You can then access the Node-RED editor by pointing your browser at http://localhost:1880.

### Storing flows

By default, Node-RED stores your flows in the directory $HOME/.node-red

### Starting Node-RED on boot
```
[Service]
Type=simple
# change to the user name you wish to run Node-RED as
User=**debian**
Group=**debian**
WorkingDirectory=/home/**debian**
```



The preferred way to autostart Node-RED is to use the built in systemd capability. The pre-installed version does this by using a nodered.service file and start and stop scripts. You may install these by running the following commands
```
sudo wget https://raw.githubusercontent.com/node-red/raspbian-deb-package/master/resources/nodered.service -O /lib/systemd/system/nodered.service
sudo wget https://raw.githubusercontent.com/node-red/raspbian-deb-package/master/resources/node-red-start -O /usr/bin/node-red-start
sudo wget https://raw.githubusercontent.com/node-red/raspbian-deb-package/master/resources/node-red-stop -O /usr/bin/node-red-stop
sudo chmod +x /usr/bin/node-red-st*
sudo systemctl daemon-reload
```
**Info:** These commands are run as root (sudo) - They download the three required files to their correct locations, make the two scripts executable and then reload the systemd daemon.

Node-RED can then be started and stopped by using the commands node-red-start and node-red-stop

To then enable Node-RED to run automatically at every boot and upon crashes
```
sudo systemctl enable nodered.service
```
It can be disabled by
```
sudo systemctl disable nodered.service
```
Systemd uses the */var/log/system.log* for logging. To filter the log use
```
sudo journalctl -f -u nodered -o cat
```
