# Baitap_2  
Nguyễn Thị Kim Huệ - K225480106026   
Bài tập 02: Lập trình web.   
NỘI DUNG BÀI TẬP:  

**1#2.1. Cài đặt Apache web server:**
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
  

Bài Làm  

 2.1 Cài đặt Apache web server  
 
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

2.2.  Cài đặt nodejs và nodered => Dùng làm backend  
2.2.1 Cài đặt nodejs   
-Tải file: https://nodejs.org/dist/v20.19.5/node-v20.19.5-x64.msi  
-Cài đặt bằng giao diện (GUI):  
  Nhấn file: node-v20.19.5-x64.msi   
  Chọn Next → I Agree → Custom.  
  Ở phần chọn đường dẫn, đổi thành: D:\nodejs  
  Bấm Next → Install.  
  Khi hoàn tất, Node.js sẽ được cài vào D:\nodejs và npm đi kèm.   
-Kiểm tra: Mở cmd(admin) và chạy:   
cd \nodejs  
node -v  
npm -v    
<img width="842" height="561" alt="image" src="https://github.com/user-attachments/assets/6ab68c45-c39a-4e29-858d-1ebebe31e43e" />  
2.2.2 Cài đặt nodered  
Chạy cmd (Admin), vào thư mục D:\nodejs, chạy lệnh npm install -g --unsafe-perm node-red --prefix "D:\nodejs\nodered"  
Sau khi chạy cmd, kết quả nodered hiển thị trong thư mục D:\nodejs  
<img width="1530" height="985" alt="image" src="https://github.com/user-attachments/assets/35d51ea8-074b-4df2-913f-5f88ade5253e" />  
Cài nssm: https://nssm.cc/release/nssm-2.24.zip.  
Tạo file "D:\nodejs\nodered\run-nodered.cmd"   
<img width="1152" height="925" alt="image" src="https://github.com/user-attachments/assets/fbecb284-96bc-454e-86ce-a92b183a31b4" />  
Cài service a1-nodered bằng nssm  
  Mở cmd (Admin), chuyển đến thư mục nodered: cd /d D:\nodejs\nodered   
  Cài đặt service "a1-nodered" bằng lệnh: nssm.exe install a1-nodered "D:\nodejs\nodered\run-nodered.cmd  
  <img width="811" height="424" alt="image" src="https://github.com/user-attachments/assets/1fd28e76-be05-407e-ab84-a6f895cb83fe" />  
2.3 Tạo csdl tuỳ ý trên mssql (sql server 2022), nhớ các thông số kết nối: ip, port, username, password, db_name, table_name   
Cơ sở dữ liệu được thiết kế nhằm lưu trữ và quản lý thông tin sản phẩm trong hệ thống.  
Tạo DB name : QL_Sach 
Table name : Sach 
Server name : KIMHUE.ThuVien-dbo.Sach  
Port : 1433    
<img width="684" height="291" alt="image" src="https://github.com/user-attachments/assets/7fc3a0fd-932d-4d81-81f5-95aa467414ef" />   
Dữ liệu mẫu :  
<img width="1048" height="851" alt="image" src="https://github.com/user-attachments/assets/a1e56c91-b818-4c39-973a-f6e5a50d1c6d" />  

