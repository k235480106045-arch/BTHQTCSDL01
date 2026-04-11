# BTHQTCSDL01
BÀI TẬP MÔN HỆ QUẢN TRỊ CƠ SỞ DỮ LIỆU

Họ và tên: Chu Đặng Trung Long

MSSV: K235480106045

Lớp: K59KMT.K01

1.	Download và cài đặt SQL Server 2025, phiên bản Developer
Chọn phiên bản SQL Server 2025 Developer
Cài đặt với 2 kiểu login (Mixed Mode): Windows Authentication (nhớ Add Current User) và SQL Server Authentication (username mặc định là sa, chỉ cần nhập mật khẩu 123 , nhớ nhập 2 chỗ: Enter password và Confirm password)
## Hình minh họa
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/85cae8cf-aecc-4efc-921c-57ed992b2b98" />

3.	Cấu hình cho SQL Server làm việc ở cổng động (Dynamic Port), TCP: enable+active yes cho 127.0.0.1, chọn cổng động là 3xxxx với xxxx là 4 số cuối của mã số sv, (nếu cổng này đã mở sẵn trước đó bởi 1 ứng dụng khác thì chọn cổng là 4xxxx hoặc 5xxxx)
## Hình minh họa
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/a2d61038-23b4-4cdf-9b17-cabeb1621c76" />
3.	Kiểm tra xem service SQL Server có đang running và mở đúng cổng đã chọn hay không?
Sử dụng lệnh trên cmd: netstat -ano | findstr LISTENING để liệt kê các cổng mà máy tính đang mở,
Nếu thấy dòng: TCP 0.0.0.0:xxxxx với xxxxx là cổng đã chọn ở bước 2 là OK.

## Hình minh họa
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/973cb22b-1aa5-4bdb-822b-403ef1c07976" />
4.	Cài đặt SQL Server Management Studio
Link tải SSMS: https://learn.microsoft.com/en-us/ssms/install/install
5.	Chạy phần mềm ssms để Đăng nhập vào SQL Server bằng 2 cách: Windows Authentication và SQL Server Authentication.
Servername: localhost,xxxxx (với xxxxx là cổng đã chọn ở bước 2)

## Hình minh họa
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/73823063-7a23-41f7-9a23-1a7a0ebce070" />
6.	Sử dụng giao diện đồ hoạ của ssms: Tạo cơ sở dữ liệu mới (create database) với tên tuỳ ý, chọn Path (nơi lưu trữ db) cho file lưu dữ liệu và file lưu log ở ổ đĩa khác với ổ C. mở path đã chọn xem 2 file đã tạo ra.

## Hình minh họa
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/d35640e9-9107-48e6-93dd-c82bfd68eb1a" />
7.	Sử dụng giao diện đồ hoạ của ssms: Tạo bảng dữ liệu (create and design table) với tên bảng tuỳ ý, có các trường dữ liệu phù hợp với dữ liệu của file data mẫu (CSV), với Khoá chính (Primary Key) là trường masv

## Hình minh họa
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/3f25d757-683f-4352-92e0-441257f59389" />
8.	Sử dụng giao diện đồ hoạ của ssms: Tìm cách import dữ liệu từ file mẫu vào trong bảng vừa tạo.

## Hình minh họa
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/f9e7e0da-c387-40ad-a165-69e42bab6ffb" />
9.	Mở cửa sổ mới để gõ lệnh trong ssms: GÕ lệnh để kiểm tra xem số dòng của bảng dữ liệu sau khi import, kết quả ok sẽ khoảng 12020 dòng.

## Hình minh họa
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/a713c066-6d02-4af4-957d-a84701b950cd" />
10.	Trong cửa sổ mới để gõ lệnh: Gõ lệnh để thêm (insert) 1 row vào bảng, với dữ liệu là thông tin cá nhân của sv đang làm bài (mỗi sv sẽ luôn khác nhau ở bước này).

## Hình minh họa
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/b7406b99-684e-42db-a684-13c3382b9b5e" />
11.	Trong cửa sổ mới để gõ lệnh: Gõ lệnh để cập nhật(update) trường noisinh thành 'Sao Hoả' cho những dòng có noisinh và diachi đều là NULL.

## Hình minh họa
<img width="975" height="549" alt="image" src="https://github.com/user-attachments/assets/bd7e6189-eab4-46d4-b2e5-7bd848c414bd" />
12.	Sử dụng giao diện đồ hoạ của ssms: Tạo bảng SaoHoa gồm những sinh viên có nơi sinh ở 'Sao Hoả', keyword gợi ý: sử dụng 1 câu lệnh: SELECT + INTO

## Hình minh họa
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/16aeb15a-86df-4109-b515-05ec6059bf14" />
13.	Trong cửa sổ mới để gõ lệnh: Gõ lệnh xoá (delete) trong bảng SaoHoa những sinh viên cùng họ với em, em họ Chu thì xoá những sv họ Chu.

## Hình minh họa
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/931f8f2e-d92d-4b81-a68f-d7c7844f111e" />
14.	Sử dụng giao diện đồ hoạ của ssms: Xuất toàn bộ kết quả của các bước 6,7,8,9,10,11,12,13 ra file dulieu.sql , keyword gợi ý: sử dụng tính năng GEN SCRIPT struct+data cho database

## Hình minh họa
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/42b124f6-f523-44a1-a061-81bcdabee081" />
15.	Sử dụng giao diện đồ hoạ của ssms: Xoá csdl đã tạo, sau khi xoá thành công, kiểm tra tại path (path chọn ở bước 6) xem còn tồn tại 2 file của bước 6 không?

## Hình minh họa
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/7767d313-e1f4-4229-80a2-74180428a1d7" />
16.	Tạo cửa sổ mới để gõ lệnh: mở file dulieu.sql của bước 15, chạy toàn bộ các lệnh này. REFRESH lại cây liệt kê các database => kiểm chứng kết quả được tạo ra tương đương với các bước 6,7,8,9,10,11,12,13.

## Hình minh họa
<img width="975" height="550" alt="image" src="https://github.com/user-attachments/assets/542fffe2-b2fb-45db-83ec-50c508e8b942" />
17.	upload file dulieu.sql lên github repository của em (repository mà em đang edit file README.md)




