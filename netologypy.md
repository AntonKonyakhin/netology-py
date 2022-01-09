### Задача 1 

```python
#!/usr/bin/env python3
a = 1
b = '2'
c = a + b
```
|Вопрос|Ответ|  
---|---  
|Какое значение будет присвоено переменной `c` ? | будет ошибка, т.к. переменные разных типов|
|Как получить для переменной `c` значение 12?| нужно переменную `a` сделать строкой `a=str(a)` или `a='1'`|
|Как получить для переменной `c` значение 3?| Нужно переменную `b` сделать типом int: 'b=2' или `b=int(b)`|


### Задача 2  
исходный код:  
```python
#!/usr/bin/env python3

import os

bash_command = ["cd ~/netology/sysadm-homeworks", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(prepare_result)
        break
```
убираем break из цикла и дописываем `pwd` для выяснения полного пути к файлам после выполнения `cd`

Ваш скрипт:  
```python
#!/usr/bin/env python3

import os

bash_command = ["cd ~/netology/sysadm-homeworks", "git status", "pwd"]
result_os = os.popen(' && '.join(bash_command)).read()
print('полный путь к файлам: ')
is_change = False
full_p=result_os.split('\n')
print(full_p[-2])
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(prepare_result)
```

Вывод скрипта при запуске при тестировании:  
```
anton@Yupiter:~/scripts$ ./z2.py
полный путь к файлам:
/home/anton/netology/sysadm-homeworks
1.txt
2.txt
```

### задача 3  
Ваш скрипт:  
```python
#!/usr/bin/env python3

import os
import sys

#bash_command = ["cd ~/netology/sysadm-homeworks", "git status"]
#print("введите путь к файлам: ")
files_path=sys.argv[1]
str_path = f"cd {files_path}"
bash_command = [f"cd {files_path}", "git status", "pwd"]
result_os = os.popen(' && '.join(bash_command)).read()
full_path = result_os.split('\n')
print('полный путь к файлам: ', full_path[-2])
is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(prepare_result)
```

Вывод скрипта при запуске при тестировании:  
```
anton@Yupiter:~/scripts$ ./z3.py ~/netology/sysadm-homeworks
полный путь к файлам:  /home/anton/netology/sysadm-homeworks
1.txt
2.txt
```

### Задача 4

скрипт:  
```python
import socket
import time
resources=("drive.google.com", "mail.google.com", "google.com")
dict={}
#заполняем эталон с чем будем сравнивать
ip_prev=list()
for res in resources:
    ip_prev.append(socket.gethostbyname(res))

#проверяем в бесконечном цикле
while (1==1):
    time.sleep(5)
    i=0
    for resource in resources:
        ip_resource=socket.gethostbyname(resource)
        dict[resource] = ip_resource
        print(resource, ' - ', ip_resource)
        if ip_prev[i] == ip_resource:
            print("ok")
        else:
            print("error", "URL service: ", resource, " IP mismatch: ", "old ip: <", ip_prev[i], "> new ip: <", ip_resource, ">")
            ip_prev[i] = ip_resource
        i+=1

```

Вывод скрипта при запуске при тестировании:

```
drive.google.com  -  64.233.164.194
ok
mail.google.com  -  74.125.131.19
ok
google.com  -  64.233.165.138
ok
drive.google.com  -  64.233.164.194
ok
mail.google.com  -  74.125.131.19
ok
google.com  -  142.251.1.100
error URL service:  google.com  IP mismatch:  old ip: < 64.233.165.138 > new ip: < 142.251.1.100 >
```