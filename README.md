# Домашнее задание к занятию "Disaster recovery и Keepalived" - Тихомиров Алексей


### Задание 1

[Схема CPT](https://github.com/Exel1992/sflt-homeworks/blob/main/hsrp_advanced_test.pkt)

![Настройка маршрутизатора](https://github.com/Exel1992/sflt-homeworks/blob/main/Sh%20r.png)

---

### Задание 2

[Bash скрипт]
```
#!/bin/bash

my_index=`test -f /var/www/html/index.html && echo $?`
my_port=`bash -c "</dev/tcp/localhost/80" && echo $?`

if [ $my_index -eq 0 ] && [ $my_port -eq 0 ]; then
       exit 0
else
      exit 1
fi

```

[Keepalived.conf на MASTER]

```
lobal_defs {
    script_user root
    enable_script_security

}

vrrp_script check_script {
        script "/etc/keepalived/check_script.sh"
        interval 3

}

vrrp_instance VI_1 {
        state MASTER
        interface enp0s3
        virtual_router_id 115
        priority 255
        advert_int 1

        virtual_ipaddress {
              192.168.0.115/24
        }

        track_script {
            check_script
        }

}

```

![Master-Status](https://github.com/Exel1992/sflt-homeworks/blob/main/Master-backup_1.png)

![Master->Backup](https://github.com/Exel1992/sflt-homeworks/blob/main/Master-backup_2.png)

---
