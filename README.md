# LAB 2: MÃ HÓA ĐỐI XỨNG HIỆN ĐẠI (MODERN SYMMETRIC ENCRYPTION)

Ứng dụng thực hành học phần An toàn và Bảo mật Thông tin triển khai các thuật toán mã hóa đối xứng hiện đại trên nền tảng Java Swing (Maven) kết hợp Microsoft SQL Server lưu trữ người dùng/lịch sử thao tác, cùng ứng dụng giao diện Python (Tkinter) cho chế độ AES-256-CBC.

## Thông tin sinh viên
- Họ và tên: Lương Hào Minh
- MSSV: 1150080066
- Lớp: 11_ĐH_CNPM1
- Đơn vị: Khoa Công nghệ Thông tin – Trường Đại học Tài nguyên và Môi trường TP.HCM

---

## Công nghệ & Thư viện sử dụng
- Java Development Kit (JDK): JDK 17+
- Build Tool: Apache Maven (`groupId: vn.edu.lab`, `artifactId: LAB2_MaHoaHienDai`)
- Thư viện Java:
  - `com.microsoft.sqlserver:mssql-jdbc:12.8.1.jre11` (Kết nối JDBC tới SQL Server)
  - `org.bouncycastle:bcprov-jdk18on:1.78.1` (Cài đặt thuật toán mã khối Twofish)
- Cơ sở dữ liệu: Microsoft SQL Server (Cổng TCP tĩnh `14333`)
- Python: Python 3.10+ với thư viện `cryptography` và giao diện `tkinter`

---

## Cấu trúc dự án

```text
├── LAB2_MaHoaHienDai/
│   ├── pom.xml
│   └── src/main/java/week_02/
│       ├── CipherUtil.java         # Tiện ích chuyển đổi UTF-8, Base64, HEX, SecureRandom, PBKDF2
│       ├── FileTextIO.java         # Đọc/ghi tệp văn bản .txt và khôi phục tham số IV/Nonce
│       ├── DbConfig.java           # Quản lý cấu hình kết nối JDBC SQL Server
│       ├── HistoryDao.java         # Truy vấn & ghi log bảng dbo.crypto_history
│       ├── UserDao.java            # Đăng ký, đăng nhập (PBKDF2-HMAC-SHA256) & cấp Ticket 128-bit
│       ├── DESCipher.java          # Thuật toán DES/CBC/PKCS5Padding
│       ├── TripleDESCipher.java    # Thuật toán DESede/CBC/PKCS5Padding
│       ├── AESCipher.java          # Thuật toán AES-256/GCM/NoPadding
│       ├── RC4Cipher.java          # Thuật toán mã dòng RC4 (KSA + PRGA)
│       ├── TwofishCipher.java      # Thuật toán Twofish/CBC/PKCS7 (BouncyCastle)
│       ├── AESCTRCipher.java       # Thuật toán AES-256/CTR/NoPadding
│       ├── frm_Menu.java           # Cửa sổ điều hướng chính (Main Class)
│       ├── frm_KetNoiSQL.java      # Giao diện cấu hình và kiểm tra kết nối SQL Server
│       ├── frm_LichSu.java         # Giao diện xem lịch sử mã hóa/giải mã từ SQL Server
│       ├── frm_DES.java            # Giao diện Bài 2.1 - DES
│       ├── frm_TripleDES.java      # Giao diện Bài 2.2 - TripleDES
│       ├── frm_AES.java            # Giao diện Bài 2.3 - AES-GCM & Register/Login
│       ├── frm_RC4.java            # Giao diện Bài 2.4 - RC4
│       ├── frm_Twofish.java        # Giao diện Bài 2.5 - Twofish
│       └── frm_AES_CTR.java        # Giao diện Bài 2.6 - AES-CTR
├── LAB2_Python/
│   └── aes_cbc_gui.py              # Bài nâng cao 02: AES-256-CBC (Tkinter + self-test)
└── sql/
    ├── 01_LAB2_Full_Schema_Data.sql
    └── 02_Tao_Tai_Khoan_SQL.sql
```

---

## Hướng dẫn cài đặt và chạy chương trình

