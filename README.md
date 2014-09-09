# LAB IPTABLE:

### I. Mô hình :
 <img src="http://i.imgur.com/nJW2MTZ.png">
### II. THÔNG TIN THIẾT BỊ
 <img src="http://i.imgur.com/eWr8Cy2.png">
### III. Danh sách lệnh :

- Cấu hình cho phép lắng nghe các gói tin đi qua :
```
echo 1 > /proc/sys/net/ipv4/ip_forward
```

- Cấm host 10.10.10.100 sử dụng SSH từ xa vào mạng 10.10.10.0/24:
```
 iptables -A INPUT -p tcp -m state --state NEW -m tcp -s 10.10.10.100  -d 10.10.10.0/24 --dport 22 -j DROP
```
- Cấu hình SNAT cho các host trong mạng 10.10.10.0/24 đi ra được Internet:
```
iptables -t nat -A POSTROUTING -o eth1  -s 10.10.10.0/24     -j SNAT --to-source 172.16.69.13
```
- Cấu hình DNAT Public Webserver ra internet( 10.10.10.72 -> 172.16.69.13):
```
iptables -A PREROUTING -d 172.16.69.13/24 -i eth1 -p tcp -m tcp --dport 80 -j DNAT --to-destination 10.10.10.72:80

iptables -A PREROUTING -d 172.16.69.13/24 -i eth1 -p tcp -m tcp --dport 443 -j DNAT --to-destination 10.10.10.72:443
```
### IV .Kết quả :

- máy host truy cập được mạng :

 <img src ="http://i.imgur.com/PSlFDjY.png">
 
- Phân giải địa chỉ server từ private -> public:
 
 <img src ="http://i.imgur.com/NKQsRt5.png">
- Cấm host 10.10.10.100 thực hiện truy cập từ xa bằng SSH :

 <img src="http://i.imgur.com/R13ukF0.png">



------------------------------------------------------------------
