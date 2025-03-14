## Tìm hiểu Bash Script
### 1. Tìm hiểu các cú pháp cơ bản trong Bash Shell Script
#### Shebang và Comment
- Shebang
    - Mỗi script thường bắt đầu bằng dòng shebang chỉ định trình thông dịch:
    ```bash
    #!/bin/bash
    ```
- Comment
    - Dòng bắt đầu bằng # là comment và không được thực thi
#### Biến và Cách Sử Dụng
- Định nghĩa biến
    - Không có khoảng trắng quanh dấu =:
    ```bash
    name="Alice"
    count=10
    ```
- Sử dụng biến
    - Truy cập biến bằng ký hiệu $:
    ```bash
    echo "Tên: $name, Số đếm: $count"
    ```
- Command Substitution
    - Gán kết quả của lệnh cho biến:
    ```bash
    current_date=$(date)
    echo "Hôm nay là: $current_date"
    ```
#### Cấu Trúc Điều Kiện (If...Else)
- Cú pháp cơ bản:
    ```bash
    if [ condition ]; then
        # Các lệnh khi điều kiện đúng
        echo "Điều kiện đúng"
    elif [ another_condition ]; then
        echo "Điều kiện elif đúng"
    else
        echo "Điều kiện sai"
    fi
    ```
    Lưu ý: Sử dụng dấu cách giữa [ và ] và các toán tử so sánh như -eq, -ne, -lt, -gt, -le, -ge cho số và =, != cho chuỗi.
#### Vòng Lặp
- Vòng lặp for:
    ```bash
    for i in {1..5}; do
        echo "Vòng lặp thứ $i"
    done
    ```
- Vòng lặp while:
    ```bash
    counter=1
    while [ $counter -le 5 ]; do
        echo "Counter = $counter"
        counter=$((counter + 1))
    done
    ```
- Loop qua mảng:
    ```bash
    fruits=("apple" "banana" "cherry")
    for fruit in "${fruits[@]}"; do
        echo "Trái cây: $fruit"
    done
    ```
#### Hàm (Function)
- Định nghĩa và gọi hàm:
    ```bash
    my_function() {
        echo "Đây là hàm của tôi"
    }

    # Gọi hàm
    my_function
    ```
    Bạn cũng có thể truyền tham số vào hàm:
    ```bash
    greet() {
        echo "Xin chào, $1!"
    }

    greet "Thế giới"
    ```
#### Các Kỹ Thuật Khác
- Toán học
    - Sử dụng $(( ... )) để thực hiện các phép tính số học:
    ```bash
    a=5
    b=3
    sum=$((a + b))
    echo "Tổng: $sum"
    ```
- Input/Output Redirection
    - Ghi kết quả của lệnh vào file:
    ```bash
    echo "Dòng dữ liệu" > file.txt  # Ghi đè
    echo "Thêm dữ liệu" >> file.txt  # Thêm vào cuối file
    ```
- Xử lý lỗi và mã thoát
    - Mã thoát của lệnh ($?) cho biết lệnh có thực hiện thành công hay không:
    ```bash
    ls /nonexistent
    if [ $? -ne 0 ]; then
        echo "Lệnh thất bại"
    fi
    ```
- Sử dụng các toán tử logic
    - Ví dụ, kết hợp các lệnh với && (nếu lệnh trước thành công) và || (nếu lệnh trước thất bại):
    ```bash
    mkdir new_folder && cd new_folder
    ```
### Bài 1: Hello world:  Tạo file /root/lab01.sh có thể chạy được và khi chạy hiển thị dòng chữ “Hello world”
```bash
vi lab01.sh
```
    echo "Hello world"
```
./lab01.sh
```
### Bài 2: Tham số và IF:  Tạo file /root/lab02.sh có 3 tham số kiểu `30 20 +` trong đó hai tham số đầu tiên là toán tử, tham số thứ3 là phép toán. Output ra màn hình khi chạy file này sẽ có dạng A + B = 50 tức là tính kết quả của phép toán đối với hai toán tử nhập vào như tham số. Lưu ý có kiểm tra phép toán chia và số chia = 0.
```
vi lab02.sh
```
    #!/bin/bash
    a=30
    b=20
    sum=$((a+b))
    echo "A+B=$sum"
```
./lab02.sh
```
