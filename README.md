# HOW I BUILT MY PROOF OF AUTHORITY BLOCKCHAIN
 
 ## SETTING BlOCKCHAIN MINNING TOOLS - GO ETHEREUM TOOLS
THe Go Ethereum Tools is one of the three original implementations of the Ethereum protocol. It is written in Go, fully open-source and licensed under the GNU LGPL v3.

In order to be able to build the POA, we will need to install the following tools

•  `Puppeth` - to generate the genesis block

•  `Geth` - a command-line tool to create keys, initialize nodes, and connect the nodes together

•  The `Clique` Proof of Authority algorithm.

The `Proof of Authority (PoA)` algorithm is typically used for private blockchain networks as it requires pre-approval of, or voting in of, the account addresses that can approve transactions (seal blocks).

Below are the steps to follows to install the GO Ethereum tools

 (1) Open your browser and navigate to the Go Ethereum Tools download page at https://geth.ethereum.org/downloads/

 (2) Scroll down to the "Stable Releases" section and proceed depending on your operating system. Depending on the OS and the version of the Windows version, you should download the 32 bit or 64 bit version of the Go Ethereum Tools.

 (3) After downloading the tools archive, open your "Downloads" folder, and you will find a file named geth-alltools-darwin-amd64-1.9.7-a718daa6.tar.gz in OS X, and a file called geth-alltools-windows-amd64-1.9.7-a718daa6.zip in Windows. Note that the last numbers in the filename could vary depending on the last built available.
 
 (4) Decompress the archive in the location of your preference in your computer's hard drive, and rename the containing folder as Blockchain-Tools. We recommend using a location that can be easily accessed from the terminal window like the user's home directory.

 ![SetUpYourCustomNode](Images/Environment.png)
 (5)  You have finished the installation process; you will use these tools to create your very own blockchain!






In what follows, I present the instructions for setting up the `PoA` blockchain.

# Instructions

## Generate Blockchain Nodes
Because the accounts must be approved, you need to generate two new nodes with new account addresses that will serve as the pre-approved sealer addresses.

For this purpose we will use **Geth** and create two nodes, **Node 11** and **Node 22** by running the following commands in Git Bash:

**Create Node 11:**

`./geth --datadir node11 account new`

**Create Node 22:**

`./geth --datadir node2 account new`

You need to run these commands from the folder in which you have your Geth installed. Once the two nodes have been generated, there will be two folders created – `node11` and `node22` – each containing a folder with a keystore for that node.

## Generate your Genesis Block
Next we will use **puppeth** to generate your genesis block:

- Run `puppeth`, name your network, and select the option to `configure a new genesis block`. I named my PoA network `dlchain`.

- Choose the `Clique` (Proof of Authority) consensus algorithm.

- Paste both account addresses from the first step one at a time into the list of accounts to seal.

- Paste both accounts again in the list of accounts to pre-fund. This is required since there are no block rewards in PoA.

- Choose no for pre-funding the pre-compiled accounts (0x1 .. 0xff) with wei.

- Complete the rest of the prompts, and when you are back at the main menu, choose the "Manage existing genesis" option.

- Export genesis configurations. This will fail to create two of the files, but we only need `dlchain.json`.

## Initialize the Nodes with the Genesis' Json File

Using geth, we initialize each node with the new `dlchain.json`.

Open side by side two Git Bash windows, one for Node 11 and the other for Node 22, and run the following commands:

`./geth --datadir node11 init dlchain.json`

`./geth --datadir node22 init dlchain.json`


## Begin Mining Blocks

Run the nodes in separate terminal windows with the commands:

`./geth --datadir node11 --unlock "SEALER_ONE_ADDRESS" --mine --rpc --allow-insecure-unlock`

`./geth --datadir node22 --unlock "SEALER_TWO_ADDRESS" --mine --port 30304 --bootnodes "enode://SEALER_ONE_ENODE_ADDRESS@127.0.0.1:30303" --ipcdisable --allow-insecure-unlock`

**NOTE**: Type your password and hit enter - even if you can't see it visually!

The PoA blockchain should be now up and running.
![StartMining](POA-Development-Chain/Screenshots/StartMining.png)

## Add the New Blockchain to MyCrypto for Testing

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