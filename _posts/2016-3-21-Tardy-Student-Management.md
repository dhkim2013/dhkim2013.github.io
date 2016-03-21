---
layout: post
title: 지각비 관리 프로그램
---

TardyStudentManagementProgram

반에서 지각비를 수거하게 되서 쉽게 관리하려고 만들어봤다.

```python

# -*- coding: utf-8 -*-
import sys

try:
    f = open('tardy.txt', 'r')
    f2 = open('pay.txt', 'r')
    f.close()
    f2.close()
except:
    f = open('tardy.txt', 'w')
    f2 = open('pay.txt', 'w')
    f.close()
    f2.close()

while True:
    user = input('''1. input
2. tardy list
3. count
4. count all
5. pay
6. pay list
7. don't pay list
8. total money
9. exit\n\n''')
    print ''

    if user == 1:
        f = open('tardy.txt', 'a')
        data = raw_input('input name\n')
        f.write(data + '\n')
        print ''
        f.close()

    if user == 2:
	f = open('tardy.txt', 'r')
        data = f.read()
        print data
        f.close()

    elif user == 3:
        f = open('tardy.txt', 'r')
        cnt = 0
        name = raw_input('input name\n')
        lines = f.readlines()
        for line in lines:
            if name in line:
                cnt += 1
        print '\n' + name + ' : ' + str(cnt) + '\n'
        f.close()

    elif user == 4:
        f = open('tardy.txt', 'r')
        tardy = f.read().split('\n')
        tardyList = list(set(tardy))
        for i in range(1, len(tardyList)):
            print tardyList[i] + ' : ' + str(tardy.count(tardyList[i]))
        print ''

    elif user == 5:
        f = open('pay.txt', 'a')
        data = raw_input('input name\n')
        f.write(data + '\n')
        print ''
        f.close()

    elif user == 6:
        f = open('pay.txt', 'r')
        lines = f.readlines()
        for line in lines:
            print line,
        print ''

    elif user == 7:
        f = open('tardy.txt', 'r')
        f2 = open('pay.txt', 'r')
        tardy = f.read().split('\n')
        tardyList = list(set(tardy))
        pay = f2.read().split('\n')

        for i in range(len(tardyList)):
            cnt = 0
            cnt = tardy.count(tardyList[i]) - pay.count(tardyList[i])
            if cnt != 0:
                print tardyList[i] + ' : ' + str(cnt * 1000) + '원'
        print ''
        f.close()
        f2.close()

    elif user == 8:
        cnt = 0
        f = open('pay.txt', 'r')
        lines = f.readlines()
        for i in lines:
            cnt += 1
        print str(cnt * 1000) + '원\n'
        f.close()

    elif user == 9:
        sys.exit()

    else:
        print 'wrong input\n'
```
