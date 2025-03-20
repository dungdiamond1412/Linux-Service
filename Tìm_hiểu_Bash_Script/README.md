## Tìm hiểu Bash Script
### 1. Tìm hiểu các cú pháp cơ bản trong Bash Shell Script
#### Shebang và Comment
- Shebang
    - Mỗi script thường bắt đầu bằng dòng shebang chỉ định trình thông dịch:
    ```
    #!/bin/bash
    ```
- Comment
    - Dòng bắt đầu bằng # là comment và không được thực thi
#### Biến và Cách Sử Dụng
- Định nghĩa biến
    - Không có khoảng trắng quanh dấu =:
    ```
    name="Alice"
    count=10
    ```
- Sử dụng biến
    - Truy cập biến bằng ký hiệu $:
    ```
    echo "Tên: $name, Số đếm: $count"
    ```
- Command Substitution
    - Gán kết quả của lệnh cho biến:
    ```
    current_date=$(date)
    echo "Hôm nay là: $current_date"
    ```
#### Cấu Trúc Điều Kiện (If...Else)
- Cú pháp cơ bản:
    ```
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
    ```
    for i in {1..5}; do
        echo "Vòng lặp thứ $i"
    done
    ```
- Vòng lặp while:
    ```
    counter=1
    while [ $counter -le 5 ]; do
        echo "Counter = $counter"
        counter=$((counter + 1))
    done
    ```
- Loop qua mảng:
    ```
    fruits=("apple" "banana" "cherry")
    for fruit in "${fruits[@]}"; do
        echo "Trái cây: $fruit"
    done
    ```
#### Hàm (Function)
- Định nghĩa và gọi hàm:
    ```
    my_function() {
        echo "Đây là hàm của tôi"
    }

    # Gọi hàm
    my_function
    ```
    Bạn cũng có thể truyền tham số vào hàm:
    ```
    greet() {
        echo "Xin chào, $1!"
    }

    greet "Thế giới"
    ```
#### Các Kỹ Thuật Khác
- Toán học
    - Sử dụng $(( ... )) để thực hiện các phép tính số học:
    ```
    a=5
    b=3
    sum=$((a + b))
    echo "Tổng: $sum"
    ```
- Input/Output Redirection
    - Ghi kết quả của lệnh vào file:
    ```
    echo "Dòng dữ liệu" > file.txt  # Ghi đè
    echo "Thêm dữ liệu" >> file.txt  # Thêm vào cuối file
    ```
- Xử lý lỗi và mã thoát
    - Mã thoát của lệnh ($?) cho biết lệnh có thực hiện thành công hay không:
    ```
    ls /nonexistent
    if [ $? -ne 0 ]; then
        echo "Lệnh thất bại"
    fi
    ```
- Sử dụng các toán tử logic
    - Ví dụ, kết hợp các lệnh với && (nếu lệnh trước thành công) và || (nếu lệnh trước thất bại):
    ```
    mkdir new_folder && cd new_folder
    ```
### Bài 1: Hello world:  Tạo file /root/lab01.sh có thể chạy được và khi chạy hiển thị dòng chữ “Hello world”
```
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
    echo -n "Nhập số a: "
    read a
    echo -n "Nhập số b: "
    read b
    echo -n "Nhập phép toán: "
    read c

    if [ "$c" = "+" ]; then
            tong=$(($a + $b))
            echo "$a + $b = $tong"
    elif [ "$c" = "-" ]; then
            hieu=$(($a - $b))
            echo "$a - $b = $hieu"
    elif [ "$c" = "*" ]; then
            tich=$(($a * $b))
            echo "$a * $b = $tich"
    elif [ "$c" = '/' ]; then
            if [ "$b" -eq 0 ]; then
                    echo "Không thể chia cho 0"

            else
                    thuong=$(($a / $b))
                    echo "$a / $b = $thuong"
            fi
    else
            echo "Không phải phép tính"
    fi
```
./lab02.sh
```

### Bài 3: Kiểm tra:  Tạo file /root/lab03.sh có 2 tham số, tham số đầu là đường dẫn đến 1 file, tham số thứ hai là dòng cần hiển thị trên màn hình. Kiểm tra sự tồn tại của file, số dòng của file có >= dòng cần hiển thị không? Nếu đúng hiện ra dòng cần hiển thị
```
vi lab03.sh
```
    #!/bin/bash
    echo -n "Nhập đường dẫn file: "
    read file
    echo -n "Nhập dòng bạn cần hiển thị: "
    read dong
    if [ ! -f "$file" ]; then
            echo "Không có file $file tồn tại"
            exit 1
    fi
    tong_dong=$(wc -l < "$file")

    if [ $dong -gt $tong_dong ]; then
            echo "Chỉ có $tong_dong, không thể hiển thị dòng số $dong"
            exit 1
    fi

    echo -n "Dòng số $dong trong file $file: "

    sed -n "$dong"p "$file"
```
./lab03.sh
```

### Bài 4: FOR:  Tạo file /root/lab04.sh có chức năng sau: Nhập một số nguyên trong khoảng 1000 đến 50000. Kiểm tra tính đúng đắn của số này, nếu đúng in ra kết quả số nhập vào là số nguyên tố hay hợp số
```
vi lab04.sh
```
    echo -n "Nhập một số nguyên trong khoảng 1000 đến 50000: "
    read nu

    if [ "$nu" -lt 1000 ] || [ "$nu" -gt 50000 ]; then
            echo "Không thuộc trong khoảng từ 1000 đến 50000"
            exit
    fi
    for ((i = 2; i * i <= $nu; i++)); do
        if (( $nu % i == 0 )); then
            echo "$nu là hợp số"
            exit 0
        fi
    done
    echo "$nu là số nguyên tố"
