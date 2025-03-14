# Tìm hiểu Linux Service
## Linux Basic
### 1. Tìm hiểu file stream: stdin, stdout, stderr.  ý nghĩa /dev/null.
#### File Stream: stdin, stdout, stderr
- **File stream** là cơ chế mà hệ điều hành sử dụng để giao tiếp giữa các chương trình, cung cấp hoặc nhận dữ liệu. Trong hệ thống Unix/Linux, ba file stream chính là:

a. stdin (Standard Input)

    Ý nghĩa: Là luồng dữ liệu đầu vào mặc định của chương trình.
    Mặc định: Bàn phím (khi chạy chương trình từ terminal).
    File descriptor: 0.
    Sử dụng:
        Dùng để nhập dữ liệu từ người dùng hoặc từ một file.
        Ví dụ trong Bash:

        read name
        echo "Hello, $name"

b. stdout (Standard Output)

    Ý nghĩa: Là luồng dữ liệu đầu ra mặc định để xuất kết quả của chương trình.
    Mặc định: Terminal (màn hình).
    File descriptor: 1.
    Sử dụng:
        In ra kết quả, thông báo hoặc dữ liệu.
        Ví dụ trong C:

        printf("Hello, World!\n");

c. stderr (Standard Error)

    Ý nghĩa: Là luồng dữ liệu đầu ra dành riêng cho thông báo lỗi.
    Mặc định: Terminal (màn hình).
    File descriptor: 2.
    Sử dụng:
        Hiển thị lỗi hoặc cảnh báo mà không làm gián đoạn stdout.
        Ví dụ trong Python:

        import sys
        sys.stderr.write("Error: Something went wrong!\n")

2. Ý nghĩa của /dev/null

a. /dev/null là gì?

    /dev/null là một file đặc biệt trong Unix/Linux, được gọi là "bit bucket" hoặc "black hole".
    Tác dụng:
        Bỏ qua bất kỳ dữ liệu nào được ghi vào đó.
        Trả về trạng thái rỗng (EOF - End Of File) nếu đọc từ nó.

b. Ứng dụng của /dev/null

    Loại bỏ đầu ra không cần thiết:
        Chuyển hướng stdout hoặc stderr vào /dev/null để bỏ qua đầu ra.

command > /dev/null  ## Bỏ stdout
command 2> /dev/null  ## Bỏ stderr
command > /dev/null 2>&1  ## Bỏ cả stdout và stderr

Kiểm tra quyền truy cập hoặc tồn tại:

    Dùng /dev/null để kiểm tra mà không tạo file.

cat file.txt > /dev/null

Dùng như một bộ đệm trống:

    Dùng khi cần đầu vào rỗng.

    command < /dev/null

c. Ví dụ thực tế

    Chạy cron jobs không tạo log:

    0 * * * * /path/to/script.sh > /dev/null 2>&1

        Loại bỏ cả đầu ra và lỗi từ script.

3. Kết luận

    stdin, stdout, stderr là các luồng cơ bản giúp quản lý dữ liệu vào/ra trong hệ thống.
    /dev/null là công cụ hữu ích để loại bỏ hoặc kiểm soát đầu ra, thường dùng khi không cần log hoặc muốn giảm bớt nhiễu trong terminal.

### 2. sự khác biệt giữa ">/dev/null 2>&1" và "2>&1 >/dev/null"
#### 1. Lệnh >/dev/null 2>&1
- **>/dev/null**
    - Chuyển hướng **stdout** (file descriptor 1) đến `/dev/null`
- **2>&1**
    - Chuyển hướng **stderr** (file descriptor 2) đến nơi mà stdout đang trỏ (hiện tại là `/dev/null`).
- **Kết quả**: Cả stdout và stderr đều được gửi đến /dev/null (tức là không hiển thị).
#### 2. Lệnh 2>&1 >/dev/null
- **2>&1**
    - Chuyển hướng **stderr** đến nơi mà stdout đang trỏ lúc này (thường là terminal).
- **>/dev/null**
    - Chuyển hướng stdout đến `/dev/null`
- **Kết quả**:

    - **stderr** đã được chuyển hướng về terminal (vẫn hiển thị).
    - **stdout** được gửi đến /dev/null (không hiển thị).

### 3. tìm hiểu `tty`
#### - `tty` trong Linux là gì?
- Trong Linux, `tty` (teletypewriter) đề cập đến terminal (giao diện dòng lệnh) mà người dùng đang sử dụng để tương tác với hệ thống. Nó có nguồn gốc từ những thiết bị đầu vào/đầu ra vật lý thời kỳ đầu, nhưng ngày nay nó chủ yếu ám chỉ các phiên terminal trên hệ thống.
#### 1. Lệnh `tty`
- Lệnh `tty` trong Linux được sử dụng để hiển thị terminal hiện tại của người dùng.
    - Cú pháp: 
    ```bash
    tty
    ```
    - Ví dụ:
    ```bash
    $ tty
    /dev/pts/0
    ```
