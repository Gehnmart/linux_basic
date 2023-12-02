#Отчет конфигурации `Linux/Unix` систем.

##*Part 1. Установка ОС:*

###Установка Ubuntu 20.04.06

![image](./images/1.png)
*<sup>Ввод комманды `cat` для вывода содержимого файла `etc/issue`</sup>*

##*Part 2. Создание пользователя:*

###Создание нового пользователя в группе adm
![image](./images/2.png)
*<sup>Добавление нового пользователя user-1.</sup>*

Ввод комманды `useradd -G adm user-1`
Вывод файла `etc/passwd`


##*Part 3. Настройка сети ОС:*

- *Изменение имяни хоста на user-1*
![image](./images/3-1.png)
- *Изменение часового пояса (`timezone`)*
![image](./images/3-2.png)
- *Вывод сетевых интерфейсов*
  - *Можно так:*
  ![image](./images/3-3-1.png)
  <sub>если установлен пакет `net-tools`</sub>
  - *Или же:*
  ![image](./images/3-3-2.png)
  - >lo *(loopback device)* –виртуальный интерфейс, присутствующий по
    >умолчанию в любом Linux. Он используется для отладки сетевых
    >программ и запуска серверных приложений на локальной машине.
    >C этим интерфейсом всегда связан адрес `127.0.0.1`. У него есть
    >dns-имя – `localhost`.
- *Вывод ip адреса устройства, на котором вы работаете, от DHCP сервера*
![image](./images/3-3-3.png)
  - >DHCP (англ. Dynamic Host Configuration Protocol — протокол
    >динамической настройки узла), позволяющий устройствам
    >автоматически получать IP-адрес, данные о DNS-сервере и другие
    >параметры, необходимые для работы в сети.

- *Вывод в консоль внешний ip-адрес шлюза (ip) и внутренний
   IP-адрес шлюза, он же ip-адрес по умолчанию (gw).*
  ![image](./images/3-4-1.png)

- Задать статичные (заданные вручную, а не полученные от DHCP сервера) настройки ip, gw, dns.

  ```yaml
    #Конфиг на языке yaml
    #/etc/netplan/config.yaml

    network:
      version: 2
      renderer: networkd
      ethernets:
        enp0s3:
          dhcp4: false
          addresses:
            - 10.0.2.15/22
          gateway4: 10.0.2.2
          nameservers:
            addresses:
              - 8.8.8.8
              - 8.8.4.4
  ```
  затем
  ```bash
  sudo netplan apply
  ifconfig
  ```
  ![image](./images/3-4-2.png)

  -Перезагрузить виртуальную машину. Убедиться, что статичные сетевые
  настройки (ip, gw, dns) соответствуют заданным в предыдущем пункте.
  ```bash
  sudo shutdown -r now
  ifconfig 
  ```
  Убедимся что вывод соответствует)
  ![image](./images/3-4-3.png)

  Используем комманду ping `ip`
  ```bash
  ping ya.ru
  ...
  ping 1.1.1.1
  ```
  ![image](./images/3-4-4.png)
  <sub>Вывод комманды ping</sub>

##*Part 4. Обновление ОС*
  - Далее обновляем нашу ос:
  ```bash
  sudo apt update && sudo apt upgrade
  ```
  ![image](./images/4-1-1.png)

##*Part 5. Использование команды sudo*
  `sudo` - позволяет строго определенным пользователям выполнять указанные программы с административными привилегиями без ввода пароля суперпользователя root.
  - Разрешить пользователю, созданному в Part 2, выполнять команду sudo:
  Вводим комманду:
  ```bash
  sudo usermod -aG sudo user-1
  ```
  ![image](./images/5-1-1.png)

##Part 6. Установка и настройка службы времени
  ![image](./images/6-1-1.png)

##Part 7. Установка и использование текстовых редакторов
   -  jpico
    ![image](./images/7-1-1.png)
      > c^s - save
       c^x - exit
   - vim
    ![image](./images/7-2-1.png)
     > i - interactive mode
       esc - command mode
       :wq - save and exit
   - nano
    ![image](./images/7-3-1.png)
     > c^s - save
       c^x - exit

  files 
  ![image](./images/7-4-1.png)

  для выхода без сохранения используем:
  jpico
  c^x нас спросят сохранить ли буфер нажимаем n
  ![image](./images/7-5-1.png)
  vim
  :q!
  ![image](./images/7-6-1.png)
  nano
  c^x нас спросят сохранить ли буфер нажимаем n
  ![image](./images/7-7-1.png)

  для замены используем:
  jpico
  `c^w` нас спросят что мы ищем далее G а затем на что мы хотим заменить
  ![image](./images/7-8-1.png)
  vim
  `:s/mart/tram`
  ![image](./images/7-9-1.png)
  nano
  `c^\` нас спросят что мы ищем а затем на что мы хотим заменить
  ![image](./images/7-10-1.png)

##*Part 8. Установка и базовая настройка сервиса SSHD*
  - В большинстве дистрибутивах sshd установлен по умолчанию
  - Добавить автостарт службы при загрузке системы.
    ```bash
    sudo systemctl enable sshd
    ```
  - Перенастроить службу SSHd на порт 2022.
    ![image](./images/8-1-1.png)
    ![image](./images/8-2-1.png)
    - Используя команду ps, показать наличие процесса sshd.
    ```bash
    ps -d | grep ssh
    ```
  
  ![image](./images/8-3-1.png)
