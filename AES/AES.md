#I. Giới thiệu về AES

Vào năm 1990, nhận thấy hệ mật Data Encryption Standard (DES) có kích thước khóa ngắn, có thể bị phá vỡ trong tương lai gần, National Institute of Standards and Technology (NIST) đã kêu gọi một cuộc thi thiết kế hệ mật mới. Cuối cùng thuật toán Rijndael được chọn và đổi tên thành Advanced Encryption Standard (AES) và chính thức được công bố vào năm 2001. Thuật toán được thiết kế bởi hai nhà mật mã học người Bỉ: Joan Daemen và Vincent Rijmen. Thuật toán này được đặt tên là “Rijndael”. Thuật toán được dựa trên bải thiết kế Square có trước đó của Deamen và Rijman, còn Square lại được thiết kế dựa trên Shark.

AES là mã khối đối xứng, vì thế cùng 1 khóa được dùng cho cả việc mã hóa và giải mã. AES được biết đến như một hệ mật tối ưu về: mức độ bảo mật, chi phí thực hiện và khả năng triển khai. Khác với DES sử dụng Feistel. AES sử dụng mạng thay thế - hoán vị. Mặc dù hai tên AES và Rijndael vẫn thương được gọi thay thế cho nhau nhưng trên thực tế thì hai thuật toán này không hoàn toàn toàn giống nhau. AES chỉ làm việc với các khối dữ liệu (đầu vào và đầu ra) 128 bits và khóa có độ dài 128, 192 và 256 bits trong khi Rijndael có thể làm việc với dự liệu và khóa có độ dài bất kỳ và bội số của 32 bits nằm trong khoảng từ 128 tới 256 bits. Các khóa con sử dụng trong các chu trình được tạo ra bởi khóa con Rijndael. 

Cho đến thời điểm này, AES vẫn chưa thể bị tấn công. Tuy nhiên có thể báo trước trong tương lai gần, AES sẽ không còn đủ vững vàng trước những siêu máy tính mới. Dù vậy, AES đã và đang được sử dụng ở rất nhiều nơi trên thế giới, ngay cả trong các thiết bị di động thông minh.

#II. Cấu trúc hệ mật 

##1. Mật mã hóa

###1.1. Các đơn vị dữ liệu sử dụng
<img src="http://i.imgur.com/32lbE8Q.png">
<img src="http://i.imgur.com/WwtfQoG.png">

###1.2. Một số phép toán sử dụng 
<img src="http://i.imgur.com/WvCe7Vm.png">

###1.3. Sơ đồ tổng quát
<img src="http://i.imgur.com/zZN7bhH.png">

Dữ liệu đầu vào được chia thành từng Block 128 bit. Các Block này tiếp tục được chuyển về dạng mảng 2 chiều State kích thước 4x4 byte rồi lần lượt được đưa vào cho AES mã hóa.

Giống với DES, AES cũng mã hóa qua nhiều vòng. Ứng với các kích thước khóa khác nhau mà số vòng lặp cũng khác nhau: Với khóa 128 bit, AES có 10 vòng lặp; 12 vòng đối với khóa 192 bit và 14 vòng lặp đối với khóa 256 bit.

Mỗi vòng lặp lại có 1 khóa riêng, các khóa này đều được sinh ra từ khóa gốc thông qua bộ sinh khóa Key Expansion

###1.4. Sơ đồ từng vòng lặp

Các bước thực hiện:

- SubBytes: Đây là phép thay thế (phi tuyến) trong đó mỗi byte trong trạng thái sẽ được thay thế bằng một byte khác theo bảng tra.

- ShiftRows: Dịch chuyển các hàng trong trạng thái theo số bước dịch khác nhau.

- MixColumns: Quá trình trộn làm việc theo các cột trong khối theo một phép biến đổi tuyến tính.

- AddRoundKey: Mỗi cột của trạng thái đầu tiên lần lượt được kết hợp với một khóa con theo thứ tự từ đầu dãy khóa.

Tại round cuối của quá trình lập mã. Bước MixColumns sẽ không thực hiện.