- Kết quả cho thấy người dùng đang sử dụng pseudo-terminal (/dev/pts/0), tức là một phiên SSH hoặc terminal giả lập trong môi trường đồ họa.
- Nếu bạn đang làm việc trực tiếp trên máy, nó có thể hiển thị như:
    ```bash
    /dev/tty1
    ```
- Điều này có nghĩa là bạn đang ở chế độ console vật lý .
#### 2. Các loại `tty` trong Linux
- Linux có hai loại tty chính:
    - Console TTY (Virtual Console)
        - Các thiết bị tty vật lý (/dev/tty1, /dev/tty2, ... /dev/tty6).
        - Bạn có thể chuyển đổi giữa các console bằng tổ hợp phím Ctrl + Alt + F1 đến Ctrl + Alt + F6.
    - Pseudo-Terminal (PTS)
        - Các terminal giả lập như GNOME Terminal, xTerm, SSH sử dụng /dev/pts/ (pseudo-terminal).
        - Ví dụ: `/dev/pts/0`, `/dev/pts/1`.
#### 3. Liệt kê các phiên `tty` đang hoạt động
- Bạn có thể dùng lệnh sau để xem danh sách các phiên `tty` đang chạy:
    ```bash
    who
    ```
- Ví dụ:
    ```bash
    $ who
    user     pts/0        2025-03-03 12:30 (192.168.1.10)
    user     pts/1        2025-03-03 12:35 (:0)
    ```
- Điều này cho thấy có hai phiên terminal đang hoạt động:
    - `pts/0`: một phiên SSH từ IP `192.168.1.10`.
    - `pts/1`: một terminal GUI.
#### 4. Chạy lệnh trên một `tty` cụ thể
- Bạn có thể gửi lệnh đến một tty khác bằng cách sử dụng echo và >/dev/ttyX:
    ```bash
    echo "Hello, tty1!" > /dev/pts/1
    ```
- Lệnh này gửi chuỗi `"Hello, tty1!"` đến phiên `tty1`.
#### 5. Mở một `tty` mới
- Để mở một terminal mới, bạn có thể dùng:
    ```bash
    ctrl + alt + F1 (hoặc F2, F3,...)
    ```
- Để quay lại GUI:
    ```bash
    ctrl + alt + F7 (hoặc F8)
    ```
- Hoặc nếu bạn đang ở trong một terminal và muốn mở một shell mới trên tty khác:
    ```bash
    chvt 2  ## Chuyển sang tty2
    ```
#### 6. Đặt chế độ terminal với `stty`
- Lệnh stty cho phép kiểm soát cài đặt tty. Ví dụ:
    ```bash
    stty -a  ## Hiển thị cấu hình terminal
    stty sane  ## Khôi phục terminal về trạng thái bình thường
    ```
#### 7. Tắt đầu vào trên một `tty` cụ thể
- Bạn có thể vô hiệu hóa đầu vào của một `tty` bằng cách:
    ```bash
    stty -F /dev/tty2 -echo
    ```
- Lệnh này tắt đầu vào của tty2.
- Để bật lại:
    ```bash
    stty -F /dev/tty2 echo
    ```
#### 8. Thoát `tty` hoặc đăng xuất
- Để đăng xuất khỏi một phiên tty, bạn có thể dùng:
    ```bash
    exit
    hoặc
    logout
    ```
- Nếu muốn đóng phiên `tty` từ xa:
    ```bash
    pkill -KILL -t pts/1
    ```
Lệnh này sẽ buộc phiên pts/1 thoát.

#### Tóm tắt nhanh
| Lệnh | Mô tả |
|------|------|
| `tty` | Kiểm tra terminal hiện tại |
| `who` | Liệt kê các phiên `tty` đang hoạt động |
| `chvt X` | Chuyển sang `ttyX` |
| `stty -a` | Xem cấu hình `tty` |
| `stty sane` | Khôi phục `tty` về trạng thái mặc định |
| `pkill -KILL -t pts/0` | Đóng một phiên `tty` cụ thể |


### 4. Crontab
#### 1. Crontab là gì?
- Crontab (Cron Table) là một tệp chứa danh sách các lệnh được lên lịch chạy tự động vào các thời điểm hoặc khoảng thời gian xác định trên hệ thống Unix/Linux. Nó hoạt động dựa trên cron daemon (crond), một trình nền chạy ngầm để thực thi các tác vụ theo lịch trình.

#### 2. Cấu trúc của Crontab
- Mỗi dòng trong Crontab đại diện cho một tác vụ (cron job) và có cú pháp:
    ```plaintext
    * * * * * command_to_run
    - - - - -
    | | | | |  
    | | | | +---- Ngày trong tuần (0 - 7) [0,7 = Chủ Nhật]  
    | | | +------ Tháng (1 - 12)  
    | | +-------- Ngày trong tháng (1 - 31)  
    | +---------- Giờ (0 - 23)  
    +------------ Phút (0 - 59)  
    ```
