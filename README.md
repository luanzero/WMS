# 🧾 HỆ THỐNG ĐĂNG NHẬP – ĐĂNG KÝ VÀ QUẢN LÝ VÍ ĐIỂM

## 🧩 Giới thiệu dự án

Dự án xây dựng một hệ thống phần mềm bằng C++ phục vụ cho việc:
- Đăng ký và đăng nhập tài khoản người dùng với xác thực bảo mật cao (có OTP).
- Phân quyền người dùng (thông thường và quản lý).
- Quản lý thông tin cá nhân, thay đổi mật khẩu, theo dõi lịch sử hoạt động.
- Hệ thống ví điểm với khả năng chuyển điểm giữa các ví, có xác thực OTP, bảo đảm tính toàn vẹn (giao dịch atom).
- Lưu trữ thông tin vào tệp và sao lưu dữ liệu đầy đủ.

## 👨‍💻 Thành viên và phân công công việc

| Họ tên             | Nhiệm vụ chính                                                                 |
|--------------------|--------------------------------------------------------------------------------|
| Nguyễn Văn A        | Thiết kế cấu trúc dữ liệu, viết chức năng đăng ký, đăng nhập, thay đổi mật khẩu |
| Trần Thị B          | Xây dựng tính năng ví điểm, giao dịch điểm, kiểm tra số dư, xử lý atom         |
| Lê Văn C            | OTP, xử lý xác thực 2 lớp, bảo mật hệ thống                                    |
| Phạm Thị D          | Phân quyền người dùng, chức năng quản lý, lịch sử giao dịch                    |
| Nguyễn Văn E        | Lưu trữ dữ liệu vào tệp, quản lý backup, hướng dẫn sử dụng, viết tài liệu      |

## 📌 Đặc tả chức năng

## 📌 Đặc tả Chức năng: Tài khoản Người dùng

### 1.1 Đăng ký tài khoản
- **Mô tả:** Cho phép người dùng tạo mới tài khoản bằng cách nhập thông tin cá nhân như tên đăng nhập, mật khẩu, họ tên, email.
- **Điều kiện:**
  - Tên đăng nhập không được trùng với bất kỳ tài khoản đã tồn tại.
  - Mật khẩu phải đủ độ mạnh (có thể yêu cầu tối thiểu 6 ký tự, gồm chữ + số).
- **Chức năng phụ:**
  - Nếu người dùng không nhập mật khẩu, hệ thống sẽ sinh mật khẩu tự động và bật cờ `forcePasswordChange = true`.

---

### 1.2 Đăng nhập hệ thống
- **Mô tả:** Cho phép người dùng đăng nhập vào hệ thống bằng tên đăng nhập và mật khẩu.
- **Xử lý:**
  - So khớp tên đăng nhập và mật khẩu đã băm với dữ liệu đã lưu.
  - Nếu `forcePasswordChange = true`, chuyển hướng người dùng đến chức năng thay đổi mật khẩu trước khi truy cập hệ thống.

---

### 1.3 Đổi mật khẩu
- **Mô tả:** Người dùng có thể thay đổi mật khẩu cũ sang mật khẩu mới.
- **Điều kiện:**
  - Xác minh đúng mật khẩu cũ.
  - Mật khẩu mới không được trùng mật khẩu cũ.
- **Bảo mật:**
  - OTP gửi đến email để xác nhận yêu cầu đổi mật khẩu.

---

### 1.4 Cập nhật thông tin cá nhân
- **Mô tả:** Người dùng có thể cập nhật các trường như họ tên, email.
- **Lưu ý:**
  - Không được phép thay đổi tên đăng nhập.
- **Xác thực:**
  - Hệ thống gửi OTP đến email hiện tại của người dùng để xác nhận cập nhật.
  - Chỉ khi mã OTP hợp lệ, thay đổi mới được ghi nhận.

---

### 1.5 Phân quyền người dùng
- **Loại 1:** Người dùng thường:
  - Truy cập thông tin cá nhân của mình.
  - Được phép đổi mật khẩu, cập nhật thông tin cá nhân (với OTP).
