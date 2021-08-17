

# devops-netology

1. В ipvs мы имеем два состояния ActiveConn и InActiveConn.

   В ActiveConn отображаются TCP соединения в статусе "Established", в InActiveConn все остальные, включая "Wait".
    
   Поэтому, ожидая ответа о завершении TCP-соединения, соединение попадает в статус  InActiveConn.



2. Схема 

   ![Screenshot](/images/schema.png)

   Файлы конфигурации приложены (netology3_keepalived.conf, netology4_keepalived.conf)

   Проверяем:
      
         vagrant@netology5:~$ for i in {1..50}; do curl -I -s 172.28.128.200>/dev/null; done

   netology3

   ![Screenshot](/images/screen_netology3.png)

   netoligy1

   ![Screenshot](/images/screen_netology1.png)

   netology2

   ![Screenshot](/images/screen_netology2.png)

   Приостанавливаем netology3:

         (base) ghost@WS13:~/Work/vagrant$ vagrant suspend netology3
         ==> netology3: Saving VM state and suspending execution...
   
   Смотрим netology4:

   ![Screenshot](/images/screen_netology4.png)

   netology1

   ![Screenshot](/images/screen_netology1_2.png)

   netology2:

   ![Screenshot](images/screen_netology2_2.png)
   


3. Если мы используем 3 балансировщика в активном режиме, то оптимально использовать 6 VIP, по два на

   каждый балансировщик, что бы иметь возможноть при падении одного из хостов перекинуть по одному VIP
   



