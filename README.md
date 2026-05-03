# BÀI KIỂM TRA SỐ 2

## Hệ Quản Trị Cơ SỞ DỮ LIỆU (SQL Server)

---

## Thông tin sinh viên

* Họ và tên: Chu Đăng Trung Long
* Mã sinh viên: K235480106045
* Lớp: K59KMT.

---

# PHẦN 1: TẠO CSDL VÀ NHẬP DỮ LIỆU

## 1. Tạo database

## Ảnh minh họa
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/1c2511eb-bebd-43c7-8ffa-01d8f92066e2" />


---

## 2. Tạo bảng

### Bảng DanhSachMonHoc

```sql
CREATE TABLE [DanhSachMonHoc] (
    [MaMonHoc] INT PRIMARY KEY IDENTITY(1,1),
    [TenMonHoc] NVARCHAR(100) NOT NULL,
    [SoTinChi] TINYINT CHECK ([SoTinChi] > 0),
    [HocPhi] MONEY
);
```

Giải thích:

* MaMonHoc: khóa chính, tự tăng
* TenMonHoc: tên môn, không được NULL
* SoTinChi: phải > 0 (ràng buộc CHECK)
* HocPhi: kiểu tiền

## Ảnh minh họa
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/21973556-651e-47b6-9610-f466c4cd5ff6" />


---

### Bảng SinhVien

```sql
CREATE TABLE [SinhVien] (
    [MaSinhVien] VARCHAR(20) PRIMARY KEY,
    [HoTen] NVARCHAR(100) NOT NULL,
    [NgaySinh] DATE,
    [Email] VARCHAR(100) UNIQUE
);
```

Giải thích:

* MaSinhVien: khóa chính
* Email: không được trùng (UNIQUE)

## Ảnh minh họa
<img width="975" height="547" alt="image" src="https://github.com/user-attachments/assets/0805abf3-6e53-437e-9e57-0d4749666ba6" />
  

---

### Bảng DangKyHoc

```sql
CREATE TABLE [DangKyHoc] (
    [MaDangKy] INT PRIMARY KEY IDENTITY(1,1),
    [MaSinhVien] VARCHAR(20),
    [MaMonHoc] INT,
    [NgayDangKy] DATETIME DEFAULT GETDATE(),
    [DiemTongKet] FLOAT CHECK ([DiemTongKet] BETWEEN 0 AND 10),

    FOREIGN KEY ([MaSinhVien]) REFERENCES [SinhVien]([MaSinhVien]),
    FOREIGN KEY ([MaMonHoc]) REFERENCES [DanhSachMonHoc]([MaMonHoc])
);
GO
```

Giải thích:

* Đây là bảng trung gian (quan hệ nhiều-nhiều)
* FOREIGN KEY đảm bảo dữ liệu hợp lệ
* DEFAULT GETDATE(): tự động lấy ngày hiện tại
* Điểm phải từ 0 đến 10

## ẢNh minh họa
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/e787a05e-4fab-4fec-b72a-de75ed8560c0" />


---

## 3. Nhập dữ liệu mẫu

```sql
INSERT INTO [DanhSachMonHoc] VALUES
(N'Kiến trúc Máy tính',3,1200000),
(N'Xử lý Tín hiệu Số',3,1200000),
(N'Thiết kế Mạch điện tử',4,1600000);

INSERT INTO [SinhVien] VALUES
('K61001',N'Trần Anh Minh','2004-05-12','minhta@gmail.com'),
('K61002',N'Lê Ngọc Ánh','2004-08-20','anhln@gmail.com'),
('K61003',N'Nguyễn Hữu Toàn','2004-11-05','toannh@gmail.com');

INSERT INTO [DangKyHoc] VALUES
('K61001',1,'2026-01-15',8.5),
('K61001',2,'2026-01-16',7.0),
('K61002',3,'2026-01-15',9.0),
('K61003',1,'2026-01-18',6.5);
GO
```

## Ảnh minh họa
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/1140dd2c-b112-4d7f-aa43-02fefbabae97" />


---

# PHẦN 2: FUNCTION

## 1. Lý thuyết

* System Function: hàm có sẵn (GETDATE, DATEDIFF, LEN...)

* Ví dụ:
  DATEDIFF(YEAR, NgaySinh, GETDATE()) dùng để tính tuổi

* User Defined Function (UDF):

  * Tự định nghĩa
  * Dùng để tái sử dụng logic phức tạp

---

## 2. Scalar Function

