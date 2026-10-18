# Tài liệu tổng hợp: Lập trình Socket với Windows Forms trong C

> Tài liệu học từ nền tảng đến triển khai ứng dụng Client--Server bằng
> C# Windows Forms. Ví dụ sử dụng `TcpListener`, `TcpClient`,
> `NetworkStream`, `Socket`, `async/await` và các nguyên tắc xử lý lỗi
> thường gặp.
>
> **Môi trường gợi ý:** Visual Studio, C#, Windows Forms trên .NET 8
> hoặc .NET Framework 4.8. Một số API có khác biệt nhỏ tùy phiên bản
> .NET.

## Mục lục

1.  Tổng quan về Socket
2.  Kiến thức mạng cần nắm
3.  TCP và UDP
4.  Mô hình Client--Server
5.  Các lớp mạng trong .NET
6.  Thiết kế giao thức ứng dụng
7.  Xây dựng TCP Server
8.  Xây dựng TCP Client
9.  Tích hợp Socket vào Windows Forms
10. Gửi và nhận dữ liệu bất đồng bộ
11. Nhiều Client và quản lý kết nối
12. Gửi chuỗi, JSON và dữ liệu nhị phân
13. Ví dụ ứng dụng chat nhiều Client
14. Ví dụ truyền file
15. Xử lý lỗi và tình huống biên
16. Bảo mật
17. Kiểm thử và gỡ lỗi
18. Các lỗi phổ biến
19. Tối ưu và kiến trúc chương trình
20. Bài tập thực hành
21. Checklist hoàn thiện đồ án
22. Bảng tra cứu nhanh

------------------------------------------------------------------------

## 1. Tổng quan về Socket

### 1.1. Socket là gì?

Socket là một điểm cuối giao tiếp cho phép hai tiến trình trao đổi dữ
liệu qua mạng hoặc trên cùng một máy. Trong ứng dụng C# Windows Forms,
Socket thường được dùng để xây dựng:

-   Ứng dụng chat.
-   Phần mềm gửi và nhận file.
-   Hệ thống quản lý Client--Server.
-   Ứng dụng giám sát thiết bị.
-   Trò chơi nhiều người chơi.
-   Công cụ trao đổi dữ liệu trong mạng LAN.

Socket không tự định nghĩa ý nghĩa nghiệp vụ của dữ liệu. Nó cung cấp cơ
chế gửi/nhận byte; ứng dụng phải quy định byte đó đại diện cho tin nhắn,
lệnh, file hay loại dữ liệu nào.

### 1.2. Phân biệt các khái niệm

-   **IP address:** xác định giao diện mạng hoặc máy đích trong mạng IP.
-   **Port:** số cổng giúp hệ điều hành chuyển dữ liệu đến đúng ứng
    dụng/dịch vụ.
-   **Protocol:** quy tắc giao tiếp, ví dụ TCP, UDP hoặc giao thức do
    ứng dụng tự thiết kế.
-   **Socket endpoint:** thường là địa chỉ IP kết hợp với port.
-   **Client:** bên chủ động kết nối đến dịch vụ.
-   **Server:** bên mở cổng chờ các Client kết nối.
-   **Payload:** phần dữ liệu thực tế mà ứng dụng muốn truyền.
-   **Packet:** đơn vị dữ liệu ở một lớp mạng. Không nên đồng nhất
    packet mạng với một lần gọi `Send` hay một lần gọi `Receive`.

### 1.3. Luồng giao tiếp điển hình

1.  Server tạo socket hoặc `TcpListener`.
2.  Server bind vào địa chỉ IP và port.
3.  Server bắt đầu lắng nghe.
4.  Client kết nối đến IP và port của Server.
5.  Hai bên gửi và nhận dữ liệu.
6.  Một bên ngắt kết nối hoặc xảy ra lỗi.
7.  Hai bên giải phóng tài nguyên.

### 1.4. Socket không đồng nghĩa với giao thức ứng dụng

TCP chỉ cung cấp một luồng byte đáng tin cậy, có thứ tự. TCP không biết
đâu là ranh giới của một tin nhắn chat. Nếu ứng dụng cần gửi nhiều tin
nhắn, cần xây dựng quy tắc phân tách tin nhắn, chẳng hạn:

-   Mỗi tin nhắn kết thúc bằng ký tự xuống dòng.
-   Mỗi tin nhắn có độ dài cố định.
-   Phần đầu chứa độ dài payload.
-   Dùng một định dạng thông điệp có cấu trúc.

Đây là một trong những nguyên tắc quan trọng nhất khi lập trình Socket.

------------------------------------------------------------------------

## 2. Kiến thức mạng cần nắm

### 2.1. IPv4 và IPv6

IPv4 thường có dạng `192.168.1.10`. IPv6 dài hơn và được biểu diễn theo
hệ thập lục phân.

Các địa chỉ thường dùng khi phát triển:

-   `127.0.0.1`: địa chỉ loopback IPv4, kết nối về chính máy đang chạy
    chương trình.
-   `localhost`: tên máy cục bộ, thường phân giải thành loopback IPv4
    hoặc IPv6.
-   `0.0.0.0`: địa chỉ bind có nghĩa lắng nghe trên mọi giao diện IPv4
    phù hợp; đây không phải địa chỉ đích thông thường để Client kết nối.
-   `::1`: loopback IPv6.
-   `::`: địa chỉ bind trên mọi giao diện IPv6, tùy cấu hình có thể liên
    quan đến IPv4-mapped IPv6.

Khi Client chạy trên máy khác, không dùng `127.0.0.1` để kết nối đến
Server. Hãy dùng địa chỉ LAN hoặc địa chỉ có thể định tuyến đến máy
Server.

### 2.2. Port

Port là số nguyên 16 bit, từ 0 đến 65535. Các port thấp thường được dành
cho dịch vụ hệ thống; khi làm bài tập có thể chọn một port cao chưa bị
sử dụng, ví dụ `5000` hoặc `9000`.

Lưu ý:

-   Port phải khớp giữa Client và Server.
-   Port có thể đang bị chương trình khác chiếm.
-   Tường lửa có thể chặn kết nối đến.
-   Không nên mặc định rằng một port cụ thể luôn trống trên mọi máy.
-   Port TCP và port UDP là hai không gian riêng; cùng số port có thể
    được dùng cho TCP và UDP.

### 2.3. LAN, WAN và NAT

-   **LAN:** mạng cục bộ như mạng gia đình hoặc phòng máy.
-   **WAN/Internet:** mạng diện rộng.
-   **NAT:** thiết bị mạng có thể chuyển đổi địa chỉ riêng sang địa chỉ
    công cộng.
-   **Firewall:** quy tắc cho phép hoặc chặn lưu lượng mạng.

Ứng dụng hoạt động trên cùng máy chưa chắc hoạt động trên hai máy khác
nhau. Khi kiểm thử LAN, cần kiểm tra địa chỉ IP, port, tường lửa và cấu
hình router nếu có.

### 2.4. DNS

DNS phân giải tên miền thành địa chỉ IP. Trong C#, có thể dùng
`Dns.GetHostAddressesAsync()` để tra cứu địa chỉ. DNS có thể trả về
nhiều địa chỉ IPv4/IPv6, nên chương trình thực tế cần xử lý khả năng một
địa chỉ không kết nối được.

------------------------------------------------------------------------

## 3. TCP và UDP

### 3.1. TCP

TCP là giao thức hướng kết nối, cung cấp luồng byte có thứ tự và cơ chế
truyền lại để tăng độ tin cậy.

Đặc điểm:

