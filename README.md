  
# TÌM HIỂU FIREWALL LINUX
### I. FIREWALL TRONG LINUX
#### 1. Hệ thống trong firewall linux:


##### 1.1 Log File 

- Một số file log chính trong hệ thống:
```
- File /var/log/messages: Chứa các thông tin log của hệ thống được daemon syslogd ghi nhận.
- File /var/log/secure : chứa các thông tin về login fail, add user,…
- File /var/log/wtmp lưu các log về  logon/reboot thành công vào hệ thống(ta có thể sử dụng last tool để xem thông tin này).
- File /var/run/utmp lưu các session hiện tại đang logon vào hệ thống(ta có thể dùng lệnh who để xem thông tin này).
```
- Giới hạn user
```
Thông qua tập tin /etc/nologin, ta có thể ngăn chặn việc login của user trong hệ thống trừ user root.

- Thư mục /etc/security/  cho phép người quản trị có thể giới hạn user CPU time, kích thước tối đa của file, số kết nối vào hệ thống(file /etc/security/limits.conf).
- Thư mục /etc/security/access.conf để giới hạn việc login của user và nhóm từ 1 vị trí cụ thể nào đó.
```
- Tham khảo về cú pháp của file /etc/security/limits.conf
```
<Domain>  <type> <item> <value>
```
Trong đó:
```
[domain]  : username, groupname.
[type]: hard, soft.
[item]: core, data, fsize,..
```
- Network security
```
Linux phân chia Network security thành hai loại chính

Loại 1: host based security
Loại 2: port based security
Host Based security
```
- Tcp_wrappers cung cấp host based access control list cho nhiều loại network services như: xinetd, sshd, portmap,…
- Tcp_wrappers cung cấp hai file cấu hình /etc/hosts.allow và /etc/hosts.deny để ngăn chặn hoặc cho phép các host request đến các dịch vụ trong hệ thống. Cú pháp của 2 file này như sau:

- Service : hosts [EXCEPT] hosts
```
Ví dụ:
ALL:  ALL  EXCEPT  .domain.com
```
- Port based security

Linux kernel cho phép thực thi chức năng packet filtering trong hệ thống thông qua công cụ iptables, ipchains.
### II. IPTABLE  
#### 1. Iptables :
-  Iptables  là một phần mềm firewall mạnh, miễn phí và được tích hợp sẵn trong môi trường các hệ điều hành dòng Linux.

- Iptables là một chương trình chạy ở không gian người dùng, cho phép người quản trị hệ thống có thể cấu hình các bảng của tường lửa trong nhân Linux (được cài đặt trong các mô đun Netfilter khác nhau) và lưu trữ các chuỗi, luật. Các mô đun nhân và chương trình khác nhau được áp dụng cho từng giao thức; iptables cho IPv4, ip6tables cho IPv6, arptables cho ARP, và ebtables cho Ethernet frames.

#### 2. Tính năng của Iptables:

- Tích hợp tốt với kernel của Linux.
- Có khả năng phân tích package hiệu quả.
- Lọc package dựa vào MAC và một số cờ hiệu trong TCP Header.
- Cung cấp chi tiết các tuỳ chọn để ghi nhận sự kiện hệ thống.
- Cung cấp kỹ thuật NAT
- Có khả năng ngăn chặn một số cơ chế tấn công theo kiểu từ chối dịch vụ(denial of service (DoS) attacks)

#### 3. Kiến trúc   của Iptables:

- IPTables được chia làm 4 bảng (table): bảng filter dùng để lọc gói dữ liệu, bảng nat dùng để thao tác với các gói dữ liệu được NAT nguồn hay NAT đích, bảng mangle dùng để thay đổi các thông số trong gói IP và bảng conntrack dùng để theo dõi các kết nối. Mỗi table gồm nhiều mắc xích (chain).Chain gồm nhiều luật (rule) để thao tác với các gói dữ liệu. Rule có thể là ACCEPT (chấp nhận gói dữ liệu), DROP (thả gói), REJECT (loại bỏ gói) hoặc tham chiếu (reference) đến một chain khác. Ta sẽ đi sau vào tìm hiểu từng bảng một trong IPTables

#### Mangle table: 
- Chịu trách nhiệm biến  đổi quality of service bits trong TCP header. Thông thường loại table này được ứng dụng trong SOHO.

#### Filter queue
- Chịu trách nhiệm thiết lập bộ lọc packet(packet filtering), có ba loại builtin chains được mô tả để thực hiện các chính sách về firewall (firewall policy rules).

 - Forward chain: Lọc packets đi qua firewall.
 - Input chain: Lọc  packets đi vào firewall.
 - Output chain: Lọc packets đi ra firewall.
 
#### NAT queue:
Thực thi chức năng NAT, cung cấp hai loại build-in chains sau đây:

 - Pre-routing chain: NATs packets khi destination address của packet cần thay đổi (NAT từ ngoài vào trong nội bộ).
 - Post-routing chain: NATs packets khi source address của packet cần thay đổi(NAT từ trong ra ngoài)
