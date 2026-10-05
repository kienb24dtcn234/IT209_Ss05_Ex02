# Báo cáo Session 05 - Bài 2: Tái cấu trúc lịch sử commit bằng Interactive Rebase

- **Đường dẫn nộp:** `homework/session_05/ex2/`

---

## 1. Mục tiêu
- Dùng Interactive Rebase (`git rebase -i`) để dọn lịch sử commit cục bộ.
- **Squash** nhiều commit vụn thành 1 commit duy nhất.
- **Reword** thông điệp commit sau khi gộp.
- **Drop** một commit chứa file rác (`temp.txt`).

---

## 2. Chuẩn bị bối cảnh (tạo 4 commit vụn)
```bash
git init

echo "function login(){}" > auth.js
git add auth.js && git commit -m "feat: khoi tao module auth"

echo "// fix" >> auth.js
git add auth.js && git commit -m "fix typo"

echo "function hashPassword(){}" >> auth.js
git add auth.js && git commit -m "adds utility functions"

echo "debug data" > temp.txt
git add temp.txt && git commit -m "add temp file for debug"
```
Xem lịch sử ban đầu (4 commit):
```bash
git log --oneline
```

---

## 3. (Khuyến nghị) Đổi trình soạn thảo sang nano cho dễ thao tác
```bash
git config --global core.editor "nano"
```
> Mặc định Git dùng `vim` — khó dùng nếu chưa quen. Đổi sang `nano` để chỉ cần gõ, lưu `Ctrl+O`, thoát `Ctrl+X`.

---

## 4. Chạy Interactive Rebase
```bash
git rebase -i HEAD~4
```
Trình soạn thảo mở ra 4 dòng (từ CŨ đến MỚI). Sửa như sau:
```
pick   <hash1> feat: khoi tao module auth
squash <hash2> fix typo
squash <hash3> adds utility functions
drop   <hash4> add temp file for debug
```
- Dòng 1: giữ `pick`.
- Dòng 2, 3: đổi `pick` → `squash` (hoặc `s`) để gộp vào dòng 1.
- Dòng 4: đổi `pick` → `drop` (hoặc `d`) để xóa hẳn commit chứa `temp.txt`.

Lưu & đóng (nano: `Ctrl+O` → Enter → `Ctrl+X`).

---

## 5. Reword thông điệp commit gộp
Sau khi lưu, Git mở editor thứ 2 chứa thông điệp của các commit được gộp. Xóa hết và thay bằng đúng 1 dòng:
```
feat: hoan thien module authentication
```
Lưu & đóng lần nữa.

---

## 6. Kiểm tra (Verification)
```bash
git log --oneline
```
**Kết quả mong đợi** — chỉ còn 1 commit sạch, không còn commit rác và không còn `temp.txt`:
```
<hash> (HEAD -> main) feat: hoan thien module authentication
```
Kiểm tra file rác đã biến mất:
```bash
ls          # không còn temp.txt
```

---

## 7. Giải thích nhanh 3 thao tác
- **squash**: gộp commit vào commit ngay phía trên → giảm số commit, gộp thay đổi.
- **reword**: viết lại thông điệp commit (ở đây thực hiện luôn khi gộp).
- **drop**: loại bỏ hoàn toàn commit khỏi lịch sử (kéo theo xóa thay đổi commit đó mang lại, ví dụ `temp.txt`).

> ⚠️ Interactive Rebase viết lại lịch sử → chỉ dùng cho commit **cục bộ chưa push**. Không rebase nhánh đã chia sẻ với người khác.

---

## 8. File nộp
- `README.md` — báo cáo này.
- Ảnh chụp **giao diện soạn thảo Interactive Rebase** (thấy rõ `pick/squash/drop`).
- Ảnh chụp **`git log --oneline`** sau khi hoàn tất (1 commit sạch).

![rebase editor](rebase-editor.png)
![git log clean](git-log.png)

```bash
mkdir -p homework/session_05/ex2
git add homework/session_05/ex2
git commit -m "Session 05 - Bai 2: Interactive Rebase"
git push origin main
```
Sau khi push, vào mục **"Nộp bài"** trên portal và dán link GitHub tới `homework/session_05/ex2/`.
