## Cài đặt các dịch vụ trên ubuntu
### 1. Cài đặt Nginx WebServer, Apache Webserver, viết bash script cài đặt tự động (script dùng được cho cả ubuntu và centos)
```
vi install.sh
```
    #!/bin/bash

    if [ -f /etc/debian_version ]; then
            lenh="apt"
    else
            lenh="yum"
    fi
    $lenh update -y
    if [ $lenh = "apt" ]; then
            $lenh install -y nginx apache2
    else
            $lenh install -y nginx httpd
    fi
```
./install.sh

```
### 2. Tìm hiểu Database, các loại database, hệ quản trị cơ sở dữ liệu
#### Database (Cơ sở dữ liệu) là gì?
- Database (CSDL - Cơ sở dữ liệu) là tập hợp có tổ chức của dữ liệu, giúp lưu trữ và quản lý thông tin một cách hiệu quả. Dữ liệu có thể được truy vấn, chỉnh sửa và xử lý thông qua hệ quản trị cơ sở dữ liệu (DBMS - Database Management System).
#### Các loại Database phổ biến
- **Database quan hệ (Relational Database - RDBMS)**
    - Dữ liệu được tổ chức thành **bảng (table)** với **hàng (row)** và **cột (column)**.
    - Dùng **SQL (Structured Query Language)** để truy vấn.
    - Ví dụ: MySQL, PostgreSQL, Microsoft SQL Server, Oracle Database.
- **Database NoSQL (Non-Relational Database)**
    - Không sử dụng mô hình bảng như RDBMS.
    - Hỗ trợ lưu trữ dữ liệu phi cấu trúc hoặc bán cấu trúc.
    - Các loại NoSQL phổ biến:
        - **Key-Value Store**: Redis, DynamoDB.
        - **Document Database**: MongoDB, CouchDB.
        - **Column-Family Store**: Apache Cassandra, HBase.
        - **Graph Database**: Neo4j, Amazon Neptune.
- **Database In-Memory**
    - Lưu trữ dữ liệu trong RAM, giúp truy vấn cực nhanh.
    - Ví dụ: Redis, Memcached.
- **Database thời gian thực (Time-Series Database - TSDB)**
    - Dùng để lưu trữ dữ liệu theo thời gian.
    - Ví dụ: InfluxDB, TimescaleDB, Prometheus.
- **Database NewSQL**
    - Kết hợp ưu điểm của RDBMS (ACID) và NoSQL (hiệu suất cao, mở rộng dễ dàng).
    - Ví dụ: Google Spanner, CockroachDB.
#### Hệ quản trị cơ sở dữ liệu (DBMS - Database Management System)
- DBMS là phần mềm giúp quản lý database, hỗ trợ lưu trữ, truy vấn và bảo mật dữ liệu. 

Có hai loại chính:
- Hệ quản trị cơ sở dữ liệu quan hệ (RDBMS)
    - **MySQL** (Miễn phí, phổ biến, hỗ trợ nhiều ứng dụng web).
    - **PostgreSQL** (Mạnh mẽ, hỗ trợ tính năng nâng cao).
    - **Microsoft SQL Server** (Dùng cho doanh nghiệp, tích hợp tốt với hệ sinh thái Microsoft).
    - **Oracle Database** (Mạnh mẽ, tối ưu cho doanh nghiệp lớn).
- Hệ quản trị cơ sở dữ liệu NoSQL
    - **MongoDB** (Document Database, dễ mở rộng).
    - **Redis** (Key-Value Store, tốc độ cao).
    - **Apache Cassandra** (Column-Family Store, hỗ trợ dữ liệu phân tán).
    - **Neo4j** (Graph Database, tối ưu cho dữ liệu mạng).
### 3. Cài đặt MYSQL Server, MariaDB Server, PHPmyadmin. Các command phổ biến làm việc với MYSQL Database, người dùng và phân quyền. So sánh Mysql và MariaDB
- Cài đặt MYSQL Server, MariaDB Server, PHPmyadmin
    - apt install -y mysql-server
    - apt install -y mariadb-server
    - apt install -y phpmyadmin
