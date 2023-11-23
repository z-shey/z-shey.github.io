---
title: 数据库概论第三章SQL Server代码
date: '2023-11-13 23:35:05'
updated: '2023-11-22 13:21:56'
permalink: /post/shu-ju-ku-gai-lun-di-san-zhang-sql-serverdai-ma-ovrga.html
comments: true
toc: true
tags:
  - 数据库
  - SQL Server
categories:
  - 数据库概论
---

# 数据库概论第三章SQL Server代码

仅供参考

## 建表

```sql
CREATE DATABASE CourseSelection_assignment;
USE CourseSelection_assignment;

CREATE SCHEMA "S-T" AUTHORIZATION db_accessadmin;

CREATE TABLE [S-T].Student
(
    Sno       CHAR(8) PRIMARY KEY,
    Sname     VARCHAR(20) UNIQUE,
    Ssex      CHAR(6),
    Sbirthdate DATE,
    Smajor    VARCHAR(40)
)

CREATE TABLE [S-T].Course
(
    Cno     CHAR(5) PRIMARY KEY,
    Cname   VARCHAR(40) NOT NULL,
    Ccredit SMALLINT,
    Cpno    CHAR(5),
    FOREIGN KEY (Cpno) REFERENCES [S-T].Course (Cno)
)

CREATE TABLE [S-T].SC
(
    Sno           CHAR(8),
    Cno           CHAR(5),
    Grade         SMALLINT,
    Semester      CHAR(5),
    Teachingclass CHAR(8),
    PRIMARY KEY (Sno, Cno),
    FOREIGN KEY (Sno) REFERENCES [S-T].Student (Sno),
    FOREIGN KEY (Cno) REFERENCES [S-T].Course (Cno)
)

INSERT INTO [S-T].Student (Sno, Sname, Ssex, Sbirthdate, Smajor)
VALUES ('20180001', '李勇', '男', '2000-3-8', '信息安全'),
       ('20180002', '刘晨', '女', '1999-9-1', '计算机科学与技术'),
       ('20180003', '王敏', '女', '2001-8-1', '计算机科学与技术'),
       ('20180004', '张立', '男', '2000-1-8', '计算机科学与技术'),
       ('20180005', '陈新奇', '男', '2001-11-1', '信息管理与信息系统'),
       ('20180006', '赵明', '男', '2000-6-12', '数据科学与大数据技术'),
       ('20180007', '王佳佳', '女', '2001-12-7', '数据科学与大数据技术');

INSERT INTO [S-T].Course (Cno, Cname, Ccredit, Cpno)
VALUES ('81001', '程序设计基础和C语言', 4, NULL),
       ('81002', '数据结构', 4, '81001'),
       ('81003', '数据库理论', 4, '81002'),
       ('81004', '信息系统概论', 4, '81003'),
       ('81005', '操作系统', 4, '81001'),
       ('81006', 'Python语言', 3, '81002'),
       ('81007', '离散数学', 4, NULL),
       ('81008', '大数据技术概论', 4, '81003');

INSERT INTO [S-T].SC (Sno, Cno, Grade, Semester, Teachingclass)
VALUES ('20180001', '81001', 85, '20192', '81001-01'),
       ('20180001', '81002', 96, '20201', '81002-01'),
       ('20180001', '81003', 87, '20202', '81003-01'),
       ('20180002', '81001', 80, '20192', '81001-02'),
       ('20180002', '81002', 98, '20201', '81002-01'),
       ('20180002', '81003', 71, '20202', '81003-02'),
       ('20180003', '81001', 81, '20192', '81001-01'),
       ('20180003', '81002', 76, '20201', '81002-02'),
       ('20180004', '81001', 56, '20192', '81001-02'),
       ('20180004', '81002', 97, '20201', '81001-02'),
       ('20180005', '81003', 68, '20202', '81003-01');
```

## Example 3.01 为用户WANG定义一个学生选课模式"S-C-SC"

```sql
CREATE SCHEMA "S-C-SC" AUTHORIZATION 【用户】;
```

## Example 3.02 未指定模式名的模式定义示例

```sql
CREATE SCHEMA AUTHORIZATION 【用户】;
```

## Example 3.03 为用户ZHANG创建一个模式Test，并在其中定义一个表Tab1

