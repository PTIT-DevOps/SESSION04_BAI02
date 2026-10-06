# BT02 - Quản lý nhánh và Giải quyết xung đột (Branching and Merge Conflict)

## 1. Khởi tạo repository và commit đầu tiên trên nhánh main

```powershell
PS D:\Work\Rikkei\devops\BT\SS04\SS04-BT02> git init
Initialized empty Git repository in D:/Work/Rikkei/devops/BT/SS04/SS04-BT02/.git/
PS D:\Work\Rikkei\devops\BT\SS04\SS04-BT02> git config --local user.name "lethinh"
PS D:\Work\Rikkei\devops\BT\SS04\SS04-BT02> git config --local user.email "lephuthinh387@gmail.com"
PS D:\Work\Rikkei\devops\BT\SS04\SS04-BT02> git add README.md
PS D:\Work\Rikkei\devops\BT\SS04\SS04-BT02> git commit -m "docs: tạo README.md ban đầu"
[main (root-commit) 4a1c9e2] docs: tạo README.md ban đầu
 1 file changed, 3 insertions(+)
 create mode 100644 README.md
```

## 2. Tạo nhánh feature-update và commit thay đổi

```powershell
PS D:\Work\Rikkei\devops\BT\SS04\SS04-BT02> git switch -c feature-update
Switched to a new branch 'feature-update'

PS D:\Work\Rikkei\devops\BT\SS04\SS04-BT02> git add README.md
PS D:\Work\Rikkei\devops\BT\SS04\SS04-BT02> git commit -m "docs: cập nhật trạng thái trên nhánh feature-update"
[feature-update 8b7d310] docs: cập nhật trạng thái trên nhánh feature-update
 1 file changed, 1 insertion(+), 1 deletion(-)
```

## 3. Quay lại main và commit thay đổi trên cùng một dòng

```powershell
PS D:\Work\Rikkei\devops\BT\SS04\SS04-BT02> git switch main
Switched to branch 'main'

PS D:\Work\Rikkei\devops\BT\SS04\SS04-BT02> git add README.md
PS D:\Work\Rikkei\devops\BT\SS04\SS04-BT02> git commit -m "docs: cập nhật trạng thái trên nhánh main"
[main 2f5e08c] docs: cập nhật trạng thái trên nhánh main
 1 file changed, 1 insertion(+), 1 deletion(-)
```

Hai nhánh cùng sửa một dòng của `README.md`, nên khi gộp Git không thể tự quyết định nội dung đúng.

## 4. Gộp nhánh và tạo xung đột

```powershell
PS D:\Work\Rikkei\devops\BT\SS04\SS04-BT02> git merge feature-update
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.

PS D:\Work\Rikkei\devops\BT\SS04\SS04-BT02> git status
On branch main
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

## 5. Xử lý xung đột thủ công

Nội dung `README.md` sau khi Git chèn các ký hiệu đánh dấu xung đột:

```text
<<<<<<< HEAD
- Trạng thái: đang phát triển trên nhánh main.
=======
- Trạng thái: đang phát triển trên nhánh feature-update.
>>>>>>> feature-update
```

Các bước xử lý:

1. Mở `README.md` bằng editor, xác định vùng giữa `<<<<<<< HEAD` và `=======` là nội dung của nhánh main, vùng giữa `=======` và `>>>>>>> feature-update` là nội dung của nhánh feature-update.
2. Xóa cả ba dòng đánh dấu `<<<<<<<`, `=======`, `>>>>>>>` và giữ lại nội dung đúng.
3. Đánh dấu đã giải quyết bằng `git add README.md`.

Nội dung sau khi giải quyết:

```text
- Trạng thái: đang phát triển trên nhánh main và feature-update.
```

## 6. Hoàn tất commit gộp nhánh

```powershell
PS D:\Work\Rikkei\devops\BT\SS04\SS04-BT02> git add README.md
PS D:\Work\Rikkei\devops\BT\SS04\SS04-BT02> git commit -m "merge: gộp nhánh feature-update vào main"
[main 9c0b7a4] merge: gộp nhánh feature-update vào main
```

## 7. Kiểm tra lịch sử commit dưới dạng đồ thị nhánh

```powershell
PS D:\Work\Rikkei\devops\BT\SS04\SS04-BT02> git log --graph --oneline
*   9c0b7a4 (HEAD -> main) merge: gộp nhánh feature-update vào main
|\
| * 8b7d310 (feature-update) docs: cập nhật trạng thái trên nhánh feature-update
* | 2f5e08c docs: cập nhật trạng thái trên nhánh main
|/
* 4a1c9e2 docs: tạo README.md ban đầu
```

Commit gộp `9c0b7a4` có hai nhánh tổ tiên (`8b7d310` và `2f5e08c`), tức Git đã thực hiện gộp 3 vùng: vùng cơ sở chung `4a1c9e2` và hai vùng thay đổi từ hai nhánh.
