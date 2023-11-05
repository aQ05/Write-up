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

Nhập lệnh `1’ order by 1--` và đến `1' order by 3--` thì bị lỗi



