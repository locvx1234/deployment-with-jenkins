## Login bằng tài khoản Google 

Note lại từ link https://kipalog.com/posts/Cai-dat-Jenkins-va-xac-thuc-bang-tai-khoan-Google-Mail đề phòng link này die :))

Trước tiên ta cần phải tạo OAuth 2.0 credentials trong [Google Developers Console](https://console.developers.google.com/apis/library)

Chọn *Credentials* và tạo một project mới

Thiết lập OAuth consent screen, lưu ý là `Homepage URL` chỉ sử dụng domain name

<img src="https://raw.githubusercontent.com/locvx1234/deployment-with-jenkins/master/images/OAuth_consent_screen.png">

Tạo OAuth client ID

<img src="https://raw.githubusercontent.com/locvx1234/deployment-with-jenkins/master/images/OAuth_client_ID.png">

Mục **Application type** chọn *Web application*, **Authorize redirect URLs** có dạng  JENKINS_ROOT_URL/securityRealm/finishLogin

<img src="https://raw.githubusercontent.com/locvx1234/deployment-with-jenkins/master/images/OAuth_client_ID2.png">

Sau khi tạo xong, `Client Id` và `Client secret` được tự động sinh ra

Điền chúng vào trong **Configure Global Security**

<img src="https://raw.githubusercontent.com/locvx1234/deployment-with-jenkins/master/images/config_global.png">

Thiết lập Authorization, cấp quyền cho các user

<img src="https://raw.githubusercontent.com/locvx1234/deployment-with-jenkins/master/images/authorization.png">

Lưu ý : cấu hình Jenkins location với domain name tương ứng trong **Configure System**

<img src="https://raw.githubusercontent.com/locvx1234/deployment-with-jenkins/master/images/jenkins_location.png">

Do Google không cho phép sử dụng IP, và cả Port trong URL Redirect, nên ta phải dùng Haproxy, Nginx để làm reverse proxy

Install HA proxy

	$ sudo apt-get install haproxy -y 
	$ sudo vi /etc/haproxy/haproxy.cfg 
	
Thêm vào các dòng sau: 

	listen jenkins
		balance     roundrobin
		bind        *:80
		server      jenkins 127.0.0.1:8080 check
	
Bây giờ khi đăng nhập vào Jenkins, ta sẽ được chuyển hướng tới Google

<img src="https://raw.githubusercontent.com/locvx1234/deployment-with-jenkins/master/images/sign_in.png">

## Matrix-based security

Matrix-based là một trong những cách authorization, nó cho phép xác định các permission cho user và group. 

Có các nhóm permission :

- Overall
- Slave
- Job
- Run
- View
- SCM

#### Overall 

|Permission|Description|
|----------|-----------|
|Administer|Thay đổi cấu hình trên toàn hệ thống |
|Read	   |Xem gần như toàn bộ các trang trong Jenkins |
|RunScripts|Chạy script Groovy |
|UploadPlugins| Upload các plugin |

#### Slave

|Permission|Description|
|----------|-----------|
|Configure | Cấu hình các slave tồn tại |
|Delete	| Xóa các slave tồn tại |
|Create | Tạo các slave mới |
|Disconnect |Ngắt kết nối với các slave hoặc đánh dấu slave là offline |
|Connect | Kết nối với các slave hoặc đánh dấu là online |

#### Job 

|Permission|Description|
|----------|-----------|
|Create    | Tạo job mới |
|Delete    | Xóa job đang tồn tại |
|Configure | Update cấu hình của job |
|Read      | Chỉ cho đọc cấu hình project |
|Discover  | Chuyển hướng người dùng ẩn danh đến một form đăng nhập thay vì đưa ra thông báo lỗi nếu họ không có quyền xem các job |
|Build 	   | Build job hoặc cancel job đang chạy |
|Workspace | Xem workspace |
|Cancel	   | cancel job đang chạy |


#### Run

|Permission|Description|
|----------|-----------|
|Delete | Xóa những lần build từ lịch sử build |
|Update | Update các mô tả và các thuộc tính khác |

#### SCM

|Permission|Description|
|----------|-----------|
|Tag | Tạo tag mới trong source code cho từng lần build |


## Về Ansible 

Link : https://github.com/locvx1234/deployment-with-jenkins/blob/master/ansible.md
