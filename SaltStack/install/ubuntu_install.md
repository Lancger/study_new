```
sudo add-apt-repository ppa:saltstack/salt

sudo apt-get install salt-minion salt-ssh

sudo systemctl restart salt-minion


#最终修改
sudo tee /etc/salt/minion << 'EOF'
master: 182.168.56.11
id: slaver.test.com
EOF
```

