```
sudo apt-get update -y
sudo apt-get install software-properties-common -y
sudo add-apt-repository ppa:saltstack/salt -y
sudo apt-get install salt-minion salt-ssh -y

sudo tee /etc/salt/minion << 'EOF'
master: 192.168.56.11
id: slaver.test.com
EOF

sudo systemctl restart salt-minion
sudo systemctl enable salt-minion
```

