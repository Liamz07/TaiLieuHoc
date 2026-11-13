# TÀI LIỆU TỔNG HỢP LẬP TRÌNH SHELL (BASH)

> Tài liệu hệ thống hóa kiến thức Bash từ nền tảng đến nâng cao, dành cho người học Linux và muốn tự viết các script tự động hóa. Các ví dụ ưu tiên Bash trên Linux. Nội dung không sử dụng mã LaTeX.

---

## Mục lục

1. [Tổng quan về Shell và Bash](#1-tổng-quan-về-shell-và-bash)
2. [Cách chạy lệnh và script](#2-cách-chạy-lệnh-và-script)
3. [Cú pháp cơ bản và quy tắc viết lệnh](#3-cú-pháp-cơ-bản-và-quy-tắc-viết-lệnh)
4. [Biến và kiểu dữ liệu](#4-biến-và-kiểu-dữ-liệu)
5. [Mở rộng biến và trích dẫn](#5-mở-rộng-biến-và-trích-dẫn)
6. [Nhập và xuất dữ liệu](#6-nhập-và-xuất-dữ-liệu)
7. [Mã thoát và xử lý lỗi](#7-mã-thoát-và-xử-lý-lỗi)
8. [Phép toán số học](#8-phép-toán-số-học)
9. [Điều kiện rẽ nhánh](#9-điều-kiện-rẽ-nhánh)
10. [Vòng lặp](#10-vòng-lặp)
11. [Mảng và xử lý danh sách](#11-mảng-và-xử-lý-danh-sách)
12. [Hàm trong Bash](#12-hàm-trong-bash)
13. [Tham số dòng lệnh](#13-tham-số-dòng-lệnh)
14. [Làm việc với tệp và thư mục](#14-làm-việc-với-tệp-và-thư-mục)
15. [Pipeline và các lệnh xử lý văn bản](#15-pipeline-và-các-lệnh-xử-lý-văn-bản)
16. [Process, job và tín hiệu](#16-process-job-và-tín-hiệu)
17. [Redirection và Here Document](#17-redirection-và-here-document)
18. [Các tính năng Bash hữu ích](#18-các-tính-năng-bash-hữu-ích)
19. [Debug và kiểm tra script](#19-debug-và-kiểm-tra-script)
20. [Viết script an toàn và dễ bảo trì](#20-viết-script-an-toàn-và-dễ-bảo-trì)
21. [Các mẫu script thực tế](#21-các-mẫu-script-thực-tế)
22. [Lỗi thường gặp](#22-lỗi-thường-gặp)
23. [Lộ trình thực hành và bài tập](#23-lộ-trình-thực-hành-và-bài-tập)
24. [Bảng tổng kết lệnh thường dùng](#24-bảng-tổng-kết-lệnh-thường-dùng)

---

# 1. Tổng quan về Shell và Bash

## 1.1. Shell là gì?

Shell là chương trình cung cấp giao diện để người dùng giao tiếp với hệ điều hành. Người dùng nhập lệnh, Shell phân tích lệnh rồi yêu cầu hệ điều hành thực thi chương trình tương ứng.

Ví dụ:

```bash
pwd
ls -la
mkdir bai_tap
```

Shell có thể hoạt động theo hai cách phổ biến:

- **Interactive shell:** người dùng nhập lệnh trực tiếp trong Terminal và nhận kết quả.
- **Non-interactive shell:** Shell đọc và thực thi các lệnh từ một tệp script.

## 1.2. Bash là gì?

Bash là viết tắt của **Bourne Again SHell**, một Shell phổ biến trên Linux và có mặt trên nhiều hệ thống Unix-like.

Bash vừa là trình thông dịch lệnh, vừa là ngôn ngữ lập trình dùng để:

- Tự động hóa các thao tác quản trị hệ thống.
- Quản lý tệp và thư mục.
- Sao lưu dữ liệu.
- Kiểm tra tiến trình và tài nguyên.
- Kết hợp nhiều chương trình thành một quy trình xử lý.
- Triển khai ứng dụng và hỗ trợ quy trình DevOps.
- Viết các công cụ dòng lệnh nhỏ.

## 1.3. Shell, Terminal và Bash khác nhau thế nào?

| Khái niệm | Ý nghĩa |
|---|---|
| Terminal | Ứng dụng cung cấp cửa sổ giao tiếp dạng văn bản |
| Shell | Chương trình đọc và thực thi lệnh |
| Bash | Một chương trình Shell cụ thể |
| Command | Lệnh được nhập vào Shell |
| Script | Tệp chứa một chuỗi lệnh để Shell thực thi |

Terminal không phải Shell. Terminal là nơi hiển thị giao diện; Bash là chương trình chạy bên trong Terminal.

## 1.4. Một số loại Shell phổ biến

- `sh`: Bourne Shell hoặc giao diện tương thích với nó.
- `bash`: Bourne Again Shell.
- `zsh`: Z Shell, có nhiều tính năng tương tác.
- `fish`: Friendly Interactive Shell.
- `ksh`: KornShell.

Các Shell có nhiều cú pháp tương đồng nhưng không hoàn toàn giống nhau. Tài liệu này tập trung vào Bash.

## 1.5. Kiểm tra Shell đang sử dụng

```bash
echo "$SHELL"
echo "$0"
bash --version
```

Lưu ý:

- `$SHELL` thường thể hiện Shell đăng nhập mặc định của người dùng.
- `$0` thể hiện tên chương trình hoặc Shell hiện tại trong ngữ cảnh đang chạy.
- `bash --version` hiển thị phiên bản Bash.

---

# 2. Cách chạy lệnh và script

## 2.1. Chạy lệnh trực tiếp

```bash
echo "Hello Bash"
date
whoami
```

Shell sẽ đọc từng lệnh và thực thi chúng.

## 2.2. Tạo một tệp Bash

Tạo tệp `hello.sh`:

```bash
#!/usr/bin/env bash

echo "Xin chao Bash!"
echo "Nguoi dung hien tai: $(whoami)"
```

Dòng đầu tiên được gọi là **shebang**:

```bash
#!/usr/bin/env bash
```

Shebang cho hệ điều hành biết chương trình thông dịch cần dùng khi chạy tệp trực tiếp. Cách dùng `env` giúp tìm Bash theo biến môi trường `PATH`.

## 2.3. Chạy script bằng Bash

```bash
bash hello.sh
```

Cách này gọi Bash để đọc tệp. Tệp không nhất thiết phải có quyền thực thi.

## 2.4. Chạy script trực tiếp

Cấp quyền thực thi:

```bash
chmod +x hello.sh
```

Sau đó chạy:

```bash
./hello.sh
```

Dấu `./` chỉ định tệp trong thư mục hiện tại. Thư mục hiện tại thường không nằm trong `PATH`, vì vậy chỉ gõ `hello.sh` có thể không chạy được.

## 2.5. Nạp script bằng source

```bash
source config.sh
```

Hoặc:

```bash
. config.sh
```

`source` thực thi nội dung tệp trong Shell hiện tại. Vì vậy, biến và hàm được khai báo trong tệp có thể tiếp tục tồn tại sau khi lệnh kết thúc.

Khác biệt:

| Cách chạy | Môi trường thực thi |
|---|---|
| `bash file.sh` | Tiến trình Bash mới |
| `./file.sh` | Chương trình thông dịch được chỉ định trong shebang |
| `source file.sh` | Shell hiện tại |

Không nên `source` một tệp không đáng tin cậy vì các lệnh trong đó có thể tác động trực tiếp đến phiên Shell hiện tại.

---

# 3. Cú pháp cơ bản và quy tắc viết lệnh

## 3.1. Một lệnh Bash có cấu trúc tổng quát

```bash
command [options] [arguments]
```

Ví dụ:

```bash
ls -l /home
```

- `ls`: tên lệnh.
- `-l`: tùy chọn.
- `/home`: đối số.

Ý nghĩa tùy chọn phụ thuộc vào từng chương trình. Có thể dùng `--help` hoặc đọc `man`:

```bash
ls --help
man ls
```

## 3.2. Dấu cách có ý nghĩa

```bash
name="An"
```

Đây là phép gán biến hợp lệ.

```bash
name = "An"
```

Đây không phải phép gán biến. Bash sẽ cố thực thi lệnh có tên `name`.

## 3.3. Dấu chấm phẩy

Dùng `;` để phân tách nhiều lệnh trên cùng một dòng:

```bash
pwd; date; whoami
```

Các lệnh được thực thi lần lượt. Nếu một lệnh thất bại, những lệnh tiếp theo vẫn có thể được chạy.

## 3.4. Toán tử AND và OR

```bash
command1 && command2
command1 || command2
```

- `&&`: chạy lệnh thứ hai nếu lệnh thứ nhất thành công.
- `||`: chạy lệnh thứ hai nếu lệnh thứ nhất thất bại.

Ví dụ:

```bash
mkdir backup && echo "Tao thu muc thanh cong"
```

```bash
cd /thu_muc_khong_ton_tai || echo "Khong the chuyen thu muc"
```

## 3.5. Comment

```bash
# Day la comment
echo "Hello" # Comment sau lenh
```

Bash bỏ qua phần comment khi thực thi. Ngoại lệ đáng chú ý là shebang ở đầu tệp, được hệ điều hành sử dụng khi chạy tệp trực tiếp.

## 3.6. Nhiều dòng và nối dòng

Có thể viết một lệnh trên nhiều dòng bằng dấu `\` ở cuối dòng:

```bash
echo "Day la mot \
chuoi tren nhieu dong"
```

Dấu `\` phải là ký tự cuối cùng trên dòng, không có khoảng trắng phía sau.

---

# 4. Biến và kiểu dữ liệu

## 4.1. Khai báo biến

```bash
ten="Nguyen Van A"
tuoi=20
diem=8.5
```

Bash không yêu cầu khai báo kiểu dữ liệu cố định như C++ hoặc Java. Giá trị biến chủ yếu được xử lý dưới dạng chuỗi; các ngữ cảnh số học sẽ diễn giải giá trị phù hợp thành số nguyên.

Quy tắc:

- Không có khoảng trắng quanh dấu `=`.
- Tên biến phân biệt chữ hoa và chữ thường.
- Tên biến nên bắt đầu bằng chữ cái hoặc dấu gạch dưới.
- Không dùng dấu gạch ngang trong tên biến.
- Nên dùng tên có ý nghĩa.

## 4.2. Truy cập giá trị biến

```bash
ten="An"
echo "$ten"
echo "${ten}"
```

`${ten}` hữu ích khi cần phân định tên biến với phần văn bản liền kề:

```bash
file="data"
echo "${file}_backup.txt"
```

## 4.3. Gán kết quả lệnh vào biến

Cú pháp khuyến nghị:

```bash
ngay_hien_tai=$(date)
nguoi_dung=$(whoami)

echo "$ngay_hien_tai"
echo "$nguoi_dung"
```

Đây gọi là **command substitution**.

Cú pháp cũ dùng dấu backtick:

```bash
ngay_hien_tai=`date`
```

Nên ưu tiên `$(...)` vì dễ đọc và có thể lồng nhau thuận tiện hơn.

## 4.4. Biến môi trường

Biến Shell thông thường chỉ tồn tại trong Shell hiện tại. Muốn truyền biến cho tiến trình con, cần export:

```bash
ten_ung_dung="QuanLyFile"
export ten_ung_dung
```

Hoặc:

```bash
export moi_truong="development"
```

Kiểm tra:

```bash
printenv moi_truong
env
```

## 4.5. Biến cục bộ trong hàm

```bash
thong_bao() {
    local noi_dung="Xin chao"
    echo "$noi_dung"
}
```

`local` giới hạn phạm vi biến trong hàm. Nên dùng `local` cho biến tạm để tránh vô tình thay đổi biến bên ngoài.

## 4.6. Một số biến đặc biệt

| Biến | Ý nghĩa |
|---|---|
| `$0` | Tên script hoặc tên Shell |
| `$1`, `$2`, ... | Đối số vị trí |
| `$#` | Số lượng đối số |
| `$@` | Tất cả đối số, giữ ranh giới từng đối số khi được trích dẫn đúng |
| `$*` | Tất cả đối số, có cách ghép khác `$@` khi đặt trong dấu nháy kép |
| `$?` | Mã thoát của lệnh vừa thực thi |
| `$$` | PID của Shell hiện tại |
| `$!` | PID của tiến trình chạy nền gần nhất |
| `$_` | Thường là đối số cuối của lệnh trước đó trong ngữ cảnh tương ứng |
| `BASH_SOURCE` | Mảng chứa đường dẫn các tệp nguồn Bash trong ngữ cảnh gọi |

Ví dụ:

```bash
echo "Ten script: $0"
echo "So doi so: $#"
echo "Doi so thu nhat: $1"
```

## 4.7. Giá trị mặc định và kiểm tra biến

```bash
echo "${ten:-Khach}"
```

Nếu `ten` chưa được đặt hoặc rỗng, biểu thức trả về `Khach` nhưng không gán lại biến.

```bash
echo "${ten:=Khach}"
```

Nếu `ten` chưa được đặt hoặc rỗng, Bash gán `Khach` vào biến.

```bash
echo "${ten:?Chua cung cap ten}"
```

Nếu biến chưa được đặt hoặc rỗng, Bash phát thông báo lỗi và trong ngữ cảnh thông thường sẽ kết thúc Shell hoặc script đang mở rộng biểu thức đó.

```bash
echo "${ten:+Da co ten}"
```

Nếu biến đã được đặt và không rỗng, biểu thức trả về `Da co ten`.

Phân biệt:

- `-`, `=`, `?`, `+`: xét cả trường hợp biến chưa được đặt hoặc có giá trị rỗng.
- `:-`, `:=`, `:?`, `:+`: cũng xét trường hợp biến rỗng.

## 4.8. Xóa biến

```bash
unset ten
```

Sau đó biến không còn được đặt.

## 4.9. readonly

```bash
readonly PI=3
```

Biến `PI` không thể được gán lại trong Shell hiện tại.

---

# 5. Mở rộng biến và trích dẫn

Đây là phần rất quan trọng vì nhiều lỗi Bash xuất phát từ việc Shell tách từ hoặc mở rộng ký tự ngoài ý muốn.

## 5.1. Dấu nháy kép

```bash
ten="Nguyen Van A"
echo "$ten"
```

Dấu nháy kép giữ khoảng trắng trong giá trị biến thành một đối số. Tuy nhiên, Bash vẫn thực hiện mở rộng biến, command substitution và một số dạng mở rộng khác bên trong dấu nháy kép.

Nên mặc định trích dẫn biến:

```bash
rm -- "$file"
```

`--` báo cho nhiều lệnh rằng các đối số tiếp theo không phải tùy chọn. Nó hữu ích khi tên tệp bắt đầu bằng dấu `-`.

## 5.2. Dấu nháy đơn

```bash
echo '$HOME'
```

Bash in nguyên văn `$HOME`, không mở rộng biến.

Dùng nháy đơn khi muốn giữ nội dung đúng như đã viết.

## 5.3. Backslash

```bash
echo "Gia tri \$HOME"
```

Backslash có thể vô hiệu hóa ý nghĩa đặc biệt của ký tự kế tiếp trong nhiều ngữ cảnh.

## 5.4. Vì sao cần quote biến?

Giả sử:

```bash
file="bao cao.txt"
cat $file
```

Bash có thể tách thành hai đối số `bao` và `cao.txt`, khiến lệnh thất bại.

Cách đúng:

```bash
cat "$file"
```

Quy tắc thực hành: trích dẫn biến bằng `"$bien"` trừ khi bạn thực sự muốn Shell thực hiện tách từ hoặc mở rộng tên tệp.

## 5.5. Word splitting và globbing

Sau khi mở rộng biến không được quote, Bash có thể:

1. Tách kết quả thành nhiều từ dựa trên `IFS`.
2. Mở rộng các ký tự đại diện như `*`, `?`, `[abc]` thành tên tệp phù hợp.

Ví dụ:

```bash
duong_dan="thu muc"
printf '%s\n' $duong_dan
```

Có thể tạo ra nhiều đối số. Trong khi đó:

```bash
printf '%s\n' "$duong_dan"
```

truyền một đối số duy nhất.

## 5.6. Mở rộng tham số chuỗi

```bash
chuoi="lap-trinh-bash"
```

Lấy độ dài:

```bash
echo "${#chuoi}"
```

Lấy phần từ vị trí:

```bash
echo "${chuoi:0:3}"
```

Xóa phần khớp ngắn nhất ở đầu:

```bash
echo "${chuoi#*-}"
```

Xóa phần khớp dài nhất ở đầu:

```bash
echo "${chuoi##*-}"
```

Xóa phần khớp ngắn nhất ở cuối:

```bash
echo "${chuoi%-*}"
```

Xóa phần khớp dài nhất ở cuối:

```bash
echo "${chuoi%%-*}"
```

Thay thế lần khớp đầu:

```bash
echo "${chuoi/bash/shell}"
```

Thay thế tất cả lần khớp:

```bash
echo "${chuoi//-/ }"
```

Các phép thay thế này dùng mẫu glob của Bash, không phải biểu thức chính quy đầy đủ.

---

# 6. Nhập và xuất dữ liệu

## 6.1. In dữ liệu với echo

```bash
echo "Xin chao"
echo "Hom nay la: $(date)"
```

`echo` phù hợp với thông báo đơn giản. Khi cần kiểm soát định dạng hoặc xử lý chuỗi đáng tin cậy, nên ưu tiên `printf`.

## 6.2. In dữ liệu với printf

```bash
printf "Ten: %s\n" "An"
printf "Tuoi: %d\n" 20
printf "Gia tri: %.2f\n" 8.567
```

Một số định dạng:

| Ký hiệu | Ý nghĩa |
|---|---|
| `%s` | Chuỗi |
| `%d` | Số nguyên thập phân |
| `%f` | Số thực |
| `%.2f` | Số thực với 2 chữ số sau dấu thập phân |
| `%b` | Diễn giải một số escape sequence trong đối số |
| `\n` | Xuống dòng |
| `\t` | Tab |

## 6.3. Nhập dữ liệu với read

```bash
read -r ten
echo "Xin chao $ten"
```

`-r` ngăn `read` xử lý backslash như ký tự escape. Đây là lựa chọn nên dùng trong hầu hết trường hợp đọc dòng văn bản.

Hiển thị lời nhắc:

```bash
read -r -p "Nhap ten: " ten
```

Nhập nhiều biến:

```bash
read -r ho ten
```

Đọc mật khẩu không hiển thị ký tự:

```bash
read -r -s -p "Nhap mat khau: " mat_khau
printf '\n'
```

## 6.4. Đọc dữ liệu từ tệp

```bash
while IFS= read -r dong || [[ -n "$dong" ]]; do
    printf '%s\n' "$dong"
done < "data.txt"
```

Ý nghĩa:

- `IFS=` tránh loại bỏ khoảng trắng đầu và cuối dòng do cơ chế tách trường.
- `read -r` giữ nguyên backslash.
- `|| [[ -n "$dong" ]]` xử lý cả dòng cuối nếu tệp không kết thúc bằng ký tự newline.
- `< "data.txt"` chuyển nội dung tệp vào đầu vào chuẩn của vòng lặp.

Đây là mẫu đọc tệp văn bản theo từng dòng thường dùng.

## 6.5. Đọc từng trường

```bash
IFS=, read -r ten tuoi thanh_pho <<< "An,20,Hanoi"
printf '%s %s %s\n' "$ten" "$tuoi" "$thanh_pho"
```

`<<<` là **here string**, đưa một chuỗi vào đầu vào chuẩn của lệnh.

Lưu ý: cách tách bằng `IFS` không phải bộ phân tích CSV hoàn chỉnh. CSV có thể chứa dấu phẩy trong trường được đặt trong dấu nháy kép; trường hợp đó cần công cụ hoặc thư viện CSV chuyên dụng.

---

# 7. Mã thoát và xử lý lỗi

## 7.1. Exit status là gì?

Mỗi lệnh thường trả về một mã thoát:

- `0`: thành công.
- Khác `0`: có lỗi hoặc một trạng thái không thành công theo quy ước của chương trình.

Kiểm tra:

```bash
mkdir thu_muc_moi
echo $?
```

Phải kiểm tra `$?` ngay sau lệnh cần quan sát vì lệnh tiếp theo sẽ ghi đè giá trị này.

## 7.2. Exit trong script

```bash
#!/usr/bin/env bash

echo "Bat dau"
exit 0
```

`exit 0` kết thúc script với trạng thái thành công.

```bash
exit 1
```

Kết thúc với trạng thái thất bại tổng quát.

Có thể dùng các mã khác nhau để biểu thị các lỗi khác nhau, nhưng nên quy ước rõ ràng trong dự án.

## 7.3. Điều kiện dựa trên mã thoát

```bash
if mkdir backup; then
    echo "Tao thanh cong"
else
    echo "Tao that bai"
fi
```

Đây thường là cách rõ ràng hơn so với chạy lệnh rồi kiểm tra `$?`.

## 7.4. set -e, set -u và pipefail

Một cấu hình thường gặp:

```bash
set -Eeuo pipefail
```

Ý nghĩa khái quát:

- `-e`: yêu cầu Bash thoát khi một lệnh thất bại trong nhiều ngữ cảnh.
- `-u`: báo lỗi khi tham chiếu biến chưa được đặt.
- `-o pipefail`: pipeline trả về trạng thái thất bại nếu có lệnh trong pipeline thất bại, thay vì chỉ xét lệnh cuối.
- `-E`: giúp ERR trap được kế thừa trong một số ngữ cảnh hàm và môi trường con.

**Cảnh báo:** `set -e` không phải cơ chế bắt mọi lỗi một cách tuyệt đối. Hành vi của nó có ngoại lệ trong điều kiện `if`, `while`, `&&`, `||`, pipeline và một số ngữ cảnh khác. Hãy chủ động kiểm tra kết quả của các thao tác quan trọng.

Ví dụ:

```bash
if cp "$nguon" "$dich"; then
    echo "Sao chep thanh cong"
else
    echo "Sao chep that bai" >&2
    exit 1
fi
```

## 7.5. Phân biệt lỗi của lệnh và lỗi của script

Một chương trình con có thể thất bại nhưng script vẫn tiếp tục nếu script không xử lý trạng thái đó. Vì vậy, script tốt cần xác định:

- Lệnh nào bắt buộc thành công?
- Lệnh nào có thể thất bại và được bỏ qua?
- Khi lỗi xảy ra thì cần dừng, thử lại hay ghi log?
- Tài nguyên tạm có cần được dọn dẹp không?

---

# 8. Phép toán số học

Bash hỗ trợ số học nguyên thông qua arithmetic expansion.

## 8.1. Arithmetic expansion

```bash
a=10
b=3

echo "$((a + b))"
echo "$((a - b))"
echo "$((a * b))"
echo "$((a / b))"
echo "$((a % b))"
```

Phép chia số nguyên sẽ bỏ phần thập phân.

## 8.2. Tăng và giảm

```bash
dem=0
((dem++))
((dem--))
((dem += 5))
```

Hoặc:

```bash
dem=$((dem + 1))
```

## 8.3. So sánh số nguyên

Trong ngữ cảnh số học:

```bash
if (( a > b )); then
    echo "a lon hon b"
fi
```

Các toán tử:

| Toán tử | Ý nghĩa |
|---|---|
| `==` | Bằng |
| `!=` | Khác |
| `>` | Lớn hơn |
| `<` | Nhỏ hơn |
| `>=` | Lớn hơn hoặc bằng |
| `<=` | Nhỏ hơn hoặc bằng |
| `&&` | AND logic |
| `||` | OR logic |
| `!` | Phủ định |

## 8.4. Số thực

Bash không hỗ trợ phép toán số thực trực tiếp trong arithmetic expansion. Có thể dùng `awk`, `bc` hoặc ngôn ngữ khác:

```bash
echo "scale=2; 10 / 3" | bc
```

Hoặc:

```bash
awk 'BEGIN { printf "%.2f\n", 10 / 3 }'
```

Không nên dùng số thực khi cần độ chính xác tài chính nếu chưa hiểu rõ sai số dấu phẩy động.

---

# 9. Điều kiện rẽ nhánh

## 9.1. if, elif, else

```bash
if [[ dieu_kien_1 ]]; then
    echo "Nhanh 1"
elif [[ dieu_kien_2 ]]; then
    echo "Nhanh 2"
else
    echo "Nhanh con lai"
fi
```

`fi` kết thúc cấu trúc `if`.

## 9.2. So sánh chuỗi

```bash
ten="An"

if [[ "$ten" == "An" ]]; then
    echo "Xin chao An"
fi
```

Các toán tử thường dùng:

| Toán tử | Ý nghĩa |
|---|---|
| `==` | Hai chuỗi bằng nhau |
| `!=` | Hai chuỗi khác nhau |
| `-z "$s"` | Chuỗi rỗng |
| `-n "$s"` | Chuỗi không rỗng |
| `<`, `>` | So sánh thứ tự từ điển trong `[[ ]]` |

Nên quote biến ngay cả trong `[[ ]]` để biểu đạt ý định rõ ràng, dù `[[ ]]` không thực hiện word splitting và pathname expansion giống `[ ]`.

## 9.3. So sánh số nguyên với test

Có thể dùng `(( ))`:

```bash
if (( tuoi >= 18 )); then
    echo "Du tuoi"
fi
```

Hoặc dùng `-eq`, `-ne`, `-lt`, `-le`, `-gt`, `-ge` trong `[[ ]]`:

```bash
if [[ "$tuoi" -ge 18 ]]; then
    echo "Du tuoi"
fi
```

Các toán tử:

| Toán tử | Ý nghĩa |
|---|---|
| `-eq` | Bằng |
| `-ne` | Khác |
| `-lt` | Nhỏ hơn |
| `-le` | Nhỏ hơn hoặc bằng |
| `-gt` | Lớn hơn |
| `-ge` | Lớn hơn hoặc bằng |

## 9.4. Kiểm tra tệp và thư mục

```bash
if [[ -f "$file" ]]; then
    echo "La tep thuong"
fi
```

Các điều kiện phổ biến:

| Kiểm tra | Ý nghĩa |
|---|---|
| `-e path` | Đường dẫn tồn tại |
| `-f path` | Tệp thông thường |
| `-d path` | Thư mục |
| `-L path` | Symbolic link |
| `-r path` | Có quyền đọc |
| `-w path` | Có quyền ghi |
| `-x path` | Có quyền thực thi |
| `-s path` | Tệp tồn tại và có kích thước lớn hơn 0 |
| `file1 -nt file2` | `file1` mới hơn `file2` |
| `file1 -ot file2` | `file1` cũ hơn `file2` |

## 9.5. Kết hợp điều kiện

```bash
if [[ -f "$file" && -r "$file" ]]; then
    echo "Tep ton tai va doc duoc"
fi
```

Có thể dùng `!` để phủ định:

```bash
if [[ ! -d "$folder" ]]; then
    mkdir -p "$folder"
fi
```

## 9.6. case

Dùng `case` khi cần phân nhánh theo nhiều mẫu:

```bash
read -r -p "Nhap lua chon: " lua_chon

case "$lua_chon" in
    1)
        echo "Them"
        ;;
    2)
        echo "Sua"
        ;;
    3)
        echo "Xoa"
        ;;
    q|Q)
        echo "Thoat"
        ;;
    *)
        echo "Lua chon khong hop le"
        ;;
esac
```

`;;` kết thúc nhánh thông thường. `esac` kết thúc cấu trúc `case`.

---

# 10. Vòng lặp

## 10.1. for theo danh sách

```bash
for ten in An Binh Chi; do
    echo "$ten"
done
```

## 10.2. for theo dãy số

```bash
for ((i = 1; i <= 5; i++)); do
    echo "$i"
done
```

Cú pháp này là cú pháp đặc trưng của Bash.

## 10.3. Dùng brace expansion

```bash
for i in {1..5}; do
    echo "$i"
done
```

Bước nhảy:

```bash
for i in {0..10..2}; do
    echo "$i"
done
```

Brace expansion được Bash thực hiện trước khi chạy lệnh và thường yêu cầu dãy được viết tĩnh, không phải biến tùy ý.

## 10.4. while

```bash
dem=1

while (( dem <= 5 )); do
    echo "$dem"
    ((dem++))
done
```

## 10.5. until

`until` lặp khi điều kiện còn sai và dừng khi điều kiện đúng:

```bash
dem=1

until (( dem > 5 )); do
    echo "$dem"
    ((dem++))
done
```

## 10.6. break và continue

- `break`: thoát khỏi vòng lặp gần nhất.
- `continue`: bỏ qua phần còn lại của lượt hiện tại và chuyển sang lượt kế tiếp.

```bash
for ((i = 1; i <= 10; i++)); do
    if (( i == 5 )); then
        continue
    fi

    if (( i == 8 )); then
        break
    fi

    echo "$i"
done
```

## 10.7. Lặp qua các tệp

```bash
for file in *.txt; do
    [[ -e "$file" ]] || continue
    printf 'Tim thay: %s\n' "$file"
done
```

Kiểm tra `-e` giúp tránh trường hợp mẫu `*.txt` không khớp tệp nào nhưng vẫn được giữ nguyên thành chuỗi `*.txt`, tùy cấu hình Bash.

## 10.8. Lặp qua đầu vào dòng

```bash
while IFS= read -r dong; do
    printf 'Dong: %s\n' "$dong"
done < input.txt
```

Tránh dùng:

```bash
for dong in $(cat input.txt)
```

Cách này tách dữ liệu theo khoảng trắng và có thể làm hỏng nội dung dòng, đồng thời gặp vấn đề với ký tự glob.

---

# 11. Mảng và xử lý danh sách

Bash hỗ trợ mảng chỉ số và mảng kết hợp.

## 11.1. Mảng chỉ số

```bash
mon_hoc=("C++" "Linux" "Network" "Security")
```

Truy cập phần tử:

```bash
echo "${mon_hoc[0]}"
echo "${mon_hoc[2]}"
```

Chỉ số mảng Bash bắt đầu từ 0 theo cách sử dụng thông thường.

## 11.2. Lấy toàn bộ phần tử

```bash
printf '%s\n' "${mon_hoc[@]}"
```

Nên dùng `"${array[@]}"` khi muốn mỗi phần tử trở thành một đối số riêng.

## 11.3. Số lượng phần tử

```bash
echo "${#mon_hoc[@]}"
```

## 11.4. Thêm phần tử

```bash
mon_hoc+=("DevOps")
```

## 11.5. Xóa phần tử

```bash
unset 'mon_hoc[1]'
```

Việc xóa một phần tử không nhất thiết đánh lại chỉ số các phần tử còn lại.

## 11.6. Lặp qua mảng

```bash
for mon in "${mon_hoc[@]}"; do
    echo "$mon"
done
```

Lặp qua chỉ số:

```bash
for i in "${!mon_hoc[@]}"; do
    printf '%s: %s\n' "$i" "${mon_hoc[$i]}"
done
```

## 11.7. Mảng kết hợp

Mảng kết hợp lưu dữ liệu theo khóa:

```bash
declare -A diem

diem["An"]=9
diem["Binh"]=8

echo "${diem["An"]}"
```

Lấy các khóa:

```bash
echo "${!diem[@]}"
```

Lấy các giá trị:

```bash
echo "${diem[@]}"
```

Mảng kết hợp cần Bash 4 trở lên. Một số hệ thống cũ có thể cài phiên bản Bash thấp hơn.

## 11.8. Khác biệt giữa $@ và $*

Trong hàm hoặc script:

```bash
for arg in "$@"; do
    printf '<%s>\n' "$arg"
done
```

`"$@"` giữ từng đối số riêng biệt. Đây là cách nên dùng khi chuyển tiếp danh sách tham số.

`"$*"` ghép các đối số thành một chuỗi duy nhất, dùng ký tự đầu tiên của `IFS` làm dấu phân cách.

---

# 12. Hàm trong Bash

## 12.1. Khai báo hàm

```bash
xin_chao() {
    echo "Xin chao!"
}
```

Gọi hàm:

```bash
xin_chao
```

## 12.2. Hàm có tham số

```bash
xin_chao() {
    local ten="$1"
    echo "Xin chao $ten"
}

xin_chao "Nguyen Van A"
```

Bên trong hàm, `$1`, `$2`, `$#`, `$@` là các đối số truyền cho lần gọi hàm đó.

## 12.3. Giá trị trả về

`return` trả về mã trạng thái từ 0 đến 255, không phải chuỗi kết quả tùy ý:

```bash
kiem_tra_so_duong() {
    local n="$1"

    if (( n > 0 )); then
        return 0
    else
        return 1
    fi
}

if kiem_tra_so_duong 5; then
    echo "So duong"
fi
```

## 12.4. Trả về dữ liệu qua stdout

```bash
tinh_tong() {
    local a="$1"
    local b="$2"

    printf '%s\n' "$((a + b))"
}

ket_qua=$(tinh_tong 10 20)
echo "$ket_qua"
```

Đây là mẫu phổ biến: hàm in kết quả ra stdout, bên gọi thu lại bằng command substitution.

Không nên trộn thông báo trạng thái vào stdout nếu bên gọi cần thu một giá trị máy có thể phân tích. Có thể đưa thông báo sang stderr.

## 12.5. Hàm và biến local

```bash
dem=100

test_ham() {
    local dem=5
    echo "$dem"
}

test_ham
echo "$dem"
```

Biến `dem` bên ngoài vẫn là `100`.

## 12.6. Đệ quy

Bash cho phép hàm gọi lại chính nó:

```bash
dem_nguoc() {
    local n="$1"

    if (( n <= 0 )); then
        return
    fi

    echo "$n"
    dem_nguoc "$((n - 1))"
}

dem_nguoc 5
```

Không nên lạm dụng đệ quy trong Bash vì Shell không phải ngôn ngữ tối ưu cho các thuật toán đệ quy sâu.

---

# 13. Tham số dòng lệnh

## 13.1. Nhận đối số

Tạo `thong_tin.sh`:

```bash
#!/usr/bin/env bash

echo "Ten script: $0"
echo "Doi so 1: $1"
echo "Doi so 2: $2"
echo "Tong so doi so: $#"
```

Chạy:

```bash
bash thong_tin.sh An 20
```

## 13.2. Duyệt tất cả đối số

```bash
for arg in "$@"; do
    printf 'Doi so: %s\n' "$arg"
done
```

## 13.3. shift

`shift` loại bỏ đối số vị trí đầu tiên, khiến `$2` trở thành `$1`, `$3` trở thành `$2`, v.v.

```bash
while (( $# > 0 )); do
    echo "Doi so: $1"
    shift
done
```

## 13.4. getopts

`getopts` hỗ trợ phân tích các tùy chọn dạng ngắn:

```bash
#!/usr/bin/env bash

ten=""
verbose=0

while getopts ":n:v" opt; do
    case "$opt" in
        n)
            ten="$OPTARG"
            ;;
        v)
            verbose=1
            ;;
        :)
            echo "Tuy chon -$OPTARG can gia tri" >&2
            exit 2
            ;;
        \?)
            echo "Tuy chon khong hop le: -$OPTARG" >&2
            exit 2
            ;;
    esac
done

printf 'Ten: %s\n' "$ten"
printf 'Verbose: %s\n' "$verbose"
```

Chạy:

```bash
./script.sh -n "An" -v
```

Trong chuỗi `":n:v"`:

- `n:` nghĩa là `-n` cần một đối số.
- `v` nghĩa là `-v` không cần đối số.
- Dấu `:` đầu tiên yêu cầu `getopts` xử lý lỗi theo chế độ im lặng để script tự tạo thông báo.

`getopts` chủ yếu xử lý tùy chọn ngắn. Với giao diện dòng lệnh phức tạp có nhiều tùy chọn dài, có thể tự phân tích `$@` hoặc dùng công cụ chuyên dụng.

---

# 14. Làm việc với tệp và thư mục

## 14.1. Các lệnh thư mục cơ bản

| Lệnh | Chức năng |
|---|---|
| `pwd` | In thư mục hiện tại |
| `ls` | Liệt kê nội dung |
| `cd` | Chuyển thư mục |
| `mkdir` | Tạo thư mục |
| `mkdir -p` | Tạo cả thư mục cha còn thiếu |
| `rmdir` | Xóa thư mục rỗng |
| `find` | Tìm tệp/thư mục |
| `tree` | Hiển thị cây thư mục nếu đã cài |

## 14.2. Các lệnh tệp cơ bản

| Lệnh | Chức năng |
|---|---|
| `touch` | Tạo tệp rỗng hoặc cập nhật thời gian |
| `cp` | Sao chép |
| `mv` | Di chuyển hoặc đổi tên |
| `rm` | Xóa |
| `cat` | In nội dung tệp |
| `less` | Xem tệp theo trang |
| `head` | Xem phần đầu |
| `tail` | Xem phần cuối |
| `file` | Nhận diện loại tệp |
| `stat` | Xem thông tin chi tiết |
| `wc` | Đếm dòng, từ, byte hoặc ký tự |

## 14.3. Tạo và kiểm tra thư mục

```bash
mkdir -p backup/logs
```

```bash
if [[ -d "backup" ]]; then
    echo "Thu muc da ton tai"
fi
```

## 14.4. Sao chép và di chuyển

```bash
cp "source.txt" "backup.txt"
cp -r "source_dir" "backup_dir"
mv "old.txt" "new.txt"
```

Luôn cẩn thận với lệnh ghi đè. Có thể dùng các tùy chọn tương tác như `-i` khi phù hợp:

```bash
cp -i source.txt destination.txt
mv -i old.txt new.txt
```

## 14.5. Xóa tệp

```bash
rm -- "temp.txt"
```

Xóa thư mục cùng nội dung:

```bash
rm -r -- "temp_dir"
```

`rm -rf` rất nguy hiểm nếu biến đường dẫn rỗng hoặc sai. Không xây dựng lệnh xóa bằng cách ghép chuỗi thiếu kiểm tra. Hãy xác thực đường dẫn và hiển thị đối tượng sẽ xóa trước khi thực hiện.

## 14.6. Tìm kiếm với find

Tìm tệp `.log`:

```bash
find . -type f -name "*.log"
```

Tìm thư mục tên `backup`:

```bash
find . -type d -name "backup"
```

Tìm tệp lớn hơn 100 MB:

```bash
find . -type f -size +100M
```

Tìm và xóa tệp `.tmp`:

```bash
find . -type f -name "*.tmp" -print
```

Sau khi kiểm tra danh sách, mới cân nhắc lệnh xóa:

```bash
find . -type f -name "*.tmp" -delete
```

Không dùng `-delete` trước khi hiểu rõ phạm vi tìm kiếm.

## 14.7. Xử lý tên tệp có khoảng trắng hoặc ký tự đặc biệt

Không nên phân tích đầu ra của `ls` bằng vòng lặp. Tên tệp có thể chứa khoảng trắng, tab hoặc newline.

Dùng `find` với NUL delimiter:

```bash
while IFS= read -r -d '' file; do
    printf 'File: %s\n' "$file"
done < <(find . -type f -print0)
```

`-print0` phân tách tên tệp bằng byte NUL; `read -d ''` đọc đến byte NUL. Đây là cách an toàn hơn khi xử lý tên tệp tùy ý trên Linux.

---

# 15. Pipeline và các lệnh xử lý văn bản

## 15.1. Pipeline là gì?

Pipeline dùng ký tự `|` để chuyển stdout của lệnh trước thành stdin của lệnh sau.

```bash
cat access.log | grep "ERROR"
```

Có thể viết gọn:

```bash
grep "ERROR" access.log
```

Không phải lúc nào cũng cần `cat` ở đầu pipeline.

## 15.2. grep

Tìm dòng chứa từ khóa:

```bash
grep "error" app.log
```

Không phân biệt hoa thường:

```bash
grep -i "error" app.log
```

Hiển thị số dòng:

```bash
grep -n "error" app.log
```

Tìm đệ quy:

```bash
grep -r "TODO" .
```

Đếm số dòng khớp:

```bash
grep -c "ERROR" app.log
```

Dùng biểu thức chính quy mở rộng:

```bash
grep -E "ERROR|WARNING" app.log
```

Chỉ in phần khớp:

```bash
grep -oE "[0-9]+" data.txt
```

## 15.3. sort

```bash
sort names.txt
sort -r names.txt
sort -n numbers.txt
sort -u names.txt
```

- `-r`: đảo thứ tự.
- `-n`: sắp xếp theo số.
- `-u`: loại bỏ các dòng trùng theo kết quả sắp xếp.

## 15.4. uniq

```bash
sort names.txt | uniq
```

`uniq` mặc định chỉ loại các dòng trùng nằm liền nhau. Vì vậy thường kết hợp với `sort`.

Đếm số lần xuất hiện:

```bash
sort names.txt | uniq -c
```

## 15.5. wc

```bash
wc file.txt
wc -l file.txt
wc -w file.txt
wc -c file.txt
```

- `-l`: số dòng.
- `-w`: số từ.
- `-c`: số byte.
- `-m`: số ký tự theo locale.

## 15.6. cut

Tách trường theo dấu phân cách:

```bash
cut -d ':' -f 1 /etc/passwd
```

Lấy cột thứ nhất, phân cách bằng dấu `:`.

Lấy nhiều cột:

```bash
cut -d ',' -f 1,3 data.csv
```

`cut` không hiểu đầy đủ cú pháp CSV có dấu phẩy nằm bên trong trường được quote.

## 15.7. tr

Thay thế ký tự:

```bash
echo "hello" | tr 'a-z' 'A-Z'
```

Xóa ký tự:

```bash
echo "a-b-c" | tr -d '-'
```

Gộp các ký tự lặp:

```bash
echo "hello     bash" | tr -s ' '
```

## 15.8. sed

Thay thế nội dung:

```bash
sed 's/old/new/' file.txt
```

Thay thế tất cả lần xuất hiện trên mỗi dòng:

```bash
sed 's/old/new/g' file.txt
```

Chỉ in dòng thứ 2:

```bash
sed -n '2p' file.txt
```

Xóa dòng chứa từ khóa:

```bash
sed '/DEBUG/d' file.txt
```

Ghi trực tiếp vào tệp với `-i` có khác biệt giữa GNU sed và BSD sed. Nên tạo bản sao hoặc kiểm tra cú pháp theo hệ điều hành trước khi dùng.

## 15.9. awk

`awk` phù hợp để xử lý văn bản theo dòng và trường.

Ví dụ dữ liệu:

```text
An 8
Binh 9
Chi 7
```

In cột 1:

```bash
awk '{print $1}' diem.txt
```

In cột 1 và cột 2:

```bash
awk '{print $1, $2}' diem.txt
```

Tính tổng cột 2:

```bash
awk '{sum += $2} END {print sum}' diem.txt
```

Dùng dấu phân cách `:`:

```bash
awk -F ':' '{print $1}' /etc/passwd
```

Lọc theo điều kiện:

```bash
awk '$2 >= 8 {print $1, $2}' diem.txt
```

## 15.10. xargs

Chuyển dữ liệu đầu vào thành đối số cho lệnh khác:

```bash
printf '%s\n' a.txt b.txt | xargs -r ls -l
```

Với tên tệp có ký tự đặc biệt, nên dùng NUL:

```bash
find . -type f -name "*.tmp" -print0 | xargs -0 -r rm --
```

`-r` thường có nghĩa là không chạy lệnh nếu đầu vào rỗng trên GNU xargs. Hãy kiểm tra tính tương thích nếu cần chạy trên nhiều hệ Unix.

## 15.11. tee

Vừa hiển thị kết quả vừa ghi vào tệp:

```bash
echo "Backup started" | tee backup.log
```

Ghi nối:

```bash
echo "Backup completed" | tee -a backup.log
```

## 15.12. Pipeline và subshell

Trong Bash, các thành phần của pipeline thường chạy trong subshell, ngoại trừ một số trường hợp được tối ưu hoặc khi bật tùy chọn `lastpipe`.

Ví dụ:

```bash
count=0
printf '%s\n' a b c | while IFS= read -r line; do
    ((count++))
done

echo "$count"
```

Trong cấu hình Bash thông thường, `count` bên ngoài có thể vẫn là `0` vì vòng lặp chạy trong subshell.

Một cách thay thế:

```bash
count=0
while IFS= read -r line; do
    ((count++))
done < <(printf '%s\n' a b c)

echo "$count"
```

Process substitution `< <(...)` giúp vòng lặp chạy trong Shell hiện tại ở mẫu này.

---

# 16. Process, job và tín hiệu

## 16.1. Process là gì?

Process là một chương trình đang chạy. Mỗi process có PID (Process ID) để hệ điều hành định danh.

Các lệnh hữu ích:

```bash
ps
ps aux
top
htop
pgrep bash
```

## 16.2. Chạy nền

Thêm `&` ở cuối lệnh:

```bash
sleep 60 &
```

Lệnh chạy nền và trả quyền điều khiển Terminal cho người dùng.

Lấy PID tiến trình nền gần nhất:

```bash
echo "$!"
```

## 16.3. Xem job

```bash
jobs
```

Các thao tác:

```bash
command &
jobs
fg
bg
```

- `fg`: đưa job lên foreground.
- `bg`: tiếp tục job ở background.
- `Ctrl + Z`: tạm dừng job foreground.
- `Ctrl + C`: gửi SIGINT tới tiến trình foreground theo cơ chế Terminal.

## 16.4. Dừng process

```bash
kill PID
```

Mặc định `kill` gửi SIGTERM. Đây là yêu cầu tiến trình kết thúc một cách có kiểm soát.

```bash
kill -9 PID
```

Gửi SIGKILL, buộc hệ điều hành kết thúc tiến trình. Tiến trình không thể bắt hoặc xử lý SIGKILL để tự dọn dẹp. Chỉ dùng khi tín hiệu kết thúc thông thường không hiệu quả và đã hiểu hậu quả.

## 16.5. Một số tín hiệu thường gặp

| Signal | Ý nghĩa khái quát |
|---|---|
| SIGINT | Ngắt, thường từ Ctrl+C |
| SIGTERM | Yêu cầu kết thúc |
| SIGKILL | Buộc kết thúc |
| SIGHUP | Hangup; thường liên quan đến terminal hoặc phiên kết nối |
| SIGSTOP | Tạm dừng tiến trình, không thể bắt |
| SIGCONT | Tiếp tục tiến trình đã dừng |
| SIGCHLD | Báo hiệu trạng thái tiến trình con thay đổi |

## 16.6. Trap

`trap` cho phép Shell thực hiện hành động khi nhận tín hiệu hoặc khi một số sự kiện Shell xảy ra.

```bash
#!/usr/bin/env bash

cleanup() {
    echo "Dang don dep..."
    rm -f -- "$tmp_file"
}

tmp_file=$(mktemp)
trap cleanup EXIT

echo "Lam viec voi tep tam"
```

`EXIT` làm hàm dọn dẹp được gọi khi Shell kết thúc. Cần viết cleanup an toàn, tránh xóa nhầm dữ liệu quan trọng.

Bắt SIGINT:

```bash
trap 'echo "Nhan duoc Ctrl+C"' INT
```

Không thể bắt hoặc xử lý SIGKILL và SIGSTOP.

## 16.7. Wait

```bash
sleep 2 &
pid=$!

wait "$pid"
echo "Tien trinh da ket thuc"
```

`wait` chờ tiến trình con kết thúc và có thể nhận mã thoát của nó.

---

# 17. Redirection và Here Document

## 17.1. Ba luồng chuẩn

Mỗi tiến trình thường có ba luồng chuẩn:

| File descriptor | Tên | Mục đích |
|---|---|---|
| `0` | stdin | Đầu vào chuẩn |
| `1` | stdout | Đầu ra chuẩn |
| `2` | stderr | Đầu ra lỗi |

## 17.2. Chuyển hướng stdout

Ghi đè:

```bash
echo "Hello" > output.txt
```

Ghi nối:

```bash
echo "World" >> output.txt
```

## 17.3. Chuyển hướng stderr

```bash
command 2> error.log
```

Ghi nối stderr:

```bash
command 2>> error.log
```

## 17.4. Chuyển hướng cả stdout và stderr

```bash
command > output.log 2>&1
```

Hoặc trong Bash:

```bash
command &> output.log
```

Thứ tự chuyển hướng rất quan trọng:

```bash
command > output.log 2>&1
```

Chuyển stdout vào tệp trước, sau đó chuyển stderr đến nơi stdout đang trỏ tới.

Trong khi:

```bash
command 2>&1 > output.log
```

stderr được nối với stdout ban đầu trước khi stdout được chuyển vào tệp, nên kết quả khác.

## 17.5. Bỏ đầu ra

```bash
command > /dev/null
```

Bỏ stderr:

```bash
command 2> /dev/null
```

Bỏ cả hai:

```bash
command > /dev/null 2>&1
```

Không nên bỏ lỗi một cách tùy tiện vì sẽ khó phát hiện sự cố.

## 17.6. Đưa tệp vào stdin

```bash
sort < names.txt
```

## 17.7. Here Document

Here document cung cấp một khối văn bản làm đầu vào:

```bash
cat <<EOF
Xin chao
Hom nay la $(date)
EOF
```

Với delimiter không được quote, các biểu thức như biến và command substitution được mở rộng.

Nếu quote delimiter:

```bash
cat <<'EOF'
Gia tri HOME la $HOME
EOF
```

Nội dung được giữ nguyên, không mở rộng `$HOME`.

## 17.8. Here String

```bash
grep "bash" <<< "hoc lap trinh bash"
```

Here string đưa một chuỗi vào stdin của lệnh và thêm newline ở cuối.

---

# 18. Các tính năng Bash hữu ích

## 18.1. PATH

`PATH` là biến môi trường chứa danh sách thư mục mà Shell tìm kiếm chương trình khi người dùng gọi lệnh không ghi đường dẫn đầy đủ.

```bash
echo "$PATH"
```

Thêm thư mục vào đầu PATH cho phiên hiện tại:

```bash
export PATH="$HOME/bin:$PATH"
```

Không nên thêm thư mục không đáng tin cậy vào PATH, đặc biệt ở vị trí ưu tiên cao.

## 18.2. Alias

```bash
alias ll='ls -lah'
alias gs='git status'
```

Xóa alias:

```bash
unalias ll
```

Alias chủ yếu tiện cho tương tác. Trong script, nên gọi lệnh hoặc hàm rõ ràng thay vì phụ thuộc alias.

## 18.3. History

```bash
history
```

Tìm lệnh đã dùng:

```bash
history | grep "docker"
```

`Ctrl + R` thường hỗ trợ tìm ngược lịch sử lệnh trong Bash tương tác.

## 18.4. Command lookup

```bash
type ls
command -v bash
which bash
```

`type` có thể cho biết tên lệnh là alias, function, builtin hay chương trình bên ngoài. `command -v` thường phù hợp để kiểm tra lệnh có sẵn.

## 18.5. Builtin và external command

Một số lệnh là Bash builtin:

```bash
cd
echo
read
export
unset
```

Một số lệnh thường là chương trình bên ngoài:

```bash
grep
sed
awk
find
```

Có thể xem thông tin:

```bash
type cd
type grep
```

## 18.6. Subshell

Dùng dấu ngoặc đơn để thực thi nhóm lệnh trong subshell:

```bash
(
    cd /tmp
    pwd
)

pwd
```

Thay đổi thư mục bên trong subshell không ảnh hưởng Shell cha.

## 18.7. Process substitution

```bash
diff <(sort file1.txt) <(sort file2.txt)
```

Bash tạo giao diện dạng tệp cho đầu ra của các lệnh con để chương trình có thể đọc như đầu vào tệp.

Có thể dùng dạng đầu ra:

```bash
command > >(tee output.log)
```

Process substitution là tính năng Bash, không phải cú pháp POSIX sh.

## 18.8. Glob patterns

| Mẫu | Ý nghĩa |
|---|---|
| `*.txt` | Tên kết thúc bằng `.txt` |
| `file?.txt` | `?` khớp một ký tự |
| `[abc].txt` | Khớp một ký tự trong nhóm |
| `[0-9].txt` | Khớp một chữ số |
| `*.{txt,log}` | Brace expansion tạo nhiều mẫu |

Bash có tùy chọn `shopt` để mở rộng glob:

```bash
shopt -s nullglob
shopt -s globstar
```

- `nullglob`: mẫu không khớp sẽ mở rộng thành danh sách rỗng thay vì giữ nguyên chuỗi.
- `globstar`: `**` có thể khớp thư mục con đệ quy.

## 18.9. IFS

`IFS` (Internal Field Separator) xác định các ký tự phân tách trường trong nhiều hoạt động của Shell.

```bash
IFS=:
read -r user pass uid gid <<< "an:x:1000:1000"
```

Nên giới hạn thay đổi `IFS` trong phạm vi cần thiết hoặc khôi phục giá trị ban đầu để tránh tác động ngoài ý muốn.

## 18.10. mktemp

Tạo tệp tạm an toàn hơn việc tự đặt tên cố định:

```bash
tmp_file=$(mktemp)
```

Tạo thư mục tạm:

```bash
tmp_dir=$(mktemp -d)
```

Kết hợp trap để dọn dẹp:

```bash
tmp_dir=$(mktemp -d)
trap 'rm -rf -- "$tmp_dir"' EXIT
```

Chỉ xóa đường dẫn vừa được tạo và đã kiểm soát. Không để biến đường dẫn tạm rỗng hoặc không hợp lệ đi vào lệnh xóa.

---

# 19. Debug và kiểm tra script

## 19.1. Chạy Bash ở chế độ debug

```bash
bash -x script.sh
```

Bash in các lệnh sau khi mở rộng để hỗ trợ tìm lỗi.

Có thể bật/tắt trong script:

```bash
set -x
echo "Debug"
set +x
```

## 19.2. Kiểm tra cú pháp

```bash
bash -n script.sh
```

Lệnh này kiểm tra cú pháp mà không thực thi các lệnh trong script.

## 19.3. ShellCheck

ShellCheck là công cụ phân tích tĩnh giúp phát hiện nhiều lỗi và cách viết Bash dễ gây lỗi.

Cài đặt tùy hệ điều hành, sau đó chạy:

```bash
shellcheck script.sh
```

Nên chạy ShellCheck thường xuyên, đặc biệt với script có nhiều điều kiện, vòng lặp, quote và redirection.

## 19.4. Ghi log

```bash
log_info() {
    printf '[INFO] %s\n' "$*" >&2
}

log_error() {
    printf '[ERROR] %s\n' "$*" >&2
}
```

Ghi log ra stderr giúp stdout vẫn có thể được dùng làm dữ liệu đầu ra.

Ghi vào tệp:

```bash
log_file="app.log"
printf '[INFO] Started at %s\n' "$(date)" >> "$log_file"
```

## 19.5. Debug biến

```bash
printf 'ten=<%s>\n' "$ten"
declare -p ten
```

`declare -p` hiển thị cách Bash khai báo biến, hữu ích để kiểm tra mảng và thuộc tính biến.

## 19.6. Xác định vị trí lỗi

Có thể dùng:

```bash
set -x
```

Hoặc định nghĩa ERR trap để ghi thông tin lỗi:

```bash
trap 'printf "Loi tai dong %s\n" "$LINENO" >&2' ERR
```

ERR trap có quy tắc kế thừa và ngoại lệ tương tự cơ chế `errexit`; không xem nó là bộ bắt lỗi tuyệt đối.

---

# 20. Viết script an toàn và dễ bảo trì

## 20.1. Dùng shebang rõ ràng

```bash
#!/usr/bin/env bash
```

## 20.2. Quote biến

Ưu tiên:

```bash
cp -- "$source" "$destination"
```

Tránh:

```bash
cp $source $destination
```

## 20.3. Kiểm tra đầu vào

```bash
if [[ $# -ne 1 ]]; then
    echo "Usage: $0 <directory>" >&2
    exit 2
fi
```

Kiểm tra thư mục:

```bash
directory="$1"

if [[ ! -d "$directory" ]]; then
    echo "Thu muc khong ton tai" >&2
    exit 1
fi
```

## 20.4. Dùng đường dẫn rõ ràng

Trong script có thể xác định thư mục chứa script:

```bash
script_dir=$(cd -- "$(dirname -- "${BASH_SOURCE[0]}")" && pwd)
```

Sau đó tham chiếu tài nguyên tương đối với script:

```bash
config_file="$script_dir/config/app.conf"
```

Điều này giúp script ít phụ thuộc vào thư mục hiện tại của người chạy.

## 20.5. Hạn chế chạy bằng root

Chỉ yêu cầu quyền quản trị khi thật sự cần. Chạy toàn bộ script bằng root làm tăng hậu quả nếu script có lỗi hoặc xử lý đầu vào không an toàn.

## 20.6. Cẩn thận với eval

`eval` yêu cầu Shell phân tích và thực thi một chuỗi như mã lệnh:

```bash
eval "$user_input"
```

Nếu chuỗi có dữ liệu do người dùng kiểm soát, có thể dẫn đến thực thi lệnh ngoài ý muốn. Tránh `eval` nếu có cách xử lý bằng mảng, biến hoặc cấu trúc Bash thông thường.

## 20.7. Tránh phân tích đầu ra ls

Không làm:

```bash
for file in $(ls); do
    ...
done
```

Nên dùng glob:

```bash
for file in *; do
    [[ -e "$file" ]] || continue
    printf '%s\n' "$file"
done
```

Nếu cần duyệt đệ quy hoặc xử lý tên tệp tùy ý, dùng `find -print0` và NUL delimiter.

## 20.8. Tạo script có cấu trúc

Một script vừa phải nên có:

1. Shebang.
2. Thiết lập chế độ và biến cấu hình.
3. Hàm hỗ trợ.
4. Hàm kiểm tra đầu vào.
5. Hàm xử lý chính.
6. Hàm `main`.
7. Lời gọi `main "$@"`.

Ví dụ:

```bash
#!/usr/bin/env bash
set -Eeuo pipefail

log() {
    printf '[INFO] %s\n' "$*" >&2
}

usage() {
    printf 'Usage: %s <name>\n' "$0" >&2
}

main() {
    if [[ $# -ne 1 ]]; then
        usage
        return 2
    fi

    local ten="$1"
    log "Bat dau chuong trinh"
    printf 'Xin chao, %s!\n' "$ten"
}

main "$@"
```

---

# 21. Các mẫu script thực tế

## 21.1. Script kiểm tra tệp

```bash
#!/usr/bin/env bash
set -u

if [[ $# -ne 1 ]]; then
    echo "Usage: $0 <file>" >&2
    exit 2
fi

file="$1"

if [[ ! -e "$file" ]]; then
    echo "Tep khong ton tai"
elif [[ -f "$file" ]]; then
    echo "Day la tep thuong"
elif [[ -d "$file" ]]; then
    echo "Day la thu muc"
else
    echo "Day la mot loai duong dan khac"
fi
```

Kiến thức sử dụng:

- Tham số dòng lệnh.
- `$#`, `$1`.
- `if`.
- Điều kiện `-e`, `-f`, `-d`.
- Exit status.

## 21.2. Script sao lưu thư mục

```bash
#!/usr/bin/env bash
set -Eeuo pipefail

if [[ $# -ne 2 ]]; then
    echo "Usage: $0 <source_dir> <backup_dir>" >&2
    exit 2
fi

source_dir="$1"
backup_dir="$2"

if [[ ! -d "$source_dir" ]]; then
    echo "Thu muc nguon khong ton tai" >&2
    exit 1
fi

mkdir -p -- "$backup_dir"

timestamp=$(date '+%Y%m%d_%H%M%S')
archive="$backup_dir/backup_$timestamp.tar.gz"

if tar -czf "$archive" -C "$(dirname -- "$source_dir")" "$(basename -- "$source_dir")"; then
    echo "Da tao ban sao luu: $archive"
else
    echo "Sao luu that bai" >&2
    exit 1
fi
```

Kiến thức sử dụng:

- Tham số dòng lệnh.
- Kiểm tra thư mục.
- `mkdir -p`.
- Command substitution.
- `tar`.
- Điều kiện kiểm tra mã thoát.

Lưu ý: script này phù hợp với đường dẫn nguồn là một thư mục và tạo archive trong thư mục đích. Trong môi trường thực tế cần bổ sung chính sách lưu trữ, kiểm tra dung lượng, quyền truy cập và xác minh archive.

## 21.3. Script đếm số tệp theo phần mở rộng

```bash
#!/usr/bin/env bash
set -u

directory="${1:-.}"
extension="${2:-txt}"

if [[ ! -d "$directory" ]]; then
    echo "Thu muc khong ton tai" >&2
    exit 1
fi

count=0

while IFS= read -r -d '' file; do
    ((count += 1))
done < <(find "$directory" -type f -name "*.$extension" -print0)

printf 'Tim thay %d tep .%s\n' "$count" "$extension"
```

Kiến thức sử dụng:

- Giá trị mặc định `${var:-default}`.
- `find`.
- Process substitution.
- `read -d ''`.
- Vòng lặp `while`.

## 21.4. Script kiểm tra tiến trình

```bash
#!/usr/bin/env bash

if [[ $# -ne 1 ]]; then
    echo "Usage: $0 <process_name>" >&2
    exit 2
fi

process_name="$1"

if pgrep -x -- "$process_name" > /dev/null; then
    echo "Tien trinh dang chay"
else
    echo "Khong tim thay tien trinh"
fi
```

`pgrep -x` tìm tiến trình có tên khớp chính xác. Tên tiến trình và cách hệ điều hành hiển thị tên có thể có giới hạn; với ứng dụng phức tạp cần xác định tiêu chí kiểm tra phù hợp.

## 21.5. Script menu đơn giản

```bash
#!/usr/bin/env bash

while true; do
    echo "===== MENU ====="
    echo "1. Xem ngay gio"
    echo "2. Xem nguoi dung"
    echo "3. Xem thu muc hien tai"
    echo "0. Thoat"

    read -r -p "Lua chon: " choice

    case "$choice" in
        1)
            date
            ;;
        2)
            whoami
            ;;
        3)
            pwd
            ;;
        0)
            echo "Tam biet!"
            break
            ;;
        *)
            echo "Lua chon khong hop le"
            ;;
    esac

    echo
done
```

Kiến thức sử dụng:

- `while`.
- `read`.
- `case`.
- `break`.

---

# 22. Lỗi thường gặp

## 22.1. Quên dấu cách trong cấu trúc điều kiện

Sai:

```bash
if[ "$a" -eq 1 ]; then
    echo "OK"
fi
```

Đúng:

```bash
if [ "$a" -eq 1 ]; then
    echo "OK"
fi
```

Bash cần khoảng trắng để phân tách từ khóa `if`, lệnh test và các đối số.

## 22.2. Có khoảng trắng quanh dấu gán

Sai:

```bash
name = "An"
```

Đúng:

```bash
name="An"
```

## 22.3. Quên quote biến

Sai:

```bash
rm $file
```

Đúng:

```bash
rm -- "$file"
```

## 22.4. Dùng dấu ngoặc đơn để mở rộng biến

```bash
name="An"
echo '$name'
```

Kết quả là chuỗi `$name`.

Muốn mở rộng biến:

```bash
echo "$name"
```

## 22.5. Nhầm `=` và `==`

Trong `[[ ]]`, có thể dùng `=` hoặc `==` để so sánh chuỗi:

```bash
[[ "$a" == "$b" ]]
```

Trong phép gán, chỉ dùng:

```bash
a="value"
```

## 22.6. Dùng `return` để trả chuỗi

`return` chỉ trả mã trạng thái. Muốn trả dữ liệu, thường dùng `printf` và command substitution.

## 22.7. Nhầm `/bin/sh` với Bash

Script dùng cú pháp Bash như `[[ ]]`, mảng hoặc process substitution không nhất thiết chạy được bằng `sh`.

Nếu cần Bash, hãy dùng shebang Bash và chạy bằng Bash:

```bash
#!/usr/bin/env bash
```

Không chạy script Bash bằng:

```bash
sh script.sh
```

## 22.8. Dùng `for x in $(cat file)`

Cách này làm hỏng dữ liệu chứa khoảng trắng, tab hoặc ký tự glob. Dùng vòng lặp `while IFS= read -r` để đọc theo dòng.

## 22.9. Quên xử lý tệp rỗng hoặc tên có khoảng trắng

Luôn kiểm tra trường hợp không có tệp phù hợp, tên tệp có khoảng trắng và dữ liệu đầu vào rỗng.

## 22.10. Tin rằng set -e xử lý tất cả lỗi

`set -e` có ngoại lệ và không thay thế thiết kế xử lý lỗi. Với thao tác quan trọng, hãy kiểm tra trạng thái rõ ràng.

---

# 23. Lộ trình thực hành và bài tập

Nên học theo thứ tự. Mỗi giai đoạn cần tự viết script, chạy thử với dữ liệu bình thường và kiểm tra các trường hợp biên.

## Giai đoạn 1: Làm quen Terminal và lệnh Linux

Kiến thức:

- Terminal, Shell, Bash.
- `pwd`, `ls`, `cd`.
- `mkdir`, `touch`, `cp`, `mv`, `rm`.
- `cat`, `less`, `head`, `tail`.
- Quyền tệp với `chmod`.

Bài tập:

1. Tạo cấu trúc thư mục `ShellPractice/Week1`.
2. Tạo 5 tệp `.txt` và 2 thư mục con.
3. Sao chép một tệp sang thư mục khác.
4. Đổi tên một tệp.
5. Xóa các tệp tạm.
6. Dùng `find` liệt kê tất cả tệp `.txt`.

## Giai đoạn 2: Biến, nhập xuất và điều kiện

Kiến thức:

- Biến và biến môi trường.
- `read`, `echo`, `printf`.
- `if`, `elif`, `else`.
- `[[ ]]`, `(( ))`.
- Exit status.

Bài tập:

1. Nhập tên và tuổi, in lời chào.
2. Nhập số nguyên, kiểm tra chẵn/lẻ.
3. Nhập ba số, tìm số lớn nhất.
4. Nhập đường dẫn, xác định tệp hay thư mục.
5. Viết chương trình đăng nhập giả lập với số lần nhập sai tối đa.
6. Kiểm tra số lượng tham số dòng lệnh.

## Giai đoạn 3: Vòng lặp và mảng

Kiến thức:

- `for`, `while`, `until`.
- `break`, `continue`.
- Mảng chỉ số và mảng kết hợp.

Bài tập:

1. In các số từ 1 đến N.
2. Tính tổng các số từ 1 đến N.
3. Kiểm tra số nguyên tố.
4. Tính giai thừa bằng vòng lặp.
5. Tìm giá trị lớn nhất trong một mảng.
6. Đếm số lần xuất hiện của từng từ trong danh sách đầu vào.
7. Viết menu chạy lặp đến khi người dùng chọn thoát.

## Giai đoạn 4: Hàm và tham số dòng lệnh

Kiến thức:

- Khai báo và gọi hàm.
- `local`, `return`.
- `$1`, `$2`, `$#`, `$@`.
- `shift`, `getopts`.

Bài tập:

1. Viết hàm tính tổng hai số.
2. Viết hàm kiểm tra số nguyên tố.
3. Viết hàm kiểm tra tệp tồn tại.
4. Viết script nhận thư mục nguồn và thư mục đích.
5. Viết chương trình hỗ trợ tùy chọn `-h`, `-v`, `-o`.
6. Viết script có hàm `usage`, `log_info`, `log_error`, `main`.

## Giai đoạn 5: Xử lý tệp và văn bản

Kiến thức:

- `find`, `grep`, `sort`, `uniq`, `cut`.
- `sed`, `awk`, `tr`, `wc`.
- Pipeline và redirection.
- NUL delimiter.

Bài tập:

1. Đếm số dòng, từ và byte của một tệp.
2. Tìm các dòng chứa `ERROR` trong log.
3. Đếm số lần xuất hiện của từng địa chỉ IP trong log mẫu.
4. Lọc các dòng có giá trị lớn hơn một ngưỡng.
5. Đổi phần mở rộng của một nhóm tệp.
6. Tìm các tệp lớn hơn 50 MB.
7. Viết script tìm tệp trùng tên trong cây thư mục.

## Giai đoạn 6: Process và tự động hóa

Kiến thức:

- PID, process, job.
- `ps`, `pgrep`, `kill`, `wait`.
- `trap`, `mktemp`.
- Cron và lịch chạy tự động.

Bài tập:

1. Viết script kiểm tra một process có đang chạy.
2. Chạy một tác vụ nền và chờ hoàn thành.
3. Tạo tệp tạm và tự động xóa khi script kết thúc.
4. Viết script giám sát dung lượng ổ đĩa.
5. Tạo bản sao lưu có timestamp.
6. Lên lịch chạy script sao lưu bằng cron.

## Giai đoạn 7: Dự án tổng hợp

### Dự án 1: Personal File Manager

Chức năng:

- Liệt kê tệp và thư mục.
- Tạo thư mục.
- Đổi tên tệp.
- Sao chép và di chuyển.
- Xóa tệp sau khi xác nhận.
- Tìm kiếm theo tên và phần mở rộng.
- Ghi log thao tác.

Kiến thức áp dụng: biến, hàm, `case`, vòng lặp, kiểm tra tệp, tham số dòng lệnh.

### Dự án 2: System Monitoring Script

Chức năng:

- Kiểm tra dung lượng ổ đĩa.
- Hiển thị RAM và CPU ở mức cơ bản.
- Kiểm tra process quan trọng.
- Ghi kết quả vào log.
- Cảnh báo khi vượt ngưỡng.
- Có thể chạy định kỳ bằng cron.

Kiến thức áp dụng: `df`, `free`, `ps`, `awk`, `grep`, điều kiện, log, cron.

### Dự án 3: Automated Backup Tool

Chức năng:

- Nhận thư mục nguồn và thư mục đích.
- Tạo archive có timestamp.
- Kiểm tra kết quả sao lưu.
- Ghi log.
- Giữ lại N bản sao lưu gần nhất.
- Dọn dẹp tệp tạm khi có lỗi.
- Hỗ trợ tùy chọn dòng lệnh.

Kiến thức áp dụng: `tar`, `find`, `getopts`, `trap`, `mktemp`, xử lý lỗi.

### Dự án 4: Log Analyzer

Chức năng:

- Đọc tệp log.
- Đếm số lượng ERROR, WARNING, INFO.
- Lọc theo khoảng thời gian hoặc từ khóa.
- Thống kê IP xuất hiện nhiều nhất.
- Xuất báo cáo dạng văn bản hoặc CSV đơn giản.

Kiến thức áp dụng: `grep`, `awk`, `sort`, `uniq`, pipeline, tham số dòng lệnh.

### Dự án 5: Deployment Helper

Chức năng:

- Kiểm tra các công cụ cần thiết.
- Kiểm tra cấu hình môi trường.
- Sao lưu phiên bản cũ.
- Tải hoặc lấy mã nguồn.
- Chạy lệnh build/test.
- Triển khai ứng dụng.
- Ghi log và báo lỗi khi một bước thất bại.

Kiến thức áp dụng: hàm, exit status, `set`, `trap`, biến môi trường, process, log.

---

# 24. Bảng tổng kết lệnh thường dùng

## 24.1. Điều hướng và quản lý tệp

| Lệnh | Công dụng |
|---|---|
| `pwd` | Xem thư mục hiện tại |
| `ls -la` | Liệt kê chi tiết |
| `cd path` | Chuyển thư mục |
| `mkdir -p path` | Tạo thư mục |
| `touch file` | Tạo tệp |
| `cp source dest` | Sao chép |
| `mv source dest` | Di chuyển/đổi tên |
| `rm file` | Xóa tệp |
| `find path ...` | Tìm tệp |
| `stat file` | Xem metadata |

## 24.2. Xử lý văn bản

| Lệnh | Công dụng |
|---|---|
| `cat` | In nội dung |
| `less` | Xem nội dung theo trang |
| `head` | Xem đầu tệp |
| `tail` | Xem cuối tệp |
| `grep` | Tìm dòng theo mẫu |
| `sort` | Sắp xếp |
| `uniq` | Xử lý dòng trùng liền kề |
| `wc` | Đếm dòng/từ/byte |
| `cut` | Cắt trường |
| `tr` | Biến đổi ký tự |
| `sed` | Biến đổi văn bản theo dòng |
| `awk` | Xử lý văn bản theo trường |
| `tee` | Vừa hiển thị vừa ghi |
| `xargs` | Chuyển stdin thành đối số |

## 24.3. Process và hệ thống

| Lệnh | Công dụng |
|---|---|
| `ps aux` | Liệt kê process |
| `top` | Theo dõi process |
| `pgrep` | Tìm PID theo tên/mẫu |
| `kill` | Gửi tín hiệu tới process |
| `jobs` | Xem job của Shell |
| `fg` | Đưa job lên foreground |
| `bg` | Tiếp tục job ở background |
| `wait` | Chờ process con |
| `df -h` | Xem dung lượng filesystem |
| `du -sh` | Xem dung lượng thư mục |
| `free -h` | Xem bộ nhớ |
| `uname -a` | Thông tin kernel/hệ thống |
| `uptime` | Thời gian hoạt động và tải hệ thống |

## 24.4. Cú pháp Bash cần nhớ

| Cú pháp | Công dụng |
|---|---|
| `$var` hoặc `${var}` | Mở rộng biến |
| `$(command)` | Lấy stdout của lệnh |
| `$((expression))` | Tính toán số nguyên |
| `[[ condition ]]` | Kiểm tra điều kiện kiểu Bash |
| `(( expression ))` | Ngữ cảnh số học |
| `"$@"` | Tất cả đối số, giữ ranh giới từng đối số |
| `"$array[@]"` | Tất cả phần tử mảng |
| `command1 && command2` | Chạy lệnh 2 nếu lệnh 1 thành công |
| `command1 || command2` | Chạy lệnh 2 nếu lệnh 1 thất bại |
| `command1 \| command2` | Pipeline |
| `> file` | Ghi đè stdout |
| `>> file` | Ghi nối stdout |
| `2> file` | Chuyển hướng stderr |
| `< file` | Lấy stdin từ tệp |
| `&` | Chạy lệnh nền |
| `source file` | Nạp tệp vào Shell hiện tại |
| `trap` | Xử lý tín hiệu/sự kiện Shell |

---

# Kết luận

Bash là công cụ quan trọng trong môi trường Linux, quản trị hệ thống và DevOps. Để sử dụng Bash hiệu quả, không chỉ cần nhớ cú pháp mà còn phải hiểu cách Shell xử lý biến, quote, đối số, luồng dữ liệu, process và mã thoát.

Thứ tự học được khuyến nghị:

1. Thành thạo các lệnh Linux cơ bản.
2. Nắm chắc biến, quote, nhập xuất và điều kiện.
3. Thành thạo vòng lặp, mảng và hàm.
4. Biết xử lý tham số dòng lệnh và tệp.
5. Kết hợp pipeline với các công cụ xử lý văn bản.
6. Học process, signal, trap và xử lý lỗi.
7. Viết các script tự động hóa có kiểm tra đầu vào, log, cleanup và tài liệu sử dụng.
8. Dùng ShellCheck và kiểm thử với nhiều trường hợp đầu vào.

Một nguyên tắc quan trọng: Bash phù hợp nhất để kết nối các lệnh hệ thống và tự động hóa quy trình. Với ứng dụng có cấu trúc dữ liệu phức tạp, xử lý logic lớn hoặc cần kiểm thử sâu, nên cân nhắc Python, Go hoặc một ngôn ngữ lập trình phù hợp hơn.
