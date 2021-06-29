

# devops-netology

1. unit-файл:
   
   [Unit]

   Description=Prometheus Node Exporter

   Wants=network-online.target

   After=network-online.target

   
   [Service]

   User=node_exporter

   Group=node_exporter 
   
   Type=simple 
   
   ExecStart=/usr/local/bin/node_exporter --web.config=/opt/node_exporter/conf.yml

   [Install]

   WantedBy=multi-user.target

   Добавляем в автозагрузку:

  $ sudo systemctl daemon-reload
  
  $ sudo systemctl enable --now node_exporter

  Смотрим статус:

  $ sudo systemctl status node_exporter

    ● node_exporter.service - Prometheus Node Exporter
    
    Loaded: loaded (/etc/systemd/system/node_exporter.service; enabled; vendor preset: enabled)
    Active: active (running) since Tue 2021-06-29 00:21:35 UTC; 8min ago
   
    Main PID: 24042 (node_exporter)
      Tasks: 4 (limit: 2281)
     Memory: 2.2M
     CGroup: /system.slice/node_exporter.service
             └─24042 /usr/local/bin/node_exporter --web.config=/opt/node_exporter/conf.yml



2. Метрики для мониторинга хоста:
    
    CPU: node_cpu_seconds_total 
    
    RAM: node_memory_MemTotal_bytes, node_memory_MemFree_bytes

    Диск: node_filesystem_avail_bytes

    Сетевой трафик: node_network_receive_bytes_total, node_network_transmit_bytes_total
   


3. 

4. Да, можно
   
   [    0.001237] CPU MTRRs all blank - virtualized system.
   
   [    0.067803] Booting paravirtualized kernel on KVM
   
   [    3.952130] systemd[1]: Detected virtualization oracle.
   
   [   27.641916] 04:57:45.685861 main     Executable: /opt/VBoxGuestAdditions-6.1.16/sbin/VBoxService



5.  По умолчанию fs.nr_open = 1048576, данный параметр задает максимально возможное
    число открытых файловых дескрипторов.
    Другой лимит - это ограничение ресурсов оболочки.
    
    vagrant@vagrant:~$ ulimit -n
    
    1024

    vagrant@vagrant:~$ ulimit -Hn

    1048576



6. root@vagrant:/# unshare -u sleep

    root@vagrant:/home/vagrant# ps aux | grep sleep

    root        1448  0.0  0.0   8080   592 pts/1    S+   07:27   0:00 unshare -f --pid --mount-proc sleep 1h

    root        1449  0.0  0.0   8076   580 pts/1    S+   07:27   0:00 sleep 1h

    root        1463  0.0  0.0   8900   736 pts/2    S+   07:30   0:00 grep --color=auto sleep

    root@vagrant:/home/vagrant# nsenter --target 1449 --pid --mount

    root@vagrant:/# ps aux | grep sleep

    root           1  0.0  0.0   8076   580 pts/1    S+   07:27   0:00 sleep 1h

    root          12  0.0  0.0   8900   736 pts/2    S+   07:33   0:00 grep --color=auto sleep



7. :(){ :|:& };: - это, так называемая, форкбомба -  программа бесконечно создающая свои копии которые в свою очередь порождают свои копии.
 
   Механизм стабилизации: cgroup: fork rejected by pids controller in /user.slice/user-1000.slice/session-4.scope

   По умолчанию:

  vagrant@vagrant:~$ cat /sys/fs/cgroup/pids/user.slice/user-1000.slice/pids.max

  5019

  Можно изменить лимит:

   ulimit -u 100
   





   
 
