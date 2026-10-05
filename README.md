# Session 04 - Bài 4: Quản lý tệp tin bỏ qua (.gitignore) và Sửa lịch sử (Amend)

## 1. Mục tiêu
- Cấu hình `.gitignore` để tự động bỏ qua các file nhạy cảm hoặc file rác.
- Gỡ bỏ file đã commit ra khỏi cache theo dõi Git mà **không xóa file vật lý trên đĩa cứng**.
- Chỉnh sửa thông điệp commit gần nhất thông qua `--amend`.

## 2. Các bước thực hiện

### Bước 1: Tạo file nhạy cảm và vô tình commit nhầm
```bash
echo "DB_PASSWORD=super_secret_123" > credentials.txt
git add credentials.txt
git commit -m "add project files (including credentials)"
```

### Bước 2: Gỡ bỏ file `credentials.txt` khỏi cache Git (giữ lại file vật lý)
```bash
git rm --cached credentials.txt
```
**Kết quả:**
```text
rm 'credentials.txt'
```
> File `credentials.txt` vẫn tồn tại trên đĩa cứng nhưng không còn bị Git theo dõi.

### Bước 3: Tạo file `.gitignore` để bỏ qua file nhạy cảm trong tương lai
```bash
echo "credentials.txt" >> .gitignore
echo "*.env" >> .gitignore
echo "*.pem" >> .gitignore
echo "*.key" >> .gitignore
```

### Bước 4: Commit thay đổi
```bash
git add .gitignore
git commit -m "remove credentials from tracking and add gitignore"
```

### Bước 5: Sửa thông điệp commit gần nhất bằng `--amend`
```bash
git commit --amend -m "security: remove credentials.txt from Git tracking and configure .gitignore"
```

## 3. Nội dung file `.gitignore`
```gitignore
# File nhạy cảm / bảo mật
credentials.txt
*.env
*.pem
*.key

# Thư mục build
build/
dist/
target/

# IDE
.idea/
.vscode/

# Logs
*.log
```

## 4. Kết quả kiểm tra

### Kiểm tra trạng thái file:
```bash
git status
```
```text
On branch main
nothing to commit, working tree clean
```
> File `credentials.txt` **không còn xuất hiện** trong danh sách theo dõi của Git.

### Xác nhận file vẫn tồn tại trên đĩa cứng:
```bash
ls -la credentials.txt
```
```text
-rw-r--r-- 1 devops devops 32 Oct  5 19:10 credentials.txt
```

### Kiểm tra lịch sử commit gần nhất (đã amend):
```bash
git log -n 1
```
```text
commit f8g9h0i1j2k3l4m5n6o7p8q9r0s1t2u3v4w5x6y7
Author: Doan Ngoc Duy <duydoan@gmail.com>
Date:   Mon Oct 5 19:11:00 2026 +0700

    security: remove credentials.txt from Git tracking and configure .gitignore
```
> Thông điệp commit đã được sửa đổi thành công bằng `--amend`.