-   Cần thiết lập kết nối trước khi trao đổi dữ liệu.
-   Dữ liệu đến theo thứ tự byte đã gửi.
-   Không bảo toàn ranh giới giữa các lần gửi.
-   Có cơ chế kiểm soát luồng và tắc nghẽn.
-   Phù hợp với chat, truyền file, đăng nhập, truy vấn dữ liệu.

TCP không bảo đảm ứng dụng ở đầu kia đã xử lý nghiệp vụ thành công chỉ
vì dữ liệu đã được gửi thành công ở phía gửi. Nếu cần xác nhận nghiệp
vụ, hãy định nghĩa thông điệp ACK ở tầng ứng dụng.

### 3.2. UDP

UDP gửi datagram độc lập, không yêu cầu thiết lập kết nối như TCP. UDP
không bảo đảm datagram sẽ đến, không bảo đảm thứ tự và không tự truyền
lại khi mất dữ liệu.

Phù hợp với các tình huống chấp nhận mất gói hoặc cần độ trễ thấp, ví dụ
một số ứng dụng thời gian thực, khám phá thiết bị trong LAN hoặc
telemetry. Nếu cần độ tin cậy, ứng dụng phải tự xây dựng cơ chế phù hợp.

### 3.3. So sánh

  Tiêu chí               TCP                   UDP
  ---------------------- --------------------- -----------------------------------
  Kiểu giao tiếp         Hướng kết nối         Datagram
  Bảo đảm thứ tự         Có trong luồng byte   Không
  Tự truyền lại          Có                    Không
  Ranh giới thông điệp   Không bảo toàn        Mỗi datagram là một đơn vị
  Overhead               Thường cao hơn        Thường thấp hơn
  Ví dụ                  Chat, truyền file     Discovery, dữ liệu thời gian thực

### 3.4. Chọn giao thức nào?

Với đồ án Windows Forms như chat hoặc gửi file, nên bắt đầu bằng TCP vì
dễ xây dựng giao tiếp đáng tin cậy. Chỉ chọn UDP khi hiểu rõ các đặc
tính mất gói, trùng lặp, sai thứ tự và cách ứng dụng sẽ xử lý chúng.

------------------------------------------------------------------------

## 4. Mô hình Client--Server

### 4.1. Server làm gì?

Server thường:

1.  Chọn địa chỉ IP và port để lắng nghe.
2.  Chấp nhận kết nối đến.
3.  Tạo trạng thái riêng cho mỗi Client.
4.  Đọc dữ liệu từ từng kết nối.
5.  Phân tích và xử lý thông điệp.
6.  Gửi phản hồi hoặc chuyển tiếp dữ liệu đến Client khác.
7.  Dọn dẹp khi Client ngắt kết nối.

### 4.2. Client làm gì?

Client thường:

1.  Nhập địa chỉ Server và port.
2.  Thiết lập kết nối.
3.  Gửi yêu cầu.
4.  Nhận phản hồi trong khi giao diện vẫn hoạt động.
5.  Hiển thị trạng thái kết nối.
6.  Ngắt kết nối và giải phóng tài nguyên.

### 4.3. Ví dụ kiến trúc chat

``` text
Client A ─────┐
              │
Client B ─────┼──── Server
              │       ├── Quản lý kết nối
Client C ─────┘       ├── Nhận thông điệp
                      └── Phát tin đến Client
```

Trong mô hình chat, Server nên là nơi quản lý danh sách kết nối và quyết
định thông điệp nào được phát đến ai. Client không nên tự giả định rằng
tất cả Client đều luôn đang kết nối.

------------------------------------------------------------------------

## 5. Các lớp mạng trong .NET

Có hai hướng phổ biến: dùng lớp mức cao như `TcpClient`/`TcpListener`,
hoặc dùng lớp nền tảng `Socket`.

### 5.1. `TcpListener`

Dùng ở phía Server để mở port và chờ kết nối TCP.

Các thao tác thường gặp:

-   `new TcpListener(IPAddress, port)`
-   `Start()`
-   `AcceptTcpClientAsync()`
-   `Stop()`

`AcceptTcpClientAsync()` bất đồng bộ chờ một Client mới. Không nên chạy
thao tác chờ này trên UI thread.

### 5.2. `TcpClient`

Đại diện cho kết nối TCP phía Client hoặc một kết nối đã được Server
chấp nhận.

Các thao tác thường gặp:

-   `ConnectAsync(host, port)`
-   `GetStream()`
-   `Close()` hoặc `Dispose()`

Sau khi kết nối, `GetStream()` cung cấp `NetworkStream` để đọc và ghi
byte.

### 5.3. `NetworkStream`

Một luồng đọc/ghi dữ liệu trên kết nối TCP.

-   `ReadAsync(...)`: đọc dữ liệu.
-   `WriteAsync(...)`: ghi dữ liệu.
-   `FlushAsync()`: có thể dùng với một số luồng, nhưng không thay thế
    cơ chế phân khung thông điệp.
-   `CanRead`, `CanWrite`: kiểm tra khả năng đọc/ghi.

Một lần đọc có thể trả về ít byte hơn số byte mong đợi. Giá trị trả về
bằng `0` khi đọc TCP thường cho biết phía bên kia đã đóng luồng một cách
bình thường.

### 5.4. `Socket`

Lớp mức thấp hơn, cho phép điều khiển chi tiết hơn. Có thể dùng với TCP
(`SocketType.Stream`, `ProtocolType.Tcp`) hoặc UDP (`SocketType.Dgram`,
`ProtocolType.Udp`).

Dùng `Socket` khi cần kiểm soát chi tiết endpoint, tùy chọn socket hoặc
kiến trúc đặc biệt. Với bài tập Client--Server đầu tiên,
`TcpClient`/`TcpListener` thường dễ học hơn.

### 5.5. Các kiểu liên quan

-   `IPAddress`: biểu diễn địa chỉ IP.
-   `IPEndPoint`: kết hợp IP và port.
-   `Dns`: tra cứu tên máy.
-   `Encoding`: chuyển đổi giữa chuỗi và byte.
-   `CancellationToken`: báo yêu cầu hủy thao tác bất đồng bộ.
-   `Task`: biểu diễn công việc bất đồng bộ.
-   `ConcurrentDictionary`: có thể dùng để quản lý kết nối đồng thời;
    vẫn cần thiết kế vòng đời và đồng bộ hóa phù hợp.

------------------------------------------------------------------------

## 6. Thiết kế giao thức ứng dụng

Đây là phần quyết định ứng dụng có hoạt động ổn định khi nhiều thông
điệp được truyền liên tục hay không.

### 6.1. Vì sao cần framing?

Giả sử Client gọi `WriteAsync` ba lần để gửi:

``` text
Xin chào
Bạn khỏe không?
Tạm biệt
```

TCP chỉ truyền một luồng byte. Server có thể đọc được:

-   Một lần đọc chứa cả ba thông điệp.
-   Mỗi lần đọc chỉ chứa một phần của một thông điệp.
-   Một thông điệp bị chia qua nhiều lần đọc.

Do đó, không được giả định một lần `ReadAsync` tương ứng đúng một lần
`WriteAsync`.

### 6.2. Cách 1: kết thúc bằng ký tự xuống dòng

Ví dụ mỗi thông điệp là một dòng UTF-8 và kết thúc bằng `\n`. Có thể
dùng `StreamReader.ReadLineAsync()` và `StreamWriter.WriteLineAsync()`.

Ưu điểm: - Dễ hiểu và dễ kiểm tra. - Phù hợp với thông điệp văn bản đơn
giản.