```sql
CREATE SCHEMA Test AUTHORIZATION 【用户】;
```

```sql
CREATE TABLE Tab1
(
    Col1 SMALLINT,
    Col2 INT,
    Col3 CHAR(20),
    Col4 NUMERIC(10, 3),
    Col5 DECIMAL(5, 2)
);
```

## Example 3.04 删除3.03中建立的模式Test

```sql
DROP TABLE Tab1
DROP SCHEMA Test;
```

## Example 3.05 建立一个学生表Student

```sql
CREATE TABLE [S-T].Student
(
    Sno       CHAR(8) PRIMARY KEY,
    Sname     VARCHAR(20) UNIQUE,
    Ssex      CHAR(6),
    Sbirthdate DATE,
    Smajor    VARCHAR(40)
)
```

## Example 3.06 建立一个课程表Course

```sql
CREATE TABLE [S-T].Course
(
    Cno     CHAR(5) PRIMARY KEY,
    Cname   VARCHAR(40) NOT NULL,
    Ccredit SMALLINT,
    Cpno    CHAR(5),
    FOREIGN KEY (Cpno) REFERENCES [S-T].Course (Cno)
)
```

## Example 3.07 建立一个学生选课表SC

```sql
CREATE TABLE [S-T].SC
(
    Sno           CHAR(8),
    Cno           CHAR(5),
    Grade         SMALLINT,
    Semester      CHAR(5),
    Teachingclass CHAR(8),
    PRIMARY KEY (Sno, Cno),
    FOREIGN KEY (Sno) REFERENCES [S-T].Student (Sno),
    FOREIGN KEY (Cno) REFERENCES [S-T].Course (Cno)
)
```

## Example 3.08 向学生表Student中增加邮箱地址列Semail，数据类型是字符型

```sql
ALTER TABLE [S-T].Student
    ADD Semail VARCHAR(30);
```

## ​​Example 3.09 将学生表Student中出生日期Sbirthdate数据类型由日期类型DATE改为字符型

```sql
ALTER TABLE [S-T].Student
    ALTER COLUMN Sbirthdate VARCHAR(20);
```

## Example 3.10 增加课程名称必须取唯一值的约束条件

```sql
ALTER TABLE [S-T].Course
    ADD UNIQUE (Cname);
```

## Example 3.11 删除学生表

```sql
DROP TABLE [S-T].Student;
```

## Example 3.12 删除Student表，若有视图，则CASCADE、RESTRICT情况下，删除的内容是有不同的

```sql
CREATE VIEW CS_Student
AS
SELECT Sno, Sname, Ssex, Sbirthdate, Smajor
FROM [S-T].Student
WHERE Smajor = '计算机科学与技术';

DROP TABLE [S-T].Student;

SELECT *
FROM CS_Student;
```

## Example 3.13 为三个关系建立索引

```sql
CREATE UNIQUE INDEX Idx_StudentSname ON [S-T].Student (Sname);
CREATE UNIQUE INDEX Idx_CourseCname ON [S-T].Course (Cname);
CREATE UNIQUE INDEX Idx_SCCno ON [S-T].SC (Sno ASC, Cno DESC);
```

## Example 3.14 将SC关系的索引名改为Idx_SCSnoCno

```sql
EXEC sp_rename 'S-T.SC.Idx_SCCno', 'Idx_SCSnoCno', 'INDEX';
```

## Example 3.15 删除学生关系索引Idx_StudentSname

```sql
DROP INDEX [S-T].Idx_StudentSname;
```

## Example 3.16 查询全体学生的学号与姓名

```sql
SELECT Sno, Sname
FROM [S-T].Student;
```

## Example 3.17 查询全体学生的姓名、学号、专业

```sql
SELECT Sname, Sno, Smajor
FROM [S-T].Student;
```

## Example 3.18 查询全体学生详细记录

```sql
SELECT *
FROM [S-T].Student;
```

## Example 3.19 查询全体学生的姓名与年龄

```sql
SELECT Sname, DATEDIFF(YEAR, Sbirthdate, GETDATE()) AS Age
FROM [S-T].Student;
```

## Example 3.20 查询全体学生的姓名、生日、专业

```sql
SELECT Sname, 'DATA OF BIRTH: ', Sbirthdate, Smajor
FROM [S-T].Student;
```

