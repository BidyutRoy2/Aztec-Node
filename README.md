# Aztec Sequencer Setup - Alpha Testnet

This repository contains an automated script for setting up and running an Aztec Sequencer node on the Alpha Testnet. The script streamlines the installation process, making it accessible for both newcomers and experienced blockchain developers.

## 💻 System Requirements

| Component      | Specification               |
|----------------|-----------------------------|
| CPU            | 8-core Processor            |
| RAM            | 16 GiB                      |
| Storage        | 1 TB SSD                    |
| Internet Speed | 25 Mbps Upload / Download   |


## Overview

The `aztec.sh` script automates the installation and configuration of an Aztec Sequencer node on the Alpha Testnet. It handles the following tasks:

- Installing Docker and Docker Compose (if not already installed)
- Installing Node.js (if not already installed)
- Installing the Aztec CLI
- Setting up the Aztec Alpha Testnet environment
- Configuring your node with the necessary RPC URLs and validator private key
- Starting your Aztec Sequencer node

## Requirements

- Ubuntu/Debian-based Linux OS
- Root or sudo privileges
- Internet connection
- L1 Execution Client (EL) RPC URL
- L1 Consensus Client (CL) RPC URL
- Validator Private Key
- Blob Sink URL (optional)

## Quick Start

1. Clone this repository:
   ```bash
   git clone https://github.com/BidyutRoy2/Aztec-Node.git
   cd Aztec-Node
   ```

2. Make the script executable:
   ```bash
   chmod +x aztec.sh
   ```

3. Run the script with sudo:
   ```bash
   sudo ./aztec.sh
   ```

4. Follow the prompts to provide the required RPC URLs and validator private key.

## Tutorial: Obtaining RPC URLs

### L1 Execution Client (EL) RPC URL

This URL is used to connect to an Ethereum execution client on the Sepolia testnet.

1. Sign up or log in at [Alchemy](https://dashboard.alchemy.com/)
2. Create a new app:
   - Click "Create App"
   - Select "Ethereum" as the chain
   - Select "Sepolia" as the network
   - Give your app a name (e.g., "Aztec Sequencer")
   - Click "Create App"
3. Once your app is created, click on "View Key"
4. Copy the HTTPS URL, which should look like:
   ```
   https://eth-sepolia.g.alchemy.com/v2/YOUR_API_KEY
   ```

### L1 Consensus Client (CL) RPC URL

This URL is used to connect to an Ethereum consensus client on the Sepolia testnet.

1. Sign up or log in at [DRPC](https://drpc.org/)
2. Create an API key:
   - Go to the "API Keys" section
   - Click "Create API Key"
   - Give your key a name (e.g., "Aztec Sequencer")
   - Select "Sepolia" network
3. Once your key is created, copy the HTTPS URL, which should look like:
   ```
   https://lb.drpc.org/ogrpc?network=sepolia&dkey=YOUR_API_KEY
   ```

### Alternative RPC Providers

You can also use other RPC providers such as:

- [Infura](https://infura.io/)
- [QuickNode](https://www.quicknode.com/)
- [Ankr](https://www.ankr.com/)
- [Chainstack](https://chainstack.com/)

Follow a similar process on these platforms to obtain your RPC URLs for the Sepolia testnet.

## Checking Node Status

After installation, you can check the status of your node:

```bash
docker-compose logs -f
```

## ⚡Commands
- You can use this command to check logs of your node
```
sudo docker logs -f --tail 100 $(docker ps -q --filter ancestor=aztecprotocol/aztec:latest | head -n 1)
```
- You can stop this node using this command
```
sudo docker stop $(docker ps -q --filter ancestor=aztecprotocol/aztec:latest | head -n 1)
```
## 🧩 Post-Installation
> [!Note]
> **After running node, you should wait at least 10 to 20 mins before your run these commands**

- Use this command to get `block-number`
```
curl -s -X POST -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","method":"node_getL2Tips","params":[],"id":67}' http://localhost:8080 | jq -r '.result.proven.number'
```
- After running this code, you will get a block number like this : 66666

- Use that block number in the places of `block-number` in the below command to get `proof`
```
curl -s -X POST -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","method":"node_getArchiveSiblingPath","params":["block-number","block-number"],"id":67}' http://localhost:8080 | jq -r ".result"
```
- After running this command, you will get an output like this

![image](https://github.com/user-attachments/assets/9920016c-da33-459d-a011-9ff55ad3b32d)

- Now navigate to `operators | start-here` channel in [Aztec Discord Server](https://discord.com/invite/aztec)
- Use the following command to get `Apprentice` role
```
/operator start
```
- It will ask the `address` , `block-number` and `proof` , Enter all of them one by one and you will get `Apprentice` instantly

## 🚀 Register as Validator
>[!WARNING]
>You may see an error like `ValidatorQuotaFilledUntil` when trying to register as a validator, which means the daily quota has been reached—convert the provided Unix timestamp to local time to know when you can try again to register as Validator.

- Replace `SEPOLIA-RPC-URL` , `YOUR-PRIVATE-KEY` , `YOUR-VALIDATOR-ADDRESS` with actual value and then execute this command
```
aztec add-l1-validator \
  --l1-rpc-urls SEPOLIA-RPC-URL \
  --private-key YOUR-PRIVATE-KEY \
  --attester YOUR-VALIDATOR-ADDRESS \
  --proposer-eoa YOUR-VALIDATOR-ADDRESS \
  --staking-asset-handler 0xF739D03e98e23A7B65940848aBA8921fF3bAc4b2 \
  --l1-chain-id 11155111
```

Your node data is stored in the `data` directory created in the same location as the script.

## Troubleshooting

If you encounter issues:

1. Check your Docker installation:
   ```bash
   docker --version
   docker-compose --version
   ```

2. Verify your RPC URLs are correct and working.

3. Ensure your server has enough resources:
   - At least 8 CPU cores
   - 16GB RAM
   - 100GB free disk space
   - 25 Mbps upload

4. Check firewall settings to ensure the required ports are open.

## Additional Resources

- [Aztec Documentation](https://docs.aztec.network/)
- [Aztec Discord](https://discord.gg/aztec)
- [Aztec Forum](https://forum.aztec.network/)

## Version Information

- Script version: v0.85.0-alpha-testnet.5
- Compatible with Aztec Protocol version: 0.85.0-alpha-testnet.5

## Disclaimer

This is an Alpha Testnet setup. It's not meant for production use and may have bugs or issues. Use at your own risk.