Nhược điểm: - Payload không được chứa newline thô nếu không có cơ chế
escape. - Cần giới hạn độ dài dòng để tránh tiêu thụ bộ nhớ không giới
hạn. - Cả hai bên phải thống nhất encoding và quy tắc xuống dòng.

### 6.3. Cách 2: tiền tố độ dài

Một định dạng thường dùng:

``` text
[4 byte độ dài][N byte payload]
```

Ví dụ: 4 byte đầu biểu diễn số byte của payload, tiếp theo là đúng số
byte đó. Hai bên phải thống nhất thứ tự byte, chẳng hạn big-endian.

Ưu điểm: - Hỗ trợ payload nhị phân và thông điệp có newline. - Có thể
xác định chính xác ranh giới thông điệp.

Nhược điểm: - Phải đọc đủ phần header và đủ số byte payload. - Phải xác
thực độ dài tối đa trước khi cấp phát bộ nhớ. - Cần thống nhất định dạng
header giữa Client và Server.

### 6.4. Quy tắc giao thức nên ghi rõ

Một giao thức tốt xác định:

-   Phiên bản giao thức.
-   Encoding.
-   Kiểu thông điệp.
-   Ranh giới thông điệp.
-   Giới hạn kích thước.
-   Trường bắt buộc và tùy chọn.
-   Quy tắc phản hồi và lỗi.
-   Cách xác thực người gửi.
-   Timeout và hành vi khi mất kết nối.

### 6.5. Ví dụ thông điệp JSON

Một thông điệp có thể biểu diễn bằng JSON:

``` json
{
  "type": "chat",
  "sender": "An",
  "content": "Xin chào",
  "timestamp": "2026-10-18T12:00:00Z"
}
```

JSON chỉ là định dạng payload, không tự giải quyết ranh giới thông điệp
trên TCP. Vẫn cần framing, chẳng hạn mỗi JSON một dòng hoặc dùng tiền tố
độ dài.

------------------------------------------------------------------------

## 7. Xây dựng TCP Server cơ bản

Ví dụ sau dùng giao thức một dòng văn bản. Server nhận một dòng từ
Client và phản hồi một dòng. Đây là ví dụ học tập, chưa phải server
production.

``` csharp
using System;
using System.IO;
using System.Net;
using System.Net.Sockets;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

public sealed class SimpleTcpServer
{
    private readonly int _port;
    private TcpListener? _listener;
    private CancellationTokenSource? _cts;

    public SimpleTcpServer(int port)
    {
        _port = port;
    }

    public async Task StartAsync(CancellationToken cancellationToken)
    {
        _cts = CancellationTokenSource.CreateLinkedTokenSource(cancellationToken);
        _listener = new TcpListener(IPAddress.Any, _port);
        _listener.Start();

        try
        {
            while (!_cts.IsCancellationRequested)
            {
                TcpClient client;

                try
                {
                    client = await _listener.AcceptTcpClientAsync(_cts.Token);
                }
                catch (OperationCanceledException)
                {
                    break;
                }
                catch (ObjectDisposedException)
                {
                    break;
                }

                // Mỗi Client được xử lý trong một task riêng.
                _ = HandleClientAsync(client, _cts.Token);
            }
        }
        finally
        {
            _listener.Stop();
            _listener = null;
        }
    }

    private static async Task HandleClientAsync(
        TcpClient client,
        CancellationToken cancellationToken)
    {
        using (client)
        using (NetworkStream stream = client.GetStream())
        using (var reader = new StreamReader(
            stream, new UTF8Encoding(false), false, 1024, leaveOpen: true))
        using (var writer = new StreamWriter(
            stream, new UTF8Encoding(false), 1024, leaveOpen: true)
        {
            AutoFlush = true
        })
        {
            try
            {
                while (!cancellationToken.IsCancellationRequested)
                {
                    string? message = await reader.ReadLineAsync(cancellationToken);

                    if (message == null)
                    {
                        // Client đã đóng kết nối.
                        break;
                    }

                    await writer.WriteLineAsync($"Server nhận: {message}");
                }
            }
            catch (IOException)
            {
                // Kết nối có thể bị ngắt đột ngột.
            }
            catch (SocketException)
            {
                // Lỗi mạng/socket.
            }
            catch (OperationCanceledException)
            {
                // Server đang dừng.
            }
        }
    }

    public void Stop()
    {
        _cts?.Cancel();
        _listener?.Stop();
    }
}
```

### 7.1. Giải thích các điểm chính

-   `IPAddress.Any` cho phép Server lắng nghe trên các giao diện IPv4
    phù hợp. Nếu chỉ muốn thử trên máy hiện tại, có thể bind
    `IPAddress.Loopback`.
-   `Start()` bắt đầu lắng nghe.
-   `AcceptTcpClientAsync()` chờ Client mới mà không chặn UI thread.
-   Mỗi Client được chuyển cho `HandleClientAsync`.
-   `using` đảm bảo các tài nguyên được giải phóng khi xử lý kết thúc.
-   `ReadLineAsync()` đọc theo giao thức dòng; phía Client phải gửi
    newline.
-   `message == null` biểu thị luồng đã kết thúc.
-   `Stop()` yêu cầu hủy và dừng listener để giải phóng thao tác chờ.

**Lưu ý về vòng đời:** Ví dụ tối giản trên minh họa ý tưởng. Với ứng
dụng thực tế, cần theo dõi các task xử lý Client, chờ chúng kết thúc khi
dừng Server, giới hạn số kết nối, đặt timeout và ghi log lỗi. Không nên
bỏ qua task mà không quản lý trong hệ thống lớn.

> API nhận `CancellationToken` trực tiếp trong các phương thức đọc dòng
> phụ thuộc phiên bản .NET. Nếu dùng .NET Framework cũ, hãy kiểm tra
> overload có sẵn và thiết kế hủy bằng cách đóng stream/socket hoặc dùng
> API tương thích.

------------------------------------------------------------------------

## 8. Xây dựng TCP Client cơ bản

Ví dụ Client kết nối đến Server, gửi một dòng và đọc phản hồi. Đây cũng
là giao thức dòng.

``` csharp
using System;
using System.IO;
using System.Net.Sockets;
using System.Text;
using System.Threading.Tasks;

public sealed class SimpleTcpClient : IDisposable
{
    private TcpClient? _client;
    private StreamReader? _reader;
    private StreamWriter? _writer;

    public async Task ConnectAsync(string host, int port)
    {
        _client = new TcpClient();
        await _client.ConnectAsync(host, port);

        NetworkStream stream = _client.GetStream();

        _reader = new StreamReader(
            stream, new UTF8Encoding(false), false, 1024, leaveOpen: true);

        _writer = new StreamWriter(
            stream, new UTF8Encoding(false), 1024, leaveOpen: true)
        {
            AutoFlush = true
        };
    }

    public async Task<string?> SendAndReceiveAsync(string message)
    {
        if (_writer == null || _reader == null)
            throw new InvalidOperationException("Client chưa kết nối.");

        await _writer.WriteLineAsync(message);
        return await _reader.ReadLineAsync();
    }

    public void Dispose()
    {
        _reader?.Dispose();
        _writer?.Dispose();
        _client?.Dispose();
    }
}
```

### 8.1. Những điểm cần lưu ý

-   `host` có thể là `127.0.0.1`, tên máy hoặc địa chỉ IP của Server.
-   `ConnectAsync` có thể thất bại do sai IP/port, Server chưa chạy hoặc
    firewall.
-   `SendAndReceiveAsync` ở ví dụ này giả định mô hình
    request--response: mỗi yêu cầu có một phản hồi tương ứng.