- **Loại 2:** Người quản lý:
  - Có quyền tạo tài khoản hộ người dùng.
  - Xem danh sách tài khoản.
  - Cập nhật thông tin tài khoản khác (với OTP xác nhận của chủ tài khoản).

---

### 1.6 Lưu trữ và sao lưu dữ liệu người dùng
- **Lưu trữ:** 
  - Dữ liệu người dùng được lưu vào file (có thể là một file chung hoặc từng file riêng cho mỗi người dùng).
  - Mật khẩu lưu dưới dạng mã băm SHA-256.
- **Sao lưu:** 
  - Tự động sao lưu dữ liệu người dùng vào thư mục backup định kỳ.
  - Có chức năng phục hồi dữ liệu từ bản sao lưu gần nhất khi có sự cố.

### 2. Bảo mật
- Mật khẩu được lưu dưới dạng băm (hash)
- OTP gửi qua email/console để xác thực các thay đổi nhạy cảm
- Thay đổi mật khẩu bắt buộc với mật khẩu tự sinh

### 3. Quản lý ví điểm
- Mỗi người dùng có 1 ví điểm
- Giao dịch chuyển điểm yêu cầu OTP xác nhận
- Giao dịch atom: chỉ thực hiện nếu cả hai thao tác (trừ và cộng điểm) đều thành công
- Lịch sử giao dịch lưu theo từng người dùng

## 📋 Bảng Cấu trúc Dữ liệu

Bảng dưới đây mô tả chi tiết các lớp dữ liệu chính được sử dụng trong hệ thống: `User`, `Wallet`, `Transaction`, `OTPManager`, và `DataManager`.  
Mỗi bảng con tương ứng với một lớp, liệt kê các thuộc tính hoặc phương thức chính, cùng với kiểu dữ liệu và mô tả rõ ràng giúp lập trình viên dễ dàng triển khai, mở rộng và bảo trì hệ thống.

---

### 1. `User`

> Lớp `User` lưu trữ thông tin tài khoản người dùng, bao gồm thông tin đăng nhập, vai trò và ví điểm gắn liền với người đó. Ngoài ra, người dùng còn có thể được yêu cầu đổi mật khẩu khi hệ thống sinh mật khẩu tự động.

| STT | Tên trường           | Kiểu dữ liệu       | Mô tả chi tiết                                         |
|-----|----------------------|--------------------|--------------------------------------------------------|
| 1   | username             | std::string        | Tên đăng nhập duy nhất                                |
| 2   | hashedPassword       | std::string        | Mật khẩu đã băm (SHA-256)                             |
| 3   | role                 | std::string        | Vai trò người dùng: `"user"` hoặc `"admin"`           |
| 4   | fullName             | std::string        | Họ tên đầy đủ                                          |
| 5   | email                | std::string        | Địa chỉ email để gửi mã OTP                           |
| 6   | forcePasswordChange | bool               | Cờ yêu cầu đổi mật khẩu nếu là mật khẩu tự sinh       |
| 7   | wallet               | Wallet             | Ví điểm gắn với người dùng                            |

---

### 2. `Wallet`

> Lớp `Wallet` đại diện cho một ví điểm gắn với mỗi người dùng. Mỗi ví có mã định danh duy nhất, số dư điểm và danh sách giao dịch từng thực hiện.

| STT | Tên trường           | Kiểu dữ liệu       | Mô tả chi tiết                                         |
|-----|----------------------|--------------------|--------------------------------------------------------|
| 1   | walletId             | std::string        | Mã định danh ví duy nhất                               |
| 2   | balance              | int                | Số điểm hiện tại trong ví                              |
| 3   | transactionHistory   | std::vector<Transaction> | Danh sách các giao dịch đã thực hiện              |

---

### 3. `Transaction`

> Lớp `Transaction` ghi lại một giao dịch điểm giữa hai ví. Thông tin gồm mã ví nguồn, ví đích, số điểm, thời gian và trạng thái giao dịch.

