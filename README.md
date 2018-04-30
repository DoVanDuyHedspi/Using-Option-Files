### 4.2.6 Using Option Files
Hầu hết chương trình MySQL có thể đọc các tùy chọn khởi động từ các tệp tùy chọn
(nhiều khi được gọi là tệp cấu hình). Tệp tùy chọn cung cấp một cách thuận tiện để
chỉ định các lựa chọn thường được sử dụng để chúng không cần nhập vào dòng lệnh 
mỗi khi chạy một chương trình. 

Để xác định chương trình có đọc các tệp tùy chọn hay không, gọi nó bằng tùy chọn 
`--help`. (với [mysqlld](https://dev.mysql.com/doc/refman/5.7/en/mysqld.html), sử
dụng `--verbose` và `--help`). Nếu chương trình đọc các tệp tùy chọn, thông báo trợ 
giúp cho biết tệp nào sẽ hiển thị và nhóm tùy chọn nào sẽ nhận ra.

> Chú ý:
> Một chương trình MySQL bắt đầu với tùy chọn `--no-default` đọc các file  không 
đọc các tệp tùy chọn nào khác ngoài `.mylogin.cnf`.

Nhiều tệp tùy chọn là các tệp văn bản thuần túy, được tạo bằng bất kỳ trình soạn thảo văn bản nào.
Ngoại lệ là tệp `.mylogin.cnf` chứa các tùy chọn đường dẫn đăng nhập. Đây là một tệp được mã hóa được tạo bởi tiện 
ích [mysql_config_editor](https://dev.mysql.com/doc/refman/5.7/en/mysql-config-editor.html).
Xem [Section 4.6.6, “mysql_config_editor — MySQL Configuration Utility”](https://dev.mysql.com/doc/refman/5.7/en/mysql-config-editor.html).
Một "đường dẫn đăng nhập" là nhóm tùy chọn chỉ cho phép một số tùy chọn nhất định:
 host, user, password, port and socket. Chương trình client chỉ định đường dẫn đăng nhập nào cần đọc từ `.mylogin.cnf` bằng cách sử dụng tùy chọn `--login-path`.

Để chỉ định tên tệp đường dẫn đăng nhập thay thế, hãy đặt biến môi trường `MYSQL_TEST_LOGIN_FILE`.
Biến này được sử dụng bởi tiện ích kiểm tra `mysql-test-run.pl`, nhưng cũng được nhận biết bởi `mysql_config_editor `
và bởi các MySQL Client như mysql, mysqladmin và vv.

MySQL tìm kiếm các tệp tùy chọn theo thứ tự được mô tả trong phần thảo luận sau và đọc bất kỳ tệp nào tồn tại. Nếu một tệp tùy chọn bạn muốn sử dụng không tồn tại,
hãy tạo nó bằng cách sử dụng phương thức thích hợp, như mới được thảo luận.

> Chú ý: Các tệp tùy chọn được sử dụng với các chương trình NDB Cluster được trình
bày trong [Section 21.3, “Configuration of NDB Cluster”](https://dev.mysql.com/doc/refman/5.7/en/mysql-cluster-configuration.html).

Trong Windows, chương trình MySQL đọc lựa chọn khởi động từ những tệp hiển thị
trong bảng dưới đây, theo thứ tự được chỉ định (các tệp được liệt kê đầu tiên được đọc trước tiên, rồi dần tiếp đến các file đằng sau)

**Bảng 4.1 Các file tùy chọn đọc trên các hệ thống Window**

| Tên file                                                                                  | Mục đích                                                       |  
| ------------------------------------------------------------------------------------------ | ------------------------------------------------------------- |  
| `%PROGRAMDATA%\MySQL\MySQL Server 5.7\my.ini`, `%PROGRAMDATA%\MySQL\MySQL Server 5.7\my.cnf` | Các tùy chọn chung                                                |  
| `%WINDIR%\my.ini`, `%WINDIR%\my.cnf`                                                     | Các tùy chọn chung                                               |  
| `C:\my.ini`, `C:\my.cnf`                                                                     | Các tùy chọn chung                                                |  
| `BASEDIR\my.ini`, `BASEDIR\my.cnf`                                                   | Các tùy chọn chung                                               |  
| `defaults-extra-file`                                                                      | Bất cứ file nào được xác định với [`\--defaults-extra-file`][8] |  
| `%APPDATA%\MySQL\.mylogin.cnf`                                                             | Các tùy chọn phần đăng nhập (chỉ cho các máy khách)  

Trong bảng trước,`% PROGRAMDATA%` thể hiện thư mục hệ thống tệp chứa dữ liệu ứng dụng cho tất cả người dùng trên máy chủ. Đường dẫn này mặc định là `C: \ ProgramData` 
trên Microsoft Windows Vista trở lên và `C: \ Documents and Settings \ All Users \ Application Data` trên các phiên bản cũ hơn của Microsoft Windows.

`% WINDIR%` đại diện cho vị trí của thư mục Windows của bạn. Điều này thường là `C: \ WINDOWS`. Sử dụng lệnh sau đây để xác định vị trí chính xác của nó từ giá 
trị của biến môi trường WINDIR:

```php
C:\> echo %WINDIR%
```
`% APPDATA%` đại diện cho giá trị của thư mục dữ liệu ứng dụng Windows. Sử dụng lệnh sau đây để xác định vị trí chính xác của nó từ giá trị của biến môi trường APPDATA:

```php
C:\> echo %APPDATA%
```

`BASEDIR` đại diện cho thư mục cài đặt cơ sở MySQL. Khi MySQL 5.7 đã được cài đặt bằng cách sử dụng MySQL Installer, đây thường là `C: \ PROGRAMDIR \ MySQL \ MySQL 5.7` 
Server nơi PROGRAMDIR đại diện cho thư mục chương trình (thường là Program Files trên các phiên bản tiếng Anh của Windows), Xem [Section 2.3.3, “MySQL Installer for Windows”](https://dev.mysql.com/doc/refman/5.7/en/mysql-installer.html).

Trên các hệ thống giống Unix và Unix, các chương trình MySQL đọc các tùy chọn khởi động từ các tệp được hiển thị trong bảng sau, theo thứ tự được chỉ định 
(các tệp được liệt kê đầu tiên được đọc trước tiên, rồi dần tiếp đến các file đằng sau).

> Chú ý: Trên các nền tảng Unix, MySQL bỏ qua các tệp cấu hình có thể ghi bởi bất kỳ ai. Đây là ý định như một biện pháp an ninh.

**Bảng 4.2 Các file tùy chọn đọc trên các hệ điều hành Unix và giống Unix**

| Tên file               | Mục đích                                                       |  
| ----------------------- | ------------------------------------------------------------- |  
| `/etc/my.cnf`           | Các tùy chọn chung                                                |  
| `/etc/mysql/my.cnf`     | Các tùy chọn chung                                                |  
| `_`SYSCONFDIR`_/my.cnf` | Các tùy chọn chung                                               |  
| `$MYSQL_HOME/my.cnf`    | Các tùy chọn riêng cho máy chủ (chỉ áp dụng trên máy chủ)                         |  
| `defaults-extra-file`   | Bầy kỳ file nào được đặc tả với [`\--defaults-extra-file`][8] |  
| `~/.my.cnf`             | Các tùy chọn riêng cho người dùng                                        |  
| `~/.mylogin.cnf`        | Các tùy chọn đường dẫn đăng nhập riêng cho người dùng (chỉ cho các máy khách)               | 

Trong bảng trước, `~` đại diện cho thư mục home của người dùng hiện tại (giá trị của $ HOME).

`SYSCONFDIR` đại diện cho thư mục được chỉ định với tùy chọn [SYSCONFDIR](https://dev.mysql.com/doc/refman/5.7/en/source-configuration-options.html#option_cmake_sysconfdir)
 cho **CMake** khi MySQL được xây dựng. Theo mặc định, đây là thư mục etc nằm trong thư mục cài đặt đã biên dịch.
 
 `MYSQL_HOME` là một biến môi trường chứa đường dẫn đến thư mục chứa tệp `my.cnf` cụ thể của máy chủ. Nếu MYSQL_HOME không được thiết lập và bạn khởi động máy chủ bằng chương 
 trình [mysqld_safe](https://dev.mysql.com/doc/refman/5.7/en/mysqld-safe.html), [mysqld_safe](https://dev.mysql.com/doc/refman/5.7/en/mysqld-safe.html) đặt nó thành **BASEDIR**, thư mục cài đặt cơ sở MySQL.
 
 `DATADIR` thường là / usr / local / mysql / data, mặc dù điều này có thể thay đổi theo từng nền tảng hoặc phương pháp cài đặt. Giá trị là vị trí thư mục dữ liệu được xây dựng trong khi MySQL được biên soạn, 
 không phải là vị trí được chỉ định với tùy chọn `--datadir` khi mysqld bắt đầu. Sử dụng `--datadir` khi chạy không ảnh hưởng đến nơi máy chủ tìm kiếm các tệp tùy chọn mà nó đọc trước khi xử lý bất kỳ tùy chọn nào.
 
 Nếu tìm thấy nhiều phiên bản của một tùy chọn đã cho, phiên bản cuối cùng được ưu tiên, với một ngoại lệ: Đối với [mysqld](https://dev.mysql.com/doc/refman/5.7/en/mysqld.html), phiên bản đầu tiên của tùy chọn `--user` 
 được sử dụng như một biện pháp phòng ngừa an ninh, để ngăn người dùng đặc tả trong một tệp tùy chọn ghi đè trên dòng lệnh.
 
 Mô tả sau đây về cú pháp tệp tùy chọn áp dụng cho các tệp bạn chỉnh sửa theo cách thủ công. Điều này loại trừ `.mylogin.cnf`, được tạo bằng [mysql_config_editor](https://dev.mysql.com/doc/refman/5.7/en/mysql-config-editor.html) và được mã hóa.
 
 Bất cứ tùy chọn nào có thể được đưa ra trong dòng lệnh khi chạy 1 chương trình MySQL đều có thể được đưa ra trong các file tùy chọn. Để lấy danh sách các tùy chọn khả dụng cho chương trình, chạy nó với tùy chọn `--help`. (Với **mysqld**, sử dụng `\--verbose` và `--help`.)
 
 Cú pháo để đặt tả các tùy chọn trong file tùy chọn tương tự như trong cú pháp dòng lệnh ( xem  [Phần 4.2.4, "Sử dụng các tùy chọn trong dòng lệnh"][14]).
 Tuy nhiên, trong 1 file tùy chọn, bạn sẽ bỏ 2 dấu gạch ngang đầu trong tên tùy chọn và bạn đặc tả chỉ 1 tùy chọn cho mỗi dòng mà thôi. Ví dụ`\--quick` và `\--host=localhost` trong dòng lệnh nên được đặc tả thành `quick` và 
 `host=localhost` trên các dòng riêng biệt trong file tùy chọn. Để đặc tả 1 tùy chọn cho định dạng `\--loose-_`tên_opt`_` trong file tùy chọn, hãy viết nó thế này `loose-_`opt_name`_`.
 
Các dòng trống trong các tệp tùy chọn bị bỏ qua. Các dòng không trống có thể lấy bất kỳ dạng nào sau đây:

- **#comment, ; commnent**

Dòng chú thích bắt đầu bằng # hoặc; Một bình luận # cũng có thể bắt đầu ở giữa một dòng.

- **[group]**

`group` là tên của chương trình hoặc nhóm mà bạn muốn đặt tùy chọn. Sau một dòng nhóm, mọi dòng thiết lập tùy chọn áp dụng cho nhóm được đặt tên cho đến khi kết thúc tệp tùy chọn hoặc một nhóm nhóm khác được cung cấp. Tên nhóm tùy chọn không phân biệt chữ hoa chữ thường.

- **opt_name**

Điều này tương đương với `--opt_name` trên dòng lệnh.

- **opt_name=value**

Điều này tương đương với `--opt_name = value` trên dòng lệnh. Trong một tệp tùy chọn, bạn có thể có khoảng trống xung quanh ký tự =, cái gì đó không đúng trên dòng lệnh. Giá trị tùy chọn có thể được đặt trong dấu nháy đơn hoặc dấu ngoặc kép, điều này hữu ích nếu giá trị chứa ký tự comment #.

Các khoảng trống phía trước và sau sẽ tự động được xóa khỏi tên và các giá trị tùy chọn.

Bạn có thể sử dụng các  escape sequences \ b, \ t, \ n, \ r, \\ và \ s trong các giá trị tùy chọn để biểu thị backspace, tab, dòng mới, dấu xuống dòng, dấu gạch chéo ngược và ký tự khoảng trắng. Trong các tệp tùy chọn, các quy tắc escaping này sẽ áp dụng. 

- 1 kí tự backslash theo sau bởi 1 kí tự escape sequence hợp lệ sẽ được chuyển thành kí tự đại diện bởi sequence. Ví dụ, `s` sẽ được chuyển thành khoảng trắng.
- 1 kí tự backflash không theo sau bởi 1 kí tự escape sequence hợp lệ  sẽ không vì thay đổi. Ví dụ `S` vẫn giữ nguyên là nó.

Các luật trên có nghĩa là 1 kí tự backslash đơn thuần có thể được đưa ra bởi `\`, hoặc như là `` nếu nó không được theo sau bởi kí tự escape sequence hợp lệ.

Các luật cho các escape sequence trong các file tùy chọn sẽ khác 1 chút so với các luật cho escape sequence trong chuỗi thông thường trong câu lệnh SQL. Trong hoàn cảnh này, nếu  "_`x`_" không phải là 1 kí tự escape sequence hợp lệ, `_`x`_` trở thành "_`x`_" thay vì `_`x`_`. 
Xem [Section 9.1.1, “String Literals”][https://dev.mysql.com/doc/refman/5.7/en/string-literals.html]. 

Các luật escaping cho các giá trị file tùy chọn đặc biệt thích hợp cho các tên đường dẫn Windows, nơi sử dụng `` để phân chia tên đường dẫn. 1 thành phần phân chia trong tên đường dẫn Windows phải được viết là `\` enếu nó được theo sau bởi kí tự escape sequence. Nó có thể được viết là `\` hoặc nếu không thì là ``.
Ngoài ra, `\` có thể được sử dụng trong các tên đường dẫn Windows và có thể được coi như là ``. Giả sử rằng bạn muốn chỉ định thư mục gốc của `C:Program FilesMySQLMySQL Server 5.7` trong 1 file tùy chọn. Điều này có thể được thực hiện bằng 1 vài cách. 1 vài ví dụ sau đây:

```php
basedir="C:\Program Files\MySQL\MySQL Server 5.7"
basedir="C:\\Program Files\\MySQL\\MySQL Server 5.7"
basedir="C:/Program Files/MySQL/MySQL Server 5.7"
basedir=C:\\Program\sFiles\\MySQL\\MySQL\sServer\s5.7
```

Nếu tên nhóm tùy chọn giống với tên chương trình, các tùy chọn trong nhóm sẽ áp dụng riêng cho chương trình đó. Ví dụ, các nhóm [mysqld](https://dev.mysql.com/doc/refman/5.7/en/mysqld.html) và [mysql](https://dev.mysql.com/doc/refman/5.7/en/mysqld.html) áp dụng cho máy chủ mysqld và chương trình máy khách mysql, tương ứng.

Nhóm tùy chọn `[client]` sẽ được đọc bởi tất cả các chương trình máy khách cung cấp trong các phân phối của MySQL.(nhưng _không phải_ bởi [mysqld](https://dev.mysql.com/doc/refman/5.7/en/mysqld.html). Để hiểu cách mà các chương trình khách bên-thứ-3 sử dụng C API có thể sử dụng các file tùy chọn, xem tài liệu C API tại 
[Section 27.8.7.50, “mysql_options()”][https://dev.mysql.com/doc/refman/5.7/en/mysql-options.html]. 

Nhóm  `[client]` cho phép bạn chỉ định các tùy chọn áp dụng cho tất cả các máy khách. Ví dụ,  là 1 nhóm thích hợp để đặc tả mật khẩu để kết nối tới máy chủ. (Nhưng đảm bảo rằng file tùy chọn chỉ có thể truy cập bởi mình bạn, để những người khác không thể tìm được mật khẩu của bạn.)
Chắc chắn rằng không đặt 1 tùy chọn nào trong nhóm `[client]`  trừ khi nó được nhận ra bởi _tất cả_ chương trình khách bạn sử dụng. Các chương trình không hiểu các tùy chọn sẽ thoát sau khỉ hiển thị thông báo lỗi nếu bạn cố gắng chạy chúng.


Liệt kê thêm những nhóm lựa chọn chung  và sau đó là các nhóm đặc thù. Ví dụ, 1 nhóm `[client]` sẽ phổ biến hơn vì nó được đọc bởi tất cả các chương trình khách, trong khi 1 nhóm  `[mysqldump]` chỉ được đọc bởi [mysqldump](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html).
Các tùy chọn đặc thù sau sẽ ghi đè các tùy chọn đặc thù trước đó, vì vậy hay đặt các nhóm tùy chọn theo thứ tự  `[client]`, `[mysqldump]` cho phép [mysqldump](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html)-tùy chọn đặc thù để ghi đè các tùy chọn `[client]`.

Đây là một tệp tùy chọn người dùng thông thường

```php
[client]
# The following password will be sent to all standard MySQL clients
password="my password"

[mysql]
no-auto-rehash
connect_timeout=2
```

Để tạo các nhóm tùy chọn chỉ đọc bởi các máy chủ mysqld từ loạt phát hành MySQL cụ thể, hãy sử dụng các nhóm có tên [mysqld-5.6], [mysqld-5.7], v.v. Nhóm sau chỉ ra rằng cài đặt sql_mode chỉ nên được sử dụng bởi các máy chủ MySQL có số phiên bản 5.7.x:

```php
[client]
[mysqld-5.7]
sql_mode=TRADITIONAL
```

Có thể sử dụng trực tiếp `!include`  trong các file tùy chọn để thêm các file tùy chọn khác và  `!includedir`  để tìm kiểm các thư mục chỉ định cho các file tùy chọn. Ví dụ, để thêm file  
`/home/mydir/myopt.cnf` , sử dụng chỉ thị sau:

```php
!include /home/mydir/myopt.cnf
```

Để tìm kiếm thư mục `/home/mydir` và đọc các tệp tùy chọn được tìm thấy ở đó, hãy sử dụng chỉ thị này:

```php
!includedir /home/mydir
```

MySQL không đảm bảo về thứ tự các tệp tùy chọn trong thư mục sẽ được đọc

>Bất cứ file nào được tìm thấy và thêm vào bằng cách sửử dụng chỉ thị `!includedir` trong các hệ điều hành  `phải` có tên file kết thục bằng  `.cnf`. Trên Windows, chỉ thị này sẽ kiểm tra các file với phần mở rộng là `.ini` hoặc `.cnf` .

Viết nội dung của các file tùy chọn được thêm như các file tùy chọn khác. Có nghĩa là, nó nên bao gồm các nhóm tùy chọn, mỗi nhóm được đứng trước bởi 1 dòng `[_`group`_]`  chỉ định chương trình mà tùy chọn áp dụng.

Trong khi 1 file được thêm đang được xử lý, chỉ những tùy chọn trong các nhóm mà chương trình hiện tại tìm kiếm được sử dụng. Các nhóm khác sẽ bị loại bỏ. Giả sử rằng file  `my.cnf` chứa dòng sau: 

```php
!include /home/mydir/myopt.cnf
```

Và giả sử rằng /home/mydir/myopt.cnf trông như thế này:

```php
[mysqladmin]
force

[mysqld]
key_buffer_size=16M
```

Nếu `my.cnf` được xử lý bởi [mysqld](https://dev.mysql.com/doc/refman/5.7/en/mysqld.html), chỉ có nhóm `[mysqld]` trong `/home/mydir/myopt.cnf` được sử dụng.  Nếu file được xử lý bởi [mysqladmin]([mysqld](https://dev.mysql.com/doc/refman/5.7/en/mysqld.html)), chỉ nhóm `[mysqladmin]` được sử dụng. Nếu file được xử lý bởi các chương trình khác, không có tùy chọn nào trong `/home/mydir/myopt.cnf` được sử dụng

Chỉ thị `!includedir`được xử lý tương tự trừ khi tất cả các file tùy chọn trong thư mục được gọi được đọc

Nếu 1 file tùy chọn bao gồm các chỉ thị`!include` hoặc `!includedir`các file được gọi bởi các chỉ thị đó được xử lý mỗi khi file tùy chọn được xử lý, không quan tâm đến việc chúng xuất hiện ở đâu trong file

