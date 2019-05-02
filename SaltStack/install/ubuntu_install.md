```
sudo add-apt-repository ppa:saltstack/salt
sudo apt-get install salt-minion salt-ssh

#最终修改
sudo tee /etc/salt/minion << 'EOF'
master: 192.168.56.11
id: slaver.test.com
EOF

sudo systemctl restart salt-minion
sudo systemctl enable salt-minion
```