<img src="http://i.imgur.com/xNb1FEe.png">

###1.5. SubBytes

Biểu diễn mỗi Byte bằng 2 ký tự hệ HEX. Sử dụng 2 ký tự này làm địa chỉ để tra bảng SubBytes Table để thu được Byte thay thế.

Bảng này thu được dựa trên 2 phép biến đổi:

- Lấy giá trị nghịch đảo nhân trong trường GF(28) của byte.

- Thực hiện phép biến đổi sau:

<img src="http://i.imgur.com/gmuAGUc.png">

Với 0 ≤ i < 8, bi là bit thứ i của byte, ci là bit thứ i của byte c có giá trị là {63} hay {01100011}. Điều này làm cho mỗi bit đều phụ thuộc vào các bit còn lại trong byte.

Biến đổi SubBytes còn có thể được biểu diễn dưới dạng ma trận:

<img src="http://i.imgur.com/1sKC0yE.png">

<img src="http://i.imgur.com/LDgHzOb.png">

Để tiện sử dụng, bảng SubBytes cho quá trình mã hóa và bảng InvSubBytes cho quá trình giải mã được xây dụng dựa trên phép biến đổi trên

<img src="http://i.imgur.com/bOl06XO.png">

<img src="http://i.imgur.com/KB0NR3H.png">

Mục đích của SubBytes là đẻ chống lại hình thức tấn công Known – plaintext. Quan hệ giữa Input và Ouput của phép SubBytes không thể mô tả bẳng một công thức toán học đơn giản.

<img src="http://i.imgur.com/bKbK8Jd.png">

Ví dụ

<img src="http://i.imgur.com/iPuDn25.png">

Quá trình SubBytes diễn ra ở phía mã hóa và InvSubBytes diễn ra ở phía giải mã. 2 quá trình này là 2 quá trình đảo của nhau.

###1.6. ShiftRows

Lần lượt dịch từng hàng của state 0,1,2,3 vị trí. Thực chất ShiftRows là một phương pháp để hoán vị.

<img src="http://i.imgur.com/bf59lW8.png">

Quá trình ShiftRows diễn ra ở phía mã hóa và InvShiftRows diễn ra ở phía giải mã. 2 quá trình này là 2 quá trình đảo của nhau.

Ví dụ:

<img src="http://i.imgur.com/FZTBIwx.png">

###1.7. MixColumns

<img src="http://i.imgur.com/rDeuN6q.png">

Thao tác MixColumns biến đổi độc lập từng cột trong State. Mỗi cột được coi như một đa thức trong trường GF(28) và được nhân với đa thức a(x) modulo x^4+1

<img src="http://i.imgur.com/nNUhJbH.png">

a(x) là đa thức được tạo ra từ các hàng của 2 ma trận cố định được dùng cho quá trình MixColumns và InvMixColumns:

<img src="http://i.imgur.com/N8Uvete.png">

Ví dụ:

<img src="http://i.imgur.com/mC77qTn.png">

Mục đích của Mix Columns: Việc nhân a(x) và sau đó modulo với n(x) làm cho mỗi bytes trong cột kết quả đều phụ thuộc vào 4 bytes trông cột ban đầu. Thao tác MixColumns kết hợp với ShiftRows đảm bảo rằng sau một vài vòng biến đổi, 128 bits trong kết quả đều phụ thuộc vào tất cả 128 bits ban đầu. Điều này tạo ra tính Diffusion cần thiết cho mã hóa.

Quá trình MixColumns diễn ra ở phía mã hóa và InvMixColumns diễn ra ở phía giải mã. 2 quá trình này là 2 quá trình đảo của nhau.

###1.8. AddRoundKey

AddRoundKey là một phép cộng ma trận xử lý từng cột mỗi lần

AddRoundKey cộng thêm 1 keyword vào mỗi cột của của state:

<img src="http://i.imgur.com/JGv6H9J.png">

<img src="http://i.imgur.com/pX3ISEw.png">

