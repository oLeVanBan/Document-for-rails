# Document-for-rails
### Xem log của server:
- Log sever môi trường development được xem hiện thị ở terminal (ubuntu), bash ( mac OS), powershell (window)
- Check log đã được lưu trữ ở log/development.log, đối với linux, có thể lọc thông tin của log bằng cách sử dụng 
``` tail -f log/development.log | grep ```
### Thông tin của log :
```sql
Started GET "/vi/products/50" for 127.0.0.1 at 2018-07-25 16:17:40 +0700
Processing by ProductsController#show as HTML
  Parameters: {"locale"=>"vi", "id"=>"50"}
  Cart Load (0.3ms)  SELECT  "carts".* FROM "carts" WHERE "carts"."id" = ? LIMIT ?  [["id", 1], ["LIMIT", 1]]
  Product Load (0.3ms)  SELECT  "products".* FROM "products" WHERE "products"."id" = ? LIMIT ?  [["id", 50], ["LIMIT", 1]]
  Rendering products/show.html.erb within layouts/application
  Brand Load (0.2ms)  SELECT "brands".* FROM "brands" ORDER BY "brands"."created_at" DESC
   (0.1ms)  SELECT COUNT(*) FROM "products" WHERE "products"."brand_id" = ?  [["brand_id", 10]]
   (0.1ms)  SELECT COUNT(*) FROM "products" WHERE "products"."brand_id" = ?  [["brand_id", 9]]
   (0.1ms)  SELECT COUNT(*) FROM "products" WHERE "products"."brand_id" = ?  [["brand_id", 8]]
   (0.1ms)  SELECT COUNT(*) FROM "products" WHERE "products"."brand_id" = ?  [["brand_id", 7]]
   (0.1ms)  SELECT COUNT(*) FROM "products" WHERE "products"."brand_id" = ?  [["brand_id", 6]]
   (0.1ms)  SELECT COUNT(*) FROM "products" WHERE "products"."brand_id" = ?  [["brand_id", 5]]
   (0.1ms)  SELECT COUNT(*) FROM "products" WHERE "products"."brand_id" = ?  [["brand_id", 4]]
   (0.1ms)  SELECT COUNT(*) FROM "products" WHERE "products"."brand_id" = ?  [["brand_id", 3]]
   (0.2ms)  SELECT COUNT(*) FROM "products" WHERE "products"."brand_id" = ?  [["brand_id", 2]]
   (0.1ms)  SELECT COUNT(*) FROM "products" WHERE "products"."brand_id" = ?  [["brand_id", 1]]
  Rendered brands/_brand.html.erb (8.5ms)
  ItemPhoto Load (0.1ms)  SELECT  "item_photos".* FROM "item_photos" WHERE "item_photos"."product_id" = ? ORDER BY "item_photos"."id" ASC LIMIT ?  [["product_id", 50], ["LIMIT", 1]]
  ItemPhoto Load (0.1ms)  SELECT "item_photos".* FROM "item_photos" WHERE "item_photos"."product_id" = ?  [["product_id", 50]]
  Brand Load (0.1ms)  SELECT  "brands".* FROM "brands" WHERE "brands"."id" = ? LIMIT ?  [["id", 7], ["LIMIT", 1]]
  Comment Load (0.1ms)  SELECT  "comments".* FROM "comments" WHERE "comments"."product_id" = ? ORDER BY "comments"."created_at" DESC LIMIT ? OFFSET ?  [["product_id", 50], ["LIMIT", 30], ["OFFSET", 0]]
  Rendered comments/_comment.html.erb (0.8ms)
  Rendered comments/_new_comment.html.erb (0.8ms)
  Rendered products/show.html.erb within layouts/application (35.3ms)
  CACHE Cart Load (0.0ms)  SELECT  "carts".* FROM "carts" WHERE "carts"."id" = ? LIMIT ?  [["id", 1], ["LIMIT", 1]]
   (0.1ms)  SELECT COUNT(*) FROM "line_items" WHERE "line_items"."cart_id" = ?  [["cart_id", 1]]
  CACHE Cart Load (0.0ms)  SELECT  "carts".* FROM "carts" WHERE "carts"."id" = ? LIMIT ?  [["id", 1], ["LIMIT", 1]]
  CACHE  (0.0ms)  SELECT COUNT(*) FROM "line_items" WHERE "line_items"."cart_id" = ?  [["cart_id", 1]]
  Rendered layouts/_header.html.erb (3.6ms)
  Rendered layouts/_footer.html.erb (0.5ms)
Completed 200 OK in 87ms (Views: 77.9ms | ActiveRecord: 3.1ms)
```
- URL được gọi đến : log tách riêng mỗi request bằng 1 dòng trống . Ở đầu dòng ta có thể thấy request, thời điểm request cho 1 URL cụ thể. Ví dụ:<br/>
  * `Started GET "/vi/products/49" for 127.0.0.1 at 2018-07-18 13:42:13 +0700 `có nghĩa là một `GET` request được gửi đến URL `/vi/products/49`.