| STT | Tên trường           | Kiểu dữ liệu       | Mô tả chi tiết                                         |
|-----|----------------------|--------------------|--------------------------------------------------------|
| 1   | fromWalletId         | std::string        | Mã ví nguồn                                            |
| 2   | toWalletId           | std::string        | Mã ví đích                                             |
| 3   | amount               | int                | Số điểm giao dịch                                      |
| 4   | timestamp            | std::string        | Thời điểm thực hiện giao dịch                          |
| 5   | status               | std::string        | Trạng thái: `"success"`, `"failed"`, `"pending"`       |

---

### 4. `OTPManager`

> Lớp `OTPManager` đảm nhiệm việc sinh và xác thực mã OTP dùng để bảo vệ các thao tác nhạy cảm như cập nhật thông tin hoặc thực hiện giao dịch điểm.

| STT | Tên hàm              | Kiểu trả về        | Mô tả chi tiết                                         |
|-----|----------------------|--------------------|--------------------------------------------------------|
| 1   | generateOTP          | std::string        | Tạo mã OTP cho người dùng                              |
| 2   | validateOTP          | bool               | Xác thực mã OTP đã nhập                                |

---

### 5. `DataManager`

> Lớp `DataManager` hỗ trợ lưu trữ và quản lý dữ liệu người dùng, bao gồm chức năng sao lưu và phục hồi để đảm bảo an toàn dữ liệu.

| STT | Tên hàm              | Kiểu trả về        | Mô tả chi tiết                                         |
|-----|----------------------|--------------------|--------------------------------------------------------|
| 1   | saveUser             | bool               | Lưu thông tin người dùng vào tệp                       |
| 2   | loadUsers            | bool               | Tải toàn bộ người dùng từ tệp                          |
| 3   | backupData           | bool               | Tạo bản sao lưu dữ liệu                                |
| 4   | restoreBackup        | bool               | Phục hồi dữ liệu từ bản sao lưu                        |


## 📥 Cách tải và dịch chương trình

### Yêu cầu:
- Compiler hỗ trợ C++11 hoặc cao hơn (g++, clang++)
- Thư viện chuẩn C++ (không dùng thư viện ngoài)

### Tải source code:
```bash
git clone https://github.com/example/point-wallet-system.git
cd point-wallet-system
```

### Dịch chương trình:
```bash
g++ main.cpp user.cpp wallet.cpp transaction.cpp -o wallet-system
```

> Có thể thay thế bằng Makefile nếu có.

## ▶️ Cách chạy chương trình

Sau khi dịch thành công, chạy:

```bash
./wallet-system
```

### Thao tác chính:
1. Đăng ký tài khoản
2. Đăng nhập
3. Với người dùng thường:
   - Cập nhật thông tin cá nhân (yêu cầu xác nhận OTP)
   - Thay đổi mật khẩu
   - Xem ví điểm và lịch sử giao dịch
   - Thực hiện chuyển điểm sang ví khác (OTP bắt buộc)
4. Với người quản lý:
   - Xem danh sách tài khoản
   - Tạo tài khoản hộ người dùng
   - Cập nhật thông tin hộ (yêu cầu OTP người dùng)
   - Theo dõi và quản lý ví điểm

## 🔐 Tệp tin và thư viện kèm theo

- `main.cpp` – chương trình chính
- `user.h / user.cpp` – quản lý người dùng
- `wallet.h / wallet.cpp` – quản lý ví
- `transaction.h / transaction.cpp` – xử lý giao dịch
- `utils.h / utils.cpp` – tiện ích (hash, sinh OTP,...)
- `data/` – thư mục chứa tệp dữ liệu người dùng và ví
- `backup/` – thư mục chứa bản sao lưu dữ liệu

## 📚 Tài liệu tham khảo

1. Tài liệu về OTP:  
   - [Wikipedia - One-time password](https://en.wikipedia.org/wiki/One-time_password)  
   - [RFC 4226 - HOTP: An HMAC-Based One-Time Password Algorithm](https://datatracker.ietf.org/doc/html/rfc4226)  
   - [RFC 6238 - TOTP: Time-Based One-Time Password Algorithm](https://datatracker.ietf.org/doc/html/rfc6238)

2. Tài liệu về bảo mật mật khẩu:
   - [OWASP Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)
   - StackOverflow, GeeksForGeeks: các thuật toán băm như SHA-256 trong C++