- Ví dụ:

   - 0 5 * * * /path/to/script.sh → Chạy script.sh vào 5:00 sáng mỗi ngày.
   - 30 22 * * 1-5 /path/to/script.sh → Chạy lúc 22:30 từ thứ Hai đến thứ Sáu.
   - 0 */6 * * * /path/to/script.sh → Chạy mỗi 6 giờ.
   - 0 0 1 * * /path/to/script.sh → Chạy vào 00:00 ngày 1 hàng tháng.

#### 3. Quản lý Crontab
##### 3.1. Kiểm tra Crontab hiện tại
```bash
crontab -l
```
*(Xem danh sách cron jobs của người dùng hiện tại)*
##### 3.2. Chỉnh sửa Crontab
```bash
crontab -e
```
*(Mở trình soạn thảo để thêm/sửa cron jobs)*
##### 3.3. Xóa Crontab
```bash
crontab -r
```
*(Xóa toàn bộ cron jobs của người dùng hiện tại)*
##### 3.4. Chỉnh sửa Crontab cho người dùng khác (chỉ dành cho root)
```bash
crontab -u username -e
```

#### 4. Các ký tự đặc biệt trong Crontab
| Lệnh | Mô tả |
|------|------|
| `*` | Bất kỳ giá trị nào |
| `,` | Liệt kê nhiều giá trị (VD: `1,5,10` → chạy vào phút 1, 5, 10) |
| `-` | Khoảng giá trị (VD: `1-5` → chạy từ phút 1 đến 5) |
| `/` | Bước nhảy (VD: `*/10` → chạy mỗi 10 phút) |

Ví dụ:
   - `*/15 * * * * /path/to/script.sh` → Chạy mỗi 15 phút.
   - `0 9-17 * * 1-5 /path/to/script.sh` → Chạy từ 9:00 đến 17:00, từ Thứ Hai đến Thứ Sáu.

#### 5. Các từ khóa đặc biệt
| Lệnh | Mô tả |
|------|------|
| `@reboot` | Chạy một lần khi hệ thống khởi động |
| `@hourly` | `0 * * * *` (Mỗi giờ một lần) |
| `@daily` | `0 0 * * *` (Mỗi ngày vào 00:00) |
| `@weekly` | `0 0 * * 0` (Mỗi tuần vào Chủ Nhật) |
| `@monthly` | `0 0 1 * *` (Mỗi tháng vào ngày 1) |
| `@yearly` | `0 0 1 1 *` (Mỗi năm vào ngày 1 tháng 1) |

#### 6. Lưu ý khi sử dụng Crontab
- Đường dẫn tuyệt đối: Khi chạy script, nên sử dụng đường dẫn đầy đủ, ví dụ:
    ```bash
    /usr/bin/php /home/user/script.php
    ```
- Kiểm tra log khi cron không chạy:
    - Log cron có thể nằm trong /var/log/cron (trên CentOS/AlmaLinux) hoặc /var/log/syslog (trên Ubuntu/Debian):
        ```bash
        tail -f /var/log/cron
        ```
    - Hoặc ghi log khi chạy:
        ```bash
        0 5 * * * /path/to/script.sh >> /var/log/myscript.log 2>&1
        ```
- Kiểm tra trạng thái cron daemon:
    ```bash
    systemctl status crond  ## CentOS/AlmaLinux
    systemctl status cron    ## Ubuntu/Debian
    ```
- Nếu cron chưa chạy, khởi động lại:
    ``` bash
    systemctl restart crond
    ```

### 5. du , df, lsblk, egrep, grep, sed , awk command
#### 1. du (Disk Usage)
- **Mục đích**: Tính toán và báo cáo dung lượng ổ đĩa đang được sử dụng bởi các tệp và thư mục.
#### 2. df (Disk Free)
- **Mục đích**: Hiển thị dung lượng trống và đã sử dụng trên các hệ thống tập tin đã được gắn (mounted).
#### 3. lsblk (List Block Devices)
- **Mục đích**: Liệt kê tất cả các thiết bị block (như ổ đĩa, phân vùng, thiết bị lưu trữ di động) theo dạng cây phân cấp.
#### 4. egrep (Extended grep)
- **Mục đích**: Tìm kiếm các mẫu văn bản trong file sử dụng biểu thức chính quy mở rộng.
#### 5. grep (Global Regular Expression Print)
- **Mục đích**: Tìm kiếm các dòng văn bản trong file phù hợp với một biểu thức chính quy.
#### 6. sed (Stream Editor)
- **Mục đích**: Chỉnh sửa và biến đổi văn bản dựa trên các lệnh, thường được dùng để thay thế nội dung.
#### 7. awk (Pattern Scanning and Processing)
- **Mục đích**: Một ngôn ngữ xử lý văn bản mạnh mẽ, được thiết kế để trích xuất và báo cáo dữ liệu.