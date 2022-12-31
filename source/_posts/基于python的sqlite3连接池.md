---
title: 基于python的sqlite3连接池
date: 2022-12-26 22:19:42
tags: sqlite3
categories: python
---
__连接池应该用单例模式，可以实例化后通过import方式引入__
> 例如: test = Database(),import test,应该保证test实例全局唯一。
<!--more-->
```python
import sqlite3
class Database():
    def __init__(self) -> None:
        self.creat_pool()
        connect = self.list.pop()
        connect.cursor().execute('''CREATE TABLE IF NOT EXISTS student
       ( 
        id_number        CHAR(50)    NOT NULL ,
   
        user_name       CHAR(50)    NOT NULL,
        gender           bool,
        password        char(20)    NOT NULL,
        img_path        char(60),
        vector          blob        ,
           
        salt            char(10)  NOT NULL ,
        cout              INT,
        PRIMARY KEY (id_number )
                 )without rowid;''')


    def creat_pool(self):
        self.list = []
        for i in range(10):
            self.conn = sqlite3.connect('./resources/company.db')
            def dict_factory(cursor, row):#重定义row_factory函数查询返回数据类型是字典形式
                d = {}
                for idx, col in enumerate(cursor.description):
                    d[col[0]] = row[idx]
                return d
            self.conn.row_factory = dict_factory
            self.list.append(self.conn)
    def get_connection(self):
            if(len(self.list)>5 ):
                return self.list.pop()
            for i in range(5):
                self.conn = sqlite3.connect('./resources/company.db')
                def dict_factory(cursor, row):#重定义row_factory函数查询返回数据类型是字典形式
                    d = {}
                    for idx, col in enumerate(cursor.description):
                        d[col[0]] = row[idx]
                    return d
                self.conn.row_factory = dict_factory
                self.list.append(self.conn)
            return self.list.pop()
    class student():##内部类可以用于操作student表，增删改查等
        def p():#静态函数，访问方式test = Database()，test.student.p()
            connect = test.list.pop()
            for i in connect.cursor().execute("select  user_name   from student"):
                print("test",i["user_name"])
            connect.close()

test = Database()
test.student.p()#访问内部类静态函数

for i in range(21):
    connect = test.get_connection()
    for i in connect.cursor().execute("select  user_name   from student"):
        print(i["user_name"])
    connect.close()

print(len(test.list))
```