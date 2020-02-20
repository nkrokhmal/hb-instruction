Apache 2
========

Install apache2
^^^^^^^^^^^^^^^

Устанавливаем Apache2 на виртуальную машину
.. code:: console

        sudo apt-get update
        sudo apt install apache2


Apache2 Configuration
^^^^^^^^^^^^^^^^^^^^^

Создаем в директории `/etc/apach2/sites-available` файл `ftpserver.conf`

.. code:: console

        <VirtualHost *:80>
        DocumentRoot /home/nkrokhmal/storage
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
        <Directory /home/nkrokhmal/storage>
                AllowOverride None
                Order allow,deny
                # allow from {your ip}
                Require all granted
        </Directory>
        <Directory /home/nkrokhmal/storage/clientavatars>
                AllowOverride None
                Order allow,deny
                <Files ~ "\.(jpg|png)$">
                        order deny,allow
                        Allow from All
                </Files>
        </Directory>
        <Directory /home/nkrokhmal/storage/dialoguevideos>
                AllowOverride None
                Order allow,deny
                <Files ~ "\.(mkv|webm|mp4)$">
                        order deny,allow
                        Allow from All
                </Files>
        </Directory>
        <Directory /home/nkrokhmal/storage/screenshots>
                AllowOverride None
                Order allow,deny
                <Files ~ "\.(jpg|png)$">
                        order deny,allow
                        Allow from All
                </Files>
        </Directory>
        <Directory /home/nkrokhmal/storage/media>
                allow from all
                AllowOverride None
                Order allow,deny
        </Directory>
        </VirtualHost>

 А файле `/etc/apache2/apach2.conf` добавляем в самое начало

 .. code:: console

        <Directory /home/nkrokhmal/storage>
                Options Indexes FollowSymLinks
                AllowOverride None
                Require all granted
        </Directory>

 И выполняем в командной строке команды

.. code:: console

        $ sudo a2ensite ftpserver.conf 
        $ sudo a2dissite 000-default.conf
        $ sudo apache2ctl configtest
        $ sudo systemctl reload apache2

При этом мы активируем наш сайт, дизейблим старый дефолтный сайт, проверяем конфиг, и перезагружаем сервис apache2.


Apache2 certificates
^^^^^^^^^^^^^^^^^^^^

Сначала добавляем репозиторий 

.. code:: console
        
        $ sudo add-apt-repository ppa:certbot/certbot

Устанавливаем пакет Certbot для Apache с помощью ``apt``:

.. code:: console

        $ sudo apt install python-certbot-apache

Получаем SSL сертификат с помощью команды

.. code::  console

        $ sudo certbot --apache -d example.com -d {your host}

где надо будет заполнить немного информации и сертификаты будут созданы
Сертификаты действительны только 90 дней, поэтом для обновления можно использовать

.. code:: console

        sudo certbot renew --dry-run
