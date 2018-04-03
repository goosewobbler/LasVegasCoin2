# VPS Masternode Setup Guide

The following sets up a single node called *lasvegascoin_node1* over IPV4.  It assumes you know how to send coins with a wallet, log into a remote server with SSH, create and edit text files on Linux.

## __Generate Node Configuration *(on master wallet)*__  

#### *< Tools menu => **Debug Console** >*  

Enter the following commands into the debug console:

```
getnewaddress lasvegascoin_node1
```

This will result in a new LasVegasCoin address.  Send exactly 1000 VGS to this address in a single transaction.  Funds can be sent to yourself - from the intended recipient wallet.


```
masternode genkey
```

The result of this is your MN private key.  You need it later.

```
masternode outputs
```

The result of this is your MN transaction id and index in the format _"{tx_id}": "{index}"_.  You need those later.  If the result is _{ }_, you have no valid transactions - check that exactly 1000 VGS has been sent to your wallet address.

## __Store Node Configuration *(on master wallet)*__ 

#### *< Tools menu => **Open Masternode Configuration File** >*

Add the following details in this file - *masternode.conf* - in the following format:

```
lasvegascoin_node1 {VPS IP address}:60702 {MN private key} {MN transaction id} {MN transaction index}
```

Save the file and close the wallet.

## __Set Up Node *(on VPS)*__  

Run the following commands to compile and install the VPS wallet:

```
git clone https://github.com/goosewobbler/vps.git && cd vps && ./install.sh -p lasvegascoin -n 4 -c 1
```

Now edit the VPS wallet config file:

```
vim /etc/masternodes/lasvegascoin_n1.conf
``` 

Insert your private key and the IP address of the VPS in the appropriate places and save the file.

Now activate the node locally:

```
/usr/local/bin/activate_masternodes_lasvegascoin
```

You will need to wait for the VPS wallet blockchain to fully sync before you can start the node remotely.  
Keep running this command, eventually the value of 'blocks' will stabilise:
 
```
/usr/local/bin/lasvegascoin-cli -conf=/etc/masternodes/lasvegascoin_n1.conf getinfo
```

The value of 'blocks' will increase until it reaches the 'up to block' value on the block explorer:

https://chainz.cryptoid.info/vgs/#



## __Activate Node Remotely *(on master wallet)*__

Once the VPS wallet is fully synced, you can start the node remotely.  Open the master wallet again, click on the Masternodes tab and click *Start MISSING*.

#### *< Masternodes tab => My Masternodes => Start MISSING >*



## __Verify Node Status *(on VPS)*__

You can verify the status of the VPS masternode with the following command.  This, along with getinfo

```
/usr/local/bin/lasvegascoin-cli -conf=/etc/masternodes/lasvegascoin_n1.conf masternode status
```

---  

## Disclaimer

This has been tested with Vultr Ubuntu 16.04 instance ($5/month).  The $2.50 instances (1 CPU / 512MB Memory / 500GB Bandwidth) should be fine for a single node - if sold out, use the $5 instance.  Similarly specified VPS instances from other providers will also work fine.

The script found at https://github.com/goosewobbler/vps is the Nodemaster script by [@Marsmench](https://twitter.com/Marsmensch), forked for LasVegasCoin integration. 

Nodemaster original/upstream repo:  
https://github.com/masternodes/vps

Nodemaster VPS guide:  
https://masternodes.github.io/vps/docs/masternode_vps.html

You can sign up for Vultr [here](https://www.vultr.com/?ref=7282546).