```
./lab04.sh
```

### Bài 5: WHILE: Tạo file /root/lab05.sh có chức năng sau: Nhập một số nguyên dương trong khoảng 1000 đến 5000. Kiểm tra tính đúng đắn của số này, nếu đúng in ra số Fibonaxy lớn nhất nhưng nhỏ hơn số mới nhập.
```
vi lab05.sh
```
    #!/bin/bash
    echo -n "Nhập một số nguyên dương trong khoảng 1000 đến 5000: "
    read nu

    if [ $nu -lt 1000 ] || [ $nu -gt 5000 ]; then
            echo "$nu không lằm trong khoảng từ 1000 đến 5000"
            exit 1
    fi
    a=0
    b=1
    i=1
    while [ $b -lt $nu ]; do
            i=$b
            fn=$((a + b))
            a=$b
            b=$fn
    done
    echo "Số Fibonacci lớn nhất nhưng nhỏ hơn $nu là: $i"
```
./lab05.sh
```

### Bài 6: Xử lý xâu, ký tự: Tạo file /root/lab06.sh có chức năng sau: Nhập một xâu ký tự từ bàn phím, đếm và in ra màn hình số ký tự hoa, số ký tự thường trong xâu

```
vi lab06.sh
```
    #!/bin/bash
    read -p "Nhập 1 xâu ký tự: " input

    hoa=$(echo -n "$input" | grep -o '[A-Z]' | wc -l)
    thuong=$(echo -n "$input" | grep -o '[a-z]' | wc -l)

    echo "Số ký tự hoa là: $hoa"
    echo "Số ký tự thường là: $thuong"
```
./lab06.sh
```

### Bài 7: Cho danh sách các package : wget, curl, mtr , httpd được liệt kê trong file. Viết bash script đọc file liệt kê các package nằm trong danh sách đã sẵn trên hệ thống , sau đó cài đặt các package chưa được cài đặt.
```
vi file.txt
```
    wget
    curl
    mtr
    apache2
```
vi install.sh
```
    #!/bin/bash

    FILE=~/file.txt
    if [ ! -f $FILE ]; then
            echo "Không có file $FILE"
            exit 1
    fi

    while IFS= read -r package; do
            if dpkg -l | grep -q "^ii  $package "; then
                    echo "Package $package đã được cài đặt"
            else
                    echo "Đang cài package: $package..."
                    apt install -y "$package"
                    if [ $? -eq 0 ]; then
                            echo "Đã cài $package thành công"
                    else
                            echo "Cài $package không thành công"
                    fi
            fi
    done < "$FILE
```
./install.sh
```

### - Tìm hiểu ý nghĩa #! trong bash shell, các cách  thực thi một file script
- **#! (Shebang) là gì**
    - **#! (Shebang)** trong các hệ thống nhân Unix, khi một Script được chạy, đầu tiên program loader sẽ dựa vào Shebang để xác định Script đó sẽ được chạy bới trình biên dịch nào.
    - Nếu không có Shebang thì mặc định sẽ được chạy bởi shell
- **Cách hệ thống xử lý shebang**   
    - Khi bạn chạy script bằng cách gọi trực tiếp:
        ```
        ./script.sh
        ```
        - Hệ thống sẽ đọc dòng đầu tiên của script.
        - Nếu có #! (shebang), nó sẽ sử dụng trình thông dịch được chỉ định.
        - Nếu không có #!, nó sẽ sử dụng shell mặc định của người dùng.
    - Ví dụ:
        ```
        #!/bin/bash
        echo "Chạy bằng Bash"
        ```
        - Khi chạy ./script.sh, hệ thống sẽ gọi /bin/bash script.sh.
        ```
        #!/usr/bin/python3
        print("Chạy bằng Python")
        ```
        - Khi chạy ./script.py, hệ thống sẽ gọi /usr/bin/python3 script.py.
    - Nếu không có #! khi chạy ./script.sh, hệ thống sẽ dùng shell mặc định của user (echo $SHELL để kiểm tra).
- **Một số shebang phổ biến**

| Shebang                | Ý nghĩa |
|------------------------|---------|
| `#!/bin/bash`         | Chạy script bằng Bash Shell. |
| `#!/bin/sh`           | Chạy script bằng Shell mặc định (thường liên kết đến Bash hoặc Dash). |
| `#!/usr/bin/env bash` | Tìm Bash trong `$PATH` và chạy script bằng Bash. |
| `#!/usr/bin/env python3` | Chạy script bằng Python 3. |
| `#!/usr/bin/env perl` | Chạy script bằng Perl. |
| `#!/usr/bin/env node` | Chạy script bằng Node.js. |
| `#!/bin/zsh`          | Chạy script bằng Zsh. |
| `#!/bin/ksh`          | Chạy script bằng KornShell (KSH). |
| `#!/usr/bin/awk -f`   | Chạy script bằng AWK. |
| `#!/usr/bin/sed -f`   | Chạy script bằng SED. |