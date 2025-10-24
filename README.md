# Baitap_2  
Nguyễn Thị Kim Huệ - K225480106026   
Bài tập 02: Lập trình web.   
NỘI DUNG BÀI TẬP:  

2.1. Cài đặt Apache web server:
- Vô hiệu hoá IIS: nếu iis đang chạy thì mở cmd quyền admin để chạy lệnh: iisreset /stop
- Download apache server, giải nén ra ổ D, cấu hình các file:
  + D:\Apache24\conf\httpd.conf
  + D:Apache24\conf\extra\httpd-vhosts.conf
  để tạo website với domain: fullname.com
  code web sẽ đặt tại thư mục: `D:\Apache24\fullname` (fullname ko dấu, liền nhau)
- sử dụng file `c:\WINDOWS\SYSTEM32\Drivers\etc\hosts` để fake ip 127.0.0.1 cho domain này
  ví dụ sv tên là: `Đỗ Duy Cốp` thì tạo website với domain là fullname ko dấu, liền nhau: `doduycop.com`
- thao tác dòng lệnh trên file `D:\Apache24\bin\httpd.exe` với các tham số `-k install` và `-k start` để cài đặt và khởi động web server apache.
  
2.2. Cài đặt nodejs và nodered => Dùng làm backend:
- Cài đặt nodejs:
  + download file `https://nodejs.org/dist/v20.19.5/node-v20.19.5-x64.msi`  (đây ko phải bản mới nhất, nhưng ổn định)
  + cài đặt vào thư mục `D:\nodejs`
