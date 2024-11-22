# Namada node
Proof-of-Stake L1 for interchain asset-agnostic privacy

![image](https://github.com/user-attachments/assets/760b6513-c832-4381-8721-0b05a10a85ea)

# overview
Namada is a proof-of-stake (L1) blockchain designed for cross-chain privacy and asset-agnosticism.It supports interoperability with fast-finality blockchains via the Inter-Blockchain Communication (IBC) protocol and with Ethereum through a bidirectional bridge.

![image](https://github.com/user-attachments/assets/2c9297f7-1c28-42de-9cc0-8d8aecb79b0b)

Namada has unveiled the ‘Namada Shielded Expedition,’ a multiplayer incentivized testnet and the final expedition for the community to test the protocol before the mainnet launch.
# VPS Configuration
To set up your masternode, you have the choice of hosting it on your own computer or opting for a Virtual Private Server (VPS), which is ideal for hosting websites, applications, or other online services, such as nodes.
In my case, I opted for Contabo, a reputable VPS rental solution.
The Namada node requires an intermediate configuration.
So, I recommend you choose the Cloud VPS M.
https://contabo.com/en/

![image](https://github.com/user-attachments/assets/71224e20-5955-4b25-9328-ea8832e9f5d6)

# Installation of Essential Components
Before diving into the installation of your node, it is crucial to update your VPS. To do this, simply execute the following command in your VPS terminal
```bash
sudo apt-get update && sudo apt-get upgrade -y  
```
![image](https://github.com/user-attachments/assets/7dc88d6f-7aea-452f-865a-5dc7bcc3e685)

To install Namada, you will need to install several dependencies and essential components.
# installing Rust:
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh  
```
![image](https://github.com/user-attachments/assets/02b0e07e-c361-444a-8ea1-06fe5bffc6ae)

# Update your VPS for Rust :
```bash
source "$HOME/.cargo/env"  
```
![image](https://github.com/user-attachments/assets/0e59fdaa-ed66-4bb8-8689-5dc4fa90d4af)

With the following command, you will install several essential components:
```bash
sudo apt-get install -y make git-core libssl-dev pkg-config unzip curl screen libclang-12-dev build-essential protobuf-compiler  
```
![image](https://github.com/user-attachments/assets/d314234a-7532-45db-b893-d140b8bc6a4c)

You will need to update one of the components (protoc):
```bash
sudo apt-get remove protobuf-compiler -y  
```
```bash
PB_REL="https://github.com/protocolbuffers/protobuf/releases"
  curl -LO $PB_REL/download/v3.15.8/protoc-3.15.8-linux-x86_64.zip  
```
```bash
unzip protoc-3.15.8-linux-x86_64.zip -d $HOME/.local  
```
```bash
export PATH="$PATH:$HOME/.local/bin"  
```
![image](https://github.com/user-attachments/assets/8b5cd56f-1f8e-463f-8a88-54935669350e)

# Install the libudev library:
```bash
apt-get install libudev-dev libhidapi-dev -y  
```
# Install the Go software:
```bash
wget https://go.dev/dl/go1.21.5.linux-amd64.tar.gz
rm -rf /usr/local/go && tar -C /usr/local -xzf go1.21.5.linux-amd64.tar.gz  
```
![image](https://github.com/user-attachments/assets/d2744925-f4fa-429c-bce1-13c215594f2b)
```bash
export PATH=$PATH:/usr/local/go/bin  
```
Check the installation:
```bash
go version  
```
![image](https://github.com/user-attachments/assets/a6a91b43-bf2d-48f4-81a2-0a971a837874)

# Namada Installation
Download Namada’s GitHub and launch the Namada installation:
```bash
git clone https://github.com/anoma/namada.git
cd namada 
make install  
```
![image](https://github.com/user-attachments/assets/87166f33-7177-4032-a6f6-edd234fbf2f8)

Namada installation may take up to an hour. If the installation stops before completion, rerun the make install command.
At the end of the installation, you will be able to use the ‘namada’ command in your terminal.

![image](https://github.com/user-attachments/assets/f1e1bab3-91ef-46f8-b3c8-781982606aa9)

Add the installed files to your configuration:
```bash
cp target/release/namada* $HOME/.local/bin/  
```
![image](https://github.com/user-attachments/assets/cb485c18-923e-430e-a2f3-404e6586fc50)

If the command doesn’t work, try this one: cp target/release/namada* /bin/
# Comet Installation
You will need the CometBFT software.
Prepare your configuration for installation:
```bash
echo export GOPATH=\"\$HOME/go\" >> ~/.bash_profile
echo export PATH=\"\$PATH:\$GOPATH/bin\" >> ~/.bash_profile  
```
Download the CometBFT software:
```bash
git clone https://github.com/cometbft/cometbft.git
cd cometbft  
```
![image](https://github.com/user-attachments/assets/d2b08bc8-217e-4107-8347-d55b842ec141)

Install CometBFT:
```bash
make install  
```
![image](https://github.com/user-attachments/assets/a13ffd6e-5db3-4f51-9041-abe4f1552afb)

Check that the correct version of Cometbft is installed
```bash
cometbft version  
```
![image](https://github.com/user-attachments/assets/263a8ae6-45df-4a34-a57a-4cce3190a921)

# Launch the Namada Full Node
Before becoming a validator, you need to start your full node and then register as a validator.
Make sure Namada is correctly located. Use the following command to check its location:
```bash
which namada ## (should return /usr/local/bin/namada)  
```
![image](https://github.com/user-attachments/assets/325a1280-4a5f-4e5c-bc9c-cc2d3b16cbf6)

The expected path is /usr/local/bin/namada.
If the returned path is not correct (as in my case), copy the obtained path with the command which namada and add an asterisk (*) at the end of namada:
```bash
cp <PATH>* /usr/local/bin/  
```
Now, you will increase the file size associated with CometBFT. First, identify the process ID associated with CometBFT using the following command:
```bash
ps aux | grep cometbft  
```
![image](https://github.com/user-attachments/assets/f2c7a871-1a39-4923-ba92-f03f0b0b60e0)

In my case, the process ID for CometBFT is 1386025.
Next, terminate this process to rebuild it with a larger memory size:
```bash
sudo kill -9 <process_id>
ulimit -s 65520  
```
Configure your full-node:
```bash
export CHAIN_ID="public-testnet-15.0dacadb8d663"
namada client utils join-network --chain-id $CHAIN_ID  
```
And:
```bash
sudo tee /etc/systemd/system/namadad.service > /dev/null <<EOF
[Unit]
Description=namada
After=network-online.target
[Service]
User=$USER
WorkingDirectory=/root/.local/share/namada
Environment=CMT_LOG_LEVEL=p2p:none,pex:error
Environment=NAMADA_CMT_STDOUT=true
ExecStart=/usr/local/bin/namada node ledger run 
StandardOutput=syslog
StandardError=syslog
Restart=always
RestartSec=10
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF  
```
![image](https://github.com/user-attachments/assets/9fbae58e-9174-486d-9518-622f2e2990e4)

And start your node:
```bash
sudo systemctl daemon-reload
sudo systemctl enable namadad  
```
Then:
```bash
sudo systemctl start namadad  
```
You can check the log of your node with the following command:
```bash
sudo journalctl -u namadad -f -o cat  
```
![image](https://github.com/user-attachments/assets/d41ca493-ca43-4930-a8c2-35e2e0e0f755)

Your full node will synchronize with the blockchain.
#  Validator Node Creation
Before initiating your validator node, you’ll need to wait for your full node to synchronize with the Namada blockchain. This may take more than ten hours.
You can already start by creating your wallet.
Replace <WALLET_NAME> with the name you want to assign to your wallet.
```bash
KEY_ALIAS="<WALLET_NAME>"
namada wallet key gen --alias $KEY_ALIAS  
```
![image](https://github.com/user-attachments/assets/68e1d899-f623-424f-afdb-1db38392a977)

During the creation of your wallet, you will be prompted to set a password. Carefully record the generated seed phrase, a sequence of 24 words. It will be necessary in case of losing access to your wallet.
After creating your wallet, you will need to add some faucet tokens to cover the gas fees for future actions.
Visit this site to claim some faucet tokens: https://faucet.heliax.click/

![image](https://github.com/user-attachments/assets/10ab6c00-0fe7-488b-bed6-b01bb130734b)

You can retrieve your wallet with this command
```bash
namada wallet address find --alias <NOM_WALLET>  
```
You will name your validator node and provide an email address:
```bash
export VALIDATOR_ALIAS="<your-validator-name>"
export EMAIL="<your-validator-email-for-communication>"
export KEY_ALIAS="<WALLET_NAME>"  
```
Initiate your validator node :
```bash
namada client init-validator \
  --alias $VALIDATOR_ALIAS \
  --account-keys $KEY_ALIAS \
  --signing-keys $KEY_ALIAS \
  --commission-rate 0.01 \
  --max-commission-rate-change 0.01 \
  --email $EMAIL  
```
![image](https://github.com/user-attachments/assets/1fc2e307-f786-4e05-890d-1a0260a214ea)

You will be asked multiple times to create a password for the creation of different sub-wallets.
To turn your full-node into a validator node, you will need to stop your node and restart it.
```bash
sudo systemctl restart namadad  
```
Check your node’s log. It will take about two hours for your node to become a validator
```bash
sudo journalctl -u namadad -f -o cat  
```
![image](https://github.com/user-attachments/assets/2137ed4c-f105-496f-8a99-e1ae7e539e4b)

The node may take a few tens of minutes to start
# Thank you for taking the time
If you have any additional questions or would like to discuss further, feel free to reach out

https://discord.com/invite/namada

https://namada.net/


https://x.com/namada

https://docs.namada.net/

























