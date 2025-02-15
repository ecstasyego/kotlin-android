# Server

## MySQL
### Installation

```bash
sudo apt update
sudo apt install mysql-server -y
```

`Version`
```bash
mysql --version
```

`Manual:service`
```bash
sudo service mysql start
sudo service mysql status
sudo service mysql restart
sudo service mysql stop
```

`Auto:systemctl`
```bash
sudo systemctl enable mysql
```
