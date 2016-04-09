MySQL User-defined function (UDF) for HTTP GET/POST

Website: http://code.google.com/p/mysql-udf-http

> change log:
> 
> 1. bugfix: crash when curl_easy_perform returns error. fix: delete the strcpy
> 2. error msg output when sth error occured in curl_easy_perform
> 3. set timeout when the destination url does not work properly, e.g. no response in time, make sure the sql will not be blocked.



====1. Install:====

ulimit -SHn 65535

wget http://mysql-udf-http.googlecode.com/files/mysql-udf-http-1.0.tar.gz
tar zxvf mysql-udf-http-1.0.tar.gz
cd mysql-udf-http-1.0/
./configure --prefix=/usr/local/webserver/mysql --with-mysql=/usr/local/webserver/mysql/bin/mysql_config
make && make install

====2. Enter to the MySQL console:====

/usr/local/webserver/mysql/bin/mysql -S /tmp/mysql.sock

====3. Create the UDF function in the MySQL console:====

mysql>

create function http_get returns string soname 'mysql-udf-http.so';
create function http_post returns string soname 'mysql-udf-http.so';
create function http_put returns string soname 'mysql-udf-http.so';
create function http_delete returns string soname 'mysql-udf-http.so';

====4. Usage:====

(1). Description:

mysql>

SELECT http_get('<url>');
SELECT http_post('<url>', '<data>');
SELECT http_put('<url>', '<data>');
SELECT http_delete('<url>');

(2). Examples:

mysql>

/* Sina Weibo Open Platform */
SELECT http_get('http://api.t.sina.com.cn/statuses/user_timeline/103500.json?count=1&source=1561596835') AS data;
SELECT http_post('http://your_sina_uid:your_password@api.t.sina.com.cn/statuses/update.xml?source=1561596835', 'status=Thins is sina weibo test information');

/* Tokyo Tyrant */
SELECT http_put('http://192.168.8.34:1978/key', 'This is value');
SELECT http_get('http://192.168.8.34:1978/key');
SELECT http_delete('http://192.168.8.34:1978/key');

====5. How to drop the UDF function:====

mysql>

drop function http_get;
drop function http_post;
drop function http_put;
drop function http_delete;

