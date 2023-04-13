# ANSIBLE
Code bao gồm cài đăt phần mềm prometheus , grafana , blackbox_exporter , node_exporter , alertmanager và kết nối alertmanager với telegram Đã fix
### Điều kiện
 Code chạy trên ubuntu server 22.04 lts, có thể phải chỉnh lại thông tin phiên bản các phần mềm còn lại để phù hợp với hệ điều hành thấp hơn.
 Cài đặt Ansible trên máy chủ control , các node cài đặt python3 
 Có thể phải thực hiện tắt tường lửa nếu cần thiết
 Phải chỉnh thêm thông số Telegram và Alertmanager nếu muốn kết nối với bot
## Sử dụng
Bước 1: Để chạy đúng chương trình chúng ta cần thay đổi thông tin ở 2 file ansible.cfg đúng tên user và file inventory thông tin host cho phù hợp
## Add ssh key
Để kết nối giữa các máy thuận tiện chúng ta nên cài đặt ssh-key giữa các máy cho thuận tiện, Ở đây sử dựng mật mã RSA(private key) để mã hóa. Chạy trên máy control node

```
 su - danghieu
 ssh-keygen -t rsa
```
O hai dong duoi chung ta co the nhap mat khau cho che do passphare hoac ko nhap de bo qua no
```
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```
sau do lan luot add ssh-key vao cac may 
```
ssh-copy-id user@ipnode1
ssh-copy-id user@ipnode2
ssh-copy-id user@ipnode3
ssh-copy-id user@ipnode4
```

Chạy toàn bộ các chương trình chúng ta sử dung lệnh:
``` 
 ansible-playbook playbook.yml -i inventory  
```
Hoặc
```
ansible-playbook playbook.yml -i inventory --extra-vars "ansible_sudo_pass=123"
```
nếu tất cả các node cần cài đặt có chung 1 password

Nếu chúng ta muốn chạy từng chương trình riêng lẻ thì chạy từng playbook riêng lẻ,Ví dụ nếu chúng ta chạy chương trình prometheus chúng ta chạy 
```
 ansible-playbook telegram_playbook.yml -i inventory 
```
## Thông tin thêm
Mỗi chương trình tương ứng 1 folder trong foler roles, Ví du prometheus tương ưng folder prometheus.
file vars/main.yml chứa thông tin phiên bản , tên service , chúng ta có thể thay đổi để phù hớp với hệ điệu hành . Ví dụ Ubuntu 22.04 thì node_exporter phiên bản 0.17 trở lên. 

Muốn kết nối bot chúng ta cần chỉnh 3 thông số

1: thông số trong file /roles/akertmanager/vas/main.yml thay đổi thông số IDtelegram phù hợp với ID phòng 

2: thông số trong file roles/telegram/vars/main.yml thay đổi thông số Telegram token

3:  thông số trong roles/telegram/vars/main.yml thay đổi thông số ID tương ứng với ID alertmanager
Cách lấy id và telegram token search trên mạng là ok
