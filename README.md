

# devops-netology

1. Есть скрипт:

         #!/usr/bin/env python3
         a = 1
         b = '2'
         c = a + b

   Какое значение будет присвоено переменной c?
   
   Как получить для переменной c значение 12?

   Как получить для переменной c значение 3?

   1. Получим ошибку,т.к. пытаемся сложить целое число и строку
   2. c = str(a) + b т.е. выполним конкатенацию строк
   3. c = a + int(b)


2. Мы устроились на работу в компанию, где раньше уже был DevOps Engineer. 
   Он написал скрипт, позволяющий узнать, какие файлы модифицированы в репозитории, 
   относительно локальных изменений. Этим скриптом недовольно начальство, 
   потому что в его выводе есть не все изменённые файлы, а также непонятен полный путь к директории, 
   где они находятся. Как можно доработать скрипт ниже, чтобы он исполнял требования вашего руководителя?
   
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

   Исправленный скрипт

         #!/usr/bin/env python3
         
         import os
         
         git_dir_path = '~/netology/sysadm-homeworks'
   
         bash_command = ["cd " + git_dir_path, "git status"]
         result_os = os.popen(' && '.join(bash_command)).read()
         #is_change = False - вооще лишняя переменная
         for result in result_os.split('\n'):
             if result.find('изменено') != -1: 
                 prepare_result = result.replace('\tизменено:      ', '')
                 print('{}'.format(git_dir_path+prepare_result))
         #        break  - тормозит скрипт на первом найденом изменении.




3. Доработать скрипт выше так, чтобы он мог проверять не только локальный репозиторий в текущей 
   директории, а также умел воспринимать путь к репозиторию, который мы передаём как входной параметр. 
   Мы точно знаем, что начальство коварное и будет проверять работу этого скрипта в директориях, 
   которые не являются локальными репозиториями.
   
         #!/usr/bin/env python3
         
         import os
         import sys
         
         def git_command(dir_path):
             bash_command = ["cd " + dir_path, "git status 2>&1"]
             result = os.popen(' && '.join(bash_command)).read()
             return result
         
         
         if __name__ == "__main__":
             if len(sys.argv) > 1:
                 result = git_command(sys.argv[1])
                 for item in result.split('\n'):
                     if item.find('fatal') != -1:
                         print('Каталог {} не является Git репозиторием'.format(sys.argv[1]))
                         break
                     elif item.find('изменено') != -1:
                         prepare_result = item.replace('\tизменено:      ', '')
                         print('{}{}'.format(sys.argv[1], prepare_result)) 
                             
             else:
                 result = git_command(os.getcwd())
                 for item in result.split('\n'):
                     if item.find('изменено') != -1:
                         prepare_result = item.replace('\tизменено:      ', '')
                         print('{}/{}'.format(os.getcwd(), prepare_result))
   

4. Наша команда разрабатывает несколько веб-сервисов, доступных по http. Мы точно знаем, 
   что на их стенде нет никакой балансировки, кластеризации, за DNS прячется конкретный IP сервера, 
   где установлен сервис. Проблема в том, что отдел, занимающийся нашей инфраструктурой очень часто 
   меняет нам сервера, поэтому IP меняются примерно раз в неделю, при этом сервисы сохраняют за собой 
   DNS имена. Это бы совсем никого не беспокоило, если бы несколько раз сервера не уезжали в такой 
   сегмент сети нашей компании, который недоступен для разработчиков. Мы хотим написать скрипт, 
   который опрашивает веб-сервисы, получает их IP, выводит информацию в стандартный вывод в виде: 
   <URL сервиса> - <его IP>. Также, должна быть реализована возможность проверки текущего IP сервиса 
   c его IP из предыдущей проверки. Если проверка будет провалена - оповестить об этом в стандартный 
   вывод сообщением: [ERROR] <URL сервиса> IP mismatch: <старый IP> <Новый IP>. 
   Будем считать, что наша разработка реализовала сервисы: drive.google.com, mail.google.com, google.com.


         


   



