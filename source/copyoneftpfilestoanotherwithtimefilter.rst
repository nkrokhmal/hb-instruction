Copy data from one FTP to another with time filter
==================================================


Directory storage/ on new FTP server must have an owner `user` user, for that run following command:

.. code:: console

         $ sudo chown `user` -R storage

All directories in storage/ folder must have write permissions, for that run following command:

.. code:: console

         $ sudo chmod 777 -R storage

Folder owner for directories: clientavatars, dialogueaudios, dialoguevideos, media, mediacontent, useravatars must be `user`.

- Install sshpass on old FTP:

.. code:: console

         $ sudo apt-get install sshpass

- Connect with old FTP and open /home/`user`/storage folder. Correct filtered time in 3 command line and run it :

.. code:: console

         for f in dialoguevideos clientavatars useravatars media mediacontent dialogueaudios
         do
           find $f/* -type f -newermt "2020-02-12 00:00:00" >> fileList.txt
         done

         for f in $(cat ./fileList.txt)
         do
           sshpass -p 'secret_password' sudo scp /home/`user`/storage/$f nkrokhmal@heedbookftp.northeurope.cloudapp.azure.com:/home/`user`/storage/$f
           echo "/home/`user`/storage/$f"
         done
         rm -rf fileList.txt 
