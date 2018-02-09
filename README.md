# deployment-with-jenkins

## Overview

Jenkins là một phần mềm mã nguồn mở giúp tự động hóa các quy trình trong phát triển phần mềm.

Nó hỗ trợ hầu hết các phần mềm quản lý mã nguồn phổ biến hiện nay như Git, Subversion, Mercurial, ClearCase... Jenkins cũng hỗ trợ cả các mã lệnh của Shell và Windows Batch, đồng thời còn chạy được các mã lệnh của Apache Ant, Maven, Gradle... 

Việc kích hoạt build dự án phần mềm bằng Jenkins có thể được thực hiện bằng nhiều cách: dựa theo các lần commit trên mã nguồn, theo các khoảng thời gian, kích hoạt qua các URL, kích hoạt sau khi các job khác được build,...

Các chức năng của Jenkins có thể được mở rộng thông qua các plugin.

## Use case

### Topo :

<img src="https://github.com/locvx1234/deployment-with-jenkins/blob/master/images/topo.png">

Chúng ta sử dụng Jenkins như một server trung gian nhằm mục đích phân quyền cho các user để deploy các repository từ Git server, và sử dụng Ansible để triển khai lên các server ứng dụng.

Jenkins sẽ sử dụng thêm các plugin : Git plugin, Ansible plugin để giao tiếp với các phần mềm này 

## Deployment

Cài đặt và cấu hình cơ bản

https://github.com/locvx1234/deployment-with-jenkins/blob/master/Deployment.md

## Research

Chi tiết hơn về redirect để login bằng account Google,  các permission cho user, ansible, cont..

https://github.com/locvx1234/deployment-with-jenkins/blob/master/Research.md

Tự động build khi có thay đổi và send mail cho admin

[Jenkins auto build and send mail](Jenkins auto build and send mail.md)

## Reference

https://vi.wikipedia.org/wiki/Jenkins_(ph%E1%BA%A7n_m%E1%BB%81m)

https://kipalog.com/posts/Su-dung-Jenkins-de-deploy-code-co-su-approval-cua-manager
