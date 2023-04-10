### 1. Switch Database
#### Đề cập đến CSDL hiện tại của bạn
```sql
db
```
#### Chuyển Database, ví dụ chuyển sang database tên là "examples"
```sql
use examples
```
##### Không cần tạo database trước khi switch, MongoDB tự dữ liệu khi ta lưu trữ lần đầu tiên. Giống như việc tạo collection đầu tiên trong database.

### 2. Insert
#### Chèn một Collection
- Mongo lưu tài liệu dưới dạng Collection. 
- Collection tương tự như bảng trong csdl quan hệ.
- Nếu *Collection* không tồn tại, MongoDB *tạo colllection* khi bạn lưu data lần đầu cho *Collection* đó.
- ##### Document là gì?: MongoDB lưu dữ bản ghi dữ liệu dưới dạng *BSON documents*. *BSON* là kiểu nhị phân thay thế cho *JSON documents*. BSON type: 
	- **field**: *values*
	- giá trị của một field có thể là bất cứ loại BSON data nào. Including other *documents*, *arrays*, and *arrays of document*
	- ![[Pasted image 20230331141022.png]]
- #Field Names là *string*
	- *_id* được dành riêng để làm khóa chính. Giá trị này bắt buộc là duy nhất trong *collection*, bất biến, và có thể là bất cứ loại gì-ngoại trừ mảng. Nếu *_id* bao gồm trường con, thì tên của trường con đó không thể bắt đầu bằng một kí tự *$*
	- Field Name không chứa kí tự *null*
	- Máy chủ cho phép lưu trữ field name có chứa *dots(.)* và *dollar signs ($)*.
- #Dot #Notation MongoDB sử dụng dotnotation để acces elements of an array and to access the fields of an embedded document
- #Array Để chỉ định hoặc truy cập an element of an array by the zero-based index position, nối liền tên array với dot(.) và zero-based index position, và kết thúc bằng dấu ngoặc kép:
```sql
"<array>.<index>"
```
- ![[Pasted image 20230331144935.png]]
- ![[Pasted image 20230331145049.png]]
- #Embeded #Document Vẫn dùng dot(.) để truy cập
	- ![[Pasted image 20230331145205.png]]
	- ##### Chú ý: Trường con ở trong object không được bắt đầu bằng dấu dot(.)
- Ví dụ:
	- Chèn một *documents* vào *collection* Movies dùng phương thức **insertMany()**
```sql
db.movies.insertMany([
   {
      title: 'Titanic',
      year: 1997,
      genres: [ 'Drama', 'Romance' ],
      rated: 'PG-13',
      languages: [ 'English', 'French', 'German', 'Swedish', 'Italian', 'Russian' ],
      released: ISODate("1997-12-19T00:00:00.000Z"),
      awards: {
         wins: 127,
         nominations: 63,
         text: 'Won 11 Oscars. Another 116 wins & 63 nominations.'
      },
      cast: [ 'Leonardo DiCaprio', 'Kate Winslet', 'Billy Zane', 'Kathy Bates' ],
      directors: [ 'James Cameron' ]
   },
   {
      title: 'The Dark Knight',
      year: 2008,
      genres: [ 'Action', 'Crime', 'Drama' ],
      rated: 'PG-13',
      languages: [ 'English', 'Mandarin' ],
      released: ISODate("2008-07-18T00:00:00.000Z"),
      awards: {
         wins: 144,
         nominations: 106,
         text: 'Won 2 Oscars. Another 142 wins & 106 nominations.'
      },
      cast: [ 'Christian Bale', 'Heath Ledger', 'Aaron Eckhart', 'Michael Caine' ],
      directors: [ 'Christopher Nolan' ]
   },
   {
      title: 'Spirited Away',
      year: 2001,
      genres: [ 'Animation', 'Adventure', 'Family' ],
      rated: 'PG',
      languages: [ 'Japanese' ],
      released: ISODate("2003-03-28T00:00:00.000Z"),
      awards: {
         wins: 52,
         nominations: 22,
         text: 'Won 1 Oscar. Another 51 wins & 22 nominations.'
      },
      cast: [ 'Rumi Hiiragi', 'Miyu Irino', 'Mari Natsuki', 'Takashi Naitè' ],
      directors: [ 'Hayao Miyazaki' ]
   },
   {
      title: 'Casablanca',
      genres: [ 'Drama', 'Romance', 'War' ],
      rated: 'PG',
      cast: [ 'Humphrey Bogart', 'Ingrid Bergman', 'Paul Henreid', 'Claude Rains' ],
      languages: [ 'English', 'French', 'German', 'Italian' ],
      released: ISODate("1943-01-23T00:00:00.000Z"),
      directors: [ 'Michael Curtiz' ],
      awards: {
         wins: 9,
         nominations: 6,
         text: 'Won 3 Oscars. Another 6 wins & 6 nominations.'
      },
      lastupdated: '2015-09-04 00:22:54.600000000',
      year: 1942
   }
])
```

