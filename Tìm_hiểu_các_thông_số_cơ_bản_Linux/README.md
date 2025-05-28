## Tìm hiểu các thông số cơ bản Linux
### 1. Process metrics
- **Process metrics** (tạm dịch: **chỉ số quy trình**) là những chỉ số định lượng dùng để theo dõi, đánh giá và cải thiện quy trình làm việc. Chúng tập trung vào việc **đo lường cách thực hiện công việc**, không phải kết quả cuối cùng.

**Các loại chỉ số quy trình phổ biến**
- **Chỉ số hiệu suất (Efficiency)**
    - **Thời gian chu kỳ (Cycle Time)**: Thời gian hoàn thành một vòng quy trình.
    - **Thông lượng (Throughput)**: Số lượng công việc hoặc sản phẩm hoàn thành trong một khoảng thời gian.
    - **Mức sử dụng tài nguyên (Resource Utilization)**: Tỷ lệ phần trăm tài nguyên được sử dụng so với tổng số sẵn có.
- **Chỉ số hiệu quả (Effectiveness)**
    - **Tỷ lệ lỗi (Defect Rate)**: Phần trăm sản phẩm hoặc kết quả bị lỗi.
    - **Tỷ lệ làm lại (Rework Rate)**: Số lượng công việc phải làm lại do lỗi hoặc yêu cầu thay đổi.
- **Chỉ số tuân thủ (Compliance)**
    - **Mức độ tuân thủ quy trình**: Quy trình có được thực hiện đúng theo tiêu chuẩn/quy định đã đề ra không?
    - **Kết quả kiểm tra (Audit Results)**: Kết quả từ các cuộc kiểm tra nội bộ hoặc bên ngoài.
- **Chỉ số năng suất (Productivity)**
    - **Dòng mã viết ra mỗi lập trình viên (trong phát triển phần mềm)**
    - **Số nhiệm vụ hoàn thành mỗi giờ/lượt**
- **Chỉ số thời gian**
    - **Thời gian chờ (Lead Time)**: Tổng thời gian từ khi bắt đầu yêu cầu đến khi hoàn thành.
    - **Takt Time**: Thời gian cần thiết để tạo ra một đơn vị sản phẩm nhằm đáp ứng nhu cầu khách hàng.
- **Chỉ số chất lượng**
    - **Tỷ lệ đạt ngay lần đầu (First Pass Yield - FPY)**: Tỷ lệ sản phẩm hoàn thành mà không cần sửa lỗi.
    - **Tỷ lệ sai sót (Error Rate)**: Số lỗi xảy ra trong quy trình.

### 2. Memory metrics
- **Memory metrics** (tạm dịch: **chỉ số bộ nhớ**) là các chỉ số được sử dụng để theo dõi và đánh giá **việc sử dụng bộ nhớ** của một hệ thống, ứng dụng hoặc quy trình (process) — đặc biệt quan trọng trong **quản trị hệ thống**, **phát triển phần mềm**, hoặc **giám sát hiệu suất máy chủ**.

**Các loại chỉ số bộ nhớ phổ biến**
- **Total Memory (Tổng bộ nhớ)**
    - Tổng dung lượng RAM vật lý (hoặc khả dụng trong môi trường ảo).
    - Không thay đổi trừ khi nâng cấp phần cứng hoặc thay đổi cấu hình máy ảo.
- **Used Memory (Bộ nhớ đã sử dụng)**
    - Dung lượng bộ nhớ đang được sử dụng (bởi hệ điều hành, ứng dụng, tiến trình).
    - = Total - Free
- **Free Memory (Bộ nhớ còn trống)**
    - Dung lượng RAM chưa được sử dụng, sẵn sàng cho các ứng dụng mới.
- **Buffered/Cache Memory**
    - **Buffered Memory**: Bộ nhớ được hệ điều hành dùng để lưu tạm thời thông tin từ I/O (ví dụ: đọc file).
    - **Cache Memory**: Bộ nhớ tạm để tăng tốc độ truy xuất dữ liệu thường dùng.

    Lưu ý: Cả buffered và cached đều có thể được giải phóng nếu cần, nên không hẳn là "đã mất".
- **Swap Usage (Sử dụng bộ nhớ ảo)**
    - Khi RAM đầy, hệ thống dùng swap (đĩa cứng) làm bộ nhớ tạm — nhưng chậm hơn rất nhiều.
    - Swap cao thường là dấu hiệu thiếu RAM.
