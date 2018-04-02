[size=12pt][u][b]VPS Masternode Setup Guide[/b][/u][/size]

The following sets up one node called lasvegas_node1 over IPV4, and assumes you know how to send yourself funds with a wallet, and can use the linux editor vim.

[b][i]<on master wallet>[/i][/b]

[i]<Send 1000 VGS to yourself - must be exact>[/i]
[i]<Tools menu, debug console>[/i]

Enter the following commands:

[code]masternode genkey[/code] 
The result of this is your MN private key.  You need it later.

[code]masternode outputs[/code] 
The result of this is your MN transaction id and index in the format "{tx_id}": "{index}".  You need those later.

[i]<Tools menu, open masternode configuration file>[/i]

Add the following details in this file in the following format:

[code]lasvegas_node1 {VPS IP address}:60702 {MN private key} {MN transaction id} {MN transaction index}[/code]

Save the file and close the wallet.  Open the wallet again.

[b][i]<on VPS>[/i][/b]
Run the following commands to compile and install the VPS wallet:

[code]
git clone https://github.com/goosewobbler/vps.git && cd vps && ./install.sh -p lasvegascoin -n 4 -c 1
[/code]

Now edit the VPS wallet config file:

[code]vim /etc/masternodes/lasvegascoin_n1.conf[/code] 

Insert your private key and the IP address of the VPS in the appropriate places and save the file.

Now activate the node locally:

[code]/usr/local/bin/activate_masternodes_lasvegascoin[/code]

You will need to wait for the VPS wallet blockchain to fully sync before you can start the node remotely.  
Keep running this command, eventually the value of 'blocks' will stabilise:
 
[code]/usr/local/bin/lasvegascoin-cli -conf=/etc/masternodes/lasvegascoin_n1.conf getinfo[/code]

The value of 'blocks' will increase until it reaches the 'up to block' value on the block explorer:

[url=https://chainz.cryptoid.info/vgs/#]https://chainz.cryptoid.info/vgs/#[/url]

When the VPS wallet is fully synced, you can start the node remotely:

[b][i]<on master wallet>[/i][/b]
[i]<Masternodes => My Masternodes => Start MISSING>[/i]

You can verify the status of the VPS wallet and masternode with the following:

[b][i]<on VPS>[/i][/b]
[code]
/usr/local/bin/lasvegascoin-cli -conf=/etc/masternodes/lasvegascoin_n1.conf masternode status
[/code]


[size=12pt][u][b]Disclaimer[/b][/u][/size]

This has been tested with Vultr Ubuntu 16.04 instance ($5/month).  The $2.50 instances should be fine for a single node, if sold out use $5.

Sign up for Vultr here:  [url=https://www.vultr.com/?ref=7282546]https://www.vultr.com/?ref=7282546[/url]