-   Với chat hai chiều, cần vòng đọc riêng chạy liên tục, không chỉ đọc
    sau mỗi lần gửi.
-   Nếu nhiều thao tác gửi đồng thời, cần đồng bộ việc ghi để các dòng
    không xen kẽ sai giao thức.

------------------------------------------------------------------------

## 9. Tích hợp Socket vào Windows Forms

### 9.1. Vì sao không được chặn UI thread?

Windows Forms dùng UI thread để xử lý sự kiện, vẽ giao diện và phản hồi
thao tác người dùng. Nếu gọi một thao tác mạng đồng bộ có thể chờ lâu,
cửa sổ sẽ đơ và hiển thị "Not Responding".

Tránh các thao tác chặn như:

-   `Connect()` đồng bộ trong event handler.
-   `AcceptTcpClient()` trên UI thread.
-   `Read()` chờ dữ liệu trên UI thread.
-   `Thread.Sleep()` trong event handler.
-   Dùng `.Wait()` hoặc `.Result` với task mạng trên UI thread.

Ưu tiên `async`/`await` với API bất đồng bộ.

### 9.2. Gợi ý bố cục Form Server

Có thể thiết kế Form với:

-   `txtPort`: nhập port.
-   `btnStart`: bắt đầu Server.
-   `btnStop`: dừng Server.
-   `lstClients`: danh sách Client.
-   `txtLog`: nhật ký hoạt động.
-   `lblStatus`: trạng thái Server.

### 9.3. Gợi ý bố cục Form Client

-   `txtHost`: địa chỉ Server.
-   `txtPort`: port.
-   `btnConnect`: kết nối.
-   `btnDisconnect`: ngắt kết nối.
-   `txtMessage`: nội dung tin nhắn.
-   `btnSend`: gửi.
-   `lstMessages` hoặc `txtChat`: hiển thị tin nhắn.
-   `lblStatus`: trạng thái kết nối.

### 9.4. Ví dụ event handler bất đồng bộ

Giả sử Form có `txtHost`, `txtPort`, `btnConnect`, `btnSend`,
`txtMessage`, `txtLog`. Các event handler nên có dạng:

``` csharp
private SimpleTcpClient? _client;

private async void btnConnect_Click(object sender, EventArgs e)
{
    btnConnect.Enabled = false;

    try
    {
        string host = txtHost.Text.Trim();

        if (!int.TryParse(txtPort.Text, out int port) ||
            port < 1 || port > 65535)
        {
            MessageBox.Show("Port không hợp lệ.");
            return;
        }

        _client?.Dispose();
        _client = new SimpleTcpClient();

        await _client.ConnectAsync(host, port);
        txtLog.AppendText("Đã kết nối đến Server." + Environment.NewLine);
    }
    catch (SocketException ex)
    {
        MessageBox.Show($"Không thể kết nối: {ex.Message}");
        _client?.Dispose();
        _client = null;
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Lỗi: {ex.Message}");
    }
    finally
    {
        btnConnect.Enabled = true;
    }
}

private async void btnSend_Click(object sender, EventArgs e)
{
    if (_client == null)
    {
        MessageBox.Show("Bạn chưa kết nối.");
        return;
    }

    string message = txtMessage.Text.Trim();
    if (message.Length == 0) return;

    btnSend.Enabled = false;

    try
    {
        string? response = await _client.SendAndReceiveAsync(message);
        txtLog.AppendText($"Server: {response}{Environment.NewLine}");
        txtMessage.Clear();
    }
    catch (Exception ex)
    {
        txtLog.AppendText($"Lỗi gửi/nhận: {ex.Message}{Environment.NewLine}");
    }
    finally
    {
        btnSend.Enabled = true;
    }
}
```

Đây là ví dụ minh họa tích hợp, không phải thiết kế chat hoàn chỉnh. Cần
thêm `using System.Net.Sockets;` ở đầu file. Hãy xử lý đóng kết nối khi
Form đóng, tránh tạo nhiều kết nối trùng lặp và tránh cho phép nhiều
thao tác gửi đồng thời trên cùng một stream.

### 9.5. Cập nhật giao diện từ thread khác

Nếu vòng nhận dữ liệu chạy trên thread không phải UI thread, không được
tùy ý sửa trực tiếp control WinForms. Các cách phổ biến:

-   `Control.Invoke(...)` để thực thi trên UI thread.
-   `Control.BeginInvoke(...)` để đưa cập nhật vào hàng đợi UI.
-   Nếu phiên bản .NET hỗ trợ, có thể dùng `InvokeAsync` với cách dùng
    phù hợp.

Ví dụ tương thích rộng:

``` csharp
private void AddLog(string message)
{
    if (txtLog.InvokeRequired)
    {
        txtLog.BeginInvoke(new Action(() => AddLog(message)));
        return;
    }

    txtLog.AppendText(message + Environment.NewLine);
}
```

Không gọi `Invoke` sau khi Form/control đã bị dispose. Khi Form đóng,
cần hủy vòng nhận và giải phóng socket trước hoặc trong quy trình đóng.

------------------------------------------------------------------------

## 10. Gửi và nhận dữ liệu bất đồng bộ

### 10.1. `async` và `await`

-   `async` cho phép phương thức dùng `await`.
-   `await` chờ một thao tác bất đồng bộ mà không chặn thread trong lúc
    chờ I/O.
-   `Task` đại diện cho công việc bất đồng bộ.
-   `CancellationToken` truyền yêu cầu hủy đến thao tác có hỗ trợ token.

`async` không có nghĩa là tự động tạo một thread mới. Với I/O mạng, lợi
ích chính là không giữ UI thread trong lúc chờ dữ liệu.

### 10.2. Vòng nhận dữ liệu

Ứng dụng chat thường cần một vòng đọc liên tục sau khi kết nối:

``` csharp
private async Task ReceiveLoopAsync(
    StreamReader reader,
    CancellationToken cancellationToken)
{
    try
    {
        while (!cancellationToken.IsCancellationRequested)
        {
            string? message = await reader.ReadLineAsync(cancellationToken);

            if (message == null)
                break;

            AddLog(message);
        }
    }
    catch (OperationCanceledException)
    {
        // Được yêu cầu dừng.
    }
    catch (IOException ex)
    {
        AddLog($"Kết nối bị gián đoạn: {ex.Message}");
    }
}
```

Nếu nhắm đến .NET Framework không có overload
`ReadLineAsync(CancellationToken)`, cần dùng overload tương thích và hủy
bằng cách đóng stream/socket hoặc cơ chế phù hợp. Khi hủy bằng cách đóng
socket, thao tác đọc thường kết thúc bằng kết quả EOF hoặc exception tùy
tình huống.

### 10.3. Tránh đọc từ cùng một stream ở nhiều nơi

Không nên có hai vòng đọc cùng lúc trên một `NetworkStream` hoặc cùng
một `StreamReader`, vì dữ liệu có thể bị chia cho các nơi đọc và làm
hỏng logic giao thức.

Một thiết kế thường dùng:

-   Một vòng đọc duy nhất cho mỗi kết nối.
-   Một đường gửi có đồng bộ hóa nếu có nhiều nơi gửi.
-   Một nơi chịu trách nhiệm đóng kết nối.
-   Các thông điệp nhận được được chuyển tới tầng xử lý hoặc UI.

### 10.4. Đồng bộ gửi dữ liệu

Nếu nhiều event cùng ghi lên một stream, hãy dùng `SemaphoreSlim` hoặc
một hàng đợi gửi để tuần tự hóa thao tác ghi.

