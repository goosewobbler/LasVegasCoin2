# VPS Masternode Setup Guide

The following sets up a single node called *lasvegascoin_node1* over IPV4.  It assumes you know how to send yourself funds with a wallet, and can use the linux editor vim.

## *__on master wallet__*

#### *< Tools menu => **Debug Console** >*  

Enter the following commands:

```
getnewaddress lasvegascoin_node1
```

This will result in a LasVegasCoin address.  Send exactly 1000 VGS to this address in a single transaction.


```
masternode genkey
```

The result of this is your MN private key.  You need it later.

```
masternode outputs
```

The result of this is your MN transaction id and index in the format _"{tx_id}": "{index}"_.  You need those later.  If the result is _{ }_, you have no valid transactions - check that exactly 1000 VGS has been sent to your wallet address.

#### *< Tools menu => **Open Masternode Configuration File** >*

Add the following details in this file in the following format:

```
lasvegascoin_node1 {VPS IP address}:60702 {MN private key} {MN transaction id} {MN transaction index}
```

Save the file and close the wallet.  Open the wallet again.

## *__on VPS__*  

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

When the VPS wallet is fully synced, you can start the node remotely:

## *__on master wallet__*
#### *< Masternodes tab => My Masternodes => Start MISSING >*

You can verify the status of the VPS wallet and masternode with the following:

## *__on VPS__*  

```
/usr/local/bin/lasvegascoin-cli -conf=/etc/masternodes/lasvegascoin_n1.conf masternode status
```

---  

## Disclaimer

This has been tested with Vultr Ubuntu 16.04 instance ($5/month).  The $2.50 instances should be fine for a single node, if sold out use $5.

You can sign up for Vultr [here](https://www.vultr.com/?ref=7282546).
