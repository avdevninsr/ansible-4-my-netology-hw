# Домашнее задание к занятию 4 «Работа с roles»
Просьба к тому сотруднику, кто писал роль-образец для Кликхауса: перепишите её, пожалуйста, она устарела наглухо, и на 2026 год НЕ РАБОТАЕТ ВООБЩЕ: дистрибутив Центос СДОХ С КОНЦАМИ и RPM-based дистрибутивы НИКОМУ в России к чёрту не упали (в госструктурах используется в основном Астра, колторая доработанный Дебиан, в Армии не смотря на наличие какой - то там МСВС используется Винда (служившие знакомые подтвердят), частники если и используют Линукс, то это либо Убунту, либо тот же Дебиан). Альт Линукс исключение, но это очень странная и дурацкая система - она RPM-based, но используются там апт и синаптик, рабочий инструментарий типа Докера на неё нормально не поставишь, инструкций нет, а в собственной документации Альта про Докер тоже нет ничего.</br>
Отдельно скажу по поводу Vector: это очень странное и кривое ПО, которое даже с дефолтными конфигами от его собственных разрабов (которые лежат в папке /etc/vector/examples) НЕ РАБОТАЕТ!!! Нормальных иснтрукций на русском языке по его установке и настройке НЕ СУЩЕСТВУЕТ! Даже на официальном сайте www.vector.dev, который в России без VPN нормально также НЕ РАБОТАЕТ (а с бесплатнымии ВПН сейчас БЕДА, я рабочие ключи для своего Hiddefy нахожу с огромнейшим трудом) имеется довольно куцая инструкция по его установке и ВСЁ. Я очень надеюсь что те, кто составлял это задание, сами пробовали устанавливать и запускать Вектор хотя бы вручную ПОСЛЕ 2023 года.</br>
По заданию там надо просто установить Кликхаус, Вектор и Лайтхаус. Я их установил.</br>
Часть ответов представлена в виде кода, плюс для экономии места удалены пустые строки.</br>
Рабочее окружение: управляющая ВМ с Ansible - Ubuntu-Server 22.04-5 с обновлённым вручную до 5.15.0-185-generic ядром и также вручную обновлёнными модулями libc6 и lacales; подчинённая ВМ, на которую всё это добро ставилось - последняя версия Debian 13 Trixie.</br>

## Выполнение