- Controller và action xử lí request: dòng tiếp theo cho biết request được xử lí bởi controller nào và action nào? Processing by `ProductsController#show` as HTML có nghĩa là GET request trên được xử lí bở ProductController và action Show, thành phần HTML đằng sau có nghĩa là định dạng HTML được request ( có thể request nhiều định dạng khác nhau Json, XML, JS :v)
- Parameter được truyền lên: mỗi request có thể truyền lên các paramter (trong controller có thể truy cập qua params), các parameter sẽ được hiển thi trên log. VD: <br/>
* `Parameters: {"locale"=>"vi", "id"=>"49"}` bên trên tương ứng với param được truyền lên là locale và id, trong controller có thể truy cập qua `params[:id]` và `params[:locale]`
- Các câu lệnh SQL được chạy: Rails hỗ trợ ORM, nên ít khi phải viết truy vẫn thuần, mọi câu lệnh SQL sinh ra chạy trong quá trình xử lí request sẽ được hiển thị tại log. Trong hình bên trên, để hiện thị page show của product (chưa kể partial trong layout), cần load thông tin của card và thông tin của product, câu lệnh SQL sinh ra được hiện thị tại log:<br/>
```sql
  Cart Load (0.7ms)  SELECT  "carts".* FROM "carts" WHERE "carts"."id" = ? LIMIT ?  [["id", 2], ["LIMIT", 1]]
  Product Load (0.1ms)  SELECT  "products".* FROM "products" WHERE "products"."id" = ? LIMIT ?  [["id", 49], ["LIMIT", 1]]

```
- Các template và partial được render : trong log list ra tất cả các view và các partial được sử dụng để compile cho froned output :<br/>
```
Rendered brands/_brand.html.erb (15.6ms)
Rendered comments/_comment.html.erb (1.1ms)
Rendered comments/_new_comment.html.erb (0.9ms)
Rendered products/show.html.erb within layouts/application (24.9ms)
Rendered layouts/_header.html.erb (6.4ms)
Rendered layouts/_footer.html.erb (1.1ms)
```
- Cache được sử dụng : trong ví dụ bên trên 
```sql
  CACHE  (0.0ms)  SELECT COUNT(*) FROM "line_items" WHERE "line_items"."cart_id" = ?  [["cart_id", 2]]
```
 câu lệnh SQL được cache và dữ liệu được lấy từ cache ra mà không truy cập vào database (thời gian excute là 0.0ms)
 - Các exception, và warning : các exception, các lỗi backtrace đều được hiển thị ở log. VD lỗi chưa migration:
 ```
 Started GET "/" for 127.0.0.1 at 2018-07-25 16:38:45 +0700
   (0.1ms)  SELECT "schema_migrations"."version" FROM "schema_migrations" ORDER BY "schema_migrations"."version" ASC

ActiveRecord::PendingMigrationError - Migrations are pending. To resolve this issue, run:

        bin/rails db:migrate RAILS_ENV=development

:

Started POST "/__better_errors/e00ddf35df005747/variables" for 127.0.0.1 at 2018-07-25 16:38:45 +0700
```

### Các quan hệ của ActiveRecord:

