# HOW I BUILT MY PROOF OF AUTHORITY BLOCKCHAIN
 
 ## 
 In this assignment, the custom POA is built with the following tools:

•  `Puppeth` - to generate the genesis block

•  `Geth` - a command-line tool to create keys, initialize nodes, and connect the nodes together

•  The `Clique` Proof of Authority algorithm.

Proof of authority (PoA) is an algorithm used with blockchains that delivers comparatively fast transactions through a consensus mechanism based on identity as a stake.
One of the advantages of POA in addition to reducing the amount of power necessary to maintain the network, the Proof of Authority algorithm simplifies the validation process. The consensus also has a series of benefits that should be considered

Below are the steps and instructions for setting up the `PoA` blockchain.

# Instructions

## Generate Blockchain Nodes

Two nodes with new account address were generated since the new account addresses will serve as the pre-approved sealer addresses.

To generate the nodes, we will use **Geth** and create two nodes, **Node 11** and **Node 22** by executing the following commands in terminals such as Git Bash:

**Create Node 11:**

`./geth --datadir node11 account new`

**Create Node 22:**

`./geth --datadir node2 account new`

It is important that to run these commands from the folder in which Geth installed on my computer. Once the two nodes have been generated, there will be two folders created – `node11` and `node22` – each containing a folder with a keystore for that node.

## Generate your Genesis Block
Next I used **puppeth** to generate the genesis block following the steps below:

- Running `puppeth` and naming my network, I selected the option to `configure a new genesis block`. I named my PoA network `samchain`.

- I chose the `Clique` (Proof of Authority) consensus algorithm.

- Paste both account addresses from the first step one at a time into the list of accounts to seal.

- Paste both accounts again in the list of accounts to pre-fund. This is required since there are no block rewards in PoA.

- Choose no for pre-funding the pre-compiled accounts (0x1 .. 0xff) with wei.

- Complete the rest of the prompts, and when you are back at the main menu, choose the "Manage existing genesis" option.

- Export genesis configurations. This will fail to create two of the files, but we only need `dlchain.json`.

## Initialize the Nodes with the Genesis' Json File

Using geth, I initialize each node with the new `samchain.json`.

Open side by side two Git Bash windows, one for Node 11 and the other for Node 22, and run the following commands:

`./geth --datadir node11 init samchain.json`

`./geth --datadir node22 init samchain.json`


## Begin Mining Blocks

I ran the nodes in separate terminal windows using the commands below:

`./geth --datadir node11 --unlock "SEALER_ONE_ADDRESS" --mine --rpc --allow-insecure-unlock`

`./geth --datadir node22 --unlock "SEALER_TWO_ADDRESS" --mine --port 30304 --bootnodes "enode://SEALER_ONE_ENODE_ADDRESS@127.0.0.1:30303" --ipcdisable --allow-insecure-unlock`

**NOTE**: I Provided my password and hit enter !

The PoA blockchain is now up and running.
![StartMining](POA-Development-Chain/Screenshots/mining3.jpg)

## To Add the New Blockchain to MyCrypto for Testing, I followed the steps below

- Open the `MyCrypto` app, then click Change Network at the bottom left
- Click "Add Custom Node", then add the custom network information that you set in the genesis.
- Make sure that you scroll down to choose Custom in the "Network" column to reveal more options like Chain ID:
![SetUpYourCustomNode](POA-Development-Chain/Screenshots/CustomNode.png)

- Type `ETH` in the Currency box.
- In the Chain ID box, type the chain id you generated during genesis creation. 
- In the URL box type: `http://127.0.0.1:8545`.  This points to the default RPC port on your local machine.

- Finally, click `Save & Use Custom Node`.

## After connecting to the custom network in MyCrypto, it can be tested by sending money between accounts.

- Select the View & Send option from the left menu pane, then click Keystore file.

![SetUpYourCustomNode](POA-Development-Chain/Screenshots/KeystoreFile.png)

- On the next screen, click Select `Wallet File`, then navigate to the keystore directory inside your Node11 directory, select the file located there, provide your password when prompted and then click `Unlock`.

![SetUpYourCustomNode](POA-Development-Chain/Screenshots/UnlockKeystoreFile.png)
![SetUpYourCustomNode](POA-Development-Chain/Screenshots/KeystorePassword.png)

- This will open your account wallet inside MyCrypto.

- You will see that your account has a huge balance. This is the balance that was pre-funded for this account in the genesis configuration; however, these millions of ETH tokens are just for testing purposes.
![SetUpYourCustomNode](POA-Development-Chain/Screenshots/AccountBalance.png)

- In the To Address box, type the account address from Node22, then fill in an arbitrary amount of ETH:
![SetUpYourCustomNode](POA-Development-Chain/Screenshots/ToAccount.png)

- Confirm the transaction by clicking "Send Transaction", and the "Send" button in the pop-up window.
![SetUpYourCustomNode](POA-Development-Chain/Screenshots/ConfirmTransaction.png)

- Click the Check TX Status when the green message pops up, confirm the logout:
![SetUpYourCustomNode](POA-Development-Chain/Screenshots/TxConf.png)
![SetUpYourCustomNode](POA-Development-Chain/Screenshots/AbtL.png)

- You should see the transaction go from Pending to Successful in around the same blocktime you set in the genesis.

- You can click the Check TX Status button to update the status.
![SetUpYourCustomNode](POA-Development-Chain/Screenshots/TxSucc.png)

- This confirms that you have successfully created your own private blockchain!