2.4. Cài đặt thư viện trên nodered:  
-Cài đặt nodejs:  
Download file tại : https://nodejs.org/dist/v22.21.0/node-v22.21.0-x64.msi  
Cài đặt vào thư mục D:\nodejs  
-Cài đặt nodered:  
Chạy cmd, vào thư mục D:\nodejs, chạy lệnh npm install -g --unsafe-perm node-red --prefix "D:\nodejs\nodered"    
Download file: https://nssm.cc/release/nssm-2.24.zip giải nén được file nssm.exe  
Copy nssm.exe vào thư mục D:\nodejs\nodered\   
Tạo file "D:\nodejs\nodered\run-nodered.cmd" với nội dung (5 dòng sau):  
@echo off  
REM fix path  
set PATH=D:\nodejs;%PATH%  
REM Run Node-RED  
node "D:\nodejs\nodered\node_modules\node-red\red.js" -u "D:\nodejs\nodered\work" %*  
<img width="972" height="601" alt="image" src="https://github.com/user-attachments/assets/32bb1b43-5d3c-48b2-b9d8-7aeca7ea87c8" />  
Chạy Node-Red  
Mở cmd với quyền Admin, chuyển đến thư mục: D:\nodejs\nodered  
Cài đặt service a1-nodered bằng lệnh: nssm.exe install a1-nodered "D:\nodejs\nodered\run-nodered.cmd"  
<img width="1259" height="236" alt="image" src="https://github.com/user-attachments/assets/9581842a-112e-4541-9f20-44290242d395" />  
Chạy service a1-nodered bằng lệnh: nssm start a1-nodered  
<img width="918" height="142" alt="image" src="https://github.com/user-attachments/assets/c819e6e9-1d82-46b7-a2e8-014e221dbad6" />  
Node-Red chạy ở http://localhost:1880  
<img width="1908" height="1023" alt="image" src="https://github.com/user-attachments/assets/7d4d3134-2a0d-4dbd-b090-92dbd0187f96" />  
Cài đặt các thư viện    
node-red-contrib-mssql-plus  
node-red-node-mysql  
node-red-contrib-telegrambot  
node-red-contrib-moment  
node-red-contrib-influxdb  
node-red-contrib-duckdns  
node-red-contrib-cron-plus  
<img width="639" height="821" alt="image" src="https://github.com/user-attachments/assets/4fb588d0-6118-40a1-b919-6d94f212488f" />  
- Cài đặt mật khẩu cho Node-Red
  Sửa file D:\nodejs\nodered\work\settings.js : tìm  adminAuth, bỏ comment # ở đầu dòng (8 dòng), thay chuỗi mã hoá mật khẩu bằng chuỗi mới
    adminAuth: {
        type: "credentials",
        users: [{
        username: "admin",
            password: "chuỗi_mã_hoá_mật_khẩu",
            permissions: "*"
        }]
    },
với mã hoá mật khẩu có thể thiết lập bằng tool: https://tms.tnut.edu.vn/pw.php
Sau đó chạy lại nodered bằng cách: mở cmd, vào thư mục D:\nodejs\nodered và chạy lệnh nssm restart a1-nodered
<img width="819" height="227" alt="image" src="https://github.com/user-attachments/assets/94c3f6f3-fe9b-4310-add4-a3f845ba9b71" />
nodered sẽ yêu cầu nhập mật khẩu mới vào được giao diện cho admin tại: http://localhost:1880
<img width="606" height="335" alt="image" src="https://github.com/user-attachments/assets/ca61fc8d-7802-40bb-88b5-ec70286801ab" />

2.5. tạo api back-end bằng nodered  
Test API (curl / browser) 
API Truy vấn tìm kiếm sach  
Tại flow1 trên nodered, sử dụng node http in và http response để tạo api  
Thêm node MSSQL để truy vấn tới cơ sở dữ liệu  
logic flow sẽ gồm 4 node theo thứ tự sau:  
-Thêm http in : dùng GET , URL đặt tuỳ ý, ví dụ: /timkiem  
<img width="635" height="467" alt="image" src="https://github.com/user-attachments/assets/61fd487a-1eee-49f4-be7d-e590fe3d4968" />  
-Thêm node function : để tiền xử lý dữ liệu gửi đến  
<img width="809" height="520" alt="image" src="https://github.com/user-attachments/assets/d6de90ac-49c2-483a-8851-6d6f805578b1" />  
Thêm node MSSQL: để truy vấn dữ liệu tới CSDL, nhận tham số từ node tiền xử lý  
<img width="632" height="818" alt="image" src="https://github.com/user-attachments/assets/e096ad97-3f3c-44c7-9e40-e17acb13a782" />  
<img width="647" height="818" alt="image" src="https://github.com/user-attachments/assets/dfd2beea-acee-43bf-a0d9-097f8bb8f1b3" />  
 -Thêm node http response: để phản hồi dữ liệu về client: Status Code=200, Header add : Content-Type = application/json  
 <img width="636" height="816" alt="image" src="https://github.com/user-attachments/assets/0983b866-d552-4367-bf89-f73e80d8326c" />  
 -Thêm node debug để quan sát giá trị trung gian.  
 <img width="650" height="542" alt="image" src="https://github.com/user-attachments/assets/752751b4-fb9f-4163-86b5-6742ba37fb95" />  