Скачиваем роли:
```
tankist2@docker-vm:~/ansible_4$ ansible-galaxy install -r requirements.yml -p roles --force
Starting galaxy role install process
- extracting clickhouse to /home/tankist2/ansible_4/roles/clickhouse
- clickhouse was installed successfully
- extracting vector to /home/tankist2/ansible_4/roles/vector
- vector was installed successfully
- extracting lighthouse to /home/tankist2/ansible_4/roles/lighthouse
- lighthouse was installed successfully
[WARNING]: Meta file /home/tankist2/ansible_4/roles/lighthouse is empty. Skipping dependencies.

tankist2@docker-vm:~/ansible_4$ ls -la roles
total 20
drwxr-xr-x  5 tankist2 tankist 4096 июн 22 20:05 .
drwxr-xr-x  3 tankist2 tankist 4096 июн 22 20:05 ..
drwxr-xr-x 10 tankist2 tankist 4096 июн 22 20:05 clickhouse
drwxr-xr-x  8 tankist2 tankist 4096 июн 22 20:05 lighthouse
drwxr-xr-x  8 tankist2 tankist 4096 июн 22 20:05 vector
```
[Репозиторий с ролью clickhouse.](https://github.com/avdevninsr/ansible-clickhouse)</br>
[Репозиторий с ролью lighhouse.](https://github.com/avdevninsr/lighthouse-role)</br>
[Репозиторий с ролью vector.](https://github.com/avdevninsr/vector-role)</br>
Запускаем плейбук:
```
tankist2@docker-vm:~/ansible_4_1$ ansible-playbook -i prod.yml site.yml
PLAY [Set hostnames for all servers] ********************************************************************************
TASK [Gathering Facts] **********************************************************************************************
The authenticity of host '192.168.0.121 (192.168.0.121)' can't be established.
ED25519 key fingerprint is SHA256:5F+OKlpYF8hyKV69M6C4v7SzWCNsjAJGGuB5Ut4We64.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:4: [hashed name]
    ~/.ssh/known_hosts:10: [hashed name]
    ~/.ssh/known_hosts:11: [hashed name]
    ~/.ssh/known_hosts:16: [hashed name]
    ~/.ssh/known_hosts:17: [hashed name]
    ~/.ssh/known_hosts:18: [hashed name]
    ~/.ssh/known_hosts:19: [hashed name]
    ~/.ssh/known_hosts:20: [hashed name]
    (2 additional names omitted)
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
[WARNING]: Platform linux on host debian is using the discovered Python interpreter at /usr/bin/python3, but future
installation of another Python interpreter could change the meaning of that path. See
https://docs.ansible.com/ansible-core/2.17/reference_appendices/interpreter_discovery.html for more information.
ok: [debian]
TASK [Set hostname based on inventory] ******************************************************************************
skipping: [debian]
PLAY [Install Clickhouse] *******************************************************************************************
TASK [Gathering Facts] **********************************************************************************************
ok: [debian]
TASK [clickhouse : Include OS Family Specific Variables] ************************************************************
ok: [debian]
TASK [clickhouse : include_tasks] ***********************************************************************************
included: /home/tankist2/ansible_4_1/roles/clickhouse/tasks/params.yml for debian
TASK [clickhouse : Set clickhouse_service_enable] *******************************************************************
ok: [debian]
TASK [clickhouse : Set clickhouse_service_ensure] *******************************************************************
ok: [debian]
TASK [clickhouse : include_tasks] ***********************************************************************************
included: /home/tankist2/ansible_4_1/roles/clickhouse/tasks/install/apt.yml for debian
TASK [clickhouse : Preparing] ***************************************************************************************
changed: [debian]
TASK [clickhouse : Download clickhouse-common-static] ***************************************************************
changed: [debian]
TASK [clickhouse : Download clickhouse-client] **********************************************************************
changed: [debian]
TASK [clickhouse : Download clickhouse-server] **********************************************************************
changed: [debian]
TASK [clickhouse : Install clickhouse-common-static] ****************************************************************
changed: [debian]
TASK [clickhouse : Install clickhouse-client] ***********************************************************************
changed: [debian]
TASK [clickhouse : Install clickhouse-server] ***********************************************************************
changed: [debian]
TASK [clickhouse : include_tasks] ***********************************************************************************
included: /home/tankist2/ansible_4_1/roles/clickhouse/tasks/configure/sys.yml for debian
TASK [clickhouse : Check clickhouse config, data and logs] **********************************************************
ok: [debian] => (item=/var/log/clickhouse-server)
changed: [debian] => (item=/etc/clickhouse-server)
changed: [debian] => (item=/var/lib/clickhouse/tmp/)
changed: [debian] => (item=/var/lib/clickhouse/)
TASK [clickhouse : Config | Create config.d folder] *****************************************************************
changed: [debian]
TASK [clickhouse : Config | Create users.d folder] ******************************************************************
changed: [debian]
TASK [clickhouse : Config | Generate system config] *****************************************************************
changed: [debian]
TASK [clickhouse : Config | Generate users config] ******************************************************************
changed: [debian]
TASK [clickhouse : Config | Generate remote_servers config] *********************************************************
skipping: [debian]
TASK [clickhouse : Config | Generate macros config] *****************************************************************
skipping: [debian]
TASK [clickhouse : Config | Generate zookeeper servers config] ******************************************************
skipping: [debian]
TASK [clickhouse : Config | Fix interserver_http_port and intersever_https_port collision] **************************
skipping: [debian]
TASK [clickhouse : Notify Handlers Now] *****************************************************************************
RUNNING HANDLER [clickhouse : Restart Clickhouse Service] ***********************************************************
ok: [debian]
TASK [clickhouse : include_tasks] ***********************************************************************************
included: /home/tankist2/ansible_4_1/roles/clickhouse/tasks/service.yml for debian
TASK [clickhouse : Ensure clickhouse-server.service is enabled: True and state: restarted] **************************
changed: [debian]
TASK [clickhouse : Wait for Clickhouse Server to Become Ready] ******************************************************
ok: [debian]
TASK [clickhouse : include_tasks] ***********************************************************************************
included: /home/tankist2/ansible_4_1/roles/clickhouse/tasks/configure/db.yml for debian
TASK [clickhouse : Set ClickHose Connection String] *****************************************************************
ok: [debian]
TASK [clickhouse : Gather list of existing databases] ***************************************************************
ok: [debian]
TASK [clickhouse : Config | Delete database config] *****************************************************************
skipping: [debian]
TASK [clickhouse : Config | Create database config] *****************************************************************
skipping: [debian]
TASK [clickhouse : include_tasks] ***********************************************************************************
included: /home/tankist2/ansible_4_1/roles/clickhouse/tasks/configure/dict.yml for debian
TASK [clickhouse : Config | Generate dictionary config] *************************************************************
skipping: [debian]
TASK [clickhouse : include_tasks] ***********************************************************************************
skipping: [debian]
PLAY [Install Lighthouse] *******************************************************************************************
TASK [Gathering Facts] **********************************************************************************************
ok: [debian]
TASK [lighthouse : Install nginx] ***********************************************************************************
changed: [debian]
TASK [lighthouse : Install git] *************************************************************************************
changed: [debian]
TASK [lighthouse : Create web root directory] ***********************************************************************
changed: [debian]
TASK [lighthouse : Clone lighthouse repository] *********************************************************************
changed: [debian]
TASK [lighthouse : Remove default nginx config] *********************************************************************
changed: [debian]
TASK [lighthouse : Create nginx config] *****************************************************************************
changed: [debian]
TASK [lighthouse : Start and enable nginx] **************************************************************************
ok: [debian]
RUNNING HANDLER [lighthouse : Restart nginx] ************************************************************************
changed: [debian]
PLAY [Install Vector] ***********************************************************************************************
TASK [Gathering Facts] **********************************************************************************************
ok: [debian]
TASK [vector : Download Vector DEB] *********************************************************************************
changed: [debian]
TASK [vector : Install Vector] **************************************************************************************
changed: [debian]
TASK [vector : Deploy Vector config] ********************************************************************************
changed: [debian]
TASK [vector : Start and enable Vector] *****************************************************************************
fatal: [debian]: FAILED! => {"changed": false, "msg": "Unable to start service vector: Job for vector.service failed because the control process exited with error code.\nSee \"systemctl status vector.service\" and \"journalctl -xeu vector.service\" for details.\n"}
PLAY RECAP **********************************************************************************************************
debian                     : ok=41   changed=23   unreachable=0    failed=1    skipped=9    rescued=0    ignored=0
```
Результат выполнения плейбука представлен на рисунках 1 и 2.</br>

<img src="Файлы/%D0%A0%D0%B5%D0%B7%D1%83%D0%BB%D1%8C%D1%82%D0%B0%D1%82%201.PNG" alt="Рисунок 1" width="auto" height="auto"></br>
Рисунок 1. Доказаительство установки софта на подчинённый хост.</br>

<img src="Файлы/%D0%A0%D0%B5%D0%B7%D1%83%D0%BB%D1%8C%D1%82%D0%B0%D1%82%202.png" alt="Рисунок 2" width="auto" height="auto"></br>
Рисунок 2. Интерфейс lighthouse в браузере.</br>
Все файлы проекта лежат [здесь](Файлы/%D0%9F%D1%80%D0%BE%D0%B5%D0%BA%D1%82/).</br>