## Example 3.21 查询选修了课程的学生学号

```sql
SELECT Sno
FROM [S-T].SC;
```

```sql
SELECT DISTINCT Sno
FROM [S-T].SC;
```

```sql
SELECT ALL Sno
FROM [S-T].SC;
```

## Example 3.22 查询主修计算机科学与技术专业全体学生的姓名

```sql
SELECT Sname
FROM [S-T].Student
WHERE Smajor = '计算机科学与技术'
```

## Example 3.23 查询2000年以及2000年后出生的所有学生的姓名以及性别

```sql
SELECT Sname, Ssex
FROM [S-T].Student
WHERE YEAR(Sbirthdate) >= 2000;
```

## Example 3.24 查询考试成绩不及格的学生的学号

```sql
SELECT DISTINCT Sno
FROM [S-T].SC
WHERE Grade < 60;
```

## Example 3.25 查询年龄在20-23之间的学生的姓名

```sql
SELECT Sname, Sbirthdate, Smajor
FROM [S-T].Student
WHERE DATEDIFF(YEAR, Sbirthdate, GETDATE()) BETWEEN 20 AND 23;
```

## Example 3.26 查询年龄不在20-23岁范围之间的学生的姓名、出生日期和主修专业

```sql
SELECT Sname, Sbirthdate, Smajor
FROM [S-T].Student
WHERE DATEDIFF(YEAR, Sbirthdate, GETDATE()) NOT BETWEEN 20 AND 23;
```

## Example 3.27 查询计算机科学与技术专业和信息安全专业的学生的姓名与性别

```sql
SELECT Sname, Ssex
FROM [S-T].Student
WHERE Smajor IN ('计算机科学与技术', '信息安全');
```

## Example 3.28 查询非计算机科学与技术专业和信息安全专业的学生的姓名和性别

```sql
SELECT Sname, Ssex
FROM [S-T].Student
WHERE Smajor NOT IN ('计算机科学与技术', '信息安全');
```

## Example 3.29 查询学号20180003的学生的详细情况

```sql
SELECT *
FROM [S-T].Student
WHERE Sno LIKE '20180003';
```

## Example 3.30 查询所有姓刘的学生的姓名、学号和性别

```sql
SELECT Sname, Sno, Ssex
FROM [S-T].Student
WHERE Sname LIKE '刘%';
```

## Example 3.31 查询2018级学生的学号和姓名

```sql
SELECT Sno, Sname
FROM [S-T].Student
WHERE Sno LIKE '2018%'
```

## Example 3.32 查询课程号为81开头，最后以一位是6的课程名称和课程号

```sql
SELECT Cname, Cno
FROM [S-T].Course
WHERE Cno LIKE '81__6';
```

## Example 3.33 查询所有不姓刘的学生的姓名、学号和性别

```sql
SELECT Sname, Sno, Ssex
FROM [S-T].Student
WHERE Sname NOT LIKE '刘%';
```

## Example 3.34 查询DB_Dsign课程的课程号和学分

```sql
SELECT Cno, Ccredit
FROM [S-T].Course
WHERE Cname LIKE 'DB\_Design' ESCAPE '\';
```

## Example 3.35 查询以DB_开头，且倒数第三的字符为i的课程的详细情况

```sql
SELECT *
FROM [S-T].Course
WHERE Cname LIKE 'DB\_%i__' ESCAPE '\';
```

## Example 3.36 查询缺少成绩的学生的学号和对应的课程号

```sql
SELECT Sno, Cno
FROM [S-T].SC
WHERE Grade IS NULL;
```

## Example 3.37 查询所有有成绩的学生的学号和选修的课程号

```sql
SELECT Sno, Cno
FROM [S-T].SC
WHERE Grade IS NOT NULL;
```

## Example 3.38 查询计算机技术与技术专业，在2000年包括2000年出生的学生的学号、姓名和性别

```sql
SELECT Sno, Sname, Ssex
FROM [S-T].Student
WHERE Smajor = '计算机科学与技术'
   AND YEAR(Sbirthdate) >= 2000;
```

## Example 3.39 查询选修了81003号课程的学生的学号及其成绩，查询结果按分数的降序排序

```sql
SELECT Sno, Grade
FROM [S-T].SC
WHERE Cno = '81003'
ORDER BY Grade DESC;
```

