

# devops-netology

1. chdir("/tmp")   

2. База данных file находится в файле /usr/share/misc/magic.mgc


3. $ cat /dev/urandom >> random

   $ ps aux | grep urandom

   vagrant     6287 34.7  0.0   8220   528 pts/0    T    01:20   0:04 cat /dev/urandom

   $ rm random

   $ lsof -p 6287 | grep random

   cat     6287 vagrant    1w   REG  253,0 902299648 2883622 /home/vagrant/random (deleted)

   $ ll /proc/6287/fd/1

   l-wx------ 1 vagrant vagrant 64 Jun 18 01:24 /proc/6287/fd/1 -> '/home/vagrant/random (deleted)'

   Обнуляем:

   $ cat /dev/null > /proc/6287/fd/1


4. Да, можно
   
   [    0.001237] CPU MTRRs all blank - virtualized system.
   
   [    0.067803] Booting paravirtualized kernel on KVM
   
   [    3.952130] systemd[1]: Detected virtualization oracle.
   
   [   27.641916] 04:57:45.685861 main     Executable: /opt/VBoxGuestAdditions-6.1.16/sbin/VBoxService



5.  /sys/fs/cgroup/unified/system.slice/systemd-udevd.service/cgroup.procs
   
    /sys/fs/cgroup/unified/system.slice/systemd-udevd.service/cgroup.threads

    /var/run/utmp

    /usr/local/share/dbus-1/system-services

    /usr/share/dbus-1/system-services

    /lib/dbus-1/system-services

    /var/lib/snapd/dbus-1/system-services/


6. $ strace uname -a

uname({sysname="Linux", nodename="vagrant", ...}) = 0

fstat(1, {st_mode=S_IFCHR|0620, st_rdev=makedev(0x88, 0), ...}) = 0

uname({sysname="Linux", nodename="vagrant", ...}) = 0

uname({sysname="Linux", nodename="vagrant", ...}) = 0

write(1, "Linux vagrant 5.4.0-58-generic #"..., 105Linux vagrant 5.4.0-58-generic #64-Ubuntu SMP Wed Dec 9 08:16:25 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
) = 105


   Строка в man: Part of the utsname information is also accessible via /proc/sys/kernel/{ostype, hostname, osrelease, version, do‐
       mainname}.


7. ; - оператор разделения команд, т.е. в случае root@netology1:~# test -d /tmp/some_dir; echo Hi

   сначала выполниться test -d /tmp/some_dir затем echo Hi, обе команды выполнятся в любом случае.

    && - управляющий оператор, echo Hi вытолнится только в том случае, если статус выхода 
    из команды test -d /tmp/some_dir будет 0, т.е. команда выполниться успешно.

    Есть ли смысл использовать в bash &&, если применить set -e? - В скрипте конструкция set -e; test -d /tmp/some_dir; echo Hi
    приведет к немедленому закрытию скрипта, если не будет найдена требуемая директория, а вот в теринале такая конструкция приведет к немедленому закрытию терминала, так что смысл быть может.  


8.  set -euxo pipefail

    -e - немедленный выход, если команда завершается с не нулевым статусом.

    -u - Рассматривать неинициализированные переменные как ошибку при подстановке.

    -x - Печатать комманды и их аргументы помере их выполнения.

    -o pipefail - Эта опция устанавливает код выхода из конвейера равным таковому для самой правой команды для выхода с ненулевым статусом или равным нулю, 
если все команды конвейера завершаются успешно. 

   Использовать данный режим в bash имеет смысл по следующим причинам:

   - параметр -e приведет к немедленному завершению сценария в случае сбоя команды, по умолчанию bash проигнорирует неудачную команду в сценарии и перейдет к следующей строке.

   -o pipefail - bash, обычно, смотрит только на код выхода последней команды конвейера, это приводит к тому, что параметр 
-e может реагировать только на код выхода последней команды конвейера. Опция -o pipefail код выхода из конвейера равным 
таковому для самой правой команды для выхода с ненулевым статусом или равным нулю, если все команды конвейера завершаются успешно.
  
   - параметр -u завершит сценарий если наткнется на неинициализированную переменную.

   - Параметр -x заставляет bash печатать каждую команду перед ее выполнением. Может быть полезно при отладке сценария.

   

9. Наиболее часто встречающийся статус -   S = 100 спящий процесс, ожидающий завершения сибытия.

  Буквы,  дополнительные к основной заглавной, это устаревцие ключи сортировки. Эти ключи используются опцией BSD O (когда она используется для сортировки).

   
 