- Các command MySQL/MariaDB phổ biến
    - **Đăng nhập vào MySQL/MariaDB**
        ```
        mysql -u root -p
        ```
    - **Quản lý Database**
        - Tạo database
            ```
            CREATE DATABASE ten_database;
            ```
        - Xem danh sách database
            ```
            SHOW DATABASES;
            ```
        - Sử dụng database
            ```
            USE ten_database;
            ```
        - Xoá database
            ```
            DROP DATABASE ten_database;
            ```
    - **Quản lý User & Phân quyền**
        - Tạo user mới
            ```
            CREATE USER 'ten_user'@'localhost' IDENTIFIED BY 'mat_khau';
            ```
        - Gán quyền cho user
            ```
            GRANT ALL PRIVILEGES ON ten_database.* TO 'ten_user'@'localhost';
            FLUSH PRIVILEGES;
            ```
        - Xem quyền user
            ```
            SHOW GRANTS FOR 'ten_user'@'localhost';
            ```
        - Xoá user
            ```
            DROP USER 'ten_user'@'localhost';
            ```
    - So sánh MySQL và MariaDB
        | Tiêu chí              | MySQL                                | MariaDB                                  |
        |----------------------|--------------------------------|---------------------------------|
        | **Hiệu suất**       | Tốt                            | Nhanh hơn MySQL (cải tiến về tốc độ) |
        | **Tính năng**       | Ít tính năng mới hơn           | Hỗ trợ nhiều tính năng mở rộng |
        | **Tính tương thích**| Oracle sở hữu, có thể hạn chế mã nguồn mở | Tương thích MySQL, hoàn toàn mã nguồn mở |
        | **Hỗ trợ JSON**     | Hỗ trợ tốt                      | Tương tự MySQL |
        | **Bảo mật**         | Tốt                            | Cải thiện bảo mật từ MySQL |
        | **Quản lý dữ liệu lớn** | Hoạt động tốt với dữ liệu lớn  | Tối ưu cho hệ thống lớn hơn |

### 4. Tìm hiểu,cài đặt nfs
#### NFS là gì?
- NFS (Network File System) là giao thức chia sẻ tệp tin qua mạng trên hệ điều hành Linux/Unix, cho phép nhiều máy truy cập chung vào một thư mục từ xa như thể nó nằm trên ổ đĩa cục bộ.
#### Cài đặt NFS trên CentOS/AlmaLinux & Ubuntu
- Trên Máy chủ (NFS Server)
    - Cài đặt NFS Server
        - CentOS / AlmaLinux / RHEL
            ```
            yum install -y nfs-utils
            ```
        - Ubuntu / Debian
            ```
            apt install -y nfs-kernel-server
            ```
    - Tạo thư mục chia sẻ & phân quyền
        ```
        mkdir -p /mnt/nfs_share
        chown nobody:nogroup /mnt/nfs_share
        chmod 777 /mnt/nfs_share
        ```
    - Cấu hình tệp /etc/exports
        - Mở file /etc/exports để chỉ định thư mục cần chia sẻ:
            ```
            vi /etc/exports
            ```
        - Thêm dòng sau:
            ```
            /mnt/nfs_share (ip được phép truy cập)(rw,sync,no_root_squash,no_subtree_check)
            ```
    - Áp dụng cấu hình
        ```
        exportfs -arv
        systemctl restart nfs-server
        ```
- Trên Máy khách (NFS Client)
    - Cài đặt NFS Client
        - CentOS / AlmaLinux / RHEL
            ```
            yum install -y nfs-utils
            ```
        - Ubuntu / Debian
            ```
            apt install -y nfs-common
            ```
    - Mount thư mục NFS
        - Tạo thư mục và mount vào thư mục vừa tạo
            ```
            mkdir -p /mnt/nfs_client
            mount -t nfs (ip nfs server):/mnt/nfs_share /mnt/nfs_client
            ```
            (Kiểm tra bằng df -h để xem thư mục đã mount chưa.)

        - Tự động mount khi khởi động: Thêm vào /etc/fstab
            ```
            (ip nfs server):/mnt/nfs_share /mnt/nfs_client nfs defaults 0 0
            ```
        