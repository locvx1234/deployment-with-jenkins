
# Ansible 

Đơn giản, thao tác trực tiếp qua ssh đến các server, không cần cài đặt thêm phần mềm ở server đích


## Cài đặt: 

- Ubuntu 14.04:

	sudo apt-add-repository -y ppa:ansible/ansible
	sudo apt-get update
	sudo apt-get install -y ansible


- Check phiên bản

	ansible --version
	
## Sử dụng

Sử dụng key hoặc pass để ssh đến server
- Config: /etc/ansible/ansible.cfg
- Untag sudo_user và ask_sudo_pass để dùng được quyền sudo. Tuy nhiên bị hỏi pass sau mỗi câu lệnh

	#inventory      = /etc/ansible/hosts
	#library        = /usr/share/my_modules/
	#module_utils   = /usr/share/my_module_utils/
	#remote_tmp     = ~/.ansible/tmp
	#local_tmp      = ~/.ansible/tmp
	#forks          = 5
	#poll_interval  = 15
	sudo_user      = root
	#ask_sudo_pass = True
	#ask_pass      = True
	#transport      = smart
	#remote_port    = 22
	#module_lang    = C
	#module_set_locale = False
	
- Cài đặt đúng path của private key. Nếu không dùng key, thêm -k sau mỗi lệnh để điền pass.

	# if set, always use this private key file for authentication, same as
	# if passing --private-key to ansible or ansible-playbook
	private_key_file = /home/hexa/.ssh/id_ansible	
	
## Một số lệnh test

Chạy lệnh ifconfig trên server test (cài đặt trong /etc/ansible/hosts)

	ansible test -m shell -a "ifconfig"

Cài đặt htop cho toàn bộ server được cấu hình (lệnh chỉ chạy được khi có cấu hình hoi pass sudo)

	ansible all -m apt -a "name=htop force=yes"
	
## Sử dụng playbook.
#### Playbook là gì?

- Playbook là 1 kịch bản viết sẵn để ansible thực thi trên nhiều server.
- Playbook có cấu trúc là 1 file .yml

#### Playbook mẫu:

- Cài đặt nginx:

	- hosts: ubuntu14
	  remote_user: hexa
	  become: yes
	  tasks:
		- name: Installs nginx web server
		  apt: pkg=nginx state=installed update_cache=true force=yes
		  notify:
			- start nginx

	  handlers:
		- name: start nginx
		  service: name=nginx state=started
	
	
+ become: yes: chạy lệnh dưới quyền của user_remote. Mặc định là root.

- Chạy 1 script:

	- hosts: ubuntu14
	  remote_user: hexa
	  become: yes
	  tasks:
		- script: /home/hexa/test.sh

#### Chạy 1 playbook:

	ansible-playbook test.yml	
	
## Vấn đề pass sudo

Một số lệnh cần sử dụng quyền sudo nên ta cần cung cấp pass sudo để ansible sử dụng.

#### Điền password bằng tay mỗi lần chạy.

	ansible-playbook playbook.yml --ask-sudo-pass

Ansible sẽ hỏi pass mỗi lần chạy

#### Điền password luôn vào câu lệnh chạy.

	ansible-playbook playbook.yml -i inventory.ini --user=username --extra-vars "ansible_become_pass=yourPassword"
	
-i inventory.ini : liệt kê các server sẽ chạy playbook

#### Config password cho mỗi server ngay trong file host.

	[elk]
	elasticsearch ansible_ssh_host=192.168.169.136
	kafka ansible_ssh_host=192.168.169.159 ansible_become_pass=password
	logstash ansible_ssh_host=192.168.169.160

	[ubuntu14]
	168 ansible_ssh_host=192.168.169.168 ansible_become_pass= password

Hoặc cấp luôn cho cả group

	[all:vars]
	ansible_become_pass=default_sudo_password_for_all_hosts

[group1:vars]

	ansible_become_pass=default_sudo_password_for_group1
	
#### Đặt password trong 1 file và trỏ đến

	- hosts: ubuntu14
	  remote_user: hexa
	  become: yes
	  vars_files:
	   - pass
	  tasks:
	   - name: Installs nginx web server
	  apt: pkg=nginx state=installed update_cache=true force=yes
	  notify:
	   - start nginx

	  handlers:
	   - name: start nginx
	   service: name=nginx state=started

File pass đặt cùng directory với play-book. Nội dung như sau:

	ansible_become_pass: password	
	
## Vấn đề mã hóa

Ansible cung cấp giải pháp mã hóa cho file play-book, hoặc các file text khác gọi là ansible-vault

#### Tạo file

	ansible-vault create <file>

Mỗi file sẽ tạo ra sẽ yêu cầu nhập 1 password riêng

#### Edit

	ansible-vault edit <file>

#### Sử dụng

	ansible-playbook site.yml --ask-vault-pass	
	
	
## Tham khảo

https://stackoverflow.com/questions/21870083/specify-sudo-password-for-ansible
	
http://docs.ansible.com/ansible/playbooks_vault.html