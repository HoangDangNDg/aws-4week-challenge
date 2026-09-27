# Ý tưởng dự án — TaskFlow

## 1. Tên dự án và mục tiêu ứng dụng

TaskFlow là ứng dụng quản lý công việc cá nhân.

Người dùng có thể tạo, xem, cập nhật, xóa công việc; thiết lập trạng thái và hạn hoàn thành. Mục tiêu của dự án là xây dựng một REST API bằng Java Spring Boot, kết nối với PostgreSQL và triển khai trên hạ tầng AWS có phân tách Public Subnet và Private Subnet.

## 2. Công nghệ sử dụng

### Backend

- Programming language: Java 21
- Framework: Spring Boot 3
- REST API: Spring Web
- Database access: Spring Data JPA và Hibernate
- Build tool: Maven
- Testing: JUnit 5 và Spring Boot Test
- API testing: Postman

### Database

- Database: PostgreSQL
- Công cụ quản lý database: pgAdmin hoặc DBeaver

### Triển khai và hạ tầng

- Containerization: Docker và Docker Compose
- Version control: Git và GitHub
- Cloud platform: AWS
- Compute: Amazon EC2
- Database hosting: Amazon RDS for PostgreSQL
- Network: Amazon VPC, Public Subnet, Private Subnet, Route Table, Internet Gateway và NAT Gateway
- Security: Security Group, Network ACL và IAM
- Monitoring: Amazon CloudWatch Logs

## 3. Ứng dụng tương tác với hạ tầng VPC

VPC được thiết kế với Public Subnet và Private Subnet trải trên hai Availability Zones.

Người dùng từ Internet gửi request đến lớp public. Trong kiến trúc hoàn chỉnh, Nginx hoặc Application Load Balancer ở Public Subnet sẽ tiếp nhận request và chuyển tiếp đến Spring Boot API.

Spring Boot API chạy trên EC2 trong Private Subnet. Application Server không có public IP và chỉ nhận traffic từ lớp public qua port 8080.

PostgreSQL được triển khai bằng Amazon RDS trong Private Subnet. Database không được truy cập trực tiếp từ Internet; chỉ Spring Boot API được phép kết nối đến port 5432 thông qua Security Group.

Application Server có thể đi Internet outbound qua NAT Gateway để cập nhật hệ thống hoặc sử dụng dịch vụ bên ngoài. Tuy nhiên, Internet không thể chủ động truy cập trực tiếp vào Application Server và Database.