## Example 3.40 查询全体学生选修课程的情况，查询结果先按照课程号升序排序，统一课程按成绩降序排序

```sql
SELECT *
FROM [S-T].SC
ORDER BY Cno, Grade DESC;
```

## Example 3.41 查询学生总人数

```sql
SELECT COUNT(*)
FROM [S-T].Student;
```

## Example 3.42 查询选修了课程的学生认识

```sql
SELECT COUNT(DISTINCT Sno)
FROM [S-T].SC;
```

## Example 3.43 计算选修81001号课程的学生平均学生成绩

```sql
SELECT AVG(Grade)
FROM [S-T].SC
WHERE Cno = '81001';
```

## Example 3.44 查询选修81001号课程的学生的最高分数

```sql
SELECT MAX(Grade)
FROM [S-T].SC
WHERE Cno = '81001';
```

## Example 3.45 查询学号为20180003的学生的选修课的总分数

```sql
SELECT SUM(Grade)
FROM [S-T].SC,
     [S-T].Course
WHERE Sno = '20180003'
  AND SC.Cno = Course.Cno;
```

## Example 3.46 求各个课程号以及选修该课程的人数

```sql
SELECT Cno, COUNT(Sno)
FROM [S-T].SC
GROUP BY Cno;
```

## Example 3.47 查询2019年第二学期选修课程数超过10门的学生学号

```sql
SELECT Sno
FROM [S-T].SC
WHERE Semester = '20192'
GROUP BY Sno
HAVING COUNT(*) >= 1;
```

## Example 3.48 查询平均成绩大于或等于90分的学生学号和平均成绩

```sql
SELECT Sno, AVG(Grade)
FROM [S-T].SC
GROUP BY Sno
HAVING AVG(Grade) >= 80;
```

## Example 3.49 查询选修数据库系统概论课程，且成绩排名在前10名的学生的学号

```sql
SELECT TOP 10 Sno
FROM [S-T].SC, [S-T].Course
WHERE Course.Cname = '数据库理论' AND SC.Cno = Course.Cno
ORDER BY Grade DESC;
```

## Example 3.50 查询平均成绩排名在第3~7名的学生的学号和平均成绩

```sql
SELECT Sno, AVG(Grade)
FROM [S-T].SC
GROUP BY Sno
ORDER BY AVG(Grade) DESC
OFFSET 2 ROWS FETCH NEXT 5 ROWS ONLY;
```

## Example 3.51 查询每个学生及其选修课程的情况

```sql
SELECT Student.*, SC.*
FROM [S-T].Student,
     [S-T].SC
WHERE Student.Sno = SC.Sno;
```

## Example 3.52 查询每个学生的学号，姓名，性别，出生日期，主修专业及该学生选修课程的课程号与成绩

```sql
SELECT Student.Sno, Sname, Ssex, Sbirthdate, Smajor, Cno, Grade
FROM [S-T].Student,
     [S-T].SC
WHERE Student.Sno = SC.Sno;
```

## Example 3.53 查询选修81002号课程，且成绩在90分以上的所有学生的学号和姓名

```sql
SELECT Student.Sno, Sname
FROM [S-T].Student,
     [S-T].SC
WHERE Student.Sno = SC.Sno
  AND SC.Cno = '81002'
  AND SC.Grade > 90;
```

## Example 3.54 查询每一门课的间接先选课

```sql
SELECT FIRST.Cno, SECOND.Cpno
FROM [S-T].Course FIRST, [S-T].Course SECOND
WHERE FIRST.Cpno = SECOND.Cno AND SECOND.Cpno IS NOT NULL;
```

## Example 3.55 -

```sql
SELECT Student.Sno, Sname, Ssex, Sbirthdate, Cno, Grade
FROM [S-T].Student
         LEFT OUTER JOIN [S-T].SC ON Student.Sno = SC.Sno;
```

## Example 3.56 查询每个学生的学号，姓名，学校的课程名及成绩

```sql
SELECT Student.Sno, Sname, Cname, Grade
FROM [S-T].Student,
     [S-T].SC,
     [S-T].Course
WHERE Student.Sno = SC.Sno
  AND SC.Cno = Course.Cno;
```

## Example 3.57 查询与刘晨在同一个主修专业的学生学号姓名和主修专业

