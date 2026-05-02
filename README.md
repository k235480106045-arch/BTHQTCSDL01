# Bài kiểm tra số 2: Hệ Quản Trị Cơ Sở Dữ Liệu (SQL Server)
## PHẦN MỞ ĐẦU
**Thông tin sinh viên:**

- Họ và tên: Chu Đặng Trung Long

- Mã sinh viên: K235480106045

- Lớp: K59KMT.K01
  
- Khoa: Điện tử
  
- Môn học: Hệ Quản Trị Cơ Sở Dữ Liệu (SQL Server)

**Yêu Cầu Đầu Bài**


Đề tài: Quản lý đăng ký môn học

Thực hiện xây dựng một hệ thống quản lý đăng ký học tín chỉ hoàn chỉnh trên SQL Server, đáp ứng các tiêu chí cụ thể sau đây:

- Toàn bộ quá trình thực hiện phải được ghi lại thông qua các screenshot minh họa, mỗi bức ảnh đều đi kèm với câu lệnh SQL tương ứng và chú thích rõ ràng về chức năng, mục đích xử lý, cũng như kết quả đạt được.
  
- Bài tập được nộp dưới dạng repository GitHub (public), bao gồm hai thành phần chính: tệp README.md chứa toàn bộ nội dung, hình ảnh minh họa và giải thích chi tiết, tệp baikiemtra2.sql chứa toàn bộ mã SQL.

  
Đánh giá dựa trên ba yếu tố quan trọng:
- Logic xử lý dữ liệu — các câu lệnh có giải quyết đúng bài toán không
  
- Quy tắc đặt tên — các bảng, cột, hàm có tuân theo chuẩn bướu Lạc Đà không
  
- Commit history — quá trình phát triển có rõ ràng, có thể theo dõi được không. Deadline nộp bài là cuối kỳ, sinh viên cần hoàn thành và push lên GitHub trước ngày hết hạn.

**Giới Thiệu Về Hệ Thống Quản Lý Đăng Ký Môn Học**


Xây dựng một hệ thống quản lý tín chỉ (QuanLyDangKyMonHoc) từ nền tảng SQL Server, bao gồm các chức năng chủ yếu như quản lý thông tin sinh viên, quản lý danh mục môn học, và quản lý các lượt đăng ký học. Mỗi sinh viên đều có hồ sơ lưu trữ với các thông tin cá nhân, email liên hệ, ngày sinh; mỗi môn học được phân loại theo số tín chỉ, có mức học phí riêng biệt; các lượt đăng ký lưu giữ mối quan hệ giữa sinh viên và môn học, cùng với thời gian đăng ký và điểm tổng kết cuối kỳ.

Toàn bộ bài làm được chia thành 5 phần chính theo thứ tự tăng độ phức tạp. 

- Thiết Kế Cơ Sở Dữ Liệu — khởi tạo các bảng SinhVien, DanhSachMonHoc, DangKyHoc với các ràng buộc toàn vẹn (Primary Key, Foreign Key, Check Constraint, Unique) và chèn dữ liệu mẫu. 
- Xây Dựng Function — tạo các hàm tính toán như tính điểm trung bình, tìm danh sách lớp, đánh giá qua môn; những hàm này giúp tái sử dụng logic và làm sạch mã. 
- Xây Dựng Stored Procedure — tạo các thủ tục lưu trữ để xử lý các nghiệp vụ phức tạp như đăng ký nhanh, quay thưởng giảm học phí, báo cáo sinh viên nợ môn.
- Trigger Xử Lý Nghiệp Vụ — định nghĩa các trigger tự động kích hoạt khi dữ liệu thay đổi (chẳng hạn, tự động xóa đăng ký khi sinh viên bị đình chỉ, hoặc hoàn tiền tự động). 
- Dùng Cursor và Xử Lý Dữ Liệu — sử dụng cursor để duyệt từng bản ghi và thực hiện các xử lý tuần tự (như phát học bổng), đồng thời so sánh hiệu năng giữa phương pháp cursor và phương pháp set-based, từ đó rút ra kết luận về tối ưu hóa.

Trong quá trình thực hiện, sẽ thiết kế cơ sở dữ liệu với tên chuẩn: QuanLyDangKyMonHoc_K235480106045. Mỗi phần lệnh SQL viết ra sẽ đi kèm một ảnh screenshot chứa mã lệnh và kết quả thực thi. Thông qua bài tập này sẽ nắm vững các khái niệm cơ bản và nâng cao của SQL Server, từ DDL đến DML, từ những truy vấn đơn giản đến những xử lý phức tạp bằng Function, Procedure, Trigger, và Cursor.

---

### Phần 1: Khởi tạo database và bảng
- Tạo cơ sở dữ liệu
<>


<p align="center">Tạo cơ sở dữ liệu QuanLyDangKyMonHoc</p>

- Bảng [DanhSachMonHoc]:

Sử dụng MaMonHoc làm Khóa chính (PK), tự động tăng (IDENTITY(1,1)).
Tên môn học dùng NVARCHAR để hỗ trợ tiếng Việt có dấu.
Số tín chỉ (SoTinChi) có ràng buộc CHECK phải lớn hơn 0. Kiểu MONEY cho học phí.
```sql
CREATE DATABASE [QuanLyDangKyMonHoc_K61CNBVMK02];
GO
USE [QuanLyDangKyMonHoc_K61CNBVMK02];
GO

CREATE TABLE [DanhSachMonHoc] (
    [MaMonHoc] INT PRIMARY KEY IDENTITY(1,1),
    [TenMonHoc] NVARCHAR(100) NOT NULL,        
    [SoTinChi] TINYINT CHECK ([SoTinChi] > 0),
    [HocPhi] MONEY                             
);