--> Việc này trả về một document bao gồm *acknowledgement indicator* và 1 array bao gồm *id* của mỗi *documents* được chèn vào thành công.
```sql
{
	acknowledged: true,
	insertedIds: {
		'0': ObjectId("642661ac5d774e7567791929"),
		'1': ObjectId("642661ac5d774e756779192a"),
		'2': ObjectId("642661ac5d774e756779192b"),
		'3': ObjectId("642661ac5d774e756779192c"),
	}
}
```

### 3. Find All
- Để *select* document từ 1 collection, ta dùng phương thức *db.collection.find()*. 
- Để *select all* documents từ collecion, dùng 1 empty documents như là *query filter document* vào method trên:
	```sql
	db.movies.find( {});
```
***Ở đây, "movies" là tên của collection
- Query filter phải là a plain object or ObjectId
### 4. Filter Data
#### Filter Data with Comparison Operators: *Lọc dữ liệu với toán tử so sánh*
- Để chỉ định quan hệ bằng nhau <**field**> = <**values**> , hãy chỉ định <**field**> : <**value**> trong query filter document và đặt vào **db.colleciton.find()** method.
```sql
db.movies.find( { "directors": "Christopher Nolan" } );
```
#### Có thể dùng toán tử quan hệ để thực hiện các truy vấn nâng cao hơn
##### Câu lệnh sau trả về các movies mà được released trước năm 2000
```sql
db.movies.find( {"released": {$lt: ISODate("2000-01-01") } } );
```
##### Câu lệnh sau trả về movies mà win hơn 100 awards:
```sql
db.movies.find( {"awards.wins": { $gt: 100} } );
```
##### Câu lệnh sau trả về movies mà mảng *language* contains  *hoặc* Japanese or Mandarin:
```sql
db.movies.find( {"language": {$in: ["Japanese", "Mandarin" ] } } )
```
### 5. Project Fields: Chỉ định Fields để trả về (Projection)
##### Để chỉ định kiểu trả về, đưa Projectionn document (kiểu tham chiếu của document) vào *db.collection.find()* method:
<*field*> : 1 thì bao gồm nó trong return documents
<*field*> : 0 thì loại trừ nó khỏi return documents
ex: *trả về* id, title, direction, year
```sql
db.movies.find( { }, { "title": 1, "directors": 1, "year": 1 } );
```
- Không cần phải chỉ định trả về *_id* field, nó luôn tự động trả về, để không trả về trường id, set nó thành **0**. 
ex: trả về chỉ duy nhất trường title và trường genres:
```sql
db.movies.find( { }, { "_id": 0, "title": 1, "genres": 1 } );
```
### 6. Aggregate: Aggregate Data($group)
Dùng aggregation để group values from nhiều documents together and return a single result.
- Quy trình aggregation cho phép thao tác với dữ liệu, thực hiện các phép tính và viết các truy vấn rõ ràng hơn, cao hơn truy vấn CRUD.
- 