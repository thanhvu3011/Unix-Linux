#Kernel là gì?
- Là nhân hệ điều hành là thành phần trung tâm của hầu hết các hệ điều hành. Kernel có nhiệm vụ quản lý các tài nguyên hệ thống (liên lạc giữa các thành phần phần cứng và phần mềm).
- Thông thường, với vai trò một thành phần cơ bản của một hệ điều hành, nhân có thể cung cấp các tầng trừu tượng mức thấp nhất cho các tài nguyên máy tính đặc biệt là bộ nhớ, CPU, và các thiết bị vào ra mà phần mềm ứng dụng cần điều khiển để thực hiện các chức năng của mình.

#Phân loại Kernel
- Các nhân khác nhau thực hiện các tác vụ của hệ điều hành theo các cách khác nhau, tùy theo thiết kế và cài đặt. Các nhân kiểu nguyên khối (Monolithic kernel) thực hiện các nhiệm vụ của mình bằng cách thực thi toàn bộ mã hệ điều hành trong cùng một địa chỉ bộ nhớ để tăng hiệu năng hệ thống. Trong khi đó các nhân loại nhỏ (Microkernel) chạy hầu hết các dịch vụ tại không gian người dùng (user space) với mục đích tăng khả năng bảo trì và tính mô đun của hệ điều hành. Có nhiều thiết kế nằm ở giữa hai thái cực này ví dụ như (Hybrid kernel) là nhân tự động phân luồng
- Nhìn chung, với hầu hết các kernel hiện nay, chúng ta có thể chia ra làm 3 loại: monolithic, microkernel, và hybrid. Linux sử dụng kernel monolithic trong khi OS X (XNU) và Windows 7 sử dụng kernel hybrid
    - Microkernel: Microkernel có đầy đủ các tính năng cần thiết để quản lý bộ vi xử lý, bộ nhớ và IPC. Có rất nhiều thứ khác trong máy tính có thể được nhìn thấy, tiếp xúc và quản lý trong chế độ người dùng
        - Microkernel với những thông số liên quan footprint rất nhỏ, tương tự với bộ nhớ và dung lượng lưu trữ, chúng còn có tính bảo mật khá cao vì chỉ định rõ ràng những tiến trình nào hoạt động trong chế độ user mode, mà không được cấp quyền như trong chế độ giám sát – supervisor mode
    - Monolithic Kernel: Monolithic chúng có chức năng bao quát rộng hơn so với microkernel, không chỉ tham gia quản lý bộ vi xử lý, bộ nhớ, IRC, chúng còn can thiệp vào trình điều khiển driver, tính năng điều phối file hệ thống, các giao tiếp qua lại giữa server
        - Monolithic tốt hơn khi truy cập tới phần cứng và đa tác vụ, bởi vì nếu 1 chương trình muốn thu thập thông tin từ bộ nhớ và các tiến trình khác, chúng cần có quyền truy cập trực tiếp và không phải chờ đợi các tác vụ khác kết thúc.
        -Nhưng đồng thời, chúng cũng là nguyên nhân gây ra sự bất ổn vì nhiều chương trình chạy trong chế độ supervisor mode hơn, chỉ cần 1 sự cố nhỏ cũng khiến cho cả hệ thống mất ổn định.
    Hybrid Kernel: Hybrid có khả năng chọn lựa và quyết định những ứng dụng nào được phép chạy trong chế độ user hoặc supervisor

#Linux Kernel
Vào năm 1991, dựa trên UNIX kernel, Linus Torvalds đã tạo ra Linux kernel chạy trên máy tính của ông ấy. Dựa vào chức năng của hệ điều hành, Linux kernel được chia làm 6 thành phần:

- *Process management*: có nhiệm vụ quản lý các tiến trình, bao gồm các công việc:
    - Tạo/hủy các tiến trình.
    - Lập lịch cho các tiến trình. Đây thực chất là lên kế hoạch: CPU sẽ thực thi chương trình khi nào, thực thi trong bao lâu, tiếp theo là chương trình nào
    -Hỗ trợ các tiến trình giao tiếp với nhau
    - Đồng bộ hoạt động của các tiến trình để tránh xảy ra tranh chấp tài nguyên.
- *Memory management*: có nhiệm vụ quản lý bộ nhớ, bao gồm các công việc:
    - Cấp phát bộ nhớ trước khi đưa chương trình vào, thu hồi bộ nhớ khi tiến trình kết thúc
    - Đảm bảo chương trình nào cũng có cơ hội được đưa vào bộ nhớ
    - Bảo vệ vùng nhớ của mỗi tiến trình
- *Device management*: có nhiệm vụ quản lý thiết bị, bao gồm các công việc:
    - Điều khiển hoạt động của các thiết bị
    - Giám sát trạng thái của các thiết bị
    - Trao đổi dữ liệu với các thiết bị
    - Lập lịch sử dụng các thiết bị, đặc biệt là thiết bị lưu trữ (ví dụ ổ cứng)
- *File system management*: có nhiệm vụ quản lý dữ liệu trên thiết bị lưu trữ (như ổ cứng, thẻ nhớ). Quản lý dữ liệu gồm các công việc: thêm, tìm kiếm, sửa, xóa dữ liệu
- *Networking management*: có nhiệm vụ quản lý các gói tin (packet) theo mô hình TCP/IP
- *System call Interface*: có nhiệm vụ cung cấp các dịch vụ sử dụng phần cứng cho các tiến trình. Mỗi dịch vụ được gọi là một system call

##Kiến trúc Ring trong CPU
Các Ring được sắp xếp có thứ bậc, từ mức có nhiều đặc quyền nhất (dành cho trusted-software, thường được đánh số 0) đến mức có ít đặc quyền nhất (dành cho untrusted-software, được đánh số cao nhất)

Dưới đây là hình minh họa các Ring trong kiến trúc CPU x86

![alt](https://manthang.files.wordpress.com/2010/10/protection-rings.jpg)

Các chương trình hoạt động tại Ring 0 có đặc quyền cao nhất, có thể tương tác trực tiếp với phần cứng như CPU, Memory…

Để cho phép các ứng dụng nằm ở Ring có trọng số cao truy cập các tài nguyên được quản lý bởi các chương trình nằm ở Ring có trọng số thấp hơn, người ta xây dựng các cổng (gate) đặc biệt. Ví dụ như system call (lời gọi hàm hệ thống) giữa các Ring.

Việc quy định chặt chẽ chương trình nào nằm tại Ring nào cộng với việc xây dựng các cổng phù hợp giữa các Ring sẽ đảm bảo tính ổn định của hệ thống, đồng thời ngăn chặn các chương trình nằm trong Ring cao sử dụng trái phép (do vô tình hoặc cố ý) các tài nguyên dành cho các chương trình khác nằm tại Ring thấp hơn

Ví dụ, một spyware đang chạy với tư cách là ứng dụng cho người dùng thông thường (thuộc untrusted software) nằm tại Ring 3 có ý định bật webcam mà không được sự đồng ý của người dùng. Hành vi này sẽ bị hệ thống ngăn chặn vì muốn truy cập tới phần cứng là thiết bị webcam nó phải sử dụng một hàm trong phần mềm điều khiển thiết bị (device driver) của webcam (thuộc trusted software) nằm tại Ring 1.

Hầu hết các hệ điều hành chỉ sử dụng 2 Ring ngay cả khi phần cứng mà hệ điều hành chạy trên đó hỗ trợ nhiều hơn 2 Ring. Ví dụ, Windows chỉ sử dụng 2 mức là Ring 0 (tương ứng với Kernel Mode) và Ring 3 (tương ứng với User Mode).

