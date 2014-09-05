  
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

Thư mục /etc/security/  cho phép người quản trị có thể giới hạn user CPU time, kích thước tối đa của file, số kết nối vào hệ thống(file /etc/security/limits.conf).
/etc/security/access.conf để giới hạn việc login của user và nhóm từ 1 vị trí cụ thể nào đó.
```
Tham khảo về cú pháp của file /etc/security/limits.conf
```
<Domain>  <type> <item> <value>
```
Trong đó:
[domain]  : username, groupname(sử dụng theo cú pháp @groupname)
[type]: hard, soft.
[item]: core, data, fsize,…(ta tham khảo file /etc/security/limits.conf)

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
#### 1.
