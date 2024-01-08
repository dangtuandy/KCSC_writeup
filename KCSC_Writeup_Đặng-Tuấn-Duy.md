**MISC & FORENSICS**

**Sister Location**

* Khi em tải 1 tệp zip về thì gồm 1 file .pdf có passwword và 1 hình ảnh .png , thì em đoán chắc là thông tin mật khẩu sẽ liên quan đến cái ảnh này và flag sẽ nằm trong file .pdf 
* Vậy bây giờ em sẽ đi tìm manh mối từ ảnh này:
* ![Final](https://hackmd.io/_uploads/BkaLBsC8p.png)
* Đầu tiên, em dùng lệnh strings để xuất chuỗi và binwalk -e nhưng không có manh mối thì lúc này em nhìn vào ảnh này thì thấy có gồm các tọa độ của alpha nhưng vẫn không biết là có mục đích gì.
* Tiếp theo thì em sẽ dùng tool này để xem [aperisolve.com](https://) này để xem có thêm manh mối gì không, đến với phần màu red, blue,green thì thấy có hiển thị các tọa độ:
  
* ![image_r_1](https://hackmd.io/_uploads/S1cNg20Up.png)

* ![image_g_1](https://hackmd.io/_uploads/BkLHe3AIT.png)

* ![image_b_1](https://hackmd.io/_uploads/rk9we30U6.png) 
 
* Lúc này em sẽ tổng hợp lại 4 giá trị coordinates red,green,blue,alpha(r,g,b,a) và quăng lên chatgpt để lấy các pixel value 4 giá trị(r,g,b,a) của ảnh(Final.png) thì được kết quả như nà
* ![Screenshot 2023-12-19 132616](https://hackmd.io/_uploads/HkAUq3RUT.png)
* Và em dựa vào hint đề cho (Bit Plane màu nào thì lấy value của mỗi màu đó.) thì em đã lọc ra được các value này:
* pixel value (red) :
76
101
115
45  
* pixel value (green) :
80
97
114
102
117
109
115
45
* pixel value (blue) :
100
101
45
* pixel value (alpha) :
76
65
109
111
117
114
* Từ đó decode sang mã Ascii từ 4 pixel value trên ghép lại đúng thứ tự (r,g,b,a), thì thu được passwword là: Les-Parfums-de-LAmour cho file .pdf 
* Nhập password vào file .pdf thì thu được ảnh chứa flag:
* ![Screenshot 2023-12-19 133349](https://hackmd.io/_uploads/B1fjq3RLa.png)
Flag : KCSC{TH3_Dr1Ve_h4S_Re@NiM4t3d}

**VBS**
 
* Đầu tiên em tải về 1 file tên code.zip, em dùng lệnh file để kiểm tra file .zip đó và dùng unzip để extract nhưng cần phải có password và có một file tên code.vbs được nén trong đó
* Và em tự đặt ra một giả thuyết là có khi nào tác giả dùng password đó đánh lừa chúng ta không và có thể có một tệp giống tệp được nén trong file zip cũng như vậy nên em dùng binwalk -e để trích xuất ra file ẩn và kết quả là ra một file code.vbs giống file trong được nén trong code.zip nhưng mà nó là một tệp rỗng => giả thuyết của em sai.
* Nên bây giờ em sẽ nghĩ đến hướng crack password và em có lên mạng tìm hiểu thì có tool John the Ripper và cũng đã xem qua cách dùng.
* Tiếp theo em dùng:
1. lệnh zip2john code.zip > code.hash: dùng để chuyển đổi data file .zip sang mã hash để thuận tiện cho việc crack.
2. em gõ lệnh john --wordlist=/usr/share/wordlists/rockyou code3.hash : dùng để so sánh danh sách password trong file rockyou.txt với password của file .zip xem có giống hoặc gần giống để nó có thể trích xuất ra passwword chính.
3. cuối cùng ta dùng lệnh john --show code.hash : để show ra password 
4. passwword là:kma9239  
* Ảnh thực hiện các bước trên:
![Screenshot 2023-12-19 182234](https://hackmd.io/_uploads/ryn5JZ1PT.png)

* nhập password vào và xem nội dung file code.vbs bên trong:

![Screenshot 2023-12-19 182843](https://hackmd.io/_uploads/ry9iJbJD6.png)

* Em thấy có một đường link nên đã paste lên google thì thu được đoạn base64:S0NTQ3tWYjRfaDAxX2xvciIiX2FlX3RoMG5nX2M0bV89KCgofQ== 
* decode base 64 ra được flag:
Flag : KCSC{Vb4_h01_lor""_ae_th0ng_c4m_=(((}