- Cài đặt nodered:
  + chạy cmd, vào thư mục `D:\nodejs`, chạy lệnh `npm install -g --unsafe-perm node-red --prefix "D:\nodejs\nodered"`
  + download file: https://nssm.cc/release/nssm-2.24.zip
    giải nén được file nssm.exe
    copy nssm.exe vào thư mục `D:\nodejs\nodered\`
  + tạo file "D:\nodejs\nodered\run-nodered.cmd" với nội dung (5 dòng sau):
@echo off
REM fix path
set PATH=D:\nodejs;%PATH%
REM Run Node-RED
node "D:\nodejs\nodered\node_modules\node-red\red.js" -u "D:\nodejs\nodered\work" %*
  + mở cmd, chuyển đến thư mục: `D:\nodejs\nodered`
  + cài đặt service `a1-nodered` bằng lệnh: nssm.exe install a1-nodered "D:\nodejs\nodered\run-nodered.cmd"
  + chạy service `a1-nodered` bằng lệnh: `nssm start a1-nodered`
  + 
2.3. Tạo csdl tuỳ ý trên mssql (sql server 2022), nhớ các thông số kết nối: ip, port, username, password, db_name, table_name  
2.4. Cài đặt thư viện trên nodered:
    
- truy cập giao diện nodered bằng url: http://localhost:1880
- cài đặt các thư viện: node-red-contrib-mssql-plus, node-red-node-mysql, node-red-contrib-telegrambot, node-red-contrib-moment, node-red-contrib-influxdb, node-red-contrib-duckdns, node-red-contrib-cron-plus
- Sửa file `D:\nodejs\nodered\work\settings.js` :
  tìm đến chỗ adminAuth, bỏ comment # ở đầu dòng (8 dòng), thay chuỗi mã hoá mật khẩu bằng chuỗi mới
    adminAuth: {
        type: "credentials",
        users: [{
            username: "admin",
            password: "chuỗi_mã_hoá_mật_khẩu",
            permissions: "*"
        }]
    },   
   với mã hoá mật khẩu có thể thiết lập bằng tool: https://tms.tnut.edu.vn/pw.php
- chạy lại nodered bằng cách: mở cmd, vào thư mục `D:\nodejs\nodered` và chạy lệnh `nssm restart a1-nodered`
  khi đó nodered sẽ yêu cầu nhập mật khẩu mới vào được giao diện cho admin tại: http://localhost:1880
  
2.5. tạo api back-end bằng nodered:
- tại flow1 trên nodered, sử dụng node `http in` và `http response` để tạo api
- thêm node `MSSQL` để truy vấn tới cơ sở dữ liệu
- logic flow sẽ gồm 4 node theo thứ tự sau (thứ tự nối dây): 
  1. http in  : dùng GET cho đơn giản, URL đặt tuỳ ý, ví dụ: /timkiem
  2. function : để tiền xử lý dữ liệu gửi đến
  3. MSSQL: để truy vấn dữ liệu tới CSDL, nhận tham số từ node tiền xử lý
  4. http response: để phản hồi dữ liệu về client: Status Code=200, Header add : Content-Type = application/json
  có thể thêm node `debug` để quan sát giá trị trung gian.
- test api thông qua trình duyệt, ví dụ: http://localhost:1880/timkiem?q=thị
  
  2.6. Tạo giao diện front-end:
- html form gồm các file : index.html, fullname.js, fullname.css
  cả 3 file này đặt trong thư mục: `D:\Apache24\fullname`
  nhớ thay fullname là tên của bạn, viết liền, ko dấu, chữ thường, vd tên là Đỗ Duy Cốp thì fullname là `doduycop`
  khi đó 3 file sẽ là: index.html, doduycop.js và doduycop.css
- index.html và fullname.css: trang trí tuỳ ý, có dấu ấn cá nhân, có form nhập được thông tin.
- fullname.js: lấy dữ liệu trên form, gửi đến api nodered đã làm ở bước 2.5, nhận về json, dùng json trả về để tạo giao diện phù hợp với kết quả truy vấn của bạn.
- 
2.7. Nhận xét bài làm của mình:

  Bài Làm:
 2.1
  Bước 1: Vô hiệu hoá IIS
Nhấn Start → gõ cmd -> Chuột phải vào Command Prompt → chọn Run as administrator -> Sau đó nhập lênh: iisreset /stop
<img width="1075" height="628" alt="Ảnh chụp màn hình 2025-10-22 210223" src="https://github.com/user-attachments/assets/a77af759-d9ca-426d-84c6-ab0136f4a08b" />

Bước 2. Download apache server
Truy cập link: https://www.apachelounge.com/download/ để download apache
Sau khi tải xong sẽ xuất hiện tệp:
<img width="655" height="50" alt="image" src="https://github.com/user-attachments/assets/ae52ef32-aec9-4099-b65d-91e7815fdc58" />   
Bước 3: Giải nén ra ổ D:
Chuột phải vào file vừa tải -> chọn Extract All... -> Chọn nơi muốn giải nén: D:\ -> Nhấn Extract
Sau khi giải nén xong, sẽ hiện thư mục: D:\Apache24\  
<img width="1379" height="554" alt="image" src="https://github.com/user-attachments/assets/6c3d4f0a-d76c-4028-bc6a-894beb9ef55c" />  
Bước 4: Cấu hình file: D:\Apache24\conf\httpd.conf  
+ Mở file: D:\Apache24\conf\httpd.conf
+ Sửa ServerRoot: ServerRoot "c:/Apache24" => ServerRoot "D:/Apache24"
+ Sau mở httpd.conf -> Tìm dòng: #Include conf/extra/httpd-vhosts.conf và bỏ dấu # để bật file vhosts.
  Bước 5: Cấu hình file: D:Apache24\conf\extra\httpd-vhosts.conf
  + Mở file: D:Apache24\conf\extra\httpd-vhosts.conf
+ Sau khi mở file sẽ hiển thị:
  <img width="994" height="897" alt="image" src="https://github.com/user-attachments/assets/9d42a1a5-2061-47e5-a281-dc48e138bab2" />
  Thêm vào cuối file nội dung sau:
  <img width="619" height="180" alt="image" src="https://github.com/user-attachments/assets/ef842d34-d0bf-425d-a606-895719f88ac2" />
  Trong file D:\Apache24\conf\httpd.conf sửa: DocumentRoot "D:/Apache24/nguyenthikimhue" và <Directory "D:/Apache24/nguyenthikimhue">
  Bước 6: Tạo thư mục D:\Apache24\nguyenthikimhue
Trong đó tạo file index.html:
Bước 7: Cấu hình file hosts để fake domain
Mở file: C:\Windows\System32\drivers\etc\hosts (Mở bằng Notepad quyền Admin (Run as Administrator))
Sau khi mở, thêm dòng: 127.0.0.1 dauvankhanh.com
<img width="845" height="751" alt="image" src="https://github.com/user-attachments/assets/62f7314d-810a-4ad7-bf7c-571d11121086" />
Bước 8: Cài đặt và khởi động Apache
Mở cmd -> chạy quyền admin (Run as Administrator)
Bước 9: Kiểm tra kết quả
Mở trình duyệt, gõ: http://nguyenthikimhue.com
<img width="852" height="600" alt="image" src="https://github.com/user-attachments/assets/0d999668-1daa-48db-a1cb-5d26ecb66092" /> 

2.2. Cài đặt nodered
Chạy cmd (Admin), vào thư mục D:\nodejs, chạy lệnh npm install -g --unsafe-perm node-red --prefix "D:\nodejs\nodered"
Sau khi chạy cmd, kết quả nodered hiển thị trong thư mục D:\nodejs  
<img width="1498" height="982" alt="image" src="https://github.com/user-attachments/assets/627f4702-68d7-45f4-9f0a-cee3577809c2" />   
Cài nssm: https://nssm.cc/release/nssm-2.24.zip.  
Tạo file "D:\nodejs\nodered\run-nodered.cmd"    




