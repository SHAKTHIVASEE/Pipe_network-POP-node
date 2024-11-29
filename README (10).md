# Pipe-Network

> Pipe Network | The decentralized CDN redefining data delivery 🌍 | Faster bandwidth, lower latency ⚡ | Built on @Solana Pipe Network is taking decentralized content delivery to the next level with a two-tier node system that ensures high performance and reliability. PoP Nodes and Guardian Nodes. 🖥️📊

> Join the Pipe Network DevNet – Be the First to Run a PoP Node.
>
> Register here [Pipe Network Dashboard](https://pipecdn.app/signup?ref=cHZzci5zYW)
>
> Check your email for the download link.

> Join our DevNet today! This initiative allows node operators to build a reputation before the incentivized testnet launch.

>💡 Prepare for Testnet: The incentivized testnet will follow, rewarding those who show strong performance. Both PoP Nodes and Guardian Nodes will play critical roles in the future of Pipe Network.

Step-by-Step Instructions

### 1. Create Directory:

```bash
sudo mkdir -p /opt/dcdn
```

### 2. Download the Pipe Tool Binary:
Replace $PIPE-URL with the link from your email (e.g., https://permissionless-labs.us10.list-manage.com/track/click?u=xxxxxxx=xxxxxx=xxxxxx).

```bash
sudo curl -L "$PIPE-URL" -o /opt/dcdn/pipe-tool
```

 ### 3. Download the Node Binary:
Replace $DCDND-URL with the link from your email.

```bash
sudo curl -L "$DCDND-URL" -o /opt/dcdn/dcdnd
```

 ### 4. Make the Binary Executable:

```bash
sudo chmod +x /opt/dcdn/pipe-tool
```
```bash
sudo chmod +x /opt/dcdn/dcdnd
```

### 5. Log In to Generate Access Token:

```bash
/opt/dcdn/pipe-tool login --node-registry-url="https://rpc.pipedev.network"
```

(This command outputs a QR code and a link. Scan the QR code or visit the link, then submit the code and sign up with your email.)

### 6. Generate Registration Token:

```bash
/opt/dcdn/pipe-tool generate-registration-token --node-registry-url="https://rpc.pipedev.network"
```

### 7. Create the Service File:

```bash
sudo cat > /etc/systemd/system/dcdnd.service << 'EOF'
[Unit]
Description=DCDN Node Service
After=network.target
Wants=network-online.target

[Service]
# Path to the executable and its arguments
ExecStart=/opt/dcdn/dcdnd \
          --grpc-server-url=0.0.0.0:8002 \
          --http-server-url=0.0.0.0:8003 \
          --node-registry-url="https://rpc.pipedev.network" \
          --cache-max-capacity-mb=1024 \
          --credentials-dir=/root/.permissionless \
          --allow-origin=*

# Restart policy
Restart=always
RestartSec=5

# Resource and file descriptor limits
LimitNOFILE=65536
LimitNPROC=4096

# Logging
StandardOutput=journal
StandardError=journal
SyslogIdentifier=dcdn-node

# Working directory
WorkingDirectory=/opt/dcdn

[Install]
WantedBy=multi-user.target
EOF
```

### 8. Open Ports:

```bash
sudo ufw allow 8002/tcp
sudo ufw allow 8003/tcp
```

### 9. Start the Node:

```bash
sudo systemctl daemon-reload
sudo systemctl enable dcdnd
sudo systemctl start dcdnd
```

### 10. Check if the Node is Running:

```bash
/opt/dcdn/pipe-tool list-nodes --node-registry-url="https://rpc.pipedev.network"
```

(If it shows as active, the node is running and operational.)

 11. Generate and Register Wallet:

```bash
/opt/dcdn/pipe-tool generate-wallet --node-registry-url="https://rpc.pipedev.network"
```

(This command will generate a wallet phrase. Save it securely.)

```bash
/opt/dcdn/pipe-tool link-wallet --node-registry-url="https://rpc.pipedev.network"
```

That’s it! You can refer to the detailed guide here: [Pipe Network Quickstart](https://docs.pipe.network/getting-started/quickstart#join-the-devnet)