```sql
CREATE FUNCTION fn_TongHocPhiSinhVien (@MaSV VARCHAR(20))
RETURNS MONEY
AS
BEGIN
    DECLARE @Tong MONEY;

    SELECT @Tong = SUM(m.HocPhi)
    FROM DangKyHoc d
    JOIN DanhSachMonHoc m ON d.MaMonHoc = m.MaMonHoc
    WHERE d.MaSinhVien = @MaSV;

    RETURN ISNULL(@Tong,0);
END;
GO
```

Giải thích:

* Hàm trả về 1 giá trị (MONEY)
* Tính tổng học phí của sinh viên

## ẢNh minh họa
<img width="975" height="554" alt="image" src="https://github.com/user-attachments/assets/79bea122-3002-4deb-9417-40a5223a3b38" />


Test:

```sql
SELECT HoTen, dbo.fn_TongHocPhiSinhVien(MaSinhVien) FROM SinhVien;
```

* Kết quả hiển thị 

## Ảnh minh họa
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/149b6ec9-1428-4d9e-ad4e-29683861426c" />


---

## 3. Inline Table Function

```sql
CREATE FUNCTION fn_DanhSachLopHoc (@MaMH INT)
RETURNS TABLE
AS
RETURN (
    SELECT s.MaSinhVien, s.HoTen, d.DiemTongKet
    FROM SinhVien s
    JOIN DangKyHoc d ON s.MaSinhVien = d.MaSinhVien
    WHERE d.MaMonHoc = @MaMH
);
GO
```

Giải thích:

* Trả về bảng
* Lấy danh sách sinh viên của 1 môn

## Ảnh minh họa
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/152bcf11-bdaa-4fc1-a409-d477fca4fd34" />


Test:

```sql
SELECT * FROM dbo.fn_DanhSachLopHoc(1);
```

* Kết quả hiển thị

## Ảnh minh họa
<img width="975" height="549" alt="image" src="https://github.com/user-attachments/assets/215ae20e-0e3f-46af-8043-f020a1feb893" />


---

## 4. Multi-statement Function

```sql
CREATE FUNCTION fn_DanhGiaQuaMon (@MaMH INT)
RETURNS @KQ TABLE (
    MaSinhVien VARCHAR(20),
    HoTen NVARCHAR(100),
    TrangThai NVARCHAR(50)
)
AS
BEGIN
    INSERT INTO @KQ
    SELECT s.MaSinhVien, s.HoTen,
           CASE 
                WHEN d.DiemTongKet >= 4 THEN N'Đạt'
                ELSE N'Học lại'
           END
    FROM SinhVien s
    JOIN DangKyHoc d ON s.MaSinhVien = d.MaSinhVien
    WHERE d.MaMonHoc = @MaMH;

    RETURN;
END;
GO
```

Giải thích:

* Dùng biến bảng @KQ
* CASE để phân loại đạt / học lại

## Ảnh minh họa
<img width="975" height="549" alt="image" src="https://github.com/user-attachments/assets/ca07d7a4-b847-45ec-b5b5-b7d338159076" />


Test:

```sql
SELECT * FROM dbo.fn_DanhGiaQuaMon(1);
```

* kết quả hiển thị

## Ảnh minh họa
<img width="975" height="549" alt="image" src="https://github.com/user-attachments/assets/2b9e6cb2-e1a4-46e8-acac-02cf2c34ec41" />


---

# PHẦN 3: STORED PROCEDURE

## 1. Lý thuyết

* SP hệ thống:

```sql
EXEC sp_help 'SinhVien';
```

Dùng để xem cấu trúc bảng

---

## 2. SP đăng ký môn

```sql
CREATE PROCEDURE sp_DangKyMoi
    @MaSV VARCHAR(20), @MaMH INT
AS
BEGIN
    IF NOT EXISTS (SELECT 1 FROM SinhVien WHERE MaSinhVien = @MaSV)
        PRINT N'Sinh viên không tồn tại';
    ELSE IF NOT EXISTS (SELECT 1 FROM DanhSachMonHoc WHERE MaMonHoc = @MaMH)
        PRINT N'Môn học không tồn tại';
    ELSE
    BEGIN
        INSERT INTO DangKyHoc(MaSinhVien, MaMonHoc)
        VALUES (@MaSV, @MaMH);

        PRINT N'Đăng ký thành công';
    END
END;
GO
```

## Ảnh minh họa
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/6586c749-dfe4-4b34-a308-6ab10c1c3654" />


Test:

```sql
EXEC sp_DangKyMoi 'K61002',1;
EXEC sp_DangKyMoi 'K99999',1;
```

