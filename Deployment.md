## Install Git 

	$ sudo apt-get update
	$ sudo apt-get install git -y
	
## Install Ansible

	$ sudo apt-add-repository -y ppa:ansible/ansible
	$ sudo apt-get update
	$ sudo apt-get install -y ansible 
	
## Install Jenkins 

	$ wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
	$ sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
	$ sudo apt-get update
	$ sudo apt-get install jenkins -y
	
## Khởi đầu với Jenkins 

Truy cập http://ip_address_or_domain_name:8080

<img src="https://assets.digitalocean.com/articles/jenkins-install-ubuntu-1604/unlock-jenkins.png">

Get pass: 
	
	$ sudo cat /var/lib/jenkins/secrets/initialAdminPassword
	
<img src="https://assets.digitalocean.com/articles/jenkins-install-ubuntu-1604/jenkins-customize.png">

Chọn *Install suggested plugins* và đợi Jenkins cài đặt các plugin cần thiết trong đó có Git plugin

Tiếp đó, một trang tạo Admin user xuất hiện, sau khi hoàn thành xong bước này, chúng ta sẽ đến với trang làm việc chính của Jenkins.

## Cấu hình cơ bản

#### Cài thêm plugin 
Các plugin cần cài thêm : Ansible plugin,  Promoted builds plugin (dùng cho việc request / approval), Git plugin (nếu khi bước Install suggested plugin chưa có), SSH Agent plugin, Google login plugin (xác thực login bằng account google) 

Thao tác mẫu với Ansible plugin

<img src="https://raw.githubusercontent.com/locvx1234/deployment-with-jenkins/master/images/manage_plugin.png">

<img src="https://raw.githubusercontent.com/locvx1234/deployment-with-jenkins/master/images/ansible_plugin.png">

#### Tạo credentials

Chọn tab Credentals bên cột bên trái Dashbroad

<img src="https://raw.githubusercontent.com/locvx1234/deployment-with-jenkins/master/images/credentials.png">

Có nhiều kiểu xác thực 

<img src="https://raw.githubusercontent.com/locvx1234/deployment-with-jenkins/master/images/credentials2.png">

#### Deploy một repo Git

Tại Dashbroad của Jenkins chọn Jenkins/New Item

Điền tên item và chọn loại Freestyle project

Tại mục *Source Code Management* chọn `Git` và điền Repository URL và chọn credentials tương ứng

*Branches to build* mặc định là nhánh `master`

<img src="https://raw.githubusercontent.com/locvx1234/deployment-with-jenkins/master/images/git.png">

Sau khi Save cấu hình, chọn `Build Now` 

<img src="https://raw.githubusercontent.com/locvx1234/deployment-with-jenkins/master/images/git_build.png">

Xem những gì đã diễn ra tại `Console Output`

#### Build một playbook

Tại mục *Build* chọn Add build step/Invoke Ansible Playbook

<img src="https://raw.githubusercontent.com/locvx1234/deployment-with-jenkins/master/images/ansible.png">

<img src="https://raw.githubusercontent.com/locvx1234/deployment-with-jenkins/master/images/ansible_build.png">

Sau đó Save và Build như trên




