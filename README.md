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