- **Page Faults (Lỗi trang bộ nhớ)**
    - Xảy ra khi hệ thống không tìm thấy dữ liệu trong RAM và phải lấy từ ổ cứng (swap hoặc ổ đĩa).
    - **Page Faults cao** có thể gây chậm hệ thống.
- **Memory Usage per Process (Bộ nhớ theo tiến trình)**
    - Hiển thị dung lượng bộ nhớ mà từng ứng dụng/tiến trình đang sử dụng.
    - Quan trọng để phát hiện tiến trình “ngốn RAM”.
- **Resident Set Size (RSS)**
    - Phần bộ nhớ thực sự đang chiếm dụng trong RAM (không tính phần đã swap hoặc dùng chung).
- **Virtual Memory Size (VSZ)**
    - Tổng dung lượng bộ nhớ mà tiến trình đặt giữ chỗ, bao gồm cả swap.

### 3. Network interface metrics: tx/rx theo packet hoặc bytes, throughput, bandwidth
- **Counters: TX/RX theo packet hoặc bytes**
    - **RX packets / TX packets**
        - Số gói nhận (RX) / gửi (TX) kể từ khi giao diện khởi động.
    - **RX bytes / TX bytes**
        - Tổng số byte nhận / gửi kể từ khi giao diện khởi động.
    
    **Cách hiển thị nhanh**:
    ```
    ip -s link show (tên card mạng)
    or
    ifconfig (tên card mạng)
    ```
- **Throughput (tốc độ thực tế)**
    
    Là tốc độ truyền dữ liệu thực tế trên giao diện, thường đo bằng *bits per second* (bps), hoặc *bytes per second* (B/s).
    - Công thức cơ bản: Throughput = (Δbytes) / (Δt)
    - **Lấy số liệu liên tục**:
        ```bash
        # sar từ sysstat
        sar -n DEV 1 5        # lấy thông tin network mỗi 1s, 5 lần
        # output: rxpck/s, txpck/s, rxkB/s, txkB/s, ...
        ```
        hoặc
        ```bash
        dstat -n            # real-time network stats
        nload eth0          # đồ thị ngay lập tức
        ```
- **Bandwidth (băng thông lý thuyết)**

    Là tốc độ tối đa mà giao diện có thể đạt được, do card và switch hỗ trợ (ví dụ: 100 Mbps, 1 Gbps, 10 Gbps).
    - **Xác định bằng:**
    ```bash
    ethtool eth0 | grep Speed
    ```
### 4. Block device metrics: Throughput,Latency,IOPS, sử dụng dd command
- Cấu trúc lệnh cơ bản:
    ```bash
    dd if=/dev/zero of=testfile bs=BLOCK_SIZE count=COUNT oflag=direct
    ```
    `if=/dev/zero`: dùng dữ liệu 0 để ghi vào file.

    `of=testfile`: đường dẫn đến file đầu ra (bạn có thể chỉ định file trên SSD/HDD cần test).

    `bs`: block size (kích thước mỗi lần ghi).

    `count`: số lượng block sẽ ghi.

    `oflag=direct`: bỏ qua cache của hệ thống, giúp kết quả chính xác hơn.

- Throughput (Tốc độ truyền tải - MB/s)
    
    Throughput là lượng dữ liệu có thể đọc hoặc ghi mỗi giây (MB/s).

    ```bash
    dd if=/dev/zero of=testfile bs=1M count=1024 oflag=direct
    ```
    - Block size lớn (1M) để kiểm tra throughput.
    - Đo kết quả từ dòng output:
        ```bash
        1073741824 bytes (1.1 GB, 1.0 GiB) copied, 2.66771 s, 402 MB/s
        ```
        -> Throughput = 402 MB/s
- Latency (Độ trễ)

    Latency là thời gian trung bình để hoàn thành một thao tác I/O.

    ```bash
    dd if=/dev/zero of=testfile bs=4K count=1 oflag=direct
    ```
    - Ghi một block nhỏ (4KB) để đo thời gian trễ.
    - Đo kết quả từ dòng output:
        ```bash
        4096 bytes (4.1 kB, 4.0 KiB) copied, 0.000799936 s, 5.1 MB/s
        ```
        -> Latency ≈ 0.8 ms
- IOPS (Số thao tác I/O mỗi giây)

    IOPS là số thao tác đọc/ghi mỗi giây, thường đo với block size nhỏ (4K).
    ```bash
    dd if=/dev/zero of=testfile bs=4K count=10000 oflag=direct
    ```
    - IOPS = Tổng số thao tác / Tổng thời gian (từ dòng output)
    - Đo kết quả từ dòng output:
        ```bash
        40960000 bytes (41 MB, 39 MiB) copied, 1.10837 s, 37.0 MB/s
        ```
        -> 10000 thao tác / 1.10837 giây ≈ 9026 IOPS
