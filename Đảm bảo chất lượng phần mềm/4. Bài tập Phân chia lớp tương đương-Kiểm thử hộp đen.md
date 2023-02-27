### Nhược điểm
- Phụ thuộc vào kinh nghiệm của lập trình viên
- Phân tích giá trị tại biên
- Các bước lấy giá trị tại biên
	- Tại biên, người ta chọn thêm 1 điểm nằm ở biên
	- Lấy một giá trị nằm ở dưới biên
	- Lấy giá trị nằm ở trên biên
#### Ex5 
	Form đăng í, có username, password và nút register
	-> Đưa ra các test case có thể có
	Username : từ 3 -> 15 kí tự, không có khoảng trắng, kí tự đặc biệt #, $, @
	Pass : nên là 8 kí tự, kí tự đầu tiên không phải 0
	Username + password ổn -> Register Successful
	Username invalid -> hiển thị "Invalid Username"
	Password invalid -> hiển thị "Invalid password"
	Cả 2 -> chỉ coi username invalid
	Valid username : từ 3 -> 15, không trắng, ko #, $, @
	Invalid username : 0 -> 2 || >= 15, có trắng, hoặc #, hoặc $, hoặc @ hoặc là trống
	Valid password : nhiều hơn 8 kí tự && kí tự đầu tiên không phải là 0
	Invalid password : kí tự đầu tiên là 0 hoặc là ít hơn 8 kí tự  hoặc là trống hoặc là chứa kí tự khác số
Miền dữ liệu
username : [3, 15] kí tự, không chứa #, $, @
	valid : [3, 15] kí tự, không chứa #, $, @
	invalid : < 3, > 15 và có chúa kí tự đặc biệt
-> EC của username
	- 3 kí tự | 15 kí tự + không chứa kí tự đặc biệt
	- 2 kí tự, no special character : ab
	- 16 kí tự : 
	- 4 trường hợp các kí tự đặc biệt : 
password : ít nhất 8 kí tự, không chứa kí tự 0
	valid: ít nhất 8 kí tự, không chứa kí tự 0
	invalid: ít hơn 8 kí tự hoặc có kí tự 0
Valid :
Invalid :

|Num|Username |Password|Result|
|-----|------------|-----------|-------|
|1|"aaa"|"aaa"|Invalid password|
|2|ab|12345678|Invalid username (2 kí tự)|
|3|iiiiiiiiiiiiiiii|12345678|Invalid username (16 kí  tự)|
|4|aweoifj 12324|12345678|Invalid username (chứ khoảng trắng)|
|5|ojg#ijwei|12345678|Invalid username (chứa #)|
|6|ojg@ioi|12345678|Invalid username (chứa @)|
|7|ojg$iojw|12345678|Invalid username (chứa $)|
|8|thienha|123|Invalid password (ít hơn 8 kí tự)|
|9|thienaha|0128888834|Invalid password (chứa kí tự 0 đầu tiên)|
|10|thienaha|0128888834j|Invalid password (chứa kí tự không phải là số)|
|11||123954829|Invalid username|
|12|thienha||Invalid password|
|13|ojg#12312|0987654321|Invalid username|

#### Ex6
	Nhận 3 giá trị ngày, tháng, năm
	Trả về ngày tiếp theo của input
	dd -> 1..31
	mm -> 1..12
	yyyy -> 1800...2999
	mm = {1, 3, 5, 7, 8, 10, 12} -> dd -> 1..31
	mm = {4, 6, 9, 11} -> dd -> 1..30
	m = {2} && yyyy nhuận -> 29 
	m = {2} && yyyy -> 28


#### Ex 7
	Tùy thuộc tình trạng hôn nhân và thời gian làm việc của mỗi người -> 1 giá
	kết hôn + >= 3 -> 70%
	kết hôn + < 3
		kết quả học tập tốt : 50 %
		else : 20%
	chưa kết hôn + >= 3 -> 60%
	chưa kết hôn + < 3
		kết quả học tập tốt : 40%
		else : 0%
	- c1 : hôn nhân
	- c2 : thời gian làm việc
	- c3 : kết quả học tập
#### Ex8
	- c1 : thỏa mãn các đẳng thức tam giác 
	- c2 : a == b
	- c3 : b == c
	- c4 : c == a
|#|Rule 1|Rule 2|Rule 3|Rule 4|Rule 5|Rule 6|Rule 7|Rule 8|Rule 9|Rule 10|Rule 11|Rule 12|Rule 13|Rule 14|Rule 15|Rule 16|
|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|
|C1|Yes|Yes|Yes|Yes|Yes|Yes|Yes|Yes|No|No|No|No|No|No|No|No|
|C2|Yes|Yes|Yes|Yes|No|No|No|No|Yes|Yes|Yes|Yes|No|No|No|No|
|C3|Yes|Yes|No|No|Yes|Yes|No|No|Yes|Yes|No|No|Yes|Yes|No|No|
|C4|Yes|No|Yes|No|Yes|No|Yes|No|Yes|No|Yes|No|Yes|No|Yes|No|
|Result|Đều|Đều|Đều|Cân|Đều|Cân|Cân|Thường|No|No|No|No|No|No|No|No|

