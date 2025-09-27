# Vps
{\rtf1\ansi\ansicpg1252\deff0\deflang16393{\fonttbl{\f0\fnil\fcharset0 Calibri;}}
{\*\generator Msftedit 5.41.21.2510;}\viewkind4\uc1\pard\sa200\sl276\slmult1\lang9\f0\fs22 pip install -r requirements.txt\par
sudo nano /etc/systemd/system/unixnodes-bot.service\par
[Unit]\par
Description=UnixNodes VPS Bot\par
After=network.target docker.service\par
Requires=docker.service\par
\par
[Service]\par
Type=simple\par
User=root\par
WorkingDirectory=/root\par
ExecStart=/usr/bin/python3 /root/bot.py\par
Restart=always\par
RestartSec=30\par
Environment="PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"\par
Environment="DOCKER_HOST=unix:///var/run/docker.sock"\par
\par
[Install]\par
WantedBy=multi-user.target\par
\par
\par
\par
sudo useradd -r -s /bin/false unixnodes\par
sudo usermod -aG docker unixnodes\par
# Reload systemd\par
sudo systemctl daemon-reload\par
\par
# Enable service to start on boot\par
sudo systemctl enable unixnodes-bot.service\par
\par
# Start the service now\par
sudo systemctl start unixnodes-bot.service\par
\par
# Check status\par
sudo systemctl status unixnodes-bot.service\par
\par
# View logs\par
sudo journalctl -u unixnodes-bot.service -f\par
\par
\par
}
