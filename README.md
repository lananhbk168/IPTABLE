# LAB IPTABLE:

### I. Mô hình :
 <img src="http://i.imgur.com/ibgXYml.png">
 
 
### II. 
- Cấu hình cho phép lắng nghe các gói tin đi qua :
```
echo 1 > /proc/sys/net/ipv4/ip_forward
```

- Cấm host 10.10.10.100 sử dụng SSH từ xa vào mạng 10.10.10.0/24:
```
 iptables -A INPUT -p tcp -m state --state NEW -m tcp -s 10.10.10.100  -d 10.10.10.0/24 --dport 22 -j ACCEPT
```
- Cấu hình SNAT cho các host trong mạng 10.10.10.0/24 đi ra được Internet:
```
iptables -t nat -A POSTROUTING -o eth1 -j SNAT --to-source 172.16.69.13
```
- Cấu hình DNAT Public Webserver ra internet( 10.10.10.72 -> 172.16.69.13):
```
iptables -A PREROUTING -d 172.16.69.13/24 -i eth1 -p tcp -m tcp --dport 80 -j DNAT --to-destination 10.10.10.72:80
```