```sql
SELECT Sno, Sname, Smajor
FROM [S-T].Student
WHERE Smajor IN (SELECT Smajor
                 FROM [S-T].Student
                 WHERE Sname = '刘晨');
```

## Example 3.58 查询选修了课程名为信息系统概论的学生的学号和姓名

```sql
SELECT Sno, Sname
FROM [S-T].Student
WHERE Sno IN (SELECT Sno
              FROM [S-T].SC
              WHERE Cno IN (SELECT Cno
                            FROM [S-T].Course
                            WHERE Cname = '数据库理论'));
```

## Example 3.59 找出每个学生超过他自己选修课程平均成绩的课程号

```sql
SELECT Sno, Cno
FROM [S-T].SC x
WHERE Grade >= (SELECT AVG(Grade)
                FROM [S-T].SC y
                WHERE y.Sno = x.Sno);
```

## Example 3.60 查询非计算机科学与技术专业中比计算机科学与技术专业任意一个年龄小的学生的姓名，出生日期和主修专业

```sql
SELECT Sname, Sbirthdate, Smajor
FROM [S-T].Student
WHERE Sbirthdate > ANY (SELECT Sbirthdate
                        FROM [S-T].Student
                        WHERE Smajor = '计算机科学与技术')
  AND Smajor <> '计算机科学与技术';
```

## Example 3.61 查询非计算机科学与技术专研中比计算机科学技术专业所有学生年龄都小的学生的姓名及出生日期

```sql
SELECT Sname, Sbirthdate
FROM [S-T].Student
WHERE Sbirthdate > ALL (SELECT Sbirthdate
                        FROM [S-T].Student
                        WHERE Smajor = '计算机科学与技术')
  AND Smajor <> '计算机科学与技术';
```

## Example 3.62 查询所有选修了81001号课程的学生姓名

```sql
SELECT Sname
FROM [S-T].Student
WHERE EXISTS(SELECT *
             FROM [S-T].SC
             WHERE Sno = Student.Sno
               AND Cno = '81001');
```

## Example 3.63 查询没有选修81001号课程的学生姓名

```sql
SELECT Sname
FROM [S-T].Student
WHERE NOT EXISTS(SELECT *
                 FROM [S-T].SC
                 WHERE SC.Sno = Student.Sno
                   AND Cno = '81001');
```

## Example 3.64 查询选修了全部课程的学生姓名

```sql
SELECT Sname
FROM [S-T].Student
WHERE NOT EXISTS(SELECT *
                 FROM [S-T].Course
                 WHERE NOT EXISTS(SELECT *
                                  FROM [S-T].SC
                                  WHERE Sno = Student.Sno
                                    AND Cno = Course.Cno))
```

## Example 3.65 查询至少选修了学生20180002选修的全部课程的学生的学号

```sql
SELECT Sno
FROM [S-T].Student
WHERE NOT EXISTS(SELECT *
                 FROM [S-T].SC SCX
                 WHERE SCX.Sno = '20180002'
                   AND NOT EXISTS(SELECT *
                                  FROM [S-T].SC SCY
                                  WHERE SCY.Sno = Student.Sno
                                    AND SCY.Cno = SCX.Cno));
```

## Example 3.66 查询计算机科学与技术专业的学生及年龄不大于19岁的学生

```sql
SELECT *
FROM [S-T].Student
WHERE Smajor = '计算机科学与技术'
UNION
SELECT *
FROM [S-T].Student
WHERE DATEDIFF(YEAR, Sbirthdate, GETDATE()) <= 19;
```

## Example 3.67 查询2020年第二学期选修了课程，81001或81002的学生

```sql
SELECT Sno
FROM [S-T].SC
WHERE Semester = '20202'
  AND Cno = '81001'
UNION
SELECT Sno
FROM [S-T].SC
WHERE Semester = '20202'
  AND Cno = '81002';
```

## Example 3.68 查询计算机科学技术专业的学生，年龄不大于19岁的学生的交集

```sql
SELECT *
FROM [S-T].Student
WHERE Smajor = '计算机科学与技术'
INTERSECT
SELECT *
FROM [S-T].Student
WHERE DATEDIFF(YEAR, Sbirthdate, GETDATE()) <= 19;
```

## Example 3.69 查询即选修了课程81001又选修了课程，8102的学生

