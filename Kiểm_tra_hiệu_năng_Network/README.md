## Kiểm tra hiệu năng Network
### 1. Tìm hiểu các command phổ triển trong netstat và ss
#### netstat – Network Statistics
- `netstat` là công cụ truyền thống được dùng để xem các kết nối mạng, routing table, interface, port đang lắng nghe, v.v.
- Các lệnh phổ biến:

| Lệnh            | Giải thích                                                                              |
| --------------- | --------------------------------------------------------------------------------------- |
| `netstat -tuln` | Hiển thị các cổng **TCP/UDP** đang lắng nghe dưới dạng **numeric** (không resolve DNS). |
| `netstat -an`   | Hiển thị tất cả các kết nối và cổng đang lắng nghe.                                     |
| `netstat -p`    | Hiển thị tiến trình (PID) sử dụng kết nối. Thường kết hợp với `-tuln`: `netstat -tulnp` |
| `netstat -r`    | Xem **routing table** (giống `route -n`).                                               |
| `netstat -i`    | Hiển thị thông tin các network interface.                                               |
| `netstat -s`    | Thống kê giao thức (TCP, UDP, ICMP…).                                                   |

#### ss – Socket Statistics
- `ss` là công cụ hiện đại và nhanh hơn `netstat`. Nó thường được cài sẵn trên các hệ thống Linux mới.
- Các lệnh phổ biến:

| Lệnh                    | Giải thích                                              |
| ----------------------- | ------------------------------------------------------- |
| `ss -tuln`              | Hiển thị các cổng TCP/UDP đang lắng nghe.               |
| `ss -s`                 | Thống kê kết nối mạng ngắn gọn (summary).               |
| `ss -tan`               | Hiển thị tất cả các kết nối TCP ở mọi trạng thái.       |
| `ss -tp`                | Hiển thị tiến trình sử dụng TCP sockets.                |
| `ss -uap`               | Hiển thị tiến trình sử dụng UDP sockets.                |
| `ss -l`                 | Chỉ hiển thị các socket đang **lắng nghe** (listening). |
| `ss -4` hoặc `ss -6`    | Lọc theo IPv4 hoặc IPv6.                                |
| `ss -state ESTABLISHED` | Lọc chỉ các kết nối đang **hoạt động**.                 |

#### So sánh :

| Tính năng          | `netstat`                       | `ss`               |
| ------------------ | ------------------------------- | ------------------ |
| Tốc độ             | Chậm hơn                        | Nhanh hơn          |
| Độ chi tiết        | Đầy đủ                          | Rất đầy đủ         |
| Tình trạng bảo trì | Đã lỗi thời trên nhiều hệ thống | Thay thế `netstat` |

### 2. Cài đặt, tìm hiểu các command trong nuttcp
- `nuttcp` là một công cụ đo hiệu suất mạng mạnh mẽ và linh hoạt, tương tự như `iperf`, được sử dụng để kiểm tra băng thông, độ trễ và mất gói giữa hai máy tính. Dưới đây là hướng dẫn cài đặt và tìm hiểu các lệnh cơ bản trong `nuttcp`.
#### Cài đặt `nuttcp`
- Trên Ubuntu/Debian:
    ```bash
    apt install nuttcp
    ```
- Trên CentOS/RHEL/Fedora:
    ```bash
    yum install nuttcp
    # hoặc
    dnf install nuttcp
    ```
-  Trên macOS (Homebrew):
    ```bash
    brew install nuttcp
    ```
- Kiểm tra phiên bản:
    ```bash
    nuttcp -V
    ```

#### Cách sử dụng cơ bản
- Chạy server `nuttcp` trên máy đích:
    ```bash
    nuttcp -S
    ```
    Máy đích sẽ lắng nghe các kết nối từ máy gửi.
- Gửi dữ liệu từ máy gửi:
    ```bash
    nuttcp <IP-máy-đích>
    ```
    Lệnh này gửi 10 giây dữ liệu TCP mặc định đến máy đích.
#### Các tuỳ chọn phổ biến trong `nuttcp`
| Lệnh         | Mô tả                                     |
| ------------ | ----------------------------------------- |
| `-S`         | Chạy ở chế độ server                      |
| `-t <sec>`   | Thời gian gửi dữ liệu (mặc định: 10 giây) |
| `-i <sec>`   | Tần suất hiển thị thống kê theo giây      |
| `-u`         | Sử dụng UDP thay vì TCP                   |
| `-R`         | Nhận dữ liệu thay vì gửi                  |
| `-r`         | Gửi và nhận dữ liệu (full-duplex test)    |
| `-w <bytes>` | Kích thước TCP window                     |
| `-n <bytes>` | Gửi tổng cộng số byte nhất định           |
| `-p <port>`  | Chỉ định cổng                             |
| `-v`         | Hiển thị thông tin chi tiết               |

### 3. Cài đặt, tìm hiểu các command iperf3
- `iperf3` là một công cụ dòng lệnh mạnh mẽ dùng để **đo kiểm hiệu suất mạng** giữa hai thiết bị. Nó thường được dùng để kiểm tra **băng thông, độ trễ, jitter**, v.v. giữa client và server.
#### Cài đặt `iperf3`
- Trên Ubuntu/Debian:
    ```bash
    apt install iperf3
    ```
- Trên CentOS/RHEL/Fedora:
    ```bash
    yum install iperf3
    ```
-  Trên macOS (Homebrew):
    ```bash
    brew install iperf3
    ```
#### Cách sử dụng cơ bản
- Chạy server `iperf3`(trên máy A):
    ```bash
    iperf3 -s
    ```
-  Chạy client (trên máy B, kết nối đến máy A):
    ```bash
    iperf3 -c <IP_của_máy_A>
    ```
#### Các lệnh phổ biến
| Lệnh                    | Mô tả                                           |
| ----------------------- | ----------------------------------------------- |
| `iperf3 -s`             | Chạy ở chế độ server                            |
| `iperf3 -c <server_ip>` | Kết nối đến server                              |
| `-t <seconds>`          | Thời gian kiểm tra, mặc định 10s                |
| `-p <port>`             | Cổng sử dụng, mặc định 5201                     |
| `-u`                    | Sử dụng UDP thay vì TCP                         |
| `-b <bandwidth>`        | Giới hạn băng thông (chỉ với UDP). VD: `-b 10M` |
| `-R`                    | Reverse test (client nhận, server gửi)          |
| `-f <format>`           | Đơn vị hiển thị (`k`, `m`, `g`)                 |
| `-i <seconds>`          | Hiển thị kết quả mỗi X giây                     |