Ví dụ ý tưởng:

``` csharp
private readonly SemaphoreSlim _sendLock = new(1, 1);

private async Task SendLineAsync(
    StreamWriter writer,
    string message,
    CancellationToken cancellationToken)
{
    await _sendLock.WaitAsync(cancellationToken);
    try
    {
        await writer.WriteLineAsync(message);
    }
    finally
    {
        _sendLock.Release();
    }
}
```

Nếu dùng phiên bản .NET cũ, chữ ký `WriteLineAsync` và hỗ trợ token có
thể khác. Chốt phiên bản .NET trước khi chọn API.

------------------------------------------------------------------------

## 11. Nhiều Client và quản lý kết nối

### 11.1. Chấp nhận nhiều Client

Server cần tiếp tục gọi `AcceptTcpClientAsync()` trong vòng lặp. Mỗi
Client có thể được xử lý bằng một task riêng. `async/await` giúp I/O chờ
mạng không phải chiếm một thread trong toàn bộ thời gian chờ.

Tuy nhiên, tạo task không có nghĩa là có thể bỏ qua quản lý vòng đời.
Server cần biết task nào đang chạy, task nào lỗi và khi nào tất cả đã
dừng.

### 11.2. Quản lý danh sách Client

Một số cấu trúc có thể dùng:

-   `List<TcpClient>` nếu toàn bộ thao tác được đồng bộ hóa.
-   `ConcurrentDictionary<Guid, ClientSession>` để quản lý phiên theo
    ID.
-   Lớp `ClientSession` chứa ID, tên hiển thị, stream, trạng thái và tài
    nguyên liên quan.

Khi Client ngắt kết nối:

1.  Xóa Client khỏi danh sách.
2.  Thông báo cho các thành phần liên quan.
3.  Giải phóng stream/socket.
4.  Ghi log trạng thái.

Không nên dùng tên người dùng làm khóa duy nhất nếu cho phép nhiều phiên
cùng tên.

### 11.3. Phát tin nhắn cho mọi Client

Quy trình đơn giản:

1.  Server nhận thông điệp từ Client A.
2.  Xác thực thông điệp và quyền gửi.
3.  Tạo thông điệp phát đi theo giao thức.
4.  Lấy danh sách Client đích.
5.  Gửi đến từng Client, xử lý riêng từng lỗi.
6.  Dọn dẹp những kết nối đã hỏng.

Cần quyết định có phát lại cho người gửi hay không, cách biểu diễn người
gửi, và xử lý ra sao nếu một Client chậm hoặc không nhận được dữ liệu.

### 11.4. Giới hạn tài nguyên

Server thực tế nên có:

-   Giới hạn số Client.
-   Giới hạn kích thước thông điệp.
-   Timeout đọc/ghi hoặc deadline.
-   Giới hạn tốc độ gửi.
-   Giới hạn số yêu cầu đang xử lý.
-   Hủy tác vụ khi Server dừng.
-   Ghi log và đo lường lỗi.
-   Chính sách khi Client gửi dữ liệu không hợp lệ.

------------------------------------------------------------------------

## 12. Gửi chuỗi, JSON và dữ liệu nhị phân

### 12.1. Chuyển chuỗi thành byte

``` csharp
byte[] data = Encoding.UTF8.GetBytes(message);
```

Chuyển byte về chuỗi:

``` csharp
string message = Encoding.UTF8.GetString(data);
```

Không nên giả định mỗi byte tương ứng một ký tự. UTF-8 có thể dùng nhiều
byte cho một ký tự; cần giữ nguyên toàn bộ byte của thông điệp trước khi
giải mã nếu dữ liệu bị chia qua nhiều lần đọc.

### 12.2. Gửi dữ liệu với `NetworkStream`

``` csharp
byte[] data = Encoding.UTF8.GetBytes("Xin chào");
await stream.WriteAsync(data, 0, data.Length);
```

API overload thay đổi theo phiên bản .NET. Trên .NET hiện đại có các
overload nhận `Memory<byte>` và `CancellationToken`; với .NET Framework
cũ, thường dùng overload mảng byte.

Đoạn code trên chỉ gửi byte. Nó không cho biết phía nhận cần đọc bao
nhiêu byte để hoàn tất thông điệp.

### 12.3. JSON trong C

Với `System.Text.Json` trên .NET hiện đại:

``` csharp
using System.Text.Json;

var message = new ChatMessage
{
    Type = "chat",
    Sender = "An",
    Content = "Xin chào"
};

string json = JsonSerializer.Serialize(message);
ChatMessage? parsed = JsonSerializer.Deserialize<ChatMessage>(json);

public sealed class ChatMessage
{
    public string Type { get; set; } = "";
    public string Sender { get; set; } = "";
    public string Content { get; set; } = "";
}
```

Nếu dùng .NET Framework cũ, có thể cần cài thư viện phù hợp hoặc dùng
`Newtonsoft.Json`. Luôn kiểm tra kết quả deserialize và xác thực các
trường đầu vào.

### 12.4. Dữ liệu nhị phân

Khi truyền ảnh hoặc file, nên dùng giao thức có độ dài rõ ràng. Không
nên chuyển file tùy ý thành chuỗi UTF-8 vì dữ liệu nhị phân không nhất
thiết là văn bản hợp lệ.

Có thể gửi metadata như tên file và kích thước, sau đó gửi đúng số byte
file. Phía nhận cần xác thực kích thước, giới hạn dung lượng và đường
dẫn lưu.

------------------------------------------------------------------------

## 13. Ví dụ ứng dụng chat nhiều Client: thiết kế tổng thể

### 13.1. Các loại thông điệp

Một giao thức chat cơ bản có thể định nghĩa:

-   `join`: Client đăng nhập hoặc tham gia phòng.
-   `chat`: gửi nội dung chat.
-   `system`: thông báo hệ thống.
-   `leave`: rời phòng.
-   `error`: báo lỗi.
-   `ping` / `pong`: kiểm tra kết nối nếu cần.

Mỗi thông điệp nên có ID hoặc thông tin phiên nếu ứng dụng cần theo dõi
và chống xử lý trùng.

### 13.2. Quy trình kết nối

1.  Client nhập host, port và tên hiển thị.
2.  Client kết nối đến Server.
3.  Client gửi thông điệp `join`.
4.  Server kiểm tra tên và chính sách.
5.  Server gửi phản hồi thành công hoặc lỗi.
6.  Client bắt đầu vòng nhận dữ liệu.
7.  Khi người dùng gửi chat, Client đưa thông điệp vào hàng đợi gửi.
8.  Server xác thực và phát thông điệp đến người nhận phù hợp.
9.  Khi ngắt kết nối, Client và Server giải phóng tài nguyên.

### 13.3. Tách lớp

Một cấu trúc dễ bảo trì:

``` text
ChatApplication/
├── Forms/
│   ├── ClientForm.cs
│   └── ServerForm.cs
├── Networking/
│   ├── ChatClient.cs
│   ├── ChatServer.cs
│   ├── ClientSession.cs
│   └── MessageFraming.cs
├── Models/
│   └── ChatMessage.cs
├── Services/
│   └── ChatService.cs
└── Program.cs
```

-   `Forms`: giao diện và xử lý sự kiện người dùng.
-   `Networking`: kết nối, gửi/nhận, framing và quản lý phiên.
-   `Models`: cấu trúc dữ liệu.
-   `Services`: logic nghiệp vụ.
-   `Program.cs`: điểm khởi chạy ứng dụng.

Không nên đặt toàn bộ vòng lặp mạng, xử lý JSON, broadcast và cập nhật
giao diện trong một event handler duy nhất.

