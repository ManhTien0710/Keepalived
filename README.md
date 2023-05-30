# install keepalived
```bash
yum install keepalived -y
yum install -y iptables-services
```
# setup file config
```bash
vi /etc/keepalived/keepalived.conf (master-server1)
vi /etc/keepalived/keepalived.conf (backup-server2)
```
# open port
```bash
iptables -I INPUT -i ens33 -d 224.0.0.0/8 -p ah -j ACCEPT
iptables -I OUTPUT -o ens33 -d 224.0.0.0/8 -p ah -j ACCEPT
iptables -I INPUT -i ens33 -d 224.0.0.0/8 -p vrrp -j ACCEPT
iptables -I OUTPUT -o ens33 -d 224.0.0.0/8 -p vrrp -j ACCEPT
service iptables save
systemctl restart iptables
systemctl enable iptables

firewall-cmd --direct --permanent --add-rule ipv4 filter INPUT 0 --in-interface ens33 --destination 224.0.0.18 --protocol vrrp -j ACCEPT
firewall-cmd --direct --permanent --add-rule ipv4 filter OUTPUT 0 --out-interface ens33 --destination 224.0.0.18 --protocol vrrp -j ACCEPT
firewall-cmd --add-rich-rule='rule protocol value="ah" accept' --permanent
firewall-cmd --reload
```
# check status service
```bash
systemctl restart keepalived
systemctl enable keepalived
systemctl status keepalived
```
