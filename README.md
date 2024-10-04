
# Các bước triển khai

### 1. Tạo kiến trúc mạng ảo (VPC)
Trước tiên, chúng ta cần tạo một Virtual Private Cloud (VPC) để lưu trữ ứng dụng web.
1. Tạo một VPC với CIDR block (ví dụ: 10.0.0.0/16).
2. Tạo 2 subnet riêng biệt trong 2 Availability Zone khác nhau, một subnet công khai (public) để chứa các máy chủ web và một subnet riêng tư để chứa cơ sở dữ liệu.
3. Cấu hình Internet Gateway cho phép truy cập internet tới subnet công khai.
4. Tạo bảng định tuyến (route table) cho các subnet công khai và tư nhân.

### 2. Tạo máy chủ web trên EC2
Tạo các instance EC2 để lưu trữ ứng dụng web.
1. Khởi chạy 2 hoặc nhiều instance EC2 sử dụng AMI của Ubuntu.
2. Đảm bảo rằng ứng dụng web được cài đặt và cấu hình để chạy trên mỗi instance EC2.

### 3. Tạo cơ sở dữ liệu MySQL trên Amazon RDS
Tạo cơ sở dữ liệu MySQL với Amazon RDS.
1. Mở Amazon RDS và tạo một database MySQL trong một subnet riêng tư.
2. Đảm bảo chỉ các máy chủ web (EC2) có thể truy cập cơ sở dữ liệu, sử dụng Security Groups để giới hạn truy cập qua đúng cổng (3306 cho MySQL).
3. Sử dụng AWS Secrets Manager để lưu trữ thông tin đăng nhập của cơ sở dữ liệu một cách an toàn.

### 4. Tạo Application Load Balancer (ALB)
Tạo Application Load Balancer để phân phối tải giữa các máy chủ web EC2.
1. Tạo ALB và liên kết với ít nhất 2 Availability Zones để tăng tính sẵn sàng.
2. Định cấu hình ALB để phân phối yêu cầu HTTP đến các instance EC2.
3. Cấu hình nhóm mục tiêu (target group) để bao gồm các instance EC2 của bạn.

### 5. Cấu hình Auto Scaling Group
Tạo Auto Scaling Group để tự động thêm hoặc gỡ bỏ các instance EC2 dựa trên nhu cầu thực tế.
1. Tạo một AMI từ instance EC2 hiện tại (hoặc tạo AMI mới).
2. Tạo một Launch Template sử dụng AMI và cấu hình scaling dựa trên CPU hoặc lưu lượng truy cập.
3. Đặt chính sách Target tracking để tự động tăng/giảm số lượng instance dựa trên tài nguyên sử dụng.

### 6. Giám sát và bảo mật
Sử dụng AWS CloudWatch để giám sát hiệu suất của ứng dụng và nhận cảnh báo khi có sự cố.
1. Cấu hình các cảnh báo CloudWatch cho CPU, bộ nhớ và lưu lượng mạng.
2. Đảm bảo rằng các Security Groups chỉ mở các cổng cần thiết (80 cho HTTP và 3306 cho MySQL).
3. Dùng IAM Roles để kiểm soát quyền truy cập giữa các dịch vụ AWS.

### 7. Kiểm tra và tải thử nghiệm
Sau khi hoàn tất triển khai, hãy truy cập ứng dụng thông qua URL của Application Load Balancer và kiểm tra các chức năng như xem, thêm, xóa và sửa bản ghi sinh viên.
