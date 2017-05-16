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


## 

