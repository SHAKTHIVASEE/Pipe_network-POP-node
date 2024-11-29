# Pipe-Network

> Join the Pipe Network DevNet incentivized testnet launch.
> Register here [Referral Link: https://pipecdn.app/signup?ref=Y3J5cHRvLm)
> Check your email for the Whitelisted link.
> Open Pipe-tool & dcdnd Node link from WL mail and save it

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

### 7. Set path :

```bash
cd /etc/systemd/system/dcdnd.service
```

### 8. Create the Service File:

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

### 9. Open Ports:

```bash
sudo ufw allow 8002/tcp
sudo ufw allow 8003/tcp
```

### 10. Start the Node:

```bash
sudo systemctl daemon-reload
sudo systemctl enable dcdnd
sudo systemctl start dcdnd
```

### 11. Check if the Node is Running:

```bash
/opt/dcdn/pipe-tool list-nodes --node-registry-url="https://rpc.pipedev.network"
```

(If it shows as active, the node is running and operational.)

### 12. Generate and Register Wallet and save the data:

```bash
/opt/dcdn/pipe-tool generate-wallet --node-registry-url="https://rpc.pipedev.network"
```

(This command will generate a wallet phrase. Save it securely.)

```bash
/opt/dcdn/pipe-tool link-wallet --node-registry-url="https://rpc.pipedev.network"
```

### 11. View logs:
```bash
  sudo journalctl -f -u dcdnd.service
```
```bash
  systemctl status dcdnd
```


That’s it! 
