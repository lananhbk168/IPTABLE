  
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

#### 3.1 Mangle table: 
- Chịu trách nhiệm biến  đổi quality of service bits trong TCP header. Thông thường loại table này được ứng dụng trong SOHO.

#### 3.2 Filter queue
- Chịu trách nhiệm thiết lập bộ lọc packet(packet filtering), có ba loại builtin chains được mô tả để thực hiện các chính sách về firewall (firewall policy rules).

 - Forward chain: Lọc packets đi qua firewall.
 - Input chain: Lọc  packets đi vào firewall.
 - Output chain: Lọc packets đi ra firewall.
 
#### 3.3 NAT queue:
- Thực thi chức năng NAT
- Cơ chế NAT :

 -	Khi một máy bên trong mạng nội bộ truy cập vào tài nguyên bên ngoài thì packet được gửi đến IPNAT, IPNAT sẽ thay đổi địa chỉ nguồn của packet bằng một địa chỉ hợp lệ trong dãy địa chỉ được cấp trên internet nếu còn.

 -	Khi một máy của mạng bên ngoài truy cập vào tài nguyên của mạng cục bộ qua IP NAT, khi packet đến IP NAT nó sẽ thay đổi địa chỉ đích của packet bằng một địa chỉ của mạng cục bộ
	
-	Để chuyển đổi trường địa chỉ nguồn hoặc đích trong packet chúng ta sử dụng 3 chuỗi luật được xây dựng sẵn:
 -	Chuỗi luật PREOTING: dùng để chuyển đổi địa chỉ các gói tin ngay sau khi đi vào firewall.
 -	Chuỗi luật POSOUTPUT: dùng để thay đổi địa chỉ gói tin trước khi chúng ra khỏi firewall.
 -	Chuỗi luật OUTPUT: dùng để thay đổi địa chỉ các gói tin được phát sinh cục bộ (trên firewall) trước khi định tuyến.
 
#### 3.4 Targets và Jumps

- Targets là cơ chế hoạt động trong iptables dùng để nhận diện và kiểm tra packet.

- Jump là cơ chế chuyển một packet đến một target nào đó để xử lý thêm một số thao tác khác. Danh sách các target được xây dựng sẳn trong iptables:
```
   ACCEPT: iptables chấp nhận chuyển data đến đích.
   DROP:  Iptables block packet.
   LOG Thông tin của packet sẽ gởi vào syslog daemon iptables tiếp tục xử lý luật tiếp theo trong bảng mô tả luật. Nếu    luật cuối cùng không match thì sẽ drop packet.
   REJECT Ngăn chặn packet và gởi thông báo cho sender.
   DNAT  Thay  đổi  địa chỉ  đích của packet (rewriting the destination IP address of the packet)
   SNAT  Thay đổi địa chỉ nguồn của packet
   MASQUERADE Được sử dụng  để thực hiện kỹ thuật NAT ( giả mạo địa chỉ nguồnvới địa chỉ của firewall’s interface)
```

#### 4. Cơ chế xử lý gói tin :
 
 Việc kiểm tra gói tin được thực hiện bởi IPTables bằng cách dùng các bảng tuần tự xây dựng sẵn (queue) được trình bày ở trên
 Hình dưới đây sẽ mô tả quá trìnhxử lí gói tin:
 
 <img src ="http://i.imgur.com/zBwxKOy.png">
                        
- Đầu tiên gói dữ liệu đi đến mạng A, tiếp đó nó được kiểm tra bởi Mangle Tables PREROUTING chain nếu cần. Tiếp theo là kiểm tra gói dữ liệu bởi Nat Tables PREROUTING chain để kiểm tra xem gói dữ liệu có cần NAT hay không, nếu có sẽ tiến hành NAT. Rồi gói dữ liệu được dẫn đi.
Nếu gói dữ liệu được bảo về thì nó sẽ được lọc bởi FORWARD chain của Filter Table, nếu cần gói dữ liệu sẽ được NAT trong POSTROUTING chain để vào mạng B.
Nếu gói dữ liệu được đưa vào firewall, nó sẽ được kiểm tra vởi INPUT chain trong managle table, nếu qua được phần này nó sẽ vào trong các chương trình của server bên trong firewall.
Khi firewall cần gửi dữ liệu ra ngoài.Gói dữ liệu sẽ đi qua OUTPUT chain trong Mangle Table. Gói tin sẽ được NAT nếu cần thiết và kiểm tra NAT and QoS trong POSTROUTING chain. Sau đó dữ liệu được đưa trở lại Internet.

#### 5.Sử dụng Iptables

- Cú pháp sử dụng :
```
 
```
