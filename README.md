

# devops-netology

1. Есть скрипт:

         a=1
         b=2
         c=a+b
         d=$a+$b
         e=$(($a+$b))
   
   -Какие значения переменным c,d,e будут присвоены?
   
   -Почему?

   Переменной "с"  будет присвоена строка 'a+b', так как в c=a+b ма не вызываем переменную($), а присваиваем переменной "c" строку (неявное определение строки).

   Переменной "d" будет присвоена  строка '1+2', так как 'd' не определенна как целочисленная переменная, 
   то bash подставит в строку значения переменных 'a' и 'b', '+' воспринимается как строковый символ.

   Переменной "e" будет присвоена сумма значений переменных "a" и "b" т.е. 3 т.к. сначала значения во внутренних скобках
   будут приведены к целочисленным и будет проведена операция сложения, затем с помощью конструкции $() результат записывается в переменную.

2. 

         while ((1==1))
         do
         curl https://localhost:4757
         if (($? != 0))
         then
         date >> curl.log
         else
         break
         fi
         done

3. 

       #!/usr/bin/env bash
         
         ip_list=(192.168.0.1 173.194.222.113 87.250.250.242)
         
         for ip_addr in ${ip_list[@]}
         do
         a=5
           while (($a>0))
           do
             curl http://$ip_addr:80 /dev/null 2>&1
             if (($? != 0))
             then
             echo "[`date +%F--%H-%M`] Connection failed [$ip_addr]" >> log
             
             else
               echo "[`date +%F--%H-%M`] Connection OK [$ip_addr]" >> log
               
             fi 
             a=$(($a-1))  
           done
           
         done

4. 
         #!/usr/bin/env bash
         
         ip_list=(192.168.0.1 173.194.222.113 87.250.250.242)
         while ((1==1))
         do 
            for ip_addr in ${ip_list[@]}
            do
               curl http://$ip_addr:80 /dev/null 2>&1
               if (($? != 0))
               then
                  echo "[`date +%F--%H-%M`] Connection failed [$ip_addr]" >> error
                  break 2
               fi
            done
         done

  


         


   



