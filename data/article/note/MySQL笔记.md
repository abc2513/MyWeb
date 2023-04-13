```mysql
MySQL
一、安装------------------------------------------------------------------------------------------
    注释行--
二、选择语句---------------------------------------------------------------------------------------
    --USE 数据库;在需要的数据库未圈出
    SELECT 列(,列2,列3……) FROM 数据表 WHERE 限制 ORDER BY (DESC) 排序依据列(,……)
    eg、SELECT l_name,ID FROM table_1 WHERE 
    说明：FROM、WHERE、ORDER BY 均为可选子句，但是要注意顺序
2、SELECT --子句
    SELECT * --选择全部列
    SELECT 列1,列2,列3 --选择多列
    SELECT 数学表达式
    SELECT 选中内容 AS 别名
    SELECT DISTINCT 内容 --删去重复项
3、WHERE --子句
    WHERE 限制条件 
    --     > >= < <= = != <> 比较符.注意MySQL不区分大小写，包括查询字符串
    eg、WHERE datetime1 > '2020-01-01'
4、AND OR NOT --运算符
    WHERE 条件 AND 条件
5、IN --运算符
    WHERE c1 IN (数据,数据,数据)
    WHERE c1 NOT IN (数据,数据,数据)
6、BETWEEN --运算符
    WHERE c1 BETWEEN 下限 AND 上限
7、LIKE --运算符(较老)
    WHERE c1 LIKE '%_ab_c%' 
    --%匹配任意个包括0个的字符
    --_匹配1个字符
8、REGEXP --运算符（正则表达式）
    WHERE c1 REGEXP 正则表达式
    eg、 REGEXP '[a-z]e' --匹配ae~ze
9、IS NULL --运算符
    WHERE c1 IS NULL
    WHERE c1 IS NOT NULL
10、ORDER BY --子句
    --没说明默认按列1~n的顺序
    ORDER BY c1 --升序
    ORDER BY c1 DESC --降序
    -- mysql可以用没选中的列排序（其他数据库不行），可以用别名。
    ORDER BY 1 --按第一列升序排序（加列后会导致混乱）
11、LIMIT --子句(常用于分页)
    LIMIT 数量 --返回数量限制
    LIMIT 偏移量,数量
三、-----------------------------------------------------------------------------------------------------------
1、内连接 在多张表中检索数据--inner join
    SELECT * FROM 表1 INNER JOIN 表2 ON 表1.列名=表2.列名 --INNER可以省略不写
    --在列名不冲突的情况下，列名前的表名可以不写
    FROM  表1 别名1 JOIN 表2 别名2 ON ……
    --用了别名之后就只能用别名
2、跨数据库连接--joining across database
    SELECT * FROM 表名1 别名1 JOIN 数据库2.表名2 别名2 ON ……
3、自连接-- Self join
    --用在要链接的数据在同一张表（不同行）
    SELECT * FROM 表名 别名1 JOIN 表名 别名2 ON ……
    --用不同别名区分开就行
4、多表连接--Joining multiple tables
    SELECT * FROM 表1 JOIN 表2 ON ……  JOIN 表3 ON …… ……
5、复合连接条件
    --复合主键：一数据表多个主键列
    JOIN …… ON …… AND ……
6、隐式连接语法
    SELECT * FROM 表1 JOIN 表2 ON 连接条件
    SELECT * FROM 表1,表2 WHERE 连接条件
    -- 忘记打WHERE子句会导致交叉连接
7、外连接--outer joins
    --内连接只会返回满足条件的行
    --外连接还会返回剩下的行（另一表的列为空）
    LEFT JOIN--返回左边所有的记录
    RIGHT JOIN--返回右边所有的记录
8、多表外连接
    --右连接应该避免使用
9、自外连接
10、USING --子句
    JOIN 表名1 ON 表名1.列名=表名2.列名 
    JOIN 表名 USING(列名)--两个列名完全相同
    复合条件
    JOIN 表名1 ON 表名1.列名1=表名2.列名1 AND 表名1.列名2=表名2.列名2
    JOIN 表名 USING(列名1,列名2)--两个列名完全相同
11、自然连接（不建议用）
    NATURAL JOIN …… --不用条件，数据库自己看着办
12、交叉连接--cross joins

13、联合--unions

第四章-------------------------------------------------------------------------------------------------------
1、列属性
varchar不占多余空间,char会
2、插入单行
INSERT INTO 表名 VALUES (100,DEFAULT,10,NULL,0)
INSERT INTO 表名(列名1,列名2,列名3) VALUES(1,2,3)
3、插入多行
INSERT INTO 表名(列名1,列名2,列名3) VALUES(1,2,3),(1,2,3),(……) ……
4、插入分层行
INSERT INTO ……；
INSERT INTO …… VALUES (LAST_INSERT_ID(),...)
5、创建表复制
CREATE TABLE 表名 AS SELECT * FROM 表名 --会忽略主键、自增的表属性 
CREATE TABLE 表名 AS SELECT * FROM 表名 WHERE ...
CREATE TABLE 表名 AS 选择语句
CREATE TABLE 表名 AS 选择语句 JOIN ...
6、更新单行
UPDATE 表名 SET 列名=... WHERE ...
UPDATE 表名 SET 列名=函数(列名) WHERE ...
7、更新单行
MySQL工作台 需要关闭安全更新--safe update
8、UPDATE中使用子查询
UPDATE ... WHERE abc=(SELECT abc FROM ...)
9、删除行
DELETE FROM 表名 --删表
DELETE FROM 表名 WHERE ...
10、恢复数据库

第五章--------------------------------------------------------------------------------------------------------
1、聚合函数
MAX()
MIN()
AVG()
SUM()
COUNT()
eg\ SELECT MAX(列a) as higest,MIN(列a) as lowest,AVG(列a) as average,COUNT(*) total_num FROM 表名

2、 GROUP BY --子句
SELECT SUM(abc) as c1,c2 FROM ... GROUP BY c3 --每个c3一个c1数值
SELECT ... FROM ... WHERE ... GROUP BY ... ORDER BY ... DESC --GROUND BY放在WHERE和ORDER BY中间
3、 HAVING --子句
--由于聚合前没有聚合数据，限制条件需要用having
HAVING 条件 --放在GROUP BY后
SELECT ... FROM ... WHERE ... GROUP BY ... HAVING ... ORDER BY ... DESC
4、ROLLUP --运算符
GROUP BY ... WITH ROLLUP --得到额外一行汇总
--只在MySQL有，不是标准SQL语言
第六章、编写复杂查询
1、介绍
2、子查询 subqueries
SELECT ... WHERE abc=(SELECT abc FROM ...)
3、IN --运算符
SELECT * FROM t1 WHERE c1 NOT IN(SELECT DISTINCT c1 FROM t2)
4、子查询vs连接
SELECT * FROM t1 WHERE c1 NOT IN(SELECT DISTINCT c1 FROM t2)--这种情况下可读性较好
SELECT * FROM t1 LEFT JOIN t2 USING (c2) WHERE c1 IS NULL
5、ALL --关键字
SELECT * FROM t1 WHERE c1 > (SELECT MAX(...)...)
SELECT * FROM t1 WHERE c1 > MAX(SELECT...)
SELECT * FROM t1 WHERE c1 > ALL(SELECT...)
6、ANY --关键字
SELECT * FROM t1 WHERE c1 > ANY(SELECT...)
7、相关子查询
SELECT * FROM t1 e WHERE c1>(SELECT AVG(c1) FROM t1 WHERE c1=e.c1)
8、EXISTS --运算符
SELECT * FROM t1 WHERE c1 IN ()
SELECT * FROM t1 e WHERE EXISTS(SELECT c1 FROM t2 WHERE c1=e.c1)
--IN 会产生结果集，数据量大时效率低
9、SELECT子句中的子查询
SELECT (SELECT AVG(...)) AS ... FROM ...
10、FROM子句中的子查询
SELECT * FROM(SELECT...)--从SELECT到的虚拟表中SELECT，但是虚拟表的选择列需要别名
SELECT ... FROM(SELECT...) --筛选
--后面可以用视图简化
第七章 -------------------------------------------------------------------------------------------------
1、数值函数
ROUND(1.23,保留小数位数) --四舍五入函数
TRUNCATE(1.23,保留小数位数) --截断函数
CEILING(1.23) --上限函数,返回大于等于这个数字的最小整数
FLOOR(1.23) --地板函数
ABS(-0.1) --绝对值函数
RAND() --0-1之间的随机浮点数
--其他
2、字符串函数
LENGTH('abc') --返回字符串长度
UPPER('Abc')--变大写
LOWER('Abc')--变小写
LTRIM('   abc')--剪除字符串左侧空格
RTRIM('abc   ')
TRIM('  abc  ')--删除所有前导和尾随空格
LEFT('abc',2)--返回左侧n个字符
RIGHT('abc',2)
SUBSTRING('abc',1,2)--返回n-m个字符，注意从1开始计数而不是0。第二个数字省略代表到字符串结尾
LOCATE('ab','aaaabc')--返回第一次出现字符串时第一个字符的位置，找不到返回0
REPLACE('zzzzzzabczz','abc','abd')--替换字符串
CONCAT('abc','dfe')--连接字符串
--其他
3、日期函数
NOW()--日期+时间 yyyy-mm-dd HH:mm:dd
CURDATE()--日期
CURTIME()--时间
YEAR(NOW()),MONTH(t),DAY(t),HOUR(t),MINUTE(t),SECOND(t)--返回整数值
DAYNAME(t),MONTHNAME(t)--返回星期、月份单词字符串
EXTRACT(DAY FROM t)
4、格式化日期和时间'2020-03-01'
DATE_FORMAT(NOW(),'%y')
'%y'两位数年份
'%Y'四位数年份
'%m'两位数月份
'%M'月份名称
'%d'两位数日数
'%H'24小时制小时
'%h'12小时制小时
'%i'分钟
'%p'返回AM或PM
5、计算日期和时间
DATE_ADD(NOW(),INTERVAL 1 DAY)--加
DATE_SUB(NOW(),INTERVAL 1 DAY)--减
DATEDIFF('2022-01-05','2021-12-02')--返回天数间隔、不包含小时分钟，输入含时间也一样，前减后
TIME_TO_SEC('09:00','00:00')--返回间隔秒数
TIME_TO_SEC('09:00')
6、ISNULL 和 COALESCE 函数
IFNULL(c1,'数值为空时的替换消息') AS e
COALESCE(c1,c2,'数值均为空时的替换消息')AS e --返回第一个非空值
7、IF函数
IF(条件,'条件为真的返回值','条件为假的返回值') AS e
8、CASE运算符
CASE 
    WHEN 条件 THEN 返回值 
    WHEN 条件 THEN 返回值 
    WHEN 条件 THEN 返回值 
    ELSE 返回值
END
AS e
第八章 视图 ---------------------------------------------------------------------------------
第九章 存储过程------------------------------------------------------------------------------
1、什么是存储过程
便于维护、性能更好、数据安全
2、创建存储过程
--创建
DELIMITER $$ --修改结束符
CREATE PROCEDURE get_data()
BEGIN
    --body
    SELECT * FROM t1
END $$
DELIMITER ; --修改结束符回去
--调用
CALL get_data()
3、使用MySQL工作台创建创建存储过程
CREATE PROCEDURE `get_data` ()
BEGIN
    SELECT * FROM t1;
END
4、删除存储过程
DROP PROCEDURE get_data
DROP PROCEDURE IF EXISTS get_data
--创建前输入DROP PROCEDURE IF EXISTS get_data
5、参数
DROP PROCEDURE IF EXISTS get_data;
DELIMITER $$
CREATE PROCEDURE getdata(num CHAR(2),)
BEGIN
    SELECT * FROM t1 
    WHERE c1=num;
END $$
DELIMITER ;
6、带默认值的参数
DROP PROCEDURE IF EXISTS get_data;
DELIMITER $$
CREATE PROCEDURE getdata(num CHAR(2),)
BEGIN
    IF num IS NULL
        THEN SET num='ab';
    END IF

    SELECT * FROM t1 
    WHERE c1=num;
END $$
DELIMITER ;
--CALL get_data(NULL)

```