wi là word thứ i của Round Key ứng với vòng lặp. 
AddRoundKey là quá trình tự nghịch đảo do sử dụng phép XOR.

###1.9. Key Expansion

Để tạo các Round Key cho mỗi vòng lặp, AES sử dụng một bộ sinh khóa gọi là Key Expansion.

Nếu số vòng lặp là Nr thì số lượng khóa do Key Expansion tạo ra sẽ là Nr+1 khóa 128 bits dựa trên khóa 128 bits đầu vào.

<img src="http://i.imgur.com/ujPzf5L.png">

Quá trình sinh khóa diễn ra như sau:
- 4 words đầu tiên (w0, w1, w2, w3) nhận được từ khóa đầu vào: cứ 4 bytes được ghép lại với nhau thành 1 word.
- Các khóa tiếp theo được tạo ra theo cách sau:

<img src="http://i.imgur.com/Ym3J47Q.png">

RotWord (Rotate Word): dịch vòng sang trái 1 byte

SubWord (Substitute Word): thay thế các byte trong word dựa trên công thức chuyển đổi giống như phần SubBytes

RCon: là 1 dãy 4 byte với byte đầu tiên là 2^(round-1) các byte còn lại đều bằng 0

<img src="http://i.imgur.com/FnQJS7k.png">

##2. Giải mật mã

Quá trình giải mật mã là quá trình làm ngược lại với quá trình mật mã hóa do toàn bộ các khối mã hóa sử dụng đều có tính nghịch đảo (SubBytes, ShiftRows và MixColumns) hoặc tự nghịch đảo (AddRoundKey). Các khối này lần lượt được gọi là InvSubBytes, InvShiftRows, InvMixColumns và AddRoundKey.

2 khối InvSubBytes và InvShiftRows có thể đổi vị trí cho nhau do 2 phép biến đổi này là độc lập, sau khi 1 State đi qua 2 khối này theo bất kỳ thứ tự nào, vẫn cho ra cùng 1 State kết quả như nhau.

2 khối InvMixColumns và AddRoundKey cũng có thể đổi vị trí cho nhau nếu chỉnh sửa lại hoạt động 2 khối nhờ tính chất:

**InvMixColumns(State XOR Round Key) = InvMixColumns(State) XOR InvMixColumns(Round Key)**

##3. Thám mã

Tính đến hiện tại mọi nỗ lực thám mã AES vẫn chưa thành công. Mọi phương pháp từng được áp dụng trên DES đều được thử trên AES:

- Brute-Force Attack - Tấn công vét cạn: AES bảo mật hơn DES vì kích thước khóa lớn hơn nhiều

- Statistical Attack - Tấn công dựa trên thống kê: Rất nhiều bài kiểm tra để thống kê các con số liên quan đến AES được thực hiện nhưng đều thất bại

- Diffential and Linear Attack – Tấn công vi sai và tiệm cận: Hiện tại chưa tìm ra được cách tấn công này trên AES

#III. Ưu nhược điểm của AES

##1. Ưu điểm của AES

- Có độ an toàn cao, và được sử dụng trong thông tin mật.

- Có mô tả toán học đơn giản, cấu trúc rõ ràng.

- Dễ dàng triển khai trên nhiều nền tảng khác nhau

- Chi phí triển khai thấp.

- Tốc độ mã hóa nhanh.

##2. Nhược điểm của AES

- Cấu trúc toán học của AES có mô tả toán học khá đơn giản. Tuy điều này chữa dẫn đến mỗi nguy hiểm nào nhưng một số nhà nghiên cứu cho rằng sẽ có người lợi dụng được cấu trúc này trong tương lai.

##3. Ứng dụng của AES

- Hiện nay, AES được sử dụng phổ biến trên toàn thế giới để bảo vệ dữ liệu ở các tổ chức ngân hàng, tài chính, chính phủ, thương mại…

- AES được ứng dụng nhanh đối với cả phần cứng và phần mềm, chỉ yêu cầu một không gian lưu trữ nhỏ, lý tưởng để sử dụng cho việc mã hóa những thiết bị cầm tay nhỏ như ổ USB flash
