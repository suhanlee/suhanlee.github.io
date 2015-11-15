---
layout: post
title:  "[Sinatra] file upload 구현하기"
date:   2015-11-16 03:00:00 +0900
categories: Sinatra
---

Sinatra로 file upload 구현하기
-------------------

>**Application**

> - sinatra 는 rack기반 프레임웍이기 때문에 file upload 구현시 기본적인 위치는 rack 기반의 다른 프레임웍들과 동일하다.
> - params[:file] 안에 파일과 관련된 정보(파일이름, 파일 내용, 파일 타입 등)가 위치한다.
> - params[:file][:tempfile] 은 실제 클라이언트가 업로드하는 파일 내용이 위치한다. 
 
```
post '/upload/:filename' do
		filename = params[:filename]
		tempfile = params[:file][:tempfile]
		save_dir = "./upload"

		begin
			FileUtils.mkdir_p(save_dir) unless File.exists?(save_dir)
			filename = File.join(save_dir, filename)
			puts params
			File.open(filename, 'wb') do |file|
				file.write(tempfile.read)
			end

			status = {:status => 200}.to_json
		rescue Exception => e
			status = {:error_message => e.message, :status => 500}.to_json
		end

		status
	end
```

>**Request**
> $ curl --trace-ascii /dev/stdout -X POST -H Accept:application/json -F 'file=@test1.txt' 'http://localhost:4567/upload/test.txt'

> **Note:**

> - --trace-ascii 은 요청과정에 데이터를 지정한 타깃에 아스키 형태로 출력한다.  여기서 /dev/stdout 은 화면이다. 참고로 --trace 옵션은 Hex 형태로 출력한다.
> - -X POST 는 METHOD 를 POST 로 요청하겠다는 것을 의미한다.
> - -H Accept:application/json 은 응답받을 데이터 타입 유형으로 json 을 선호한다는 것을 의미한다.
> - -F 'file=@test1.txt' multipart 형태로 file 을 지정하는 것을 의미. @는 필수다.

>**Result:**
>```
== Info:   Trying 127.0.0.1...
== Info: Connected to localhost (127.0.0.1) port 4567 (#0)
=> Send header, 235 bytes (0xeb)
0000: POST /upload/test.txt HTTP/1.1
0020: Host: localhost:4567
0036: User-Agent: curl/7.43.0
004f: Accept:application/json
0068: Content-Length: 246
007d: Expect: 100-continue
0093: Content-Type: multipart/form-data; boundary=--------------------
00d3: ----463b44025b1d2727
00e9: 
== Info: Done waiting for 100-continue
=> Send data, 139 bytes (0x8b)
0000: --------------------------463b44025b1d2727
002c: Content-Disposition: form-data; name="file"; filename="test1.txt
006c: "
006f: Content-Type: text/plain
0089: 
=> Send data, 59 bytes (0x3b)
0000: This is a test contents...dfdf.df.d.f.d.f.d..f.d.f.d.fd.f..
=> Send data, 48 bytes (0x30)
0000: 
0002: --------------------------463b44025b1d2727--
<= Recv header, 17 bytes (0x11)
0000: HTTP/1.1 200 OK
<= Recv header, 39 bytes (0x27)
0000: Content-Type: text/html;charset=utf-8
<= Recv header, 20 bytes (0x14)
0000: Content-Length: 14
<= Recv header, 33 bytes (0x21)
0000: X-XSS-Protection: 1; mode=block
<= Recv header, 33 bytes (0x21)
0000: X-Content-Type-Options: nosniff
<= Recv header, 29 bytes (0x1d)
0000: X-Frame-Options: SAMEORIGIN
<= Recv header, 512 bytes (0x200)
0000: Set-Cookie: rack.session=BAh7CEkiD3Nlc3Npb25faWQGOgZFVEkiRTcwNGE
0040: 5M2RkZWZjZjgyYWMzMDBj%0AZDExOTZlZGM5MWMxNGRmMzc0NDM2NmU4Y2QxOGI5
0080: M2VlNmM5MTA3NjU4NjkG%0AOwBGSSIJY3NyZgY7AEZJIiU5OWRkNjU4ZjJiZDE1Y
00c0: jFiMjQwMDljMzNiZjE3%0ANDRhOAY7AEZJIg10cmFja2luZwY7AEZ7B0kiFEhUVF
0100: BfVVNFUl9BR0VOVAY7%0AAFRJIi05MDZiNjg3YzRiN2M5MWEwNmZhNzlmZTg1Njh
0140: mYmEyNzgxYzM3NDg0%0ABjsARkkiGUhUVFBfQUNDRVBUX0xBTkdVQUdFBjsAVEki
0180: LWRhMzlhM2VlNWU2%0AYjRiMGQzMjU1YmZlZjk1NjAxODkwYWZkODA3MDkGOwBG%
01c0: 0A--df9e15dfb0acd09eb41b686e72b89c36571ed903; path=/; HttpOnly
<= Recv header, 24 bytes (0x18)
0000: Connection: keep-alive
<= Recv header, 14 bytes (0xe)
0000: Server: thin
<= Recv header, 2 bytes (0x2)
0000: 
<= Recv data, 14 bytes (0xe)
0000: {"status":200}
== Info: Connection #0 to host localhost left intact
```