### 13.4. Luồng dữ liệu

``` text
Người dùng nhập tin
        |
        v
ClientForm
        |
        v
ChatClient / MessageFraming
        |
        v
TCP connection
        |
        v
ChatServer / ClientSession
        |
        v
Kiểm tra thông điệp và quyền
        |
        v
Broadcast / gửi phản hồi
        |
        v
ChatClient receive loop
        |
        v
ClientForm cập nhật UI
```

------------------------------------------------------------------------

## 14. Ví dụ truyền file: quy trình và quy tắc

Truyền file qua TCP cần một giao thức có cấu trúc. Một quy trình khả
thi:

1.  Client gửi yêu cầu bắt đầu truyền.
2.  Server kiểm tra quyền và hạn mức.
3.  Client gửi metadata: tên hiển thị, kích thước, loại nội dung nếu
    cần.
4.  Server xác thực metadata và quyết định chấp nhận/từ chối.
5.  Client gửi dữ liệu file theo từng khối.
6.  Server đọc đúng số byte đã khai báo.
7.  Server ghi dữ liệu ra file tạm.
8.  Server kiểm tra kích thước và checksum nếu giao thức có quy định.
9.  Server đổi tên file tạm thành tên hoàn tất.
10. Server gửi kết quả cuối cùng.

### 14.1. Các rủi ro cần xử lý

-   File rỗng hoặc quá lớn.
-   Kết nối bị đứt giữa chừng.
-   Tên file chứa đường dẫn như `../../...`.
-   Ghi đè file đã có.
-   Ổ đĩa hết dung lượng.
-   Client khai báo kích thước giả.
-   File có nội dung nguy hiểm.
-   Người dùng không có quyền ghi vào thư mục đích.

### 14.2. Nguyên tắc an toàn

-   Tạo tên file phía Server thay vì tin hoàn toàn tên do Client gửi.
-   Chuẩn hóa và kiểm tra đường dẫn.
-   Chỉ lưu trong thư mục được cho phép.
-   Giới hạn kích thước và số file.
-   Ghi vào file tạm rồi mới chuyển thành file hoàn chỉnh.
-   Không cấp phát một mảng khổng lồ theo kích thước chưa xác thực.
-   Nếu cần tính toàn vẹn, dùng hash/checksum và so sánh ở hai đầu.
-   Không cho rằng TCP bảo đảm file đã được lưu thành công; Server cần
    phản hồi kết quả sau khi hoàn tất xử lý.

------------------------------------------------------------------------

## 15. Xử lý lỗi và tình huống biên

### 15.1. Nhóm lỗi thường gặp

-   `SocketException`: lỗi kết nối, bind, port hoặc mạng.
-   `IOException`: lỗi đọc/ghi stream hoặc kết nối bị đóng bất thường.
-   `ObjectDisposedException`: thao tác trên tài nguyên đã được dispose.
-   `OperationCanceledException`: thao tác bị hủy.
-   `InvalidOperationException`: trạng thái đối tượng không phù hợp.
-   `JsonException`: dữ liệu JSON không hợp lệ.

Không nên bắt mọi exception rồi bỏ qua hoàn toàn. Cần ghi log, cập nhật
trạng thái và dọn dẹp đúng cách. Cũng không nên hiển thị nguyên văn
thông tin lỗi nội bộ nhạy cảm cho người dùng cuối.

### 15.2. Server không bind được port

Nguyên nhân có thể:

-   Port đang được dùng.
-   Không đủ quyền cho một số port hoặc cấu hình.
-   Địa chỉ bind không tồn tại trên máy.
-   Cấu hình IPv4/IPv6 không phù hợp.

Cách kiểm tra: - Xác nhận port nhập vào hợp lệ. - Thử port khác. - Kiểm
tra tiến trình đang sử dụng port. - Kiểm tra địa chỉ bind. - Xem thông
tin exception và log.

### 15.3. Client không kết nối được

Kiểm tra theo thứ tự:

1.  Server đã chạy chưa?
2.  Host/IP có đúng không?
3.  Port có khớp không?
4.  Hai máy có kết nối mạng không?
5.  Firewall trên máy Server có cho phép không?
6.  Server có bind đúng giao diện không?
7.  Client có đang dùng `127.0.0.1` nhầm khi kết nối từ máy khác không?

### 15.4. Đọc dữ liệu bị treo

Nguyên nhân phổ biến là chương trình đang chờ thông điệp nhưng phía gửi
không gửi dấu phân cách hoặc chưa gửi đủ payload.

Cách giải quyết: - Xác định giao thức framing rõ ràng. - Đảm bảo phía
gửi tuân thủ framing. - Đặt timeout/deadline. - Cho phép hủy thao tác. -
Xử lý EOF và ngắt kết nối. - Không giả định một lần đọc nhận toàn bộ
thông điệp.

### 15.5. Client đóng đột ngột

Phía Server cần xử lý EOF hoặc exception, loại Client khỏi danh sách,
dừng vòng đọc và dispose tài nguyên. Không nên để một Client lỗi làm
dừng toàn bộ Server.

### 15.6. Form bị treo

Nguyên nhân thường là I/O đồng bộ trên UI thread, vòng lặp vô hạn hoặc
`.Wait()`/`.Result`. Chuyển sang API bất đồng bộ, không chặn UI và có cơ
chế hủy.

------------------------------------------------------------------------

## 16. Bảo mật

Một chương trình Socket chạy được chưa có nghĩa là an toàn. Khi đưa ra
mạng thật, cần xem xét:

### 16.1. Xác thực và phân quyền

-   Không cho phép mọi Client thực hiện mọi lệnh.
-   Xác thực danh tính theo yêu cầu của ứng dụng.
-   Kiểm tra quyền cho từng loại thông điệp.
-   Không tin tên người gửi do Client tự khai báo.
-   Hạn chế thử mật khẩu và đăng nhập liên tục.

### 16.2. TLS

TCP thông thường không mã hóa payload. Nếu dữ liệu nhạy cảm truyền qua
mạng, cần dùng TLS phù hợp, ví dụ `SslStream`, đồng thời xác thực chứng
chỉ máy chủ đúng cách.

Không tắt kiểm tra chứng chỉ chỉ để làm cho kết nối "chạy được". Với
.NET, cần tìm hiểu cách cấu hình `SslStream`, chứng chỉ và hostname theo
môi trường triển khai.

### 16.3. Xác thực dữ liệu đầu vào

-   Giới hạn kích thước thông điệp.
-   Kiểm tra kiểu thông điệp và trường bắt buộc.
-   Không deserialize rồi tin mọi dữ liệu.
-   Không dùng dữ liệu nhận được trực tiếp làm đường dẫn file, câu lệnh
    hoặc truy vấn.
-   Có giới hạn tốc độ và timeout.
-   Xử lý lỗi mà không làm lộ stack trace hay bí mật.

### 16.4. Không dùng dữ liệu mạng để thực thi tùy tiện

Không thực thi lệnh hệ điều hành, script hoặc mã nhận từ Client nếu
không có thiết kế bảo mật chuyên biệt. Nếu ứng dụng cần lệnh quản trị,
hãy dùng danh sách lệnh cho phép và kiểm tra quyền nghiêm ngặt.

### 16.5. Nhật ký

Ghi các sự kiện cần thiết: kết nối, ngắt kết nối, lỗi giao thức, xác
thực thất bại và hoạt động quản trị. Không ghi mật khẩu, token hoặc nội
dung nhạy cảm một cách không cần thiết.

------------------------------------------------------------------------