### 5. Process, thread ( có mấy cách để run 1 process và sự chuyển đổi giữa chúng, process có những loại nào, sự khác biệt giữa chúng )

- **Khái niệm**
    - **Process (tiến trình)**: Là chương trình đang được thực thi, bao gồm mã lệnh, bộ nhớ, tài nguyên, trạng thái.
    - **Thread (luồng)**: Đơn vị nhỏ nhất trong việc lập lịch thực thi của hệ điều hành, chia sẻ chung tài nguyên của process (vùng nhớ, file, tài nguyên khác).

- **Các cách để run (khởi động) một process**
    - Thông thường, có 4 cách chính để run một process:

        | Phương thức     | Mô tả                              | Vídụ                                                                |
        | --------------- | ------------------------------------------- | ----------------------------------------------------------- |
        | **Interactive** | Người dùng trực tiếp khởi động từ UI, terminal      | Chạy ứng dụng, gõ lệnh trong terminal (`./app`)                      |
        | **Batch**       | Khởi động theo lịch trình hoặc script tự động       | Cron job, Task Scheduler                                             |
        | **System Call** | Process gọi system call để tạo ra tiến trình mới    | `fork()`, `exec()` trong Unix/Linux, `CreateProcess()` trong Windows |
        | **System Boot** | Hệ điều hành tự khởi động process khi khởi động máy | Các daemon/service (`systemd`, `init`)                               |

- **Sự chuyển đổi trạng thái của một process**
    - Một process thông thường chuyển đổi qua lại giữa các trạng thái như sau:
        ```bash
        New (mới tạo)
        ↓
        Ready (sẵn sàng để chạy)
        ↕   ↕
        Running (đang chạy) ↔ Waiting/Blocked (đợi I/O hoặc sự kiện)
        ↓
        Terminated (đã kết thúc)
        ```
    - **New → Ready**: Process vừa tạo và chuẩn bị sẵn sàng để chạy.
    - **Ready → Running**: Process được cấp CPU và chạy.
    - **Running → Waiting**: Process đang chạy nhưng phải chờ I/O hoặc sự kiện.
    - **Waiting → Ready**: Hoàn thành I/O hoặc sự kiện được xử lý, quay trở lại trạng thái sẵn sàng.
    - **Running → Ready**: Scheduler hết lượt, trả về trạng thái sẵn sàng.
    - **Running → Terminated**: Kết thúc, process bị hủy hoặc hoàn thành.

- **Các loại process và sự khác biệt giữa chúng**
    | Loại process                                 | Mô tả                                                                 | Điểm khác biệt chính                                                             |
    | -------------------------------------------- | --------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
    | **Independent Process** (tiến trình độc lập) | Không chia sẻ dữ liệu hay ảnh hưởng trực tiếp đến các tiến trình khác | - Không giao tiếp trực tiếp.<br>- Thực hiện độc lập                              |
    | **Cooperating Process** (tiến trình hợp tác) | Có thể giao tiếp, chia sẻ dữ liệu với nhau để cùng thực hiện nhiệm vụ | - Cần các cơ chế giao tiếp (IPC).<br>- Phức tạp, quản lý đồng bộ hóa, đồng thuận |

- **Process vs Thread (sự khác biệt chính)**
    | Tiêu chí   | **Process**                          | **Thread**                                          |
    | ---------- | ------------------------------------ | --------------------------------------------------- |
    | Bộ nhớ     | Bộ nhớ riêng biệt                    | Bộ nhớ chia sẻ chung                                |
    | Tài nguyên | Có không gian địa chỉ riêng          | Dùng chung không gian địa chỉ                       |
    | Overhead   | Cao (tạo và quản lý process tốn kém) | Thấp (tạo và quản lý thread nhẹ hơn)                |
    | Hiệu năng  | Chậm hơn                             | Nhanh hơn                                           |
    | Độ tin cậy | Cao hơn (1 process lỗi ít ảnh hưởng) | Thấp hơn (1 thread lỗi có thể gây crash cả process) |
    | Giao tiếp  | Dùng IPC phức tạp                    | Chia sẻ trực tiếp dễ dàng                           |