```sql
SELECT Sno
FROM [S-T].SC
WHERE Cno = '81001'
INTERSECT
SELECT Sno
FROM [S-T].SC
WHERE Cno = '81002';
```

## Example 3.70 查询计算机科学技术专业的学生与年龄不大于19岁的学生的差距

```sql
SELECT *
FROM [S-T].Student
WHERE Smajor = '计算机科学与技术'
EXCEPT
SELECT *
FROM [S-T].Student
WHERE DATEDIFF(YEAR, Sbirthdate, GETDATE()) <= 19;
```

## Example 3.71 将一个新学生元组('20180009', '陈冬', '男', '信息管理与信息系统', '2000-5-22')插入Student表

```sql
INSERT INTO [S-T].Student(Sno, Sname, Ssex, Smajor, Sbirthdate)
VALUES ('20180009', '陈冬', '男', '信息管理与信息系统', '2000-5-22');
```

## Example 3.72 将学生张成民的信息插入Student表

```sql
INSERT INTO [S-T].Student
VALUES ('20180008', '张成民', '男', '计算机科学与技术', '2000-4-15');
```

## Example 3.73 插入一条选课记录

```sql
INSERT INTO [S-T].SC (Sno, Cno, Semester, Teachingclass)
VALUES ('20180005', '81004', '20202', '81004-01');
```

## Example 3.74 对每一个专业求学生的平均年龄，并把结果存入数据库

```sql
CREATE TABLE [S-T].Smajor_age
(
    Smajor  VARCHAR(40),
    Avg_age SMALLINT
);

INSERT INTO [S-T].Smajor_age(Smajor, Avg_age)
SELECT Smajor, AVG(DATEDIFF(YEAR, Sbirthdate, GETDATE()))
FROM [S-T].Student
GROUP BY Smajor;
```

## Example 3.75 将学生20180001的出生日期改为2001年3月18日

```sql
UPDATE [S-T].Student
SET Sbirthdate = '2001-3-18'
WHERE Sno = '20180001'
```

## Example 3.76 将2020年第一学期选修了81002课程所有学生的成绩减5分

```sql
UPDATE [S-T].SC
SET Grade = Grade - 5
WHERE Semester = '20201'
  AND Cno = '81002';
```

## Example 3.77 将计算机科学技术专业学生的成绩置零

```sql
UPDATE [S-T].SC
SET Grade = 0
WHERE Sno IN (SELECT Sno
              FROM [S-T].Student
              WHERE Smajor = '计算机科学与技术');
```

## Example 3.78 删除学号为20180007的学生记录

```sql
DELETE
FROM [S-T].Student
WHERE Sno = '20180007'
```

## Example 3.79 删除所有学生的选课记录

```sql
DELETE
FROM [S-T].SC
WHERE 1 = 1;
```

## Example 3.80 删除计算机科学与技术专业所有学生的选课记录

```sql
DELETE
FROM [S-T].SC
WHERE Sno IN (SELECT Sno
              FROM [S-T].Student
              WHERE Smajor = '计算机科学与技术');
```

## Example 3.81 向SC表中插入一个元组，学号为20180006，课程号为81004，2021年的第一学期，选课班没有确定，没有课程成绩

```sql
INSERT INTO [S-T].SC (Sno, Cno, Grade, Semester, Teachingclass)
VALUES ('20180006', '81004', NULL, '20211', NULL);
```

## Example 3.82 将student表中学号为20180006的学生的主修专业改为空值

```sql
UPDATE [S-T].Student
SET Smajor = NULL
WHERE Sno = '20180006';
```

## Example 3.83 从student表中找出遗漏了数据的学生信息

```sql
SELECT *
FROM [S-T].Student
WHERE Sname IS NULL
   OR Ssex IS NULL
   OR Sbirthdate IS NULL
   OR Smajor IS NULL;
```

## Example 3.84 找出选修81001号课程且成绩不及格的学生

```sql
SELECT Sno
FROM [S-T].SC
WHERE Grade < 60
  AND Cno = '81001';
```

## Example 3.85 选出选修81001号课程且成绩不及格的学生以及缺考的学生

```sql
SELECT Sno
FROM [S-T].SC
WHERE Grade < 60
  AND Cno = '81001'
UNION
SELECT sno
FROM [S-T].SC
WHERE Grade IS NULL
  AND Cno = '81001';
```

