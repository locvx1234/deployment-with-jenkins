## Login bằng tài khoản Google 

Note lại từ link https://kipalog.com/posts/Cai-dat-Jenkins-va-xac-thuc-bang-tai-khoan-Google-Mail đề phòng link này die :))

Trước tiên ta cần phải tạo OAuth 2.0 credentials trong [Google Developers Console](https://console.developers.google.com/apis/library)

Chọn *Credentials* và tạo một project mới

Thiết lập OAuth consent screen, lưu ý là `Homepage URL` chỉ sử dụng domain name

<img src="">

Tạo OAuth client ID

<img src="">

Mục **Application type** chọn *Web application*, **Authorize redirect URLs** có dạng  JENKINS_ROOT_URL/securityRealm/finishLogin

<img src="">

Sau khi tạo xong, `Client Id` và `Client secret` được tự động sinh ra

Điền chúng vào trong **Configure Global Security**

<img src="">

Thiết lập Authorization, cấp quyền cho các user

<img src="">

Lưu ý : cấu hình Jenkins location với domain name tương ứng trong **Configure System**

<img src="">

Do Google không cho phép sử dụng IP, và cả Port trong URL Redirect, nên ta phải dùng Haproxy, Nginx để làm reverse proxy

Install HA proxy

	$ sudo apt-get install haproxy -y 
	$ sudo vi /etc/haproxy/haproxy.cfg 
	
Thêm vào các dòng sau: 

	listen jenkins
		balance     roundrobin
		bind        *:80
		server      jenkins 127.0.0.1:8080 check
	