### 6. Monitoring tool: top, uptime, vmstat, ps, pstree, free, iostat,sar, netstat,tcpdump, nmon,...
- Tổng hợp các công cụ monitoring phổ biến trên Linux:
    - **top**
        - Hiển thị các tiến trình đang chạy, dùng CPU, RAM theo thời gian thực.
        - Dùng để xem tình trạng tổng quan hệ thống.
    - **uptime**
        - Hiển thị thời gian hệ thống đã chạy liên tục (uptime).
        - Hiển thị load average (tải trung bình 1, 5, 15 phút).
    - **vmstat** (Virtual Memory Statistics)
        - Thống kê bộ nhớ, swap, CPU, I/O, hệ thống.
        - Thường dùng để theo dõi hiệu năng và tài nguyên hệ thống.
    - **ps** (process status)
        - Liệt kê các tiến trình đang chạy.
        - Có nhiều tuỳ chọn lọc tiến trình theo người dùng, trạng thái, PID
    - **pstree**
        - Hiển thị các tiến trình theo cây (quan hệ cha-con).
        - Dễ hình dung luồng tạo tiến trình.
    - **free**
        - Hiển thị bộ nhớ RAM, swap còn trống và đã dùng.
        - Thường dùng để kiểm tra bộ nhớ nhanh.
    - **iostat** (I/O statistics)
        - Thống kê hoạt động đọc/ghi trên thiết bị lưu trữ.
        - Dùng để theo dõi tốc độ I/O, độ trễ ổ cứng, phân vùng.
    - **sar** (System Activity Reporter)
        - Thu thập, báo cáo thông tin sử dụng CPU, bộ nhớ, I/O, mạng, tiến trình...
        - Có thể lưu lại số liệu dài hạn.
        - Phần của gói sysstat.
    - **netstat**
        - Hiển thị kết nối mạng, bảng định tuyến, giao diện mạng, thống kê mạng.
        - Đã bị thay thế phần nào bởi `ss`.
    - **tcpdump**
        - Bắt gói tin mạng (packet capture) để phân tích mạng.
        - Dùng để debug sự cố mạng, xem lưu lượng mạng.
    - **nmon** (Nigel's Monitor)
        - Giám sát hệ thống toàn diện: CPU, RAM, I/O, mạng, tiến trình, ảo hoá...
        - Giao diện ncurses rất trực quan.
    - **htop**
        - Phiên bản nâng cao của top, có giao diện màu, dễ dùng, filter, sắp xếp.
    - **glances**
        - Giám sát toàn diện tài nguyên theo thời gian thực.
        - Có thể chạy qua web hoặc terminal.
    - **iftop**
        - Giám sát băng thông mạng theo từng kết nối.
    - **iptraf**
        - Giám sát lưu lượng mạng theo giao diện.
    - **dstat**
        - Công cụ thay thế vmstat, iostat, netstat với báo cáo tổng hợp.
    -  **lsof** (List Open Files)
        - Hiển thị các file đang được mở, bao gồm file mạng, thiết bị.
    - **pidstat**
        - Giám sát chi tiết tiến trình, thống kê CPU, bộ nhớ theo tiến trình.
    -  **sar** (đã đề cập) có thể kết hợp với:
        - **mpstat**: thống kê CPU nhiều lõi.
        - **pidstat**: theo dõi tiến trình.
    - **watch**
        - Lặp lại chạy một lệnh và hiển thị kết quả theo thời gian thực (vd: watch -n 1 free -m).
    - **systemd-cgtop**
        - Hiển thị resource usage của các nhóm control groups trên systemd.
    - **bpftrace / eBPF tools**
        - Công cụ nâng cao giám sát hệ thống kernel-level hiện đại (đòi hỏi kiến thức nâng cao).
- Công cụ phân tích log và giám sát hệ thống tập trung (gián tiếp)
    - **journalctl**: đọc log của systemd.
    - **logrotate**: quản lý log.
    - **collectd** / **prometheus node exporter**: thu thập số liệu.
    - **Grafana** / **Prometheus**: dashboard và giám sát tập trung.
- Bảng công cụ theo nhóm:
    | Nhóm           | Công cụ chính                       | Mục đích                           |
    | -------------- | ----------------------------------- | ---------------------------------- |
    | **Tiến trình** | top, htop, ps, pstree, pidstat      | Quản lý và theo dõi tiến trình     |
    | **Bộ nhớ**     | free, vmstat, sar                   | Theo dõi RAM, swap, bộ nhớ ảo      |
    | **I/O**        | iostat, dstat                       | Theo dõi thiết bị lưu trữ          |
    | **Mạng**       | netstat, ss, tcpdump, iftop, iptraf | Theo dõi kết nối và lưu lượng mạng |
    | **Tổng hợp**   | nmon, glances, dstat, sar           | Giám sát toàn diện                 |
    | **Log**        | journalctl                          | Xem và phân tích log hệ thống      |