- Kết quả
  <img width="1436" height="336" alt="image" src="https://github.com/user-attachments/assets/369098b4-eaee-4f07-bd0e-ea52e4bd5fa9" />
Test API : Tìm kiếm sách
  Ví dụ : tìm kiếm bánh http://localhost:1880/timkiem?q=tin
  <img width="614" height="475" alt="image" src="https://github.com/user-attachments/assets/e84051e1-594a-41ad-8139-4fcd3425f6ea" />
2.6. Tạo giao diện front-end
  Web form gồm các file : index.html, nguyenthikimhue.js, nguyenthikimhue.css cả 3 file này đặt trong thư mục: D:\Apache\Apache24\nguyenthikimhue
  <img width="1919" height="1074" alt="image" src="https://github.com/user-attachments/assets/53327ce3-dbfe-4420-855c-594cc8784982" />
  Giao diện
  <img width="1919" height="1074" alt="image" src="https://github.com/user-attachments/assets/60a76b5a-571e-47c4-a5de-4c8d4d0aa2fb" />
Tìm kiếm
<img width="1918" height="1079" alt="image" src="https://github.com/user-attachments/assets/c2c51486-3337-4fbd-aa66-134ba22edd4e" />
2.7 Kết luận & tự đánh giá
 - Về quá trình cài đặt phần mềm và các thư viện:
Em đã hiểu rõ các bước cài đặt Node-RED, SQL Server và các gói thư viện cần thiết. Biết cách cấu hình kết nối cơ sở dữ liệu MSSQL trong Node-RED và kiểm tra hoạt động của API
Hiểu được quy trình cài đặt và cấu hình các công cụ cần thiết như Node.js, Node-RED, Microsoft SQL Server và các node mở rộng (node-mssql, node-file, node-http).
Quá trình cài đặt giúp em hiểu cách các thành phần này phối hợp để hình thành một hệ thống back-end hoàn chỉnh.
Biết cài thêm package vào Node-RED qua Manage Palette.
-Về sử dụng Node-RED để tạo API back-end:
Em đã hiểu rõ quy trình xử lý logic trong Node-RED khi tạo một API RESTful, cụ thể là:
Node HTTP In: nhận yêu cầu từ người dùng, ví dụ /timkiem?q=ten.
Node Function: xử lý logic, lấy tham số q từ URL, sau đó ghép vào câu truy vấn SQL
Node MSSQL: nhận câu truy vấn và thực hiện truy xuất dữ liệu từ SQL Server.
Node HTTP Response: gửi lại dữ liệu dạng JSON về cho client (trình duyệt hoặc ứng dụng).
Nhờ quy trình này, Em hiểu rằng Node-RED hoạt động như một server trung gian: nhận yêu cầu → truy xuất dữ liệu → xử lý → trả kết quả về.
-Hiểu cách front-end tương tác với back-end
Ở phía front-end, mình sử dụng HTML, CSS, JavaScript để tạo giao diện tìm kiếm và hiển thị kết quả.
HTML: tạo ô nhập liệu để người dùng nhập từ khóa.
CSS: định dạng bố cục, màu sắc, khoảng cách để trang web dễ nhìn.
JavaScript: đóng vai trò trung gian giữa giao diện và API Node-RED. Khi người dùng nhập từ khóa và nhấn tìm kiếm, JS sẽ gọi đến API
API trả về dữ liệu JSON chứa danh sách sách hoặc sản phẩm, sau đó JavaScript sẽ duyệt qua kết quả và hiển thị ra giao diện HTML.
Qua đó, Em hiểu được luồng tương tác giữa front-end và back-end:
Người dùng thao tác trên trình duyệt → JavaScript gửi yêu cầu đến API Node-RED → Node-RED truy xuất dữ liệu từ SQL Server → trả kết quả JSON → JavaScript nhận và hiển thị lên web.
* Sau bài làm, mình không chỉ hiểu cách cài đặt các phần mềm mà còn nắm được toàn bộ luồng xử lý dữ liệu trong một ứng dụng web đơn giản:
Database (SQL Server) → API (Node-RED) → Giao diện (HTML/CSS/JS).


  
















  





  




