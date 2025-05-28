# 🧪 Virtual Host Lab – Mô phỏng lỗi phân giải tên miền liên quan đến Host Header

## 🧠 Mục tiêu của Lab

Mô phỏng một môi trường web với nhiều `VirtualHost` sử dụng Apache trong Docker.  
Môi trường này tái hiện hành vi phổ biến của các ứng dụng web phân tách domain theo `Host` header – một kỹ thuật có thể gây ra **lỗi bảo mật nghiêm trọng nếu không xử lý đúng**.

---

## 🔍 Mô tả tình huống

Trong môi trường này:

- Có 2 website được phục vụ trên cùng 1 IP (`127.0.0.1`) và port (`8080`) nhưng phân biệt nhau bằng tên miền:
  - `site1.local`
  - `site2.local`
- Hiện tại ta, có thể truy cập được tên miền site1.local, nhưng site2.local thì lại không thể
- Nếu truy cập bằng ip `http://127.0.0.1:8080`, Apache sẽ **redirect sang `http://site2.local/`**.
- Nếu máy chưa cấu hình `/etc/hosts` cho `site2.local`, trình duyệt sẽ báo lỗi **không phân giải được tên miền**.

---

## 🛡️ Ý nghĩa & Tác động bảo mật (Security Impact)

### **Sai định danh dịch vụ (Virtual Host Confusion)**

- Nếu dịch vụ không kiểm soát đúng các `VirtualHost`, người dùng có thể truy cập tài nguyên không đúng đối tượng (access control bypass).

---

## 🛠️ Cách chạy lab
1. Chạy docker-compose.yml:
```bash
sudo docker-compose up --build
```

2. Thử truy cập Link:
- [site1.local](http://site1.local:8080)
- [127.0.0.1](http://127.0.0.1:8080)

## Write up:
- Điều chỉnh file hosts trỏ về tên miền site2.local và lấy flag
