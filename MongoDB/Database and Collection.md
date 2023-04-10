# Overview
*MongoDB* lưu trữ Data dưới dạng *documents* (cụ thể là BSON documents), được tập hợp lại với nhau trong các *colections*. Một *database* lưu trữ 1 hoặc nhiều *collections* of *documents*
# Database
Trong MongoDB, database chứa một hoặc nhiều collections of documents 
(Collection như Table, document như dòng dữ liệu, field là trường dữ liệu)
## Sử dụng DB:
```sql
use myDB
```
## Create a Database
Nếu database của bạn không tồn tại, MongoDB tạo database khi bạn lưu data lần đầu tiên cho nó. Vậy nên ta có thể chuyển đến 1 database không có sẵn và thực hiện hành động:
ex: 
```sql
use myNewDB

db.myNewCollection1.insertOne( { x: 1 } )
```
Hành động *insertOne()* tạo đồng thời database mới *myNewDB* và tạo collection mới *myNewCollection1* nếu nó không có sẵn. Điều kiện cần là đặt tên database và collection đúng.
### Collection
MongoDB lưu *documents* in *collections*. *Collections* tương tự *Table*
Tạo nếu chưa có
ex: 
```sql
db.myNewCollection2.insertOne( { x: 1 } )
db.myNewCollection3.createIndex( { y: 1 } )
```
Cả insertOne() và createIndex() đều tạo và ghi document vào collection mới.
- Cũng có phương thức dùng riêng để tạo 1 collection: *db.createCollection()*