### 1. Thiết lập Cơ sở dữ liệu (SQL Server)
1. Bật giao thức TCP/IP trong SQL Server Configuration Manager, đặt cổng tĩnh `TCP Port = 14333` (xóa trống mục TCP Dynamic Ports), bật chế độ `SQL Server and Windows Authentication mode` và khởi động lại dịch vụ SQL Server.
2. Mở SQL Server Management Studio (SSMS) và chạy lần lượt 2 tệp trong thư mục `sql/`:
   - `01_LAB2_Full_Schema_Data.sql`: Tạo database `LAB2_MaHoaHienDai` cùng 2 bảng `dbo.app_users` và `dbo.crypto_history`.
   - `02_Tao_Tai_Khoan_SQL.sql`: Tạo tài khoản đăng nhập `lab2_user` (Mật khẩu: `LabUser@2026`).

### 2. Chạy ứng dụng Java (NetBeans / Maven)
1. Mở project `LAB2_MaHoaHienDai` trong Apache NetBeans.
2. Nhấp phải project chọn Clean and Build (`Shift + F11`) để Maven tải thư viện `mssql-jdbc` và `bcprov-jdk18on`.
3. Bấm Run Project (`F6`) để khởi chạy giao diện chính `week_02.frm_Menu`.
4. Chọn Kết nối SQL Server, nhập mật khẩu `LabUser@2026` và bấm Test Connection trước khi sử dụng chức năng Đăng ký/Đăng nhập hoặc lưu lịch sử.

### 3. Chạy ứng dụng Python (Bài nâng cao 02 – AES-CBC)
Mở terminal tại thư mục `LAB2_Python` và chạy các lệnh sau:

```bash
# Cài đặt thư viện cryptography
py -m pip install cryptography

# Chạy bộ kiểm thử tự động
py aes_cbc_gui.py --self-test

# Khởi chạy giao diện đồ họa
py aes_cbc_gui.py
```

---

## Danh sách chức năng & Bộ dữ liệu kiểm thử

| Bài | Thuật toán / Chế độ | Dữ liệu kiểm thử chuẩn | Kết quả kỳ vọng |
| :--- | :--- | :--- | :--- |
| 2.1 | DES (`DES/CBC/PKCS5Padding`) | Plaintext: `Xin chào Lab 2 - Mã hóa hiện đại!`<br>Key: `12345678` (8 byte) | Giải mã khôi phục đúng bản rõ; mỗi lần mã hóa sinh IV 8 byte ngẫu nhiên nên tạo bản mã Base64 khác nhau. |
| 2.2 | TripleDES (`DESede/CBC/PKCS5Padding`) | Key: `123456789012345678901234` (24 byte) | Mã hóa/giải mã chính xác; nhập khóa thiếu/thừa độ dài (ví dụ 23 ký tự) hệ thống báo lỗi hợp lệ. |
| 2.3 | AES-256-GCM + Auth | Đăng ký & Đăng nhập tài khoản mới (`>= 8` ký tự) | Cấp Ticket ngẫu nhiên 128-bit (32 ký tự HEX); ghi log vào SQL Server; sửa 1 bit bản mã bị từ chối (`AEADBadTagException`). |
| 2.4 | RC4 (Stream Cipher) | Plaintext: `Plaintext`<br>Key: `Key` | Bản mã HEX xuất ra chính xác: `BBF316E8D940AF0AD3`. |
| 2.5 | Twofish (CBC + PKCS7) | Key: `Twofish@Lab2`<br>IV: Sinh bằng Generate IV (16 byte) | Giải mã đúng với cặp Key + IV; chức năng Save File / Open File lưu và khôi phục đúng IV cùng bản mã. |
| 2.6 | AES-256-CTR (Java) | Key 32 byte + Nonce 12 byte (Base64) | Không cần đệm (NoPadding), độ dài bản mã bằng đúng độ dài bản rõ. |
| 2.6 | AES-256-CBC (Python) | Văn bản tiếng Việt dài (`> 1000` ký tự) | Giải mã toàn văn chính xác; chế độ `--self-test` trả về `SELF-TEST OK`. |