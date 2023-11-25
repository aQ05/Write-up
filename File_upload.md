[ File upload - Double extensions](https://github.com/File_upload#File---upload-Double-extensions)
# File upload - Double extensions
**Statement**

Your goal is to hack this photo galery by uploading PHP code.

Retrieve the validation password in the file .passwd at the root of the application.

## Cách làm
Chall: http://challenge01.root-me.org/web-serveur/ch20/

![image](https://github.com/aQ05/Write-up.training/assets/121664384/dd695a09-b2e1-47d8-9102-008a93ac9808)

Từ gợi ý đề bài, ta sẽ tìm cách upload file lên server và đọc file `.passwd` để tìm password thì nhận được lỗi `403 Forbidden`, có nghĩa là có file này nhưng bị giới hạn access:

![image](https://github.com/aQ05/Write-up.training/assets/121664384/2f2a7a05-2042-4dff-9351-fbbe61f4a6a5)

Mở phần upload của server, web chỉ cho upload file ảnh có đuôi `only .gif, .jpeg and .png`.

![image](https://github.com/aQ05/Write-up.training/assets/121664384/091bf4af-0ae0-4b91-8a37-f8e757db6daa)

Thử upload một file:

![image](https://github.com/aQ05/Write-up.training/assets/121664384/ff191227-9f42-4a2f-be32-fcc88ebe2b97)

Như vậy, để đọc được `.passwd`, ta cần phải đi ngược folder 3 lần nằm tại `../../../.passwd`

Để đọc được `.passwd`, ta thử sử dụng cổng này để upload file `.php` lên server và buộc server sử dụng `shell_exec()` để thực thi shell:
```php
<?php
$output = shell_exec('cat ../../../.passwd');
echo "<pre>$output</pre>";
?>
```
> Lệnh `cat` cho phép người dùng tạo một hoặc nhiều file, xem nội dung file, nối file và chuyển hướng đầu ra trong terminal hoặc file.
> [shell_exec](https://www.php.net/manual/en/function.shell-exec.php): thực thi lệnh qua shell và trả về đầu ra hoàn chỉnh dưới dạng chuỗi

Đề bài đã gợi ý cho ta sử dụng Double Extensions, do đó thử rename file `.php` thành `.php.png` và upload:

![image](https://github.com/aQ05/Write-up.training/assets/121664384/e814115b-022a-435a-b933-0237c030467c)

Bấm vào link ta lấy được flag:

![image](https://github.com/aQ05/Write-up.training/assets/121664384/410d2f24-3879-48a5-830f-f5c2796dfffc)

## Flag
`Gg9LRz-hWSxqqUKd77-_q-6G8`

# File upload - MIME type
# File upload - ZIP
# File upload - Polyglot
# File upload - Null byte