## 17. Kiểm thử và gỡ lỗi

### 17.1. Kiểm thử trên cùng máy

1.  Chạy Server trước.
2.  Bind vào loopback hoặc địa chỉ phù hợp.
3.  Chạy Client với host `127.0.0.1`.
4.  Dùng đúng port.
5.  Gửi thông điệp ngắn, dài, có dấu tiếng Việt và ký tự đặc biệt.
6.  Thử ngắt kết nối khi đang gửi/nhận.

### 17.2. Kiểm thử trên LAN

1.  Xác định địa chỉ LAN của máy Server.
2.  Bind Server vào địa chỉ phù hợp hoặc `IPAddress.Any`.
3.  Cho phép ứng dụng/port qua Windows Firewall theo phạm vi cần thiết.
4.  Kết nối Client từ máy khác bằng IP LAN.
5.  Kiểm tra khi Wi-Fi ngắt, Server dừng hoặc Client đóng bất ngờ.
6.  Không mở port ra Internet nếu không hiểu rõ cấu hình và yêu cầu bảo
    mật.

### 17.3. Bộ ca kiểm thử nên có

-   Kết nối thành công.
-   Server chưa chạy.
-   Sai port.
-   Host không hợp lệ.
-   Gửi chuỗi rỗng.
-   Gửi tiếng Việt có dấu.
-   Gửi nhiều tin liên tiếp rất nhanh.
-   Tin nhắn dài hơn giới hạn.
-   Client ngắt kết nối bất ngờ.
-   Server dừng khi có nhiều Client.
-   Client bấm gửi nhiều lần.
-   Hai Client gửi cùng lúc.
-   Dữ liệu JSON sai định dạng.
-   Dữ liệu thiếu header hoặc payload chưa đầy đủ.
-   Kết nối chậm hoặc timeout.

### 17.4. Công cụ hỗ trợ

-   Visual Studio Debugger.
-   Output/Debug window và logging.
-   Windows Firewall settings.
-   `ipconfig` để xem cấu hình IP.
-   `ping` để kiểm tra khả năng tiếp cận IP ở mức cơ bản; ping bị chặn
    không nhất thiết nghĩa là TCP không kết nối được.
-   `netstat` hoặc công cụ tương đương để kiểm tra port đang lắng nghe.
-   Wireshark để quan sát lưu lượng mạng khi cần hiểu sâu hơn.

------------------------------------------------------------------------

## 18. Các lỗi phổ biến

### Lỗi 1: Dùng `127.0.0.1` từ máy Client khác

`127.0.0.1` luôn chỉ máy đang chạy Client. Dùng IP LAN hoặc tên máy của
Server.

### Lỗi 2: Nghĩ một lần `Read` là một tin nhắn

TCP là luồng byte, không bảo toàn ranh giới lần gửi. Dùng framing.

### Lỗi 3: Chạy vòng nhận trên UI thread

Giao diện bị treo. Dùng `async/await` và vòng đọc bất đồng bộ.

### Lỗi 4: Không đóng tài nguyên

Socket, stream và listener bị giữ lại. Dùng `using`, `Dispose`, và quy
trình dừng rõ ràng.

### Lỗi 5: Không xử lý Client ngắt đột ngột

Server có thể giữ Client "ma" trong danh sách. Xử lý EOF/exception và
dọn dẹp phiên.

### Lỗi 6: Nhiều nơi đọc cùng một stream

Dữ liệu bị chia không đúng nơi mong muốn. Mỗi kết nối nên có một vòng
đọc chính.

### Lỗi 7: Nhiều nơi ghi đồng thời

Thông điệp có thể xen kẽ hoặc sai framing. Tuần tự hóa thao tác ghi hoặc
dùng hàng đợi gửi.

### Lỗi 8: Không giới hạn kích thước dữ liệu

Client có thể gửi dữ liệu quá lớn gây tiêu thụ bộ nhớ. Đặt giới hạn và
xác thực độ dài trước khi cấp phát.

### Lỗi 9: Bắt exception nhưng không ghi log

Lỗi bị che giấu, rất khó chẩn đoán. Ghi log có kiểm soát và cập nhật
trạng thái giao diện.

### Lỗi 10: Nhầm gửi thành công với xử lý thành công

Ghi dữ liệu vào stream không chứng minh Server đã lưu file hoặc thực
hiện lệnh. Nếu cần xác nhận, hãy thiết kế phản hồi ở tầng ứng dụng.

------------------------------------------------------------------------

## 19. Tối ưu và kiến trúc chương trình

### 19.1. Tách giao diện khỏi mạng

Không để Form quản lý trực tiếp tất cả chi tiết socket. Tạo lớp
`ChatClient` hoặc `ChatServer` để giao diện chỉ gọi các phương thức như
`ConnectAsync`, `SendAsync`, `DisconnectAsync`.

### 19.2. Dùng sự kiện hoặc callback

Lớp mạng có thể phát sự kiện khi nhận thông điệp hoặc thay đổi trạng
thái. Khi xử lý sự kiện, cần chú ý thread: event có thể chạy trên thread
nhận dữ liệu, không phải UI thread.

### 19.3. Dùng hàng đợi gửi

Nếu nhiều thành phần cần gửi, có thể đưa thông điệp vào một hàng đợi và
chỉ có một tác vụ đọc hàng đợi để gửi tuần tự. Cách này giúp kiểm soát
thứ tự, backpressure và lỗi gửi.

### 19.4. Backpressure

Nếu Client nhận chậm nhưng Server phát dữ liệu liên tục, hàng đợi gửi có
thể phình to. Cần giới hạn dung lượng hàng đợi, chính sách bỏ/throttle
hoặc ngắt Client chậm theo yêu cầu nghiệp vụ.

### 19.5. Cancellation và shutdown

Khi dừng Server hoặc đóng Form:

1.  Ngừng nhận kết nối mới.
2.  Báo hủy các vòng đọc/ghi.
3.  Đóng socket để giải phóng thao tác đang chờ nếu cần.
4.  Chờ các task kết thúc trong giới hạn hợp lý.
5.  Dispose tài nguyên.
6.  Cập nhật trạng thái giao diện.

Tránh hủy tài nguyên trong khi một thao tác khác vẫn đang dùng chúng mà
không có quy ước vòng đời rõ ràng.

### 19.6. Logging

Dùng `ILogger` hoặc thư viện logging phù hợp khi dự án lớn hơn. Log nên
có thời gian, cấp độ, Client/session ID và thông tin lỗi đủ để chẩn
đoán, nhưng không chứa bí mật.

### 19.7. Cấu hình

Đưa host, port, giới hạn kích thước, timeout và thư mục nhận file ra cấu
hình thay vì rải số cố định khắp mã nguồn. Luôn xác thực cấu hình khi
khởi động.

------------------------------------------------------------------------

## 20. Bài tập thực hành theo mức độ

### Mức 1 --- Nền tảng

**Bài 1: Client gửi một chuỗi, Server phản hồi**

Yêu cầu: - Nhập host và port. - Server nhận một dòng. - Server trả về
nội dung đã nhận. - Xử lý lỗi khi Server chưa chạy.

Kiến thức: `TcpListener`, `TcpClient`, `NetworkStream`, framing dòng,
`async/await`.

**Bài 2: Hiển thị trạng thái kết nối**

Yêu cầu: - Nút Connect/Disconnect. - Hiển thị trạng thái rõ ràng. -
Không cho gửi khi chưa kết nối. - Dọn dẹp kết nối khi đóng Form.

Kiến thức: vòng đời kết nối, trạng thái UI, `Dispose`.

