# GET aHEAD
## Đề bài 
Tìm flag được giữ trên máy chủ http://mercury.picoctf.net:47967/


## Gợi ý 
1. Có thể bạn có nhiều hơn 2 lựa chọn
2. Kiểm tra các công cụ như Burpsuite để sửa đổi yêu cầu của bạn và xem phản hồi

## Cách làm
1. Khi vào link đề cho, chúng ta sẽ thấy giao diện
  <img width="1280" alt="Screenshot 2023-10-14 145305" src="https://github.com/aQ05/Write-up/assets/121664384/3bd9fda0-e1e5-4847-b15d-8c46010f190a">

  <img width="1272" alt="image" src="https://github.com/aQ05/Write-up/assets/121664384/80477ace-ee21-47b2-b264-a374b279e1e1">


2. Dựa vào gợi ý. Sử dụng Burpsuite, ta có được mã HTML như sau
```html
HTTP/1.1 200 OK
Content-type: text/html; charset=UTF-8


<!doctype html>
<html>
<head>
    <title>Red</title>
    <link rel="stylesheet" type="text/css" href="//maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css">
	<style>body {background-color: red;}</style>
</head>
	<body>
		<div class="container">
			<div class="row">
				<div class="col-md-6">
					<div class="panel panel-primary" style="margin-top:50px">
						<div class="panel-heading">
							<h3 class="panel-title" style="color:red">Red</h3>
						</div>
						<div class="panel-body">
							<form action="index.php" method="GET">
								<input type="submit" value="Choose Red"/>
							</form>
						</div>
					</div>
				</div>
				<div class="col-md-6">
					<div class="panel panel-primary" style="margin-top:50px">
						<div class="panel-heading">
							<h3 class="panel-title" style="color:blue">Blue</h3>
						</div>
						<div class="panel-body">
							<form action="index.php" method="POST">
								<input type="submit" value="Choose Blue"/>
							</form>
						</div>
					</div>
				</div>
			</div>
		</div>
	</body>
</html>
```
Ở đây ta có thể thấy có 2 phương pháp khác nhau được sử dụng: phương thức `GET` sử dụng cho `Red` và phương thức `POST` sử dụng cho `Blue`

<img width="329" alt="image" src="https://github.com/aQ05/Write-up/assets/121664384/c1c1b71e-515f-43c7-a754-8ae9171bb6db">

Ở gợi ý 1, chúng ta có nhiều hơn 2 lựa chọn, nghĩa là ngoài phương thức `GET` và `POST` ta có thể sử dụng phương thức khác, lại nhìn vào tiêu đề `GET aHEAD` ta có thể thử yêu cầu với phương thức `HEAD`. 

Sau khi đổi yêu cầu với phương thức `HEAD`, ta tìm được flag

<img width="909" alt="image" src="https://github.com/aQ05/Write-up/assets/121664384/0b398cc2-9ea2-446d-87ee-b7515b5504a4">

## Flag
`picoCTF{r3j3ct_th3_du4l1ty_cca66bd3}`

# Cookies
## Đề bài
Who doesn't love cookies? Try to figure out the best one. http://mercury.picoctf.net:27177/
## Cách làm
1. Truy cập vào link

<img width="613" alt="Screenshot 2023-10-14 165457" src="https://github.com/aQ05/Write-up/assets/121664384/c4bb187e-f201-4eb2-96a8-e7374af891eb">

2. Kiểm tra cookie

<img width="1198" alt="Screenshot 2023-10-14 170324" src="https://github.com/aQ05/Write-up/assets/121664384/42462452-4fb7-4bf8-a852-810c5b2a12ec">

Khi nhập `snickerdoodle` ta sẽ nhận được hiển thị `I love snickerdoodle cookies!`

<img width="582" alt="Screenshot 2023-10-14 165559" src="https://github.com/aQ05/Write-up/assets/121664384/ba47a67e-052e-4e35-90b1-d8ce4f6615fb">

Sau khi nhập từ theo gợi ý, ta nhận được giá trị 0 ở cột `value` 

<img width="994" alt="Screenshot 2023-10-14 170412" src="https://github.com/aQ05/Write-up/assets/121664384/1f3bbb8f-761c-4f83-93c2-0c846bfccc00">

Giá trị ở cột `value` đã được thay đổi từ -1 sang 0. Giờ thì hãy thay đổi giá trị ở cột này và khi nó bằng 18 chúng ta sẽ tìm được flag