## Ảnh minh họa
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/a9d3121d-a8bf-4151-a045-1bc6010eb213" />


* 1 lần thành công
* 1 lần báo lỗi

---

## 3. SP OUTPUT

```sql
CREATE PROCEDURE sp_DemSoMonHoc
    @MaSV VARCHAR(20),
    @SoLuong INT OUTPUT
AS
BEGIN
    SELECT @SoLuong = COUNT(*)
    FROM DangKyHoc
    WHERE MaSinhVien = @MaSV;
END;
GO
```
## Ảnh minh họa
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/795dd97e-716d-401e-b0e7-ddac92499e2c" />


Test:

```sql
DECLARE @KQ INT;
EXEC sp_DemSoMonHoc 'K61001', @KQ OUTPUT;
PRINT @KQ;
```

## Ảnh minh họa
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/721f6712-b522-457d-aa5e-7e927c4b7933" />


* Giá trị đếm được

---

## 4. SP bảng điểm

```sql
CREATE PROCEDURE sp_XemBangDiem (@MaSV VARCHAR(20))
AS
BEGIN
    SELECT s.HoTen, m.TenMonHoc, d.DiemTongKet
    FROM DangKyHoc d
    JOIN SinhVien s ON d.MaSinhVien = s.MaSinhVien
    JOIN DanhSachMonHoc m ON d.MaMonHoc = m.MaMonHoc
    WHERE d.MaSinhVien = @MaSV;
END;
GO
```
## Ảnh minh họa
<img width="975" height="547" alt="image" src="https://github.com/user-attachments/assets/39aba98d-f595-4f0a-b97e-33dd684c7123" />


Test:

```sql
EXEC sp_XemBangDiem 'K61001';
```

## Ảnh minh họa
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/fd9c27f1-c4f4-4e8e-97ba-f209a7e236f7" />


* Bảng điểm đầy đủ

---

# PHẦN 4: TRIGGER

```sql
CREATE TABLE LogXoaDangKy (
    LogID INT IDENTITY PRIMARY KEY,
    MaSinhVien VARCHAR(20),
    MaMonHoc INT,
    ThoiGianXoa DATETIME DEFAULT GETDATE()
);
GO

CREATE TRIGGER trg_LuuVetXoaDangKy
ON DangKyHoc
AFTER DELETE
AS
BEGIN
    INSERT INTO LogXoaDangKy(MaSinhVien, MaMonHoc)
    SELECT MaSinhVien, MaMonHoc FROM deleted;
END;
GO
```
## Ảnh minh họa
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/4128b20f-c9ca-4e8f-bdfe-f3466579beb4" />
<img width="975" height="547" alt="image" src="https://github.com/user-attachments/assets/b24b881c-8564-440d-8ed5-904fc95e023f" />


Test:

```sql
DELETE FROM DangKyHoc
WHERE MaSinhVien = 'K61003' AND MaMonHoc = 1;

SELECT * FROM LogXoaDangKy;
```

## Ảnh minh họa
<img width="975" height="552" alt="image" src="https://github.com/user-attachments/assets/67632492-4682-4b73-824f-f35ff87085ee" />


* Bảng log có dữ liệu sau khi xóa

---

# PHẦN 5: CURSOR

```sql
INSERT INTO DangKyHoc (MaSinhVien, MaMonHoc, DiemTongKet)
VALUES ('K61003',2,NULL);
GO

DECLARE @MaDK INT;

DECLARE cur CURSOR FOR
SELECT MaDangKy FROM DangKyHoc WHERE DiemTongKet IS NULL;

OPEN cur;
FETCH NEXT FROM cur INTO @MaDK;

WHILE @@FETCH_STATUS = 0
BEGIN
    UPDATE DangKyHoc
    SET DiemTongKet = 0
    WHERE MaDangKy = @MaDK;

    FETCH NEXT FROM cur INTO @MaDK;
END

CLOSE cur;
DEALLOCATE cur;
GO
```

## Ảnh minh họa
<img width="975" height="550" alt="image" src="https://github.com/user-attachments/assets/46602660-c7af-4343-b92b-8d4450505b99" />


## Ảnh minh họa
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/d2eea3a9-3181-430b-83fd-87655963ae5e" />


* Điểm NULL đã thành 0

---

## Cách tối ưu (không dùng Cursor)

```sql
UPDATE DangKyHoc
SET DiemTongKet = 0
WHERE DiemTongKet IS NULL;
```

Giải thích:

* Nhanh hơn nhiều
* Không duyệt từng dòng
* Tận dụng engine SQL

---
