# Vps
#!/bin/bash
# Script to setup UnixNodes VPS Bot as a systemd service

# Install required Python packages
pip install -r requirements.txt

# Create systemd service file
SERVICE_FILE="/etc/systemd/system/unixnodes-bot.service"

cat <<EOF | sudo tee $SERVICE_FILE
[Unit]
Description=UnixNodes VPS Bot
After=network.target docker.service
Requires=docker.service

[Service]
Type=simple
User=root
WorkingDirectory=/root
ExecStart=/usr/bin/python3 /root/bot.py
Restart=always
RestartSec=30
Environment="PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
Environment="DOCKER_HOST=unix:///var/run/docker.sock"

[Install]
WantedBy=multi-user.target
EOF

# Create unixnodes user
sudo useradd -r -s /bin/false unixnodes

# Add user to docker group
sudo usermod -aG docker unixnodes

# Reload systemd daemon
sudo systemctl daemon-reload

# Enable service to start on boot
sudo systemctl enable unixnodes-bot.service

# Start the service now
sudo systemctl start unixnodes-bot.service

# Display service status
sudo systemctl status unixnodes-bot.service

# Follow service logs
sudo journalctl -u unixnodes-bot.service -f