<img width="990" alt="Screenshot 2023-10-14 170651" src="https://github.com/aQ05/Write-up/assets/121664384/27bb9ed5-ee6c-4455-8479-ba32bf4e41aa">

## Flag
`picoCTF{3v3ry1_l0v3s_c00k135_064663be}`

# HTTP - User-agent
Admin is really dumb...
## Cách làm
 Truy cập vào link http://challenge01.root-me.org/web-serveur/ch2/ 

<img width="921" alt="image" src="https://github.com/aQ05/Write-up/assets/121664384/52ef7186-a47c-40d0-8c03-c52d45fe8135">

Khi gửi request, ta nhận được `Wrong user-agent: you are not the "admin" browser!`. 

<img width="341" alt="image" src="https://github.com/aQ05/Write-up/assets/121664384/15e4988f-0d60-40b0-8c27-e39d737d96df">

Sử dụng Burpsuite, trong phần request sửa phần user-agent thành `admin` ta sẽ lấy được password

<img width="716" alt="image" src="https://github.com/aQ05/Write-up/assets/121664384/17f168ea-93db-4f7d-950b-4abea688c4a1">

## Password
`rr$Li9%L34qd1AAe27`

# HTTP - Headers
HTTP response give informations

Statement: Get an administrator access to the webpage.
## Cách làm
Truy cập vào link http://challenge01.root-me.org/web-serveur/ch5/

 <img width="351" alt="image" src="https://github.com/aQ05/Write-up/assets/121664384/34a50ace-a623-44ac-a7a2-9dcb7b1e458a">

 Khi gửi request, ta nhận được `Content is not the only part of an HTTP response!`

 Sử dụng Burpsuite, thấy ở phần response có `Header-RootMe-Admin` mà request không có.
 
 <img width="720" alt="image" src="https://github.com/aQ05/Write-up/assets/121664384/e7b08ef8-2087-44e2-bbac-2a2ef2efe04d">

Thêm header này vào phần request với giá trị là `YES`, ta thấy phần password ở phần response

<img width="720" alt="image" src="https://github.com/aQ05/Write-up/assets/121664384/0e21bf39-f2a5-4296-97ec-84b85b2009af">

## Password
`HeadersMayBeUseful`
# HTTP - POST
Do you know HTTP?

Statement: Find a way to beat the top score!
## Cách làm
Truy cập vào link http://challenge01.root-me.org/web-serveur/ch56/. Sau khi ấn `Give a try`

<img width="507" alt="image" src="https://github.com/aQ05/Write-up/assets/121664384/d811011b-764b-4781-a697-7c378cd66137">

Sử dụng Burpsuite, vào phần POST Request của nó ta thấy giá trị nhận được khi ấn `Give a try`. 

<img width="399" alt="image" src="https://github.com/aQ05/Write-up/assets/121664384/e749c468-4a09-48b5-9a42-8ab4c6ba2c28">

Để lấy được password, ta cần thay một giá trị lớn hơn `999999`

<img width="413" alt="image" src="https://github.com/aQ05/Write-up/assets/121664384/ad138bb4-0456-44b0-99e8-a5ff8f23c878">

Khi đó ta thu được flag

<img width="452" alt="image" src="https://github.com/aQ05/Write-up/assets/121664384/1c9fb1c0-5bd9-4ee5-bc76-6b045dfe7653">

## Flag
`H7tp_h4s_N0_s3Cr37S_F0r_y0U`

# HTTP - IP restriction bypass
Only local users will be able to access the page

Statement
Dear colleagues,

We’re now managing connections to the intranet using private IP addresses, so it’s no longer necessary to login with a username / password when you are already connected to the internal company network.

Regards,

The network admin
## Cách làm
Truy cập vào link http://challenge01.root-me.org/web-serveur/ch68/

<img width="397" alt="Screenshot 2023-10-15 003237" src="https://github.com/aQ05/Write-up/assets/121664384/fb0f8055-a88f-46c7-a653-4dcc7c06ccae">

<img width="433" alt="image" src="https://github.com/aQ05/Write-up/assets/121664384/a6a6a673-bf0a-4b17-b8bf-733d4d27683d">


<img width="345" alt="image" src="https://github.com/aQ05/Write-up/assets/121664384/7f3acaf4-446a-4ced-9d06-0aca4d49bf58">

## Password
`Ip_$po0Fing`