### Mức 2 --- Trung bình

**Bài 3: Chat hai chiều**

Yêu cầu: - Có vòng nhận chạy liên tục. - Gửi và nhận độc lập. - Hiển thị
timestamp. - Hỗ trợ tiếng Việt. - Không làm treo giao diện.

Kiến thức: vòng nhận bất đồng bộ, thread UI, encoding.

**Bài 4: Chat nhiều Client**

Yêu cầu: - Server nhận nhiều kết nối. - Client gửi tin đến Server. -
Server broadcast đến các Client. - Client rời đi không làm dừng
Server. - Có danh sách người dùng đang kết nối.

Kiến thức: quản lý phiên, đồng bộ hóa, broadcast, xử lý lỗi.

**Bài 5: Gửi JSON**

Yêu cầu: - Định nghĩa class thông điệp. - Có `type`, `sender`,
`content`. - Serialize/deserialize JSON. - Xử lý thông điệp sai cấu
trúc.

Kiến thức: protocol, serialization, validation.

### Mức 3 --- Khá

**Bài 6: Gửi file**

Yêu cầu: - Hiển thị tên và kích thước file. - Có thanh tiến trình. - Hủy
truyền file. - Kiểm tra kích thước và kết quả cuối. - Không ghi file ra
ngoài thư mục cho phép.

Kiến thức: truyền nhị phân, framing, stream, kiểm soát tài nguyên.

**Bài 7: Phòng chat**

Yêu cầu: - Tạo và tham gia phòng. - Chỉ phát tin trong cùng phòng. - Có
thông báo vào/rời phòng. - Server kiểm tra quyền và tên phòng.

Kiến thức: logic nghiệp vụ, trạng thái phiên, protocol.

### Mức 4 --- Nâng cao

**Bài 8: Chat có đăng nhập**

Yêu cầu: - Tài khoản và mật khẩu được quản lý an toàn. - Xác thực trên
Server. - Phân quyền. - Giới hạn thử đăng nhập. - Không gửi mật khẩu
hoặc token trong log.

**Bài 9: Kết nối TLS**

Yêu cầu: - Bảo vệ dữ liệu trên đường truyền. - Kiểm tra chứng chỉ
Server. - Xử lý chứng chỉ hết hạn hoặc không hợp lệ. - Tài liệu hóa cấu
hình triển khai.

**Bài 10: Server ổn định**

Yêu cầu: - Giới hạn số Client. - Timeout và hủy tác vụ. - Logging. -
Kiểm thử tải và Client gửi dữ liệu bất thường. - Đóng Server an toàn.

------------------------------------------------------------------------

## 21. Checklist hoàn thiện đồ án Socket Windows Forms

### Chức năng

-   [ ] Client kết nối đến Server bằng host và port.
-   [ ] Có trạng thái kết nối rõ ràng.
-   [ ] Gửi và nhận dữ liệu đúng giao thức.
-   [ ] Hỗ trợ nhiều Client nếu đề bài yêu cầu.
-   [ ] Xử lý ngắt kết nối bình thường và đột ngột.
-   [ ] Có thông báo lỗi thân thiện.

### Kỹ thuật

-   [ ] Không chặn UI thread bằng I/O đồng bộ.
-   [ ] Có framing rõ ràng.
-   [ ] Không giả định một lần đọc tương ứng một thông điệp.
-   [ ] Có một vòng đọc chính cho mỗi kết nối.
-   [ ] Đồng bộ hóa thao tác ghi khi cần.
-   [ ] Dùng `CancellationToken` hoặc cơ chế dừng tương đương.
-   [ ] Dispose socket, stream và listener.
-   [ ] Giới hạn kích thước dữ liệu.
-   [ ] Quản lý task xử lý Client.
-   [ ] Có logging đủ để chẩn đoán.

### Bảo mật

-   [ ] Xác thực đầu vào.
-   [ ] Không tin tên người gửi do Client tự khai báo.
-   [ ] Có giới hạn kích thước và tốc độ.
-   [ ] Không lưu file theo đường dẫn tùy ý từ Client.
-   [ ] Dùng TLS nếu dữ liệu cần bảo mật trên mạng.
-   [ ] Không ghi mật khẩu/token vào log.

### Giao diện

-   [ ] Nút được bật/tắt theo trạng thái hợp lý.
-   [ ] Có trạng thái đang kết nối/đang gửi/lỗi.
-   [ ] Không cập nhật control sai thread.
-   [ ] Đóng Form sẽ dừng tác vụ và giải phóng tài nguyên.
-   [ ] Tin nhắn dài không làm giao diện mất phản hồi.

### Tài liệu báo cáo

-   [ ] Mô tả kiến trúc Client--Server.
-   [ ] Sơ đồ luồng dữ liệu.
-   [ ] Mô tả protocol và framing.
-   [ ] Giải thích các lớp .NET đã dùng.
-   [ ] Có ảnh giao diện.
-   [ ] Có bảng ca kiểm thử và kết quả.
-   [ ] Nêu giới hạn hiện tại và hướng phát triển.

------------------------------------------------------------------------

## 22. Bảng tra cứu nhanh

  Thành phần               Vai trò
  ------------------------ ---------------------------------------------
  `TcpListener`            Server lắng nghe và chấp nhận TCP Client
  `TcpClient`              Đại diện kết nối TCP
  `NetworkStream`          Đọc/ghi byte qua kết nối
  `Socket`                 API socket mức thấp
  `IPAddress`              Địa chỉ IP
  `IPEndPoint`             Địa chỉ IP kết hợp port
  `Dns`                    Phân giải tên máy
  `Encoding.UTF8`          Chuyển chuỗi thành byte UTF-8 và ngược lại
  `StreamReader`           Đọc văn bản từ stream
  `StreamWriter`           Ghi văn bản vào stream
  `CancellationToken`      Truyền yêu cầu hủy
  `Task`                   Đại diện công việc bất đồng bộ
  `SemaphoreSlim`          Đồng bộ hóa thao tác, ví dụ tuần tự hóa gửi
  `ConcurrentDictionary`   Cấu trúc từ điển hỗ trợ truy cập đồng thời
  `SslStream`              Lớp stream hỗ trợ TLS

## 23. Thứ tự học đề xuất

1.  Ôn IP, port, TCP, UDP và Client--Server.
2.  Viết Server TCP tối giản bằng `TcpListener`.
3.  Viết Client bằng `TcpClient`.
4.  Trao đổi một dòng văn bản theo quy ước rõ ràng.
5.  Đưa thao tác mạng sang `async/await`.
6.  Tích hợp vào Windows Forms và cập nhật UI đúng thread.
7.  Tạo vòng nhận liên tục và xử lý ngắt kết nối.
8.  Thiết kế framing để hỗ trợ nhiều thông điệp.
9.  Quản lý nhiều Client và broadcast.
10. Thêm JSON, logging, timeout và giới hạn tài nguyên.
11. Xây dựng truyền file.
12. Học xác thực, TLS và kiểm thử bảo mật.

## Kết luận

Khi học lập trình Socket với Windows Forms C#, hãy tập trung vào bốn nền
tảng: **mô hình Client--Server, giao thức và framing, I/O bất đồng bộ,
quản lý vòng đời kết nối**. Giao diện đẹp không thể bù cho giao thức sai
hoặc vòng nhận bị treo. Một ứng dụng tốt phải xử lý đúng dữ liệu bị chia
thành nhiều lần đọc, nhiều Client hoạt động đồng thời, ngắt kết nối đột
ngột và các dữ liệu đầu vào không đáng tin cậy.
