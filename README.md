# 🧪 Virtual Host Lab – Simulating DNS Resolution Issues Related to Host Header

## 🧠 Lab Objective

Simulate a web environment with multiple `VirtualHost`s using Apache inside Docker.  
This environment recreates the behavior of applications that separate services based on the `Host` header – a technique that can lead to **critical security vulnerabilities if not handled properly**.

---

## 🔍 Scenario Description

In this environment:

- Two websites are served on the same IP (`127.0.0.1`) and port (`8080`), but distinguished by domain names:
  - `localhost`
  - `site2.local`
- Currently, `localhost` is accessible, but `site2.local` is not.
- When accessing `http://127.0.0.1:8080`, Apache **redirects to `http://site2.local:8080`**.
- If the system hasn’t configured `/etc/hosts` for `site2.local`, the browser will report a **DNS resolution error**.

---

🧪 Simulated Scenario:
- Previously, site2.local was the main website of the service.
- As a result, accessing the IP address 127.0.0.1 would be automatically reversed (or redirected) to site2.local, since the Apache configuration used to route raw IP traffic to it.

- Later, due to internal restructuring or policy changes, the development team moved the primary site to localhost (or site1.local).

- However, a critical mistake was made:
The developer forgot to update the reverse IP redirection logic inside the Apache configuration.
- As a result, requests to http://127.0.0.1:8080 are still redirected to site2.local, which was supposed to be deprecated or hidden.

- This leftover behavior introduces a new attack surface for hackers.
- Because if a malicious actor discovers the old domain site2.local, they may exploit forgotten or unmaintained functionality — commonly known as "Virtual Host Confusion" or "Hidden Admin Panels" due to legacy misconfiguration.

---

## 🛡️ Security Impact

### **Virtual Host Confusion**

- If the service does not properly control `VirtualHost` configuration, users may access resources not intended for them (access control bypass).

---

## 🛠️ How to Run the Lab

1. Run the `docker-compose.yml`:
```bash
sudo docker-compose up --build
```

2. Try accessing the following links:

- [localhost](http://localhost:8080)
- [127.0.0.1](http://127.0.0.1:8080)

## Write-up:
- Modify the hosts file to map site2.local to 127.0.0.1, then access the site and retrieve the flag.

---

# Vietnamese

# 🧪 Virtual Host Lab – Mô phỏng lỗi phân giải tên miền liên quan đến Host Header

## 🧠 Mục tiêu của Lab

Mô phỏng một môi trường web với nhiều `VirtualHost` sử dụng Apache trong Docker.  
Môi trường này tái hiện hành vi phổ biến của các ứng dụng web phân tách domain theo `Host` header – một kỹ thuật có thể gây ra **lỗi bảo mật nghiêm trọng nếu không xử lý đúng**.

---

## 🔍 Mô tả tình huống

Trong môi trường này:

- Có 2 website được phục vụ trên cùng 1 IP (`127.0.0.1`) và port (`8080`) nhưng phân biệt nhau bằng tên miền:
  - `localhost`
  - `site2.local`
- Hiện tại ta, có thể truy cập được tên miền site1.local, nhưng site2.local thì lại không thể
- Nếu truy cập bằng ip `http://127.0.0.1:8080`, Apache sẽ **redirect sang `http://site2.local/`**.
- Nếu máy chưa cấu hình `/etc/hosts` cho `site2.local`, trình duyệt sẽ báo lỗi **không phân giải được tên miền**.

---

🧪 Tình huống giả lập:
- Trước đây, site2.local là trang chính của hệ thống.
Do đó, khi truy cập bằng địa chỉ IP 127.0.0.1, Apache sẽ tự động chuyển hướng sang site2.local, vì cấu hình máy chủ mặc định như vậy.

- Tuy nhiên, sau một thời gian, nhóm phát triển đã chuyển trang chính sang localhost (hay site1.local).
- Vấn đề phát sinh là lập trình viên quên cập nhật lại cấu hình chuyển hướng IP, khiến cho 127.0.0.1 vẫn tiếp tục trỏ về site2.local khi người dùng truy cập bằng IP.

- Điều này dẫn đến một bề mặt tấn công mới cho hacker:
  + Hacker có thể tìm ra và khai thác site2.local – một website cũ, có thể không còn được bảo trì đầy đủ, và dễ chứa lỗi bảo mật.

- Đây là một ví dụ điển hình của lỗi "Virtual Host Confusion" – sai lệch trong định danh domain qua Host header.

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
- [localhost](http://localhost:8080)
- [127.0.0.1](http://127.0.0.1:8080)

## Write up:
- Điều chỉnh file hosts trỏ về tên miền site2.local và lấy flag