- Trong rails, association là kết nối giữa 2 model. Việc sử dụng association giúp cho việc xử lí và truy vấn trở nên đơn giản, dễ vận hành hơn , cũng gần như việc ORM giúp cho việc thao tác với DB trở nên đơn giản hơn , và hầu như ko phải viết SQL thuần =)).
##### 1. Belongs_to và has_one:
Cả hai đều là kết nối một - một giữa 2 model:<br />
![](https://github.com/ninhle9984/Document-for-rails/blob/master/images/belongs_to.png)
</br>
Ví dụ: mỗi người dùng có một tài khoản, ta có 2 model User và Account có thể setup như sau:<br/>
Trong migration file:
```ruby
class CreateUsers < ActiveRecord::Migration[5.2]
  def change
    create_table :users do |t|
      t.string :name
      ………………
      t.timestamps
    end
  end
end

class CreateAccounts < ActiveRecord::Migration[5.2]
  def change
    create_table :accounts do |t|
      t.email :name
      t.belongs_to :user, index: true
      …………………………
      t.timestamps
    end
  end
end


```

  Dòng ` t.belongs_to :user, index: true ` khi chạy migration sẽ tạo ra cột user_id được đánh index tương ứng trong bảng `Account`. Khi đó trong model ta sẽ khai báo các association tương ứng:
  ```ruby
  class Account < ApplicationRecord
     belongs_to :user
  end
  
  class User < ApplicationRecord
     has_one :account
  end
  ```
**Note:** belongs_to và has_one khi khai báo trong model phải sử dụng singular (số ít), nếu ko kết quả chạy khi truy vấn sẽ không đúng như mong đợi: <br/>
VD: với `belongs_to`  nếu khai báo  `belongs_to :users` thì kết quả của Account.first.users trả về  `nil`
     với `has_one` nếu khai báo has_one :users thì kết quả truy vấn của `User.first.account` sẽ trả về error
     `NameError (uninitialized constant User::Accounts)` <br/>
     - Khi sử dụng `belongs_to` sẽ không thể tạo  được account nếu thiếu `ser_id` hoặc `user_id`không tồn tại nhưng `has_one` thì có, có thể tạo nhiều account, ko có `user_id`, `user_id` ko tồn tại hoặc trùng nhau. Trong trường hợp trùng nhau thì khi truy vấn `user.accoun`t sẽ trả về bản ghi đầu tiên.<br/>
     - Câu lệnh SQL tương ứng sinh ra:<br/>
     + VD: Khi truy vấn tài khoản của user đầu tiên `has_one` :<br/>
 ```sql
     Account Load (0.2ms)  SELECT  "accounts".* FROM "accounts" WHERE "accounts"."user_id" = ? LIMIT ?  [["user_id", 1], ["LIMIT", 1]]
```
vì có limit nên trường hợp tạo nhiều account thì cũng trả về bản ghi đầu tiên. <br/>
+ VD: Khi truy vấn thông tin của ngừời sở hữu account thứ nhất:
```sql
User Load (0.2ms)  SELECT  "users".* FROM "users" WHERE "users"."id" = ? LIMIT ?  [["id", 1], ["LIMIT", 1]]
```
+ Đối với việc khởi tạo quan hệ `has_one - belongs_to` khi khởi tạo nên cho thuộc tính `unique` cho ` foreign_key` để
chắc chắn rằng mỗi `user` chỉ có duy nhất một `account`. Như đã lưu ý ở trên `has_one` sử dụng limit khi truy vấn nên luôn chỉ trả về bản ghi đầu tiên, trong trường hợp setup như trên ko có `unique` có thể gán nhiều `account` cho một `user` làm cho nó giống với quan hệ `has_many - belongs_to`  nhưng về ngữ nghĩa thì 2 quan hệ hoàn toàn khác nhau. Migration của foreign cho quan hệ `has_many - belongs_to` được viết lại:
```sql
t.belongs_to :user,index: { unique: true }, foreign_key: true
```
### 2. Has_many:
Kết nối một - nhiều giữa một model với model khác. <br />
![](https://github.com/ninhle9984/Document-for-rails/blob/master/images/has_many.png) <br/>
Trường hợp vẫn set up như trên, coi một `user` có nhiều `account` ta chỉ việc khai báo lại trong model *User* :
<br />
```ruby
class User < ApplicationRecord
   has_many :accounts
end
```
Theo quy ước thì khi khai báo has_many thì sử dụng số nhiều (accounts), dùng số ít (account) thì vẫn chạy cho kết quả đúng, nhưng để cho đúng nggữ nghĩa thì nên dùng số nhiều.<br />
Câu lệnh SQL sinh ra khi truy vấn tài khoản của user đầu tiên  :
```sql
Account Load (0.2ms)  SELECT  "accounts".* FROM "accounts" WHERE "accounts"."user_id" = ? [["user_id", 1]]
```
So với khi khai báo sử dụng `has_one` thì không còn sử dụng LIMIT trong câu lệnh SQL nữa.<br/>

##### 3. Has_one :through
Kết nối một - một với một model khác, nhưng thông qua model trung gian. Vẫn ví dụ trên 1.1, có thêm model Exchange quan hệ một - một với Account.<br/>
![](https://github.com/ninhle9984/Document-for-rails/blob/master/images/has_one_through.png)
<br/>
Migration của exchange:<br/>
```sql
class CreateExchanges < ActiveRecord::Migration[5.2]
  def change
    create_table :exchanges do |t|
      t.string :exchange_type
      t.belongs_to :account, index: true

      t.timestamps
     end
   end
end
```
Trong model của sẽ viết như sau: 
```ruby
class User < ApplicationRecord
  has_one :account
  has_one :exchange, through: :account
end

class Account < ApplicationRecord
  belongs_to :user
  has_one :exchange
end

class Exchange < ApplicationRecord
  belongs_to :account
end
```
Khi đó có thể sử dụng phương thức `user.exchange` để truy vấn exchange của thay vì `user.account.exchage` <br/>
Câu lệnh SQL tương ứng sinh ra sẽ join 2 bảng *Exchange* và *Account* để lấy kết quả thay vì 2 câu lệnh `SELECT WHERE` khi sử dụng ` user.account.exchage`

##### 4. Has_many :through
  Kết nối nhiều - nhiều với một model khác thông qua model trung gian.<br/>
  ![](https://github.com/ninhle9984/Document-for-rails/blob/master/images/has_many_throght.png)
  <br />
   VD: một user có tham gia nhiều club, một club có nhiều user, ta xây dụng mô hình `has_many :through` qua một model trung gian là ClubUser , khai báo như sau:<br/>
   Migration file tương ứng của *User* vẫn không thay đổi còn của  *ClubUser* và *Club* như sau:
```ruby
class CreateClubUsers < ActiveRecord::Migration[5.2]
  def change
    create_table :club_users do |t|
      t.belongs_to :user, index: true
      t.belongs_to :club, index: true

      t.timestamps
    end
  end
end

class CreateClubs < ActiveRecord::Migration[5.2]
  def change
    create_table :clubs do |t|
      t.string :name
      ..................................
      t.timestamps
    end
  end
end

```
  Trong model sẽ được khai báo như sau:
```ruby
class User < ApplicationRecord
  has_many :club_users
  has_many :clubs, through: :club_users
end

class Club < ApplicationRecord
  has_many :club_users
  has_many :users, through: :club_users
end

class ClubUser < ApplicationRecord
  belongs_to :user
  belongs_to :club
end
```
Với khai báo ta có thể sử dụng `user.clubs` và `club.users` để in ra danh sách các club mà user tham gia hoặc danh sách user của club.<br/>
Với quan hệ như vậy thì SQL sẽ sử dụng join bảng trung gian *ClubUser* với bảng tương ứng là *User* hoặc *Club* và dụng `WHERE` để lọc dữ liệu:<br/>
`user.clubs`:
```sql
Club Load (0.3ms)  SELECT "clubs".* FROM "clubs" INNER JOIN "club_users" ON "clubs"."id" = "club_users"."club_id" WHERE "club_users"."user_id" = ?  [["user_id", 1]]
```
`club.users`:
```sql
User Load (0.5ms)  SELECT "users".* FROM "users" INNER JOIN "club_users" ON "users"."id" = "club_users"."user_id" WHERE "club_users"."club_id" = ?  [["club_id", 1]]
```

##### 5. Has_and_belongs_to_many
Tương tự has_many :through là kết nhiều nhiều - nhiều giữa 2 model nhưng qua JoinTable chứ ko qua model thứ 3.<br/>
Khi đó ta tạo bảng Join table có file migration như sau:
```ruby
class CreateJoinTableUserClub < ActiveRecord::Migration[5.2]
  def change
    create_table :clubs_users, id: false do |t|
      t.belongs_to :user, index: true
      t.belongs_to :club, index: true
    end
  end
end
```
Trong model:
```ruby
 class User < ApplicationRecord
   has_and_belongs_to_many :clubs
 end

 class Club < ApplicationRecord
   has_and_belongs_to_many :users
 end
```
Khi đó ta  có thể dùng các phương thức `user.clubs` và `club.users` để truy vấn dữ liệu tương ứng như `has_many :through`. Kết quả câu lệnh SQL cũng chạy tương tự, là join các bảng. Rails cũng cung cấp 1 list các method để có thể thêm xóa các đối tượng quan hệ (vì ko có model cho bảng join) như `collection << (object), collection.delete(object)`.<br/>
Về việc lựa chọn giữa `has_many :through` và `has_and_belongs_to_many` thì nên lựa chọn `has_many :through` trong trường hợp cần làm việc với model trung gian (trường hợp bảng trung gian ko chỉ có 2 trường foreign_key của 2  bảng đơn thuần), còn lại nên dùng `has_and_belongs_to_many` việc setup đơn giản hơn mà cũng cho hiệu quả tương tự.

### N+1 query:

##### 1. N+1 query là gì?
Trường hợp trong bảng User và Account (1), muốn lấy email của account của 10 user đầu tiên ta làm như sau:
```ruby
users = User.limit(10)
users.each do |user|
   puts user.account.email
end
```
Query sinh ra như sau:
```sql
  User Load (0.3ms)  SELECT  "users".* FROM "users" LIMIT ?  [["LIMIT", 10]]
    Account Load (0.5ms)  SELECT  "accounts".* FROM "accounts" WHERE "accounts"."user_id" = ? LIMIT ?  [["user_id", 1], ["LIMIT", 1]]
janae@millergrady.name
  Account Load (0.2ms)  SELECT  "accounts".* FROM "accounts" WHERE "accounts"."user_id" = ? LIMIT ?  [["user_id", 2], ["LIMIT", 1]]
terrill_lind@larkin.biz
  Account Load (0.2ms)  SELECT  "accounts".* FROM "accounts" WHERE "accounts"."user_id" = ? LIMIT ?  [["user_id", 3], ["LIMIT", 1]]
cindy_sauer@dickinson.name
  Account Load (0.0ms)  SELECT  "accounts".* FROM "accounts" WHERE "accounts"."user_id" = ? LIMIT ?  [["user_id", 4], ["LIMIT", 1]]
anibal_okeefe@doyle.net
  Account Load (0.0ms)  SELECT  "accounts".* FROM "accounts" WHERE "accounts"."user_id" = ? LIMIT ?  [["user_id", 5], ["LIMIT", 1]]
pasquale@stehrkonopelski.com
  Account Load (0.0ms)  SELECT  "accounts".* FROM "accounts" WHERE "accounts"."user_id" = ? LIMIT ?  [["user_id", 6], ["LIMIT", 1]]
sam.rodriguez@hoegergoldner.org
  Account Load (0.0ms)  SELECT  "accounts".* FROM "accounts" WHERE "accounts"."user_id" = ? LIMIT ?  [["user_id", 7], ["LIMIT", 1]]
thomas@bergstrom.name
  Account Load (0.0ms)  SELECT  "accounts".* FROM "accounts" WHERE "accounts"."user_id" = ? LIMIT ?  [["user_id", 8], ["LIMIT", 1]]
gideon@hyattzieme.name
  Account Load (0.0ms)  SELECT  "accounts".* FROM "accounts" WHERE "accounts"."user_id" = ? LIMIT ?  [["user_id", 9], ["LIMIT", 1]]
payton@schroeder.org
  Account Load (0.0ms)  SELECT  "accounts".* FROM "accounts" WHERE "accounts"."user_id" = ? LIMIT ?  [["user_id", 10], ["LIMIT", 1]]
blanche_bernier@kozeybuckridge.io
```
Có tất cả 11 câu query, câu đầu tiên dùng để load 10 user, 10 câu sau dùng để load account của 10 user đó, nếu có N user thì sẽ có N câu query tương ứng => N + 1 query
##### 2. Xử lí N+1 query
###### 2.1 Preload
Trường hợp trên sử dụng preload như sau:
```ruby
users = User.preload(:account).limit(10)
users.each do |user|
   puts user.account.email
end

```
Dùng preload, giúp ngăn N + 1 query, khi đó câu lệnh truy vấn sinh ra:
```sql
 User Load (0.3ms)  SELECT  "users".* FROM "users" LIMIT ?  [["LIMIT", 10]]
 Account Load (0.3ms)  SELECT "accounts".* FROM "accounts" WHERE "accounts"."user_id" IN (?, ?, ?, ?, ?, ?, ?, ?, ?, ?)  [["user_id", 1], ["user_id", 2], ["user_id", 3], ["user_id", 4], ["user_id", 5], ["user_id", 6], ["user_id", 7], ["user_id", 8], ["user_id", 9], ["user_id", 10]]

```
Chỉ có 2 câu query, câu đầu dùng để load toàn bộ `user`, câu thứ 2 dùng để load `account`, nhưng thay vì load đơn lẻ từng account dùng `WHERE` thì lại sử dụng `WHERE IN` load tất cả trong 1 câu query, nhanh hơn hẳn 0,5 ms =)), hay 2,66 lần trong trường hợp này.
###### 2.2 Eager_load:
  Thay vì sử dụng `WHERE IN` thì eager_load lại sử dụng `JOIN` để khử N + 1 query, câu lệnh trên được viết lại như sau:
```ruby
users = User.eager_load(:account).limit(10)
users.each do |user|
  puts user.account.email
end
```
 Query sinh ra:
 ```sql
  SQL (0.6ms)  SELECT  "users"."id" AS t0_r0, "users"."name" AS t0_r1, "users"."created_at" AS t0_r2, "users"."updated_at" AS t0_r3, "accounts"."id" AS t1_r0, "accounts"."email" AS t1_r1, "accounts"."user_id" AS t1_r2, "accounts"."created_at" AS t1_r3, "accounts"."updated_at" AS t1_r4 FROM "users" LEFT OUTER JOIN "accounts" ON "accounts"."user_id" = "users"."id" LIMIT ?  [["LIMIT", 10]]
 ```
###### 2.3 Includes:
  Trong điều kiện bình thường Includes hoạt động giống preload, nghĩa là sử dụng `WHERE IN` để truy vấn, trong một số trường hợp  thì Includes hoạt động giống eager_load nghĩa là sử dụng `LEFT OUTER JOIN`. Để hiểu kỹ hơn có thể xem qua ví dụ:
  -  Có thêm điều kiện `WHERE` trong cùng bảng:
  Đối với preload `users = User.preload(:account).where("users.id = 1")`, câu lệnh trên chạy cho kết quả:
```ruby
 User Load (0.3ms)  SELECT "users".* FROM "users" WHERE (users.id = 1)
 Account Load (0.2ms)  SELECT "accounts".* FROM "accounts" WHERE "accounts"."user_id" = ?  [["user_id", 1]]
```
Trường hợp này sử dụng includes `users = User.includes(:account)` thay cho preload cho kết qủa hoàn toàn giống nhau
- Trong trường hợp sử dụng điều kiện truy vấn đối với bảng quan hệ:
`User.includes(:account).where("accounts.id = ?", 1)` và `User.preload(:account).where("accounts.id = ?", 1)` đều trả lỗi ` no such column: accounts.id: SELECT  "users".* FROM "users" WHERE (accounts.id = 1) LIMIT ?`.<br />
- Sử dụng nested hash: `users = User.includes(:account).where(accounts: {id: 'joanie@johnston.org'})` includes sẽ tự convert câu lệnh sql sang join giống như trường hợp eager_load có thêm điều kiện `WHERE`:
```sql
SQL (0.1ms)  SELECT  "users"."id" AS t0_r0, "users"."name" AS t0_r1, "users"."created_at" AS t0_r2, "users"."updated_at" AS t0_r3, "accounts"."id" AS t1_r0, "accounts"."email" AS t1_r1, "accounts"."user_id" AS t1_r2, "accounts"."created_at" AS t1_r3, "accounts"."updated_at" AS t1_r4 FROM "users" LEFT OUTER JOIN "accounts" ON "accounts"."user_id" = "users"."id" WHERE "accounts"."id" = ? LIMIT ?  [["id", 0], ["LIMIT", 11]]
```
còn trường hợp sử dụng `eager_load` trả lỗi `Can't join 'User' to association named 'preload'`. Nếu muốn `includes` sử dụng `join` thay cho `WHERE IN` có thể sử dụng refereneces: `users = User.includes(:account).references(:account)`.
```sql
  SQL (0.3ms)  SELECT  "users"."id" AS t0_r0, "users"."name" AS t0_r1, "users"."created_at" AS t0_r2, "users"."updated_at" AS t0_r3, "accounts"."id" AS t1_r0, "accounts"."email" AS t1_r1, "accounts"."user_id" AS t1_r2, "accounts"."created_at" AS t1_r3, "accounts"."updated_at" AS t1_r4 FROM "users" LEFT OUTER JOIN "accounts" ON "accounts"."user_id" = "users"."id" LIMIT ?  [["LIMIT", 11]]
```
###### 2.4 Joins:
Khác với eager_load sử dụng LEFT OUTER JOIN thì joins sử dụng INNER JOIN để lấy dữ liệu<br/>
Trong VD trên viết lai sử dụng join như sau:
```ruby
users = User.joins(:account).select("user.*, accounts.email")
users.each do |user|
  puts user.email
end
```
Khi load chỉ sinh ra một câu query duy nhất:
```sql
 User Load (0.6ms)  SELECT users.*, accounts.email FROM "users" INNER JOIN "accounts" ON "accounts"."user_id" = "users"."id"
```
