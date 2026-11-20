# TỔNG HỢP CÁC LỆNH THƯỜNG DÙNG TRÊN HỆ ĐIỀU HÀNH LINUX

> Tài liệu dành cho người mới học Linux, thực hành trên Ubuntu, Debian,
> Kali Linux và các bản phân phối phổ biến khác. Một số lệnh hoặc tùy
> chọn có thể khác nhau tùy bản phân phối và phiên bản.

## Mục lục

1.  [Cấu trúc chung của một lệnh
    Linux](#1-cấu-trúc-chung-của-một-lệnh-linux)
2.  [Trợ giúp và tìm hiểu lệnh](#2-trợ-giúp-và-tìm-hiểu-lệnh)
3.  [Di chuyển và xem thư mục](#3-di-chuyển-và-xem-thư-mục)
4.  [Tạo, sao chép, di chuyển và xóa](#4-tạo-sao-chép-di-chuyển-và-xóa)
5.  [Đọc và xử lý nội dung tệp](#5-đọc-và-xử-lý-nội-dung-tệp)
6.  [Tìm kiếm tệp và nội dung](#6-tìm-kiếm-tệp-và-nội-dung)
7.  [Quyền truy cập và chủ sở hữu](#7-quyền-truy-cập-và-chủ-sở-hữu)
8.  [Người dùng và nhóm](#8-người-dùng-và-nhóm)
9.  [Tiến trình và tác vụ](#9-tiến-trình-và-tác-vụ)
10. [Thông tin hệ thống và phần
    cứng](#10-thông-tin-hệ-thống-và-phần-cứng)
11. [Ổ đĩa, phân vùng và dung lượng](#11-ổ-đĩa-phân-vùng-và-dung-lượng)
12. [Mạng máy tính](#12-mạng-máy-tính)
13. [Nén và giải nén](#13-nén-và-giải-nén)
14. [Quản lý gói phần mềm](#14-quản-lý-gói-phần-mềm)
15. [Lịch sử lệnh và bí danh](#15-lịch-sử-lệnh-và-bí-danh)
16. [Chuyển hướng và đường ống](#16-chuyển-hướng-và-đường-ống)
17. [Lệnh văn bản và xử lý dữ liệu](#17-lệnh-văn-bản-và-xử-lý-dữ-liệu)
18. [SSH, SCP và truyền tệp](#18-ssh-scp-và-truyền-tệp)
19. [Biến môi trường và thông tin
    shell](#19-biến-môi-trường-và-thông-tin-shell)
20. [Một số lệnh quản trị và bảo
    trì](#20-một-số-lệnh-quản-trị-và-bảo-trì)
21. [Ký tự đại diện và quy tắc viết đường
    dẫn](#21-ký-tự-đại-diện-và-quy-tắc-viết-đường-dẫn)
22. [Các ví dụ kết hợp thực tế](#22-các-ví-dụ-kết-hợp-thực-tế)
23. [Lưu ý an toàn khi sử dụng
    Linux](#23-lưu-ý-an-toàn-khi-sử-dụng-linux)
24. [Bảng tra cứu nhanh](#24-bảng-tra-cứu-nhanh)

------------------------------------------------------------------------

## 1. Cấu trúc chung của một lệnh Linux

Một lệnh Linux thường có cấu trúc:

``` bash
command [options] [arguments]
```

Trong đó:

-   `command`: tên lệnh hoặc chương trình cần chạy.
-   `options`: tùy chọn để thay đổi cách lệnh hoạt động.
-   `arguments`: đối tượng hoặc dữ liệu mà lệnh xử lý, ví dụ tên tệp,
    thư mục, địa chỉ IP.
-   Dấu `[]` trong tài liệu biểu thị thành phần tùy chọn, không phải ký
    tự cần gõ.

Ví dụ:

``` bash
ls -l /home
```

Phân tích:

-   `ls`: lệnh liệt kê nội dung thư mục.
-   `-l`: option yêu cầu hiển thị dạng danh sách chi tiết.
-   `/home`: argument, là thư mục cần liệt kê.

### 1.1. Option ngắn và option dài

Option ngắn thường có một dấu gạch ngang:

``` bash
ls -l
```

Option dài thường có hai dấu gạch ngang:

``` bash
ls --all
```

Nhiều lệnh cho phép kết hợp option ngắn:

``` bash
ls -lah
```

Tương đương về ý nghĩa với:

``` bash
ls -l -a -h
```

Ý nghĩa:

-   `-l`: hiển thị chi tiết.
-   `-a`: hiển thị cả tệp ẩn.
-   `-h`: hiển thị dung lượng dễ đọc, ví dụ KB, MB, GB.

Không phải mọi lệnh đều cho phép gộp option. Cần kiểm tra tài liệu của
từng lệnh.

### 1.2. Đường dẫn tuyệt đối và tương đối

-   Đường dẫn tuyệt đối bắt đầu từ `/`, ví dụ `/home/user/Documents`.
-   Đường dẫn tương đối được tính từ thư mục hiện tại, ví dụ
    `Documents/report.txt`.
-   `.` đại diện cho thư mục hiện tại.
-   `..` đại diện cho thư mục cha.
-   `~` thường đại diện cho thư mục home của người dùng hiện tại.
-   `/` là thư mục gốc của hệ thống tệp.

### 1.3. Phân biệt chữ hoa và chữ thường

Linux thường phân biệt chữ hoa và chữ thường trong tên tệp và thư mục.
Ví dụ `Report.txt` và `report.txt` có thể là hai tệp khác nhau.

------------------------------------------------------------------------

## 2. Trợ giúp và tìm hiểu lệnh

### 2.1. `man` -- xem trang hướng dẫn

Cú pháp:

``` bash
man [section] command
```

Ví dụ:

``` bash
man ls
man chmod
man 5 passwd
```

Giải thích:

-   `man`: mở trang hướng dẫn (manual).
-   `ls`, `chmod`: tên lệnh cần tìm hiểu.
-   `5`: nhóm tài liệu số 5, thường dành cho định dạng tệp cấu hình.
-   `passwd`: trong ví dụ cuối là tên trang hướng dẫn cần mở.

Phím thường dùng trong giao diện `man`:

-   `Space`: xuống một trang.
-   `b`: lên một trang.
-   `/keyword`: tìm từ khóa.
-   `n`: đến kết quả tìm tiếp theo.
-   `q`: thoát.

### 2.2. `--help` -- xem trợ giúp nhanh

``` bash
ls --help
cp --help
grep --help
```

Hiển thị cú pháp, option và cách sử dụng cơ bản của lệnh.

### 2.3. `whatis` -- mô tả ngắn về lệnh

``` bash
whatis ls
whatis chmod
```

Trả về mô tả ngắn nếu cơ sở dữ liệu whatis đã được cập nhật.

### 2.4. `apropos` -- tìm lệnh theo chủ đề

``` bash
apropos network
apropos copy
```

Tìm trong phần mô tả của các trang manual những mục có chứa từ khóa.

### 2.5. `type` -- xác định loại của lệnh

``` bash
type ls
type cd
type python
```

Cho biết tên được shell hiểu là alias, function, builtin hay chương
trình bên ngoài.

### 2.6. `which` và `command -v`

``` bash
which python
command -v python
```

-   `which`: tìm đường dẫn chương trình thông qua `PATH`.
-   `command -v`: cách kiểm tra tổng quát hơn trong shell; có thể nhận
    diện cả alias hoặc shell builtin tùy shell.

------------------------------------------------------------------------

## 3. Di chuyển và xem thư mục

### 3.1. `pwd` -- in thư mục hiện tại

``` bash
pwd
```

Tên đầy đủ là print working directory. Lệnh hiển thị đường dẫn tuyệt đối
của thư mục làm việc hiện tại.

### 3.2. `ls` -- liệt kê nội dung thư mục

``` bash
ls
ls /home
ls -l
ls -a
ls -lh
ls -lah
ls -R
ls -lt
ls -lS
```

Các option phổ biến:

  Option   Ý nghĩa
  -------- ----------------------------------------------------------
  `-l`     Hiển thị dạng danh sách chi tiết
  `-a`     Hiển thị tất cả, bao gồm tên bắt đầu bằng dấu chấm
  `-A`     Hiển thị tệp ẩn nhưng bỏ `.` và `..`
  `-h`     Hiển thị dung lượng dễ đọc khi kết hợp với dạng chi tiết
  `-R`     Liệt kê đệ quy các thư mục con
  `-t`     Sắp xếp theo thời gian sửa đổi
  `-S`     Sắp xếp theo dung lượng
  `-r`     Đảo ngược thứ tự sắp xếp

Trong kết quả `ls -l`, một dòng thường có dạng:

``` text
-rw-r--r-- 1 alice developers 1200 Nov 20 10:30 notes.txt
```

Các thành phần:

1.  `-rw-r--r--`: loại đối tượng và quyền truy cập.
2.  `1`: số hard link.
3.  `alice`: chủ sở hữu.
4.  `developers`: nhóm sở hữu.
5.  `1200`: kích thước tính bằng byte.
6.  `Nov 20 10:30`: thời điểm sửa đổi gần nhất.
7.  `notes.txt`: tên tệp.

Ký tự đầu tiên có thể là `-` (tệp thông thường), `d` (thư mục), `l`
(liên kết tượng trưng), cùng một số loại đặc biệt khác.

### 3.3. `cd` -- thay đổi thư mục

``` bash
cd /home
cd Documents
cd ..
cd .
cd ~
cd -
```

Giải thích:

-   `cd /home`: chuyển đến `/home`.
-   `cd Documents`: chuyển đến thư mục con `Documents` tính từ vị trí
    hiện tại.
-   `cd ..`: chuyển lên thư mục cha.
-   `cd .`: vẫn ở thư mục hiện tại.
-   `cd ~`: về thư mục home.
-   `cd -`: quay lại thư mục làm việc trước đó.

`cd` thường là shell builtin vì nó cần thay đổi thư mục hiện tại của
chính shell đang chạy.

### 3.4. `tree` -- hiển thị cấu trúc cây

``` bash
tree
tree -L 2
tree -a
tree /home/user
```

-   `-L 2`: giới hạn độ sâu hiển thị ở hai cấp.
-   `-a`: hiển thị cả tệp ẩn.
-   Có thể cần cài đặt `tree` trước khi dùng.

------------------------------------------------------------------------

## 4. Tạo, sao chép, di chuyển và xóa

### 4.1. `touch` -- tạo tệp rỗng hoặc cập nhật thời gian

``` bash
touch note.txt
touch file1.txt file2.txt
touch -c existing.txt
```

-   Nếu tệp chưa tồn tại, `touch` tạo tệp rỗng.
-   Nếu tệp đã tồn tại, lệnh cập nhật thời gian truy cập/sửa đổi theo
    hành vi mặc định.
-   `-c`: không tạo tệp nếu tệp chưa tồn tại.

### 4.2. `mkdir` -- tạo thư mục

``` bash
mkdir Documents
mkdir folder1 folder2
mkdir -p project/src/components
mkdir -v backup
```

-   `mkdir`: tạo thư mục.
-   `-p`: tạo cả các thư mục cha còn thiếu; không báo lỗi chỉ vì thư mục
    đã tồn tại.
-   `-v`: hiển thị thông tin về thư mục được tạo.

### 4.3. `cp` -- sao chép tệp và thư mục

``` bash
cp source.txt destination.txt
cp source.txt /home/user/Documents/
cp -r project project_backup
cp -i report.txt backup/
cp -v file.txt backup/
cp -a website website_backup
```

Các thành phần:

-   Đối số đầu: nguồn cần sao chép.
-   Đối số cuối: đích sao chép.
-   `-r` hoặc `-R`: sao chép thư mục cùng nội dung bên trong theo cách
    đệ quy.
-   `-i`: hỏi trước khi ghi đè.
-   `-v`: hiển thị các thao tác.
-   `-a`: chế độ lưu trữ, cố gắng giữ thuộc tính và cấu trúc liên kết
    phù hợp; thường hữu ích khi sao lưu.

Lưu ý: nếu đích là thư mục đã tồn tại, tệp nguồn thường được đặt bên
trong thư mục đó. Hãy kiểm tra đường dẫn đích trước khi sao chép số
lượng lớn tệp.

### 4.4. `mv` -- di chuyển hoặc đổi tên

``` bash
mv old.txt new.txt
mv report.txt Documents/
mv folder1 /tmp/
mv -i file.txt backup/
mv -v old_name new_name
```

-   Hai đối số là tên cũ và tên mới: đổi tên.
-   Đối số cuối là thư mục: di chuyển vào thư mục đó.
-   `-i`: hỏi trước khi ghi đè.
-   `-v`: hiển thị thao tác.

### 4.5. `rm` -- xóa tệp và thư mục

``` bash
rm file.txt
rm -i file.txt
rm -v file.txt
rm -r old_folder
rm -f temporary.txt
```

-   `rm`: xóa tệp.
-   `-i`: hỏi xác nhận trước khi xóa.
-   `-v`: hiển thị đối tượng bị xóa.
-   `-r` hoặc `-R`: xóa thư mục và nội dung bên trong theo cách đệ quy.
-   `-f`: không hỏi xác nhận và bỏ qua một số lỗi do tệp không tồn tại.

**Cảnh báo:** `rm` thường không chuyển tệp vào thùng rác. Dữ liệu có thể
khó hoặc không thể khôi phục. Tránh chạy lệnh xóa với quyền quản trị nếu
chưa kiểm tra kỹ đường dẫn. Không sao chép máy móc các lệnh `rm -rf` từ
Internet.

### 4.6. `rmdir` -- xóa thư mục rỗng

``` bash
rmdir empty_folder
rmdir -p parent/child
```

-   Chỉ xóa thư mục rỗng.
-   `-p`: xóa lần lượt các thư mục cha nếu chúng cũng rỗng.

### 4.7. `ln` -- tạo liên kết

``` bash
ln original.txt hardlink.txt
ln -s /var/log/syslog syslog_link
```

-   `ln`: tạo hard link mặc định.
-   `ln -s`: tạo symbolic link (liên kết mềm).
-   Hard link tham chiếu cùng dữ liệu inode với tên khác; thường không
    dùng để liên kết thư mục và không vượt qua ranh giới filesystem.
-   Symbolic link lưu đường dẫn trỏ đến đối tượng khác; có thể trỏ đến
    thư mục hoặc đường dẫn trên filesystem khác. Nếu đích bị xóa/di
    chuyển, liên kết có thể trở thành liên kết hỏng.

------------------------------------------------------------------------

## 5. Đọc và xử lý nội dung tệp

### 5.1. `cat` -- hiển thị hoặc nối nội dung tệp

``` bash
cat file.txt
cat file1.txt file2.txt
cat file1.txt file2.txt > combined.txt
```

-   Đối số là các tệp đầu vào.
-   Khi có nhiều tệp, `cat` in nội dung lần lượt.
-   `>` chuyển đầu ra sang tệp khác và có thể ghi đè nội dung tệp đích.

### 5.2. `less` -- xem tệp theo từng trang

``` bash
less /var/log/syslog
less large_file.txt
```

Phù hợp với tệp lớn vì không cần in toàn bộ nội dung ra terminal cùng
lúc.

Phím thường dùng:

-   `Space` hoặc `Page Down`: xuống trang.
-   `b` hoặc `Page Up`: lên trang.
-   `/keyword`: tìm kiếm.
-   `n`: kết quả tiếp theo.
-   `N`: kết quả trước.
-   `g`: đầu tệp.
-   `G`: cuối tệp.
-   `q`: thoát.

### 5.3. `more` -- xem nội dung theo trang

``` bash
more file.txt
```

Công dụng tương tự `less` ở mức cơ bản, nhưng khả năng điều hướng thường
hạn chế hơn.

### 5.4. `head` -- xem phần đầu tệp

``` bash
head file.txt
head -n 5 file.txt
head -c 100 file.txt
```

-   Mặc định hiển thị 10 dòng đầu.
-   `-n 5`: hiển thị 5 dòng đầu.
-   `-c 100`: hiển thị 100 byte đầu.

### 5.5. `tail` -- xem phần cuối tệp

``` bash
tail file.txt
tail -n 20 file.txt
tail -f /var/log/syslog
tail -F /var/log/myapp.log
```

-   Mặc định hiển thị 10 dòng cuối.
-   `-n 20`: hiển thị 20 dòng cuối.
-   `-f`: tiếp tục theo dõi dữ liệu mới được ghi vào tệp.
-   `-F`: hữu ích khi tệp log có thể được xoay vòng hoặc tạo lại; hành
    vi cụ thể tùy phiên bản.

### 5.6. `wc` -- đếm dòng, từ và byte

``` bash
wc file.txt
wc -l file.txt
wc -w file.txt
wc -c file.txt
wc -m file.txt
```

-   `-l`: đếm số dòng.
-   `-w`: đếm số từ.
-   `-c`: đếm số byte.
-   `-m`: đếm số ký tự theo locale.
-   Không có option: in số dòng, số từ, số byte.

### 5.7. `file` -- nhận diện loại tệp

``` bash
file image.png
file document.pdf
file script.sh
```

Dựa vào nội dung và dấu hiệu định dạng để mô tả loại tệp, không chỉ dựa
vào phần mở rộng.

### 5.8. `stat` -- xem thông tin chi tiết của tệp

``` bash
stat report.txt
stat /var/log
```

Có thể hiển thị kích thước, inode, quyền, chủ sở hữu, thời gian truy
cập, sửa đổi và thay đổi metadata.

### 5.9. `nl` -- đánh số dòng

``` bash
nl script.sh
nl -ba script.sh
```

`-ba` yêu cầu đánh số tất cả các dòng, kể cả dòng trống.

------------------------------------------------------------------------

## 6. Tìm kiếm tệp và nội dung

### 6.1. `find` -- tìm tệp và thư mục

Cú pháp tổng quát:

``` bash
find [starting_path] [conditions] [actions]
```

Ví dụ:

``` bash
find . -name "report.txt"
find /home -type f
find . -type d
find . -name "*.cpp"
find . -iname "*.jpg"
find . -type f -size +10M
find . -type f -mtime -7
find . -name "*.log" -delete
```

Giải thích:

-   `.`: bắt đầu tìm từ thư mục hiện tại.
-   `/home`: bắt đầu tìm từ `/home`.
-   `-name`: so khớp tên có phân biệt chữ hoa/chữ thường.
-   `-iname`: so khớp tên không phân biệt chữ hoa/chữ thường.
-   `-type f`: chỉ tìm tệp thông thường.
-   `-type d`: chỉ tìm thư mục.
-   `-size +10M`: tệp lớn hơn 10 MiB theo cách diễn giải đơn vị của
    `find`.
-   `-mtime -7`: thời điểm sửa đổi nằm trong khoảng chưa đủ 7 ngày theo
    cách tính của `find`.
-   `-delete`: xóa kết quả phù hợp.

**Cẩn trọng với `-delete`:** hãy chạy câu lệnh tìm kiếm trước để kiểm
tra danh sách kết quả, sau đó mới thêm hành động xóa.

### 6.2. `locate` -- tìm nhanh theo cơ sở dữ liệu tên tệp

``` bash
locate report.txt
locate "*.conf"
```

`locate` thường tìm nhanh vì tra cứu cơ sở dữ liệu đã lập chỉ mục, nhưng
kết quả có thể chưa cập nhật. Một số hệ thống dùng `updatedb` để cập
nhật chỉ mục; quyền chạy và lịch cập nhật tùy cấu hình.

### 6.3. `grep` -- tìm chuỗi trong nội dung

``` bash
grep "error" app.log
grep -i "error" app.log
grep -n "main" program.cpp
grep -r "TODO" .
grep -v "debug" app.log
grep -w "root" /etc/passwd
grep -E "error|warning" app.log
grep -F "a.b" file.txt
```

Các option:

-   `-i`: không phân biệt chữ hoa/chữ thường.
-   `-n`: hiển thị số dòng.
-   `-r`: tìm đệ quy trong thư mục.
-   `-v`: lấy các dòng không khớp.
-   `-w`: khớp toàn bộ từ.
-   `-E`: sử dụng extended regular expression.
-   `-F`: tìm chuỗi cố định, không diễn giải biểu thức chính quy.

### 6.4. `which` và `whereis` -- tìm chương trình

``` bash
which gcc
whereis bash
```

-   `which`: tìm chương trình trong các thư mục thuộc `PATH`.
-   `whereis`: tìm vị trí binary, source hoặc manual nếu có trong cơ sở
    dữ liệu/vị trí mà công cụ kiểm tra.

------------------------------------------------------------------------

## 7. Quyền truy cập và chủ sở hữu

Linux quản lý quyền cơ bản theo ba nhóm:

-   `u` (user): chủ sở hữu.
-   `g` (group): nhóm sở hữu.
-   `o` (others): những người dùng khác.
-   `a` (all): cả ba nhóm.

Ba quyền cơ bản:

-   `r` (read): đọc.
-   `w` (write): ghi hoặc chỉnh sửa.
-   `x` (execute): thực thi; với thư mục, `x` cho phép đi qua/truy cập
    các mục bên trong khi có đủ quyền liên quan.

### 7.1. `chmod` -- thay đổi quyền

Cách 1: dùng ký hiệu:

``` bash
chmod u+x script.sh
chmod g-w report.txt
chmod o+r public.txt
chmod a+r file.txt
chmod u=rw,g=r,o= file.txt
```

Giải thích:

-   `u+x`: thêm quyền thực thi cho chủ sở hữu.
-   `g-w`: bỏ quyền ghi của nhóm.
-   `o+r`: thêm quyền đọc cho người khác.
-   `a+r`: thêm quyền đọc cho tất cả.
-   `u=rw,g=r,o=`: đặt chính xác quyền của từng nhóm.

Cách 2: dùng số bát phân:

``` bash
chmod 755 script.sh
chmod 644 report.txt
chmod 700 private.sh
```

Giá trị quyền:

  Quyền              Giá trị
  ---------------- ---------
  `r`                      4
  `w`                      2
  `x`                      1
  Không có quyền           0

Cộng các giá trị để tạo quyền cho từng nhóm:

-   `7 = 4 + 2 + 1 = rwx`.
-   `5 = 4 + 1 = r-x`.
-   `4 = r--`.
-   `0 = ---`.

Vì vậy:

-   `755` tương ứng `rwxr-xr-x`.
-   `644` tương ứng `rw-r--r--`.
-   `700` tương ứng `rwx------`.

Ba chữ số lần lượt là quyền của chủ sở hữu, nhóm và người khác.

### 7.2. `chown` -- thay đổi chủ sở hữu

``` bash
sudo chown alice file.txt
sudo chown alice:developers file.txt
sudo chown -R alice:developers project/
```

-   `alice`: người dùng mới sở hữu.
-   `developers`: nhóm mới sở hữu.
-   `-R`: áp dụng đệ quy cho nội dung bên trong thư mục.
-   `sudo`: chạy lệnh với quyền quản trị nếu tài khoản được phép.

### 7.3. `chgrp` -- thay đổi nhóm sở hữu

``` bash
sudo chgrp developers report.txt
sudo chgrp -R developers project/
```

### 7.4. `umask` -- đặt mặt nạ quyền mặc định

``` bash
umask
umask 022
umask 027
```

`umask` quy định những quyền sẽ bị loại khỏi quyền mặc định khi tạo tệp
hoặc thư mục. Nó không đơn giản là một bộ quyền được gán trực tiếp.

Ví dụ thường gặp:

-   `022`: thường tạo thư mục với quyền `755` và tệp với quyền `644`.
-   `027`: thường tạo thư mục với quyền `750` và tệp với quyền `640`.

Quyền mặc định còn phụ thuộc chương trình tạo đối tượng và quyền cơ sở
mà chương trình yêu cầu.

------------------------------------------------------------------------

## 8. Người dùng và nhóm

### 8.1. `whoami` -- tên người dùng hiện tại

``` bash
whoami
```

### 8.2. `id` -- UID, GID và nhóm

``` bash
id
id alice
```

Hiển thị user ID, group ID chính và các nhóm người dùng thuộc về.

### 8.3. `who` -- người dùng đang đăng nhập

``` bash
who
who -a
```

Hiển thị các phiên đăng nhập hiện tại theo thông tin hệ thống cung cấp.

### 8.4. `w` -- người dùng và hoạt động

``` bash
w
```

Cho biết người dùng đăng nhập, thời gian hoạt động và một số thông tin
về tiến trình/hoạt động hiện tại.

### 8.5. `groups` -- liệt kê nhóm

``` bash
groups
groups alice
```

### 8.6. `sudo` -- chạy lệnh với quyền khác

``` bash
sudo apt update
sudo systemctl restart ssh
sudo -u alice whoami
```

-   `sudo`: yêu cầu chạy lệnh với quyền được cấu hình trong chính sách
    sudo.
-   Mặc định thường chạy với quyền root.
-   `-u alice`: chạy lệnh dưới danh tính người dùng `alice`.

Chỉ dùng `sudo` khi cần thiết. Không nên thêm `sudo` một cách tùy tiện
để che đi lỗi quyền truy cập.

### 8.7. `su` -- chuyển người dùng

``` bash
su alice
su -
```

-   `su alice`: chuyển sang người dùng `alice`.
-   `su -`: mở shell đăng nhập của người dùng đích, mặc định thường là
    root nếu không chỉ định.
-   `exit`: rời shell được mở bởi `su`.

### 8.8. Một số lệnh quản trị tài khoản

``` bash
sudo adduser alice
sudo useradd -m alice
sudo passwd alice
sudo usermod -aG sudo alice
sudo deluser alice
sudo userdel -r alice
```

-   `adduser`: công cụ tương tác cấp cao trên một số bản phân phối
    Debian/Ubuntu.
-   `useradd`: công cụ tạo tài khoản mức thấp hơn, thường cần chỉ rõ
    option.
-   `-m`: tạo thư mục home.
-   `passwd`: đặt hoặc thay đổi mật khẩu.
-   `usermod -aG sudo alice`: thêm `alice` vào nhóm `sudo`; `-a` rất
    quan trọng vì giữ lại các nhóm phụ hiện có.
-   `deluser` / `userdel`: xóa tài khoản; `-r` có thể xóa cả thư mục
    home và mail spool tùy hệ thống.

Các lệnh này có thể ảnh hưởng trực tiếp đến khả năng đăng nhập. Hãy kiểm
tra kỹ trước khi xóa hoặc thay đổi tài khoản quản trị.

------------------------------------------------------------------------

## 9. Tiến trình và tác vụ

### 9.1. `ps` -- xem tiến trình

``` bash
ps
ps -ef
ps aux
ps -u alice
```

Một số dạng phổ biến:

-   `ps -ef`: hiển thị danh sách tiến trình theo định dạng đầy đủ kiểu
    System V.
-   `ps aux`: hiển thị nhiều tiến trình cùng thông tin người dùng, CPU,
    RAM... theo cú pháp kiểu BSD.
-   `ps -u alice`: lọc theo người dùng.

Các cột thường gặp:

-   `PID`: mã định danh tiến trình.
-   `PPID`: PID của tiến trình cha.
-   `%CPU`: tỷ lệ CPU.
-   `%MEM`: tỷ lệ bộ nhớ.
-   `STAT`: trạng thái tiến trình.
-   `CMD` hoặc `COMMAND`: lệnh khởi chạy tiến trình.

### 9.2. `top` -- theo dõi tiến trình trực tiếp

``` bash
top
```

Hiển thị tiến trình và mức sử dụng tài nguyên được cập nhật liên tục.

Phím thường dùng:

-   `q`: thoát.
-   `P`: sắp xếp theo CPU.
-   `M`: sắp xếp theo bộ nhớ.
-   `k`: gửi tín hiệu đến một PID.
-   `1`: hiển thị thông tin CPU theo từng lõi trên nhiều phiên bản.

### 9.3. `htop` -- trình theo dõi tương tác

``` bash
htop
```

Giao diện trực quan hơn `top`, hỗ trợ cuộn, lọc và thao tác tiến trình.
Có thể cần cài đặt riêng.

### 9.4. `kill` -- gửi tín hiệu đến tiến trình

``` bash
kill 1234
kill -TERM 1234
kill -KILL 1234
kill -l
```

-   `1234`: PID của tiến trình.
-   `-TERM` hoặc `-15`: yêu cầu tiến trình kết thúc một cách có kiểm
    soát.
-   `-KILL` hoặc `-9`: buộc kernel kết thúc tiến trình; tiến trình không
    thể xử lý hoặc bỏ qua tín hiệu này.
-   `-l`: liệt kê tên tín hiệu.

Nên thử `TERM` trước. Chỉ dùng `KILL` khi tiến trình không phản hồi và
đã xác định đúng PID.

### 9.5. `pkill` và `killall`

``` bash
pkill -f "python app.py"
killall firefox
```

-   `pkill`: gửi tín hiệu đến tiến trình khớp tiêu chí, thường theo tên
    hoặc mẫu.
-   `-f`: khớp với toàn bộ command line.
-   `killall`: gửi tín hiệu đến tiến trình theo tên.

Cẩn thận vì một mẫu quá rộng có thể tác động đến nhiều tiến trình.

### 9.6. `jobs`, `bg`, `fg`

``` bash
jobs
bg %1
fg %1
```

-   `jobs`: liệt kê các job được shell hiện tại quản lý.
-   `bg %1`: tiếp tục job số 1 ở chế độ nền.
-   `fg %1`: đưa job số 1 lên tiền cảnh.

Có thể nhấn `Ctrl + Z` để tạm dừng job tiền cảnh, sau đó dùng `bg` hoặc
`fg`.

### 9.7. Chạy nền với `&` và `nohup`

``` bash
long_task &
nohup python3 app.py > app.log 2>&1 &
```

-   `&`: yêu cầu shell chạy lệnh ở nền.
-   `nohup`: giúp lệnh tiếp tục chạy khi phiên terminal đóng theo cơ chế
    SIGHUP thông thường.
-   `> app.log`: chuyển stdout vào tệp log.
-   `2>&1`: chuyển stderr đến cùng nơi với stdout.
-   `&` cuối: chạy job ở nền.

`nohup` không thay thế trình quản lý dịch vụ. Với ứng dụng chạy lâu dài,
nên cân nhắc `systemd`, container hoặc công cụ giám sát phù hợp.

------------------------------------------------------------------------

## 10. Thông tin hệ thống và phần cứng

### 10.1. `uname` -- thông tin kernel

``` bash
uname
uname -a
uname -r
uname -m
```

-   `-a`: hiển thị nhiều thông tin hệ thống.
-   `-r`: phiên bản kernel.
-   `-m`: kiến trúc máy, ví dụ `x86_64` hoặc `aarch64`.

### 10.2. `hostname` -- tên máy

``` bash
hostname
hostname -I
```

-   Không option: hiển thị hostname.
-   `-I`: hiển thị các địa chỉ IP được gán cho máy trên một số hệ thống.

### 10.3. `date` -- ngày giờ

``` bash
date
date "+%Y-%m-%d %H:%M:%S"
date -u
```

Một số ký hiệu định dạng:

-   `%Y`: năm bốn chữ số.
-   `%m`: tháng.
-   `%d`: ngày.
-   `%H`: giờ 24 tiếng.
-   `%M`: phút.
-   `%S`: giây.
-   `-u`: hiển thị theo UTC.

### 10.4. `cal` -- lịch

``` bash
cal
cal 2026
cal 11 2026
```

Hiển thị lịch tháng hoặc năm; có thể cần cài gói `ncal` tùy bản phân
phối.

### 10.5. `uptime` -- thời gian hoạt động

``` bash
uptime
uptime -p
```

Cho biết máy đã chạy bao lâu, số người dùng đăng nhập và load average.

### 10.6. `free` -- bộ nhớ RAM và swap

``` bash
free
free -h
free -h -s 2
```

-   `-h`: đơn vị dễ đọc.
-   `-s 2`: cập nhật định kỳ mỗi 2 giây.

Cột thường gặp:

-   `total`: tổng dung lượng.
-   `used`: đang sử dụng theo cách tính của công cụ.
-   `free`: bộ nhớ hoàn toàn chưa dùng.
-   `available`: ước lượng bộ nhớ có thể cấp phát cho ứng dụng mà không
    cần swap đáng kể.
-   `buff/cache`: bộ nhớ dùng cho buffer và cache.

Trên Linux, RAM được dùng làm cache là bình thường; không nên chỉ nhìn
cột `free` để kết luận máy thiếu RAM.

### 10.7. `lscpu` -- thông tin CPU

``` bash
lscpu
```

Hiển thị kiến trúc, số CPU logic, lõi, socket và một số đặc điểm CPU.

### 10.8. `lsblk` -- danh sách thiết bị block

``` bash
lsblk
lsblk -f
```

Hiển thị ổ đĩa, phân vùng, filesystem và điểm mount.

### 10.9. `lspci` và `lsusb` -- thiết bị phần cứng

``` bash
lspci
lsusb
```

-   `lspci`: thiết bị kết nối qua PCI/PCIe.
-   `lsusb`: thiết bị USB.

### 10.10. `dmesg` -- thông báo kernel

``` bash
dmesg
dmesg -T
dmesg | tail -n 30
```

Dùng kiểm tra thông báo kernel, thiết bị, driver và lỗi phần cứng. Một
số hệ thống giới hạn quyền đọc toàn bộ nội dung.

------------------------------------------------------------------------

## 11. Ổ đĩa, phân vùng và dung lượng

### 11.1. `df` -- dung lượng filesystem

``` bash
df
df -h
df -T
df -h /home
```

-   `-h`: dung lượng dễ đọc.
-   `-T`: hiển thị loại filesystem.
-   Đối số `/home`: xem filesystem chứa đường dẫn đó.

### 11.2. `du` -- dung lượng tệp/thư mục

``` bash
du -h file.txt
du -sh project/
du -h --max-depth=1 .
du -ah | sort -h
```

-   `-h`: đơn vị dễ đọc.
-   `-s`: chỉ hiển thị tổng.
-   `-a`: tính cả tệp.
-   `--max-depth=1`: chỉ hiển thị đến một cấp thư mục con trên phiên bản
    hỗ trợ.
-   `sort -h`: sắp xếp theo giá trị có hậu tố dung lượng.

### 11.3. `mount` -- gắn filesystem

``` bash
mount
sudo mount /dev/sdb1 /mnt
```

-   `mount`: liệt kê các filesystem đang được gắn khi không có đối số.
-   `/dev/sdb1`: thiết bị/phân vùng.
-   `/mnt`: thư mục mount point.

Cần chuẩn bị mount point và xác định đúng phân vùng. Gắn sai thiết bị
hoặc thao tác ghi có thể gây mất dữ liệu.

### 11.4. `umount` -- tháo filesystem

``` bash
sudo umount /mnt
sudo umount /dev/sdb1
```

Filesystem cần được tháo an toàn trước khi rút thiết bị lưu trữ. Nếu báo
đang bận, hãy kiểm tra terminal hoặc chương trình nào đang sử dụng thư
mục đó.

### 11.5. `blkid` -- thông tin định danh filesystem

``` bash
sudo blkid
```

Hiển thị UUID, loại filesystem và các thuộc tính liên quan của thiết bị
block.

### 11.6. `fsck` -- kiểm tra filesystem

``` bash
sudo fsck /dev/sdb1
```

`fsck` là công cụ kiểm tra/sửa lỗi filesystem. Cách sử dụng phụ thuộc
loại filesystem. Không chạy tùy tiện trên filesystem đang được mount để
ghi; cần đọc hướng dẫn của filesystem và chuẩn bị bản sao dữ liệu quan
trọng.

------------------------------------------------------------------------

## 12. Mạng máy tính

### 12.1. `ip` -- cấu hình và xem thông tin mạng

``` bash
ip addr
ip link
ip route
ip -br addr
```

-   `ip addr`: địa chỉ IP trên các interface.
-   `ip link`: trạng thái và thông tin interface lớp liên kết.
-   `ip route`: bảng định tuyến.
-   `-br`: định dạng ngắn gọn.

Một số thao tác quản trị:

``` bash
sudo ip link set eth0 up
sudo ip link set eth0 down
```

-   `link set`: thay đổi thuộc tính interface.
-   `eth0`: tên interface ví dụ; máy thực tế có thể dùng tên như
    `ens33`, `enp0s3`, `wlan0`.
-   `up` / `down`: bật / tắt interface.

### 12.2. `ping` -- kiểm tra khả năng liên lạc IP

``` bash
ping 8.8.8.8
ping -c 4 example.com
```

-   `-c 4`: gửi 4 gói rồi dừng.
-   `example.com`: tên miền cần kiểm tra.

Ping sử dụng ICMP. Máy đích hoặc firewall có thể chặn ICMP, vì vậy ping
thất bại không nhất thiết có nghĩa dịch vụ TCP/UDP không hoạt động.

### 12.3. `ip route` -- xem định tuyến

``` bash
ip route
ip route get 8.8.8.8
```

Lệnh thứ hai cho biết kernel dự kiến sử dụng tuyến/interface nào để đi
đến địa chỉ đích.

### 12.4. `ss` -- xem socket và cổng mạng

``` bash
ss -tuln
ss -tunap
ss -ltn
```

Các option:

-   `-t`: TCP.
-   `-u`: UDP.
-   `-l`: socket đang lắng nghe.
-   `-n`: hiển thị số thay vì phân giải tên.
-   `-a`: tất cả socket.
-   `-p`: hiển thị tiến trình liên quan nếu có đủ quyền.

Ví dụ `ss -tuln` thường được dùng để xem các cổng TCP/UDP đang lắng
nghe.

### 12.5. `curl` -- gửi yêu cầu đến URL

``` bash
curl https://example.com
curl -I https://example.com
curl -L https://example.com
curl -o page.html https://example.com
curl -X GET https://api.example.com/items
curl -H "Accept: application/json" https://api.example.com/items
```

-   URL: địa chỉ tài nguyên.
-   `-I`: chỉ lấy header trong yêu cầu HTTP HEAD.
-   `-L`: theo redirect.
-   `-o page.html`: lưu nội dung vào tệp.
-   `-X GET`: chỉ định phương thức HTTP.
-   `-H`: thêm HTTP header.

`curl` còn hỗ trợ gửi dữ liệu POST, xác thực, cookie và nhiều giao thức
khác. Khi gửi dữ liệu nhạy cảm, tránh đặt mật khẩu trực tiếp trong lịch
sử shell.

### 12.6. `wget` -- tải tệp từ Internet

``` bash
wget https://example.com/file.zip
wget -O archive.zip https://example.com/file.zip
wget -c https://example.com/large.iso
```

-   `-O`: đặt tên tệp đầu ra.
-   `-c`: tiếp tục tải phần còn thiếu nếu máy chủ hỗ trợ.

### 12.7. `ssh` -- kết nối máy từ xa

``` bash
ssh alice@192.0.2.10
ssh -p 2222 alice@example.com
ssh -i ~/.ssh/id_ed25519 alice@example.com
```

-   `alice`: tài khoản trên máy đích.
-   `192.0.2.10` hoặc `example.com`: địa chỉ máy đích.
-   `-p 2222`: cổng SSH.
-   `-i`: chỉ định private key dùng xác thực.

### 12.8. `scp` -- sao chép tệp qua SSH

``` bash
scp report.txt alice@192.0.2.10:/home/alice/
scp alice@192.0.2.10:/home/alice/report.txt .
scp -r project/ alice@example.com:/home/alice/
```

-   Cú pháp nguồn/đích từ xa: `user@host:path`.
-   `-r`: sao chép thư mục đệ quy.
-   `.`: thư mục hiện tại.

### 12.9. `traceroute` hoặc `tracepath` -- xem đường đi mạng

``` bash
traceroute example.com
tracepath example.com
```

Hiển thị các hop trung gian trên đường đi đến đích. Có thể cần cài đặt;
một số router không phản hồi probe nên kết quả có thể có dấu `*`.

### 12.10. `dig` và `nslookup` -- truy vấn DNS

``` bash
dig example.com
dig example.com A
dig example.com MX
nslookup example.com
```

-   `A`: bản ghi địa chỉ IPv4.
-   `AAAA`: địa chỉ IPv6.
-   `MX`: máy chủ nhận email.
-   `NS`: máy chủ tên miền có thẩm quyền.

### 12.11. `hostname -I` và `ip addr`

Dùng `ip addr` khi cần xem địa chỉ gắn trên từng interface và trạng thái
chi tiết. `hostname -I` thuận tiện để xem nhanh các địa chỉ IP được hệ
thống báo cáo.

------------------------------------------------------------------------

## 13. Nén và giải nén

### 13.1. `tar` -- đóng gói tệp

Tạo archive:

``` bash
tar -cvf backup.tar project/
```

Giải nén:

``` bash
tar -xvf backup.tar
```

Các option thường gặp:

-   `-c`: tạo archive.
-   `-x`: giải nén/extract.
-   `-v`: hiển thị tên tệp đang xử lý.
-   `-f`: chỉ định tên archive; thường đặt ngay trước tên tệp archive.
-   `-t`: liệt kê nội dung archive.

### 13.2. `tar.gz` -- đóng gói và nén gzip

``` bash
tar -czvf backup.tar.gz project/
tar -xzvf backup.tar.gz
tar -tzvf backup.tar.gz
```

-   `-z`: dùng gzip.
-   `-c`: tạo.
-   `-x`: giải nén.
-   `-v`: hiển thị tiến trình.
-   `-f`: tên archive.

### 13.3. `tar.bz2` và `tar.xz`

``` bash
tar -cjvf backup.tar.bz2 project/
tar -xjvf backup.tar.bz2

tar -cJvf backup.tar.xz project/
tar -xJvf backup.tar.xz
```

-   `-j`: bzip2.
-   `-J`: xz.

### 13.4. `gzip` và `gunzip`

``` bash
gzip file.txt
gunzip file.txt.gz
gzip -k file.txt
```

-   `gzip`: nén tệp; mặc định thường thay tệp nguồn bằng tệp `.gz`.
-   `gunzip`: giải nén gzip.
-   `-k`: giữ tệp nguồn.

### 13.5. `zip` và `unzip`

``` bash
zip archive.zip file1.txt file2.txt
zip -r project.zip project/
unzip archive.zip
unzip -l archive.zip
unzip archive.zip -d output/
```

-   `-r`: thêm thư mục và nội dung đệ quy.
-   `-l`: liệt kê nội dung.
-   `-d`: chọn thư mục đích giải nén.

### 13.6. Chọn công cụ phù hợp

-   `tar`: đóng gói nhiều tệp/thư mục thành một archive; có thể kết hợp
    thuật toán nén.
-   `gzip`: nén luồng/tệp đơn, thường dùng cùng `tar`.
-   `zip`: định dạng archive phổ biến, thuận tiện trao đổi với Windows.
-   `xz` / `bzip2`: thuật toán nén khác, có đặc điểm về tỷ lệ nén và tốc
    độ riêng.

------------------------------------------------------------------------

## 14. Quản lý gói phần mềm

Cách quản lý gói phụ thuộc bản phân phối.

### 14.1. Ubuntu/Debian -- `apt`

Cập nhật danh sách gói:

``` bash
sudo apt update
```

Nâng cấp gói đã cài:

``` bash
sudo apt upgrade
```

Nâng cấp có thể xử lý thay đổi phụ thuộc:

``` bash
sudo apt full-upgrade
```

Cài phần mềm:

``` bash
sudo apt install nginx
```

Gỡ phần mềm:

``` bash
sudo apt remove nginx
sudo apt purge nginx
```

Dọn phụ thuộc không còn cần:

``` bash
sudo apt autoremove
```

Tìm gói:

``` bash
apt search nginx
apt show nginx
```

Giải thích:

-   `apt`: giao diện quản lý gói cấp cao.
-   `update`: tải/cập nhật metadata danh sách gói, không đồng nghĩa nâng
    cấp phần mềm.
-   `upgrade`: nâng cấp các gói đã cài theo chính sách của lệnh.
-   `install`: cài gói.
-   `remove`: gỡ chương trình nhưng thường giữ một số cấu hình.
-   `purge`: gỡ chương trình và cấu hình do gói quản lý.
-   `autoremove`: gỡ các phụ thuộc được cài tự động và hiện không còn
    cần.

### 14.2. Debian/Ubuntu -- `dpkg`

``` bash
dpkg -l
dpkg -L nginx
sudo dpkg -i package.deb
sudo dpkg -r package-name
```

-   `-l`: liệt kê gói.
-   `-L`: liệt kê tệp do gói cài đặt.
-   `-i`: cài gói `.deb`.
-   `-r`: gỡ gói.

`dpkg` xử lý gói ở mức thấp hơn `apt` và không tự giải quyết đầy đủ các
phụ thuộc từ kho phần mềm như `apt`.

### 14.3. Fedora/RHEL hiện đại -- `dnf`

``` bash
sudo dnf check-update
sudo dnf install nginx
sudo dnf remove nginx
sudo dnf upgrade
dnf search nginx
dnf info nginx
```

### 14.4. Arch Linux -- `pacman`

``` bash
sudo pacman -Syu
sudo pacman -S nginx
sudo pacman -R nginx
pacman -Ss nginx
pacman -Qi nginx
```

-   `-Syu`: đồng bộ cơ sở dữ liệu và nâng cấp hệ thống.
-   `-S`: cài gói.
-   `-R`: gỡ gói.
-   `-Ss`: tìm trong kho.
-   `-Qi`: xem thông tin gói đã cài.

Không trộn lẫn lệnh quản lý gói giữa các bản phân phối nếu không hiểu hệ
thống gói đang dùng.

------------------------------------------------------------------------

## 15. Lịch sử lệnh và bí danh

### 15.1. `history` -- xem lịch sử

``` bash
history
history 20
```

Hiển thị các lệnh đã nhập trong shell hiện tại hoặc lịch sử được shell
lưu lại.

Chạy lại một lệnh theo số thứ tự:

``` bash
!125
```

Chạy lại lệnh gần nhất bắt đầu bằng chuỗi:

``` bash
!git
```

Cần cẩn trọng với cú pháp `!` vì nó có thể thực thi lệnh ngay lập tức.

### 15.2. `clear` -- xóa màn hình terminal

``` bash
clear
```

Chỉ làm sạch phần hiển thị terminal, không xóa lịch sử lệnh hay dữ liệu.

### 15.3. `alias` -- tạo bí danh

``` bash
alias ll='ls -lah'
alias update='sudo apt update'
```

Sau đó có thể gọi:

``` bash
ll
update
```

Xóa alias:

``` bash
unalias ll
```

Alias thường chỉ tồn tại trong phiên shell hiện tại, trừ khi được khai
báo trong tệp cấu hình shell như `~/.bashrc`.

### 15.4. `exit` -- thoát shell

``` bash
exit
```

Đóng shell hoặc kết thúc phiên terminal hiện tại. Có thể dùng `logout`
trong một số shell đăng nhập.

------------------------------------------------------------------------

## 16. Chuyển hướng và đường ống

Đây là nhóm kiến thức rất quan trọng khi làm việc với Linux shell.

### 16.1. Ba luồng chuẩn

Mỗi tiến trình thường có ba luồng chuẩn:

  Luồng      File descriptor Ý nghĩa
  -------- ----------------- -----------------------
  stdin                    0 Dữ liệu đầu vào chuẩn
  stdout                   1 Kết quả đầu ra chuẩn
  stderr                   2 Thông báo lỗi chuẩn

### 16.2. `>` -- chuyển stdout và ghi đè

``` bash
echo "Hello Linux" > output.txt
```

Ghi stdout vào `output.txt`. Nếu tệp đã tồn tại, nội dung cũ thường bị
ghi đè.

### 16.3. `>>` -- chuyển stdout và nối thêm

``` bash
echo "New line" >> output.txt
```

Thêm dữ liệu vào cuối tệp, không xóa nội dung cũ.

### 16.4. `<` -- lấy đầu vào từ tệp

``` bash
sort < names.txt
```

Nội dung `names.txt` trở thành stdin của lệnh `sort`.

### 16.5. `2>` -- chuyển stderr

``` bash
find /root -name "*.txt" 2> errors.log
```

-   `2`: file descriptor stderr.
-   `>`: chuyển hướng.
-   `errors.log`: nơi lưu thông báo lỗi.

### 16.6. `&>` -- chuyển cả stdout và stderr

``` bash
command &> all_output.log
```

Đây là cú pháp phổ biến trong Bash. Cú pháp POSIX tương đương thường
viết:

``` bash
command > all_output.log 2>&1
```

### 16.7. `/dev/null` -- bỏ đầu ra

``` bash
command > /dev/null
command 2> /dev/null
command > /dev/null 2>&1
```

`/dev/null` là thiết bị đặc biệt loại bỏ dữ liệu được ghi vào nó.

### 16.8. `|` -- pipe, truyền đầu ra sang đầu vào

``` bash
ls -l | less
ps aux | grep firefox
cat access.log | wc -l
```

Dấu `|` nối stdout của lệnh bên trái vào stdin của lệnh bên phải.

Ví dụ:

``` bash
ps aux | grep firefox
```

Quy trình:

1.  `ps aux` liệt kê tiến trình.
2.  Pipe chuyển kết quả sang `grep`.
3.  `grep firefox` chỉ giữ các dòng chứa chuỗi `firefox`.

Lưu ý: pipe thông thường chỉ truyền stdout; stderr vẫn đi riêng, trừ khi
chủ động chuyển hướng.

### 16.9. `tee` -- vừa hiển thị vừa ghi tệp

``` bash
ls -l | tee listing.txt
ls -l | tee -a listing.txt
```

-   `tee`: đọc stdin, ghi ra stdout và một hoặc nhiều tệp.
-   `-a`: nối thêm thay vì ghi đè.

### 16.10. `&&`, `||`, `;`

``` bash
mkdir project && cd project
command1 || echo "Command failed"
date; whoami; pwd
```

-   `&&`: chạy lệnh sau nếu lệnh trước thành công (exit status bằng 0).
-   `||`: chạy lệnh sau nếu lệnh trước thất bại (exit status khác 0).
-   `;`: chạy lần lượt, không phụ thuộc trạng thái thành công/thất bại
    của lệnh trước.

------------------------------------------------------------------------

## 17. Lệnh văn bản và xử lý dữ liệu

### 17.1. `echo` -- in chuỗi

``` bash
echo "Hello"
echo $HOME
echo -n "No newline"
```

-   `echo`: in đối số ra stdout.
-   `-n`: không in ký tự xuống dòng cuối cùng trên các bản triển khai hỗ
    trợ.

Với xử lý chuỗi phức tạp, `printf` thường dễ dự đoán hơn.

### 17.2. `printf` -- in theo định dạng

``` bash
printf "Name: %s\nAge: %d\n" "Alice" 20
```

-   `%s`: chuỗi.
-   `%d`: số nguyên.
-   `\n`: xuống dòng.

### 17.3. `sort` -- sắp xếp dòng

``` bash
sort names.txt
sort -r names.txt
sort -n numbers.txt
sort -u names.txt
sort -k 2 data.txt
```

-   `-r`: đảo thứ tự.
-   `-n`: sắp xếp theo giá trị số.
-   `-u`: loại các dòng trùng trong kết quả.
-   `-k 2`: sắp xếp theo khóa/cột thứ hai theo quy tắc của `sort`.

### 17.4. `uniq` -- xử lý các dòng trùng liên tiếp

``` bash
uniq names.txt
sort names.txt | uniq
sort names.txt | uniq -c
```

`uniq` chủ yếu so sánh các dòng liền kề. Vì vậy thường kết hợp `sort`
trước để gom các dòng giống nhau.

-   `-c`: đếm số lần xuất hiện của từng dòng liên tiếp.
-   `-d`: chỉ hiển thị dòng bị lặp.
-   `-u`: chỉ hiển thị dòng không lặp.

### 17.5. `cut` -- cắt trường hoặc ký tự

``` bash
cut -d ':' -f 1 /etc/passwd
cut -c 1-5 file.txt
```

-   `-d ':'`: dùng dấu `:` làm dấu phân cách.
-   `-f 1`: lấy trường đầu tiên.
-   `-c 1-5`: lấy ký tự từ vị trí 1 đến 5.

### 17.6. `tr` -- chuyển đổi hoặc xóa ký tự

``` bash
echo "hello" | tr 'a-z' 'A-Z'
echo "a,b,c" | tr ',' '\n'
echo "hello  linux" | tr -s ' '
```

-   `tr`: chuyển đổi các ký tự trong stdin.
-   `'a-z' 'A-Z'`: đổi chữ thường thành chữ hoa.
-   `-s`: gộp các lần lặp liên tiếp của ký tự được chỉ định.
-   `-d`: xóa ký tự thuộc tập được chỉ định.

### 17.7. `sed` -- xử lý văn bản theo dòng

``` bash
sed 's/old/new/' file.txt
sed 's/old/new/g' file.txt
sed -n '1,10p' file.txt
sed '/error/d' app.log
```

-   `s/old/new/`: thay lần xuất hiện đầu tiên của `old` trên mỗi dòng
    bằng `new`.
-   `g`: thay tất cả lần xuất hiện phù hợp trên mỗi dòng.
-   `-n`: tắt in tự động.
-   `1,10p`: in dòng 1 đến 10.
-   `/error/d`: xóa các dòng khớp `error` khỏi luồng đầu ra.

Mặc định `sed` không sửa tệp gốc nếu không dùng option in-place. Cú pháp
`-i` có khác biệt giữa GNU sed và BSD sed; nên sao lưu trước khi sửa
hàng loạt.

### 17.8. `awk` -- xử lý dữ liệu theo trường

``` bash
awk '{print $1}' file.txt
awk -F ':' '{print $1, $3}' /etc/passwd
awk 'NR == 1 {print}' file.txt
awk '{count++} END {print count}' file.txt
```

Khái niệm:

-   `awk`: ngôn ngữ xử lý văn bản theo bản ghi và trường.
-   `$0`: toàn bộ dòng hiện tại.
-   `$1`, `$2`: trường thứ nhất, thứ hai.
-   `-F ':'`: đặt dấu `:` làm dấu phân cách trường.
-   `NR`: số thứ tự bản ghi đã đọc.
-   `END`: khối lệnh chạy sau khi đọc hết đầu vào.

### 17.9. `diff` -- so sánh nội dung

``` bash
diff file1.txt file2.txt
diff -u file1.txt file2.txt
```

-   `diff`: chỉ ra các dòng khác nhau.
-   `-u`: định dạng unified, thường dễ đọc và dùng trong patch.

### 17.10. `cmp` -- so sánh từng byte

``` bash
cmp file1.bin file2.bin
```

Phù hợp để kiểm tra hai tệp có giống nhau ở mức byte hay không.

### 17.11. `tee`, `xargs` -- kết hợp xử lý

``` bash
printf '%s\n' file1.txt file2.txt | xargs wc -l
find . -name "*.tmp" -print0 | xargs -0 rm
```

-   `xargs`: đọc các mục từ stdin và xây dựng đối số cho lệnh.
-   `-0`: đọc các mục được phân cách bằng byte NUL; kết hợp với
    `find -print0` để xử lý tên tệp có dấu cách hoặc ký tự đặc biệt an
    toàn hơn.

Với thao tác xóa hàng loạt, hãy thay `rm` bằng `printf '%s\n'` hoặc
`echo` để xem trước danh sách đối số trước khi thực thi.

------------------------------------------------------------------------

## 18. SSH, SCP và truyền tệp

### 18.1. Tạo SSH key

``` bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

-   `ssh-keygen`: tạo và quản lý khóa SSH.
-   `-t ed25519`: chọn thuật toán Ed25519.
-   `-C`: thêm nhãn/comment để nhận diện khóa.

Lệnh tạo private key và public key. Không chia sẻ private key; chỉ cài
public key lên máy chủ hoặc dịch vụ cần xác thực.

### 18.2. Sao chép public key lên máy chủ

``` bash
ssh-copy-id alice@example.com
```

Cài public key vào tài khoản từ xa nếu máy chủ và môi trường hỗ trợ.

### 18.3. Kết nối SSH với verbose

``` bash
ssh -v alice@example.com
ssh -vv alice@example.com
```

Hiển thị thông tin chẩn đoán khi kết nối gặp lỗi.

### 18.4. `rsync` -- đồng bộ tệp và thư mục

``` bash
rsync -av project/ backup/
rsync -avz project/ alice@example.com:/home/alice/project/
```

-   `-a`: archive mode, giữ nhiều thuộc tính và sao chép đệ quy.
-   `-v`: hiển thị chi tiết.
-   `-z`: nén dữ liệu truyền qua mạng.
-   Dấu `/` cuối `project/` thường có nghĩa đồng bộ nội dung bên trong
    thư mục; bỏ dấu `/` có thể làm thay đổi cấu trúc đích.

Có thể dùng `--dry-run` để xem trước thao tác mà chưa thực hiện:

``` bash
rsync -av --dry-run project/ backup/
```

------------------------------------------------------------------------

## 19. Biến môi trường và thông tin shell

### 19.1. `env` -- xem hoặc chạy với môi trường

``` bash
env
env | grep PATH
```

Hiển thị các biến môi trường hoặc chạy một lệnh với môi trường được chỉ
định.

### 19.2. `printenv` -- in biến môi trường

``` bash
printenv
printenv HOME
printenv PATH
```

### 19.3. `export` -- xuất biến cho tiến trình con

``` bash
NAME="Alice"
export NAME
export APP_ENV="development"
```

-   `NAME="Alice"`: tạo biến shell.
-   `export NAME`: đánh dấu biến để các tiến trình con nhận được.
-   Biến được đặt trong shell hiện tại thường không tự động tồn tại sau
    khi đóng terminal.

### 19.4. `unset` -- xóa biến

``` bash
unset NAME
```

Xóa biến shell hoặc biến môi trường trong môi trường hiện tại.

### 19.5. `echo $PATH` -- xem đường dẫn tìm chương trình

``` bash
echo "$PATH"
```

`PATH` là danh sách thư mục, phân cách bằng dấu `:`. Khi nhập tên lệnh
không có đường dẫn, shell tìm chương trình trong các thư mục này theo
thứ tự.

### 19.6. `source` -- nạp tệp vào shell hiện tại

``` bash
source ~/.bashrc
```

Đọc và thực thi nội dung tệp trong shell hiện tại. Dạng viết tắt trong
Bash là:

``` bash
. ~/.bashrc
```

Khác với chạy một script trong tiến trình shell con: thay đổi biến và
thư mục từ `source` có thể tác động đến shell hiện tại.

------------------------------------------------------------------------

## 20. Một số lệnh quản trị và bảo trì

### 20.1. `systemctl` -- quản lý dịch vụ systemd

``` bash
systemctl status ssh
sudo systemctl start ssh
sudo systemctl stop ssh
sudo systemctl restart ssh
sudo systemctl reload nginx
sudo systemctl enable nginx
sudo systemctl disable nginx
```

-   `status`: xem trạng thái.
-   `start`: khởi động dịch vụ.
-   `stop`: dừng dịch vụ.
-   `restart`: khởi động lại.
-   `reload`: yêu cầu dịch vụ nạp lại cấu hình nếu dịch vụ hỗ trợ.
-   `enable`: cấu hình tự khởi động khi boot.
-   `disable`: bỏ cấu hình tự khởi động.

Tên dịch vụ có thể khác nhau giữa các bản phân phối, ví dụ `ssh` hoặc
`sshd`.

### 20.2. `journalctl` -- xem log của systemd

``` bash
journalctl
journalctl -u nginx
journalctl -u nginx -f
journalctl -b
journalctl -p err
```

-   `-u nginx`: lọc theo unit.
-   `-f`: theo dõi log mới.
-   `-b`: log từ lần khởi động hiện tại.
-   `-p err`: lọc mức ưu tiên error.

### 20.3. `crontab` -- lập lịch công việc

``` bash
crontab -l
crontab -e
crontab -r
```

-   `-l`: liệt kê lịch của người dùng hiện tại.
-   `-e`: chỉnh sửa lịch.
-   `-r`: xóa toàn bộ lịch của người dùng hiện tại; cần hết sức cẩn
    thận.

Một dòng cron có dạng:

``` text
* * * * * command
```

Năm trường thời gian lần lượt là:

1.  Phút: `0–59`.
2.  Giờ: `0–23`.
3.  Ngày trong tháng: `1–31`.
4.  Tháng: `1–12`.
5.  Thứ trong tuần: `0–7` tùy triển khai; `0` và `7` thường biểu thị Chủ
    nhật.

Ví dụ:

``` cron
0 2 * * * /home/alice/backup.sh
```

Chạy script lúc 02:00 mỗi ngày theo múi giờ và cấu hình cron của hệ
thống.

### 20.4. `shutdown` -- tắt hoặc khởi động lại

``` bash
sudo shutdown now
sudo shutdown +10
sudo shutdown -r now
sudo shutdown -c
```

-   `now`: thực hiện ngay.
-   `+10`: sau 10 phút.
-   `-r`: khởi động lại.
-   `-c`: hủy lịch shutdown đang chờ nếu còn có thể.

### 20.5. `reboot` và `poweroff`

``` bash
sudo reboot
sudo poweroff
```

-   `reboot`: khởi động lại máy.
-   `poweroff`: tắt máy.

### 20.6. `timedatectl` -- quản lý thời gian hệ thống

``` bash
timedatectl
timedatectl list-timezones
sudo timedatectl set-timezone Asia/Ho_Chi_Minh
```

-   Hiển thị thời gian, múi giờ và trạng thái đồng bộ.
-   `list-timezones`: liệt kê múi giờ.
-   `set-timezone`: đặt múi giờ.

### 20.7. `journalctl`, `dmesg` và log

Khi chẩn đoán lỗi:

-   `journalctl -u service`: xem log của dịch vụ.
-   `journalctl -b`: xem log từ lần boot hiện tại.
-   `dmesg`: xem thông báo kernel.
-   `/var/log/`: nơi chứa nhiều tệp log truyền thống trên các hệ thống.

Không phải mọi bản phân phối đều lưu log theo cùng một cách; một số log
có thể chỉ có trong journal.

------------------------------------------------------------------------

## 21. Ký tự đại diện và quy tắc viết đường dẫn

### 21.1. Wildcard `*`

Đại diện cho không hoặc nhiều ký tự trong tên đường dẫn:

``` bash
ls *.txt
cp *.cpp backup/
```

### 21.2. Wildcard `?`

Đại diện cho đúng một ký tự:

``` bash
ls file?.txt
```

Có thể khớp `file1.txt`, `fileA.txt`, nhưng không khớp `file10.txt` theo
mẫu này.

### 21.3. Ký tự lớp `[]`

``` bash
ls file[123].txt
ls report[0-9].txt
```

Khớp một ký tự thuộc tập hoặc khoảng ký tự.

### 21.4. Dấu nháy đơn và nháy kép

``` bash
echo '$HOME'
echo "$HOME"
```

-   Nháy đơn: giữ nguyên hầu hết ký tự bên trong, không mở rộng biến.
-   Nháy kép: vẫn mở rộng biến như `$HOME` và một số biểu thức shell,
    nhưng hạn chế việc tách từ và mở rộng wildcard.
-   Không có dấu nháy: shell có thể thực hiện mở rộng biến, tách từ và
    pathname expansion.

### 21.5. Escape bằng dấu `\`

``` bash
touch my\ report.txt
echo \$HOME
```

Dấu `\` giúp ký tự kế tiếp được hiểu theo nghĩa literal trong nhiều ngữ
cảnh shell.

### 21.6. Dùng `--` để kết thúc option

``` bash
rm -- -oldfile
```

`--` báo cho nhiều lệnh rằng các đối số phía sau không còn là option.
Hữu ích khi tên tệp bắt đầu bằng dấu `-`.

------------------------------------------------------------------------

## 22. Các ví dụ kết hợp thực tế

### 22.1. Tạo cấu trúc dự án

``` bash
mkdir -p my_project/{src,include,docs,tests}
touch my_project/README.md
touch my_project/src/main.cpp
tree my_project
```

Lệnh này sử dụng brace expansion của Bash để tạo nhiều thư mục trong một
lần.

### 22.2. Tìm tất cả tệp C++ trong dự án

``` bash
find . -type f \( -name "*.cpp" -o -name "*.h" -o -name "*.hpp" \)
```

-   `\(` và `\)`: nhóm điều kiện cho `find`.
-   `-o`: phép OR.
-   `-type f`: chỉ tìm tệp thông thường.

### 22.3. Tìm dòng chứa lỗi trong log

``` bash
grep -inE "error|failed|exception" app.log
```

-   `-i`: không phân biệt hoa thường.
-   `-n`: hiển thị số dòng.
-   `-E`: hỗ trợ biểu thức chính quy mở rộng.
-   `error|failed|exception`: khớp một trong các từ khóa.

### 22.4. Đếm số dòng chứa từ khóa

``` bash
grep -c "ERROR" app.log
```

`-c` đếm số dòng khớp, không phải tổng số lần từ khóa xuất hiện.

### 22.5. Xem 20 tiến trình sử dụng nhiều CPU

``` bash
ps aux --sort=-%cpu | head -n 20
```

-   `--sort=-%cpu`: sắp xếp CPU giảm dần.
-   `head -n 20`: lấy 20 dòng đầu.

### 22.6. Tìm thư mục chiếm nhiều dung lượng

``` bash
du -h --max-depth=1 . | sort -hr
```

-   `du`: tính dung lượng.
-   `--max-depth=1`: giới hạn độ sâu.
-   `sort -h`: hiểu hậu tố dung lượng.
-   `-r`: thứ tự giảm dần.

### 22.7. Sao lưu thư mục thành tar.gz

``` bash
tar -czvf project_backup.tar.gz project/
```

Giải nén vào thư mục hiện tại:

``` bash
tar -xzvf project_backup.tar.gz
```

Giải nén vào vị trí cụ thể:

``` bash
mkdir -p restore
tar -xzvf project_backup.tar.gz -C restore/
```

### 22.8. Theo dõi log trực tiếp

``` bash
tail -f /var/log/syslog
```

Dừng theo dõi bằng `Ctrl + C`.

### 22.9. Kiểm tra cổng đang lắng nghe

``` bash
sudo ss -tulnp
```

Xem giao thức, địa chỉ local, cổng và tiến trình liên quan nếu quyền cho
phép.

### 22.10. Tìm và xóa tệp tạm một cách thận trọng

Xem trước:

``` bash
find ./temp -type f -name "*.tmp" -print
```

Chỉ sau khi xác nhận danh sách đúng mới xóa:

``` bash
find ./temp -type f -name "*.tmp" -delete
```

### 22.11. Ghi cả kết quả và lỗi vào log

``` bash
./script.sh > script.log 2>&1
```

-   `>` chuyển stdout vào `script.log`.
-   `2>&1` chuyển stderr đến cùng đích hiện tại của stdout.
-   Thứ tự chuyển hướng quan trọng. Cách viết trên bảo đảm cả hai luồng
    cùng vào tệp.

### 22.12. Kiểm tra kết nối HTTP và mã trạng thái

``` bash
curl -I https://example.com
```

Đọc status line và header phản hồi để kiểm tra máy chủ web có phản hồi
hay không.

------------------------------------------------------------------------

## 23. Lưu ý an toàn khi sử dụng Linux

1.  **Kiểm tra đường dẫn trước khi xóa:** đặc biệt với `rm -r`, `rm -f`
    và `find -delete`.
2.  **Hạn chế dùng `sudo`:** chỉ nâng quyền cho lệnh thật sự cần thiết.
3.  **Không chạy script lạ ngay lập tức:** đọc nội dung, kiểm tra nguồn
    và quyền thực thi trước.
4.  **Cẩn trọng với chuyển hướng `>`:** có thể làm mất nội dung tệp cũ
    ngay cả khi lệnh phía sau thất bại.
5.  **Kiểm tra lệnh trước khi dùng `xargs`:** tên tệp có dấu cách hoặc
    ký tự đặc biệt cần xử lý đúng; ưu tiên `-print0` và `-0`.
6.  **Không chia sẻ private SSH key:** chỉ chia sẻ public key.
7.  **Sao lưu trước khi sửa cấu hình hoặc filesystem:** nhất là các thao
    tác mount, fsck, phân vùng và quyền đệ quy.
8.  **Đọc thông báo lỗi:** lỗi `Permission denied`,
    `No such file or directory`, `command not found` và
    `Device or resource busy` chỉ ra các nhóm vấn đề khác nhau.
9.  **Phân biệt `apt update` và `apt upgrade`:** một lệnh cập nhật danh
    sách gói, lệnh còn lại nâng cấp phần mềm đã cài.
10. **Hiểu exit status:** mã `0` thường biểu thị thành công; mã khác `0`
    thường biểu thị lỗi hoặc trạng thái đặc biệt.
11. **Không tin tuyệt đối vào ping:** ICMP có thể bị chặn dù dịch vụ ứng
    dụng vẫn hoạt động.
12. **Thử nghiệm trong môi trường an toàn:** với lệnh quản trị, nên thực
    hành trên máy ảo hoặc container trước khi áp dụng lên máy quan
    trọng.

------------------------------------------------------------------------

## 24. Bảng tra cứu nhanh

  Nhu cầu                      Lệnh
  ---------------------------- -------------------------------
  Xem thư mục hiện tại         `pwd`
  Liệt kê tệp                  `ls`
  Liệt kê chi tiết và tệp ẩn   `ls -lah`
  Chuyển thư mục               `cd`
  Tạo tệp                      `touch`
  Tạo thư mục                  `mkdir`
  Sao chép                     `cp`
  Di chuyển/đổi tên            `mv`
  Xóa tệp                      `rm`
  Xóa thư mục rỗng             `rmdir`
  Đọc tệp                      `cat`, `less`
  Xem đầu/cuối tệp             `head`, `tail`
  Theo dõi log                 `tail -f`
  Đếm dòng/từ/byte             `wc`
  Tìm tệp                      `find`
  Tìm nội dung                 `grep`
  Xem quyền tệp                `ls -l`, `stat`
  Đổi quyền                    `chmod`
  Đổi chủ sở hữu               `chown`
  Xem người dùng hiện tại      `whoami`
  Xem UID/GID                  `id`
  Chạy quyền quản trị          `sudo`
  Xem tiến trình               `ps`, `top`, `htop`
  Gửi tín hiệu tiến trình      `kill`, `pkill`
  Xem RAM                      `free -h`
  Xem dung lượng filesystem    `df -h`
  Xem dung lượng thư mục       `du -sh`
  Xem ổ đĩa/phân vùng          `lsblk`
  Xem địa chỉ mạng             `ip addr`
  Kiểm tra kết nối             `ping`
  Xem cổng lắng nghe           `ss -tuln`
  Gửi HTTP request             `curl`
  Tải tệp                      `wget`
  Kết nối từ xa                `ssh`
  Sao chép qua SSH             `scp`
  Nén/giải nén                 `tar`, `gzip`, `zip`, `unzip`
  Cài phần mềm Ubuntu/Debian   `apt install`
  Xem dịch vụ                  `systemctl status`
  Xem log dịch vụ              `journalctl -u`
  Xem lịch sử                  `history`
  Xóa màn hình                 `clear`
  Xem trợ giúp                 `man`, `--help`

------------------------------------------------------------------------

## Kết luận

Để sử dụng Linux thành thạo, không cần học thuộc mọi option ngay từ đầu.
Nên nắm chắc cấu trúc lệnh, đường dẫn, quyền truy cập, chuyển hướng,
pipe và cách đọc tài liệu trợ giúp. Sau đó luyện tập theo nhóm công
việc: quản lý tệp, tìm kiếm, quản trị tiến trình, mạng, quản lý dịch vụ
và xử lý log.

Một quy trình học hiệu quả:

1.  Đọc cú pháp bằng `man` hoặc `--help`.
2.  Thử lệnh trên thư mục và tệp mẫu.
3.  Thay đổi từng option để quan sát kết quả.
4.  Kết hợp các lệnh bằng pipe và chuyển hướng.
5.  Thực hành trong máy ảo Linux trước khi thao tác trên hệ thống quan
    trọng.

**Ghi nhớ:** hiểu rõ từng thành phần của lệnh quan trọng hơn việc chỉ
sao chép một dòng lệnh mà không biết nó sẽ tác động đến đâu.
