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

Mở file đã upload:

![image](https://github.com/aQ05/Write-up.training/assets/121664384/410d2f24-3879-48a5-830f-f5c2796dfffc)

## Flag
`Gg9LRz-hWSxqqUKd77-_q-6G8`

# File upload - MIME type

**Statement**

Your goal is to hack this photo galery by uploading PHP code.

Retrieve the validation password in the file .passwd at the root of the application.

## Cách làm
Chall: http://challenge01.root-me.org/web-serveur/ch21/

![image](https://github.com/aQ05/Write-up.training/assets/121664384/80b89c10-5442-4504-856a-cea0e8fe2c85)

Tương tự như bài trên, web chỉ cho upload file ảnh có đuôi `only .gif, .jpeg and .png`.

![image](https://github.com/aQ05/Write-up.training/assets/121664384/67334c23-c269-4918-af2b-33b7f5cdd3fe)

Thử với web shell như bài trên nhưng không thu được gì

![image](https://github.com/aQ05/Write-up.training/assets/121664384/ca747dad-3b0e-4451-9559-07eec4fc7ea8)

Tên chall là MIME ta nghĩ đến cách thử đổi Content-type:

![image](https://github.com/aQ05/Write-up.training/assets/121664384/0b125dd9-0f87-4912-b416-d7ec9d4b32b9)

Khi tải lên file `shell.php` ta sẽ có Content-Type là `application/octet-stream` chính vì vậy nên file bị filter và dẫn đến `Wrong file type!`. Giờ chỉ cần đổi Content-type sang `image/png` là được: 

![image](https://github.com/aQ05/Write-up.training/assets/121664384/ed4c1152-b404-4c12-b36c-f59728a358b1)

![image](https://github.com/aQ05/Write-up.training/assets/121664384/711e82b0-c021-4bbb-90ae-c05b486c3e34)

Mở file đã upload

![image](https://github.com/aQ05/Write-up.training/assets/121664384/e8151dae-6cb3-4881-8bcf-8b6ba21e700d)

## Flag
`a7n4nizpgQgnPERy89uanf6T4`

# File upload - Null byte
**Statement**

Your goal is to hack this photo galery by uploading PHP code.

## Cách làm
Chall: http://challenge01.root-me.org/web-serveur/ch22/

![image](https://github.com/aQ05/Write-up.training/assets/121664384/9f610409-6857-44a9-ae4a-031a77dec262)

Tương tự như bài trên, web chỉ cho upload file ảnh có đuôi `only .gif, .jpeg and .png`.

Thử với web shell như các bài trên và tất nhiên không được. Tên chall là Null Byte nên ta có thể bypass bằng cách sử dụng `%00`

![image](https://github.com/aQ05/Write-up.training/assets/121664384/58155bf0-d180-472a-b81e-3c4e1b5838de)

> `%00` tương đương giá trị null, khi nhận tên file vào đến giá trị null thì bị dừng lại và ta có thể tải lên file shell.php

Mở file đã upload

![image](https://github.com/aQ05/Write-up.training/assets/121664384/88e2e80f-38db-4582-b321-11b6b5ca13f5)

## Flag
`YPNchi2NmTwygr2dgCCF`


# File upload - ZIP
**Statement**

Your goal is to read index.php file.

## Cách làm
Chall: http://challenge01.root-me.org/web-serveur/ch51/

![image](https://github.com/aQ05/Write-up.training/assets/121664384/679fecca-4a61-4244-bfa4-aa8097f93152)

Thử tải web shell, ta không thu được gì cả:

![image](https://github.com/aQ05/Write-up.training/assets/121664384/4b25320a-4fae-4b9f-bfbe-01935f4c5116)

Tiếp tục thử với một file `shell.zip`

![image](https://github.com/aQ05/Write-up.training/assets/121664384/e2c6991c-7e5b-4c86-9983-246e25f6d58f)

Khi upload file zip thì server đổi tên file và giải nén file đó. File zip chứa media, document thì mở ra xem bình thường. Tuy nhiên file `.php` khi mở thì bị lỗi (403 Forbidden):

![image](https://github.com/aQ05/Write-up.training/assets/121664384/7219587c-b252-4cc3-a280-2267537325ab)

Tạo file `.txt`


> [symbolic / soft link](https://clammy-snowstorm-0d2.notion.site/symbolic-soft-link-05ca57bf671a42acae52cfba49f19cb5) được sử dụng để tạo lối tắt đến tệp hoặc thư mục, giúp chúng ta có thể truy cập được từ các vị trí khác nhau trong hệ thống tệp.
>
> `ln -s ../../../index.php index.txt`
> 
> `ln`: tạo link trong linux
> 
> `-s`: chỉ định một liên kết symbolic được tạo
# File upload - Polyglot
**Statement**

Your friend who is a photography fan has created a site to allow people to share their beautiful photos. He assures you that his site is secure because he checks that the file sent is a JPEG, and that it is not a disguised PHP file. Prove him wrong!
## Cách làm
Chall: http://challenge01.root-me.org:59072/

![image](https://github.com/aQ05/Write-up.training/assets/121664384/10c3195c-2e88-431e-bdd3-4807668c7c77)