## Example 3.86 建立信息管理与信息系统专业学生的视图

```sql
CREATE VIEW [S-T].IS_Student
AS
SELECT Sno, Sname, Ssex, Sbirthdate, Smajor
FROM [S-T].Student
WHERE Smajor = '信息管理与信息系统';
```

## Example 3.87 建立信息管理与信息系统专业学生的视图，并要求进行插入修改和删除操作时，仍需保证该视图只有信息管理与信息系统专业的学生

```sql
CREATE VIEW [S-T].IS_Student
AS
SELECT Sno, Sname, Ssex, Sbirthdate, Smajor
FROM [S-T].Student
WHERE Smajor = '信息管理与信息系统'
WITH CHECK OPTION;
```

## Example 3.88 建立信息管理与信息系统专业选修81001号课程的学生的视图

```sql
CREATE VIEW [S-T].IS_C1(Sno, Sname, Grade)
AS
SELECT Student.Sno, Sname, Grade
FROM [S-T].Student,
     [S-T].SC
WHERE Smajor = '信息管理与信息系统'
  AND Student.Sno = SC.Sno
  AND SC.Cno = '81001';
```

## Example 3.89 建立信息管理与信息系统，专业学修81001号课程，且成绩在90分以上的学生的视图

```sql
CREATE VIEW [S-T].IS_C2
AS
SELECT Sno, Sname, Grade
FROM [S-T].IS_C1
WHERE Grade >= 90;
```

## Example 3.90 将学生的学号，姓名，年龄定义为一个视图

```sql
CREATE VIEW [S-T].S_AGE(Sno, Sname, Sage)
AS
SELECT Sno, Sname, DATEDIFF(YEAR, Sbirthdate, GETDATE())
FROM [S-T].Student;
```

## Example 3.91 将学生的学号及平均成绩定义为一个视图

```sql
CREATE VIEW [S-T].S_GradeAVG(Son, Gavg)
AS
SELECT Sno, AVG(Grade)
FROM [S-T].SC
GROUP BY Sno;
```

## Example 3.92 将student表中所有的女生记录定义为一个视图

```sql
CREATE VIEW [S-T].F_Student(Fsno, Fname, Fsex, Fbirthdate, Fmajor)
AS
SELECT *
FROM [S-T].Student
WHERE Ssex = '女';
```

## Example 3.93 删除视图，S_AGE和视图IS_C1

```sql
DROP VIEW [S-T].S_AGE;
DROP VIEW [S-T].IS_C1;
```

## Example 3.94 在信息管理与信息系统专业的学生的视图中，找出年龄小于或等于20岁的学生

```sql
SELECT Sno, Sbirthdate
FROM [S-T].IS_Student
WHERE DATEDIFF(YEAR, Sbirthdate, GETDATE()) <= 20;
```

## Example 3.95 查询选修81001号课程的信息管理与信息系统专业学生

```sql
SELECT IS_Student.Sno, Sname
FROM [S-T].IS_Student,
     [S-T].SC
WHERE [S-T].IS_Student.Sno = SC.Sno
  AND SC.Cno = '81001';
```

## Example 3.96 在例3.91中定义的S_GradeAVG视图中，平均查询平均成绩在90分以上的学生学号和平均成绩

```sql
SELECT *
FROM [S-T].S_GradeAVG
WHERE Gavg >= 90;
```

## Example 3.97 将信息管理与信息系统专业学生视图IS_Student中20180005号学生姓名改为刘新奇

```sql
UPDATE [S-T].IS_Student
SET Sname = '刘新奇'
WHERE Sno = '20180005';
```

## Example 3.98 向信息管理信息系统专业学生视图IS_Student中插入一个新的学生记录('20180207', '赵新', '男', '2001-7-19', '信息管理与信息系统')

```sql
INSERT INTO [S-T].IS_Student(Sno, Sname, Ssex, Sbirthdate, Smajor)
VALUES ('20180207', '赵新', '男', '2001-7-19', '信息管理与信息系统');
```

## Example 3.99 删除信息管理与信息系统专业学生视图IS_Student中20180207号学生的记录

```sql
DELETE
FROM [S-T].IS_Student
WHERE Sno = '20180207';
```
