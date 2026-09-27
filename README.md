# CMM Backend – Hệ thống RESTful API Quản lý Thi đua & Nề nếp Học đường

Hệ thống máy chủ backend xây dựng trên nền tảng **Node.js + Express**, cung cấp toàn bộ dịch vụ dữ liệu và logic nghiệp vụ cho hệ sinh thái CMM (ứng dụng di động **CMM Mobile** và hệ thống quản trị **CMM Admin Web**).

---

## 📌 Tính năng & Nghiệp vụ cốt lõi

- **Xác thực & Phân quyền (Authentication & Authorization)**:
  - Cơ chế xác thực tài khoản dựa trên JWT (JSON Web Token) và kiểm tra API Key.
  - Phân quyền chặt chẽ theo vai trò: Quản trị viên, Ban giám hiệu, Đoàn trường, Đội Cờ đỏ / Sao đỏ, Giáo viên chủ nhiệm và Học sinh.
- **Quản lý Học sinh & Lớp học**:
  - Quản lý danh mục khối, lớp học và thông tin học sinh trong nhà trường.
  - Tra cứu hồ sơ vi phạm và quá trình rèn luyện nề nếp của từng học sinh.
- **Quản lý Vi phạm & Nội quy**:
  - Thiết lập danh mục quy định, khung điểm trừ và mức độ vi phạm.
  - Tiếp nhận, ghi nhận và rà soát các biên bản vi phạm nề nếp theo ngày/tuần.
- **Quản lý Sổ đầu bài điện tử**:
  - Lưu trữ và quản lý dữ liệu sổ đầu bài từng tiết học, số tiết vắng, xếp loại giờ học.
- **Quản lý & Phân công Lịch trực**:
  - Lập và phân bổ lịch trực cờ đỏ/sao đỏ theo từng tuần học và khu vực.
- **Tính điểm & Bảng xếp hạng thi đua**:
  - Tự động tổng hợp điểm thi đua, trừ điểm theo vi phạm và cộng điểm phong trào.
  - Xếp hạng thi đua theo tuần, tháng và học kỳ cho các lớp/khối.
- **Tài liệu hóa API trực quan**:
  - Tích hợp tài liệu Swagger UI tự động phục vụ tích hợp giao diện và kiểm thử API.

---

## 🛠️ Công nghệ sử dụng

| Thành phần | Công nghệ / Thư viện |
| :--- | :--- |
| **Runtime & Core Framework** | Node.js, Express.js |
| **Cơ sở dữ liệu** | MySQL (sử dụng connection pool qua thư viện `mysql2`) |
| **Xác thực & Bảo mật** | `jsonwebtoken` (JWT), `bcryptjs`, `express-rate-limit`, `cors` |
| **Kiểm thực dữ liệu & Upload** | `joi`, `multer`, `body-parser` |
| **Tài liệu API (OpenAPI/Swagger)** | `swagger-ui-express`, `swagger-autogen` |

---

## 📂 Cấu trúc dự án

Dự án được tổ chức theo kiến trúc phân tầng (Layered Architecture: Route $\rightarrow$ Controller $\rightarrow$ Service $\rightarrow$ Repository $\rightarrow$ Database) giúp tách biệt rõ ràng giữa luồng xử lý HTTP, logic nghiệp vụ và truy vấn cơ sở dữ liệu:

```text
cmm-backend/
├── routes/                 # Định nghĩa các route HTTP endpoint
│   ├── auth.js             # API Đăng nhập / Đăng ký / Xác thực
│   ├── class.js            # API Quản lý lớp học
│   ├── feedback.js         # API Tiếp nhận ý kiến phản hồi
│   ├── lichtruc.js         # API Quản lý lịch trực
│   ├── rules.js            # API Quy định & Tiêu chí vi phạm
│   ├── score.js            # API Điểm thi đua & Bảng xếp hạng
│   ├── sodaubai.js         # API Sổ đầu bài điện tử
│   ├── student.js          # API Quản lý hồ sơ học sinh
│   ├── users.js            # API Quản lý người dùng & phân quyền
│   ├── vipham.js           # API Ghi nhận & Tra cứu vi phạm
│   └── week.js             # API Quản lý tuần học
├── src/
│   ├── config/             # Cấu hình hệ thống chung
│   ├── controllers/        # Tiếp nhận request, điều phối service và trả response
│   ├── db/                 # Kết nối MySQL và quản lý connection pool
│   ├── middleware/         # Middleware xác thực JWT, kiểm tra quyền hạn, rate limit
│   ├── models/             # Định nghĩa schema và cấu trúc dữ liệu
│   ├── repositories/       # Tầng truy vấn trực tiếp cơ sở dữ liệu MySQL
│   ├── services/           # Xử lý logic nghiệp vụ trung tâm
│   └── utils/              # Các hàm tiện ích dùng chung (format, helper, logger)
├── index.js                # Khởi động Express server và nạp các middleware cốt lõi
├── swagger.js              # Script tự động sinh tài liệu Swagger OpenAPI
├── swagger-output.json     # File định nghĩa OpenAPI được sinh tự động
└── package.json            # Quản lý dependencies và scripts
```

---

## 🚀 Cài đặt & Khởi chạy

### Yêu cầu tiên quyết
- **Node.js**: Phiên bản LTS (Khuyến nghị từ v18 trở lên)
- **MySQL Server**: Cơ sở dữ liệu MySQL đang hoạt động
- **Trình quản lý gói**: `npm` hoặc `yarn`

### Các bước thực hiện

1. **Cài đặt các gói phụ thuộc:**
   ```bash
   npm install
   ```

2. **Cập nhật tài liệu API Swagger (nếu có thay đổi router):**
   ```bash
   npm run swagger
   ```

3. **Khởi động máy chủ backend:**
   ```bash
   npm start
   ```

4. **Truy cập tài liệu API:**
   - Sau khi server khởi chạy thành công (mặc định tại cổng `3000`), truy cập giao diện tài liệu tương tác Swagger tại:
   - `http://localhost:3000/doc`

---

## 📑 Danh mục nhóm API chính

| Nhóm API | Tiền tố Route | Mô tả chức năng |
| :--- | :--- | :--- |
| **Auth** | `/auth` | Đăng nhập, đăng ký và xác thực tài khoản |
| **Users** | `/users` | Quản lý danh sách người dùng, vai trò và phân quyền |
| **Classes** | `/classes` | Quản lý danh sách khối, lớp học và giáo viên phụ trách |
| **Students** | `/students` | Quản lý học sinh, danh sách thành viên lớp, lịch sử nề nếp |
| **Violations** | `/vipham` | Ghi nhận lỗi, duyệt vi phạm nề nếp và biên bản xử lý |
| **Rules** | `/rules` | Danh mục quy chế thi đua và mức điểm trừ tương ứng |
| **Logbook** | `/sodaubai` | Quản lý sổ đầu bài, đánh giá tiết học và chuyên cần |
| **Duty** | `/lichtruc` | Quản lý lịch trực cờ đỏ/sao đỏ hàng tuần |
| **Scores** | `/score` | Điểm thi đua, bảng xếp hạng lớp và khối theo tuần/tháng |
| **Weeks** | `/week` | Danh mục các tuần học trong năm học |
| **Feedback** | `/feedback` | Tiếp nhận và xử lý phản ánh, khiếu nại thi đua |
