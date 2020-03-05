How to create user on ftp with limited rules
============================================

1. Connect with ssh:
^^^^^^^^^^^^^^^^^^^^
   - test server: **ssh heedbookadmi@13.81.51.123**
   - password **--------**


2. Create user
^^^^^^^^^^^^^^
.. code:: bash
	
	sudo useradd -d /home/nkrokhmal/storage client
	sudo passwd client

-----------------------------

    - **-d** - home directory */home/nkrokhmal/storage*
    - **client** - user name
    - **Test_User12345** - password

3. Set rules for user
^^^^^^^^^^^^^^^^^^^^^

.. code:: bash

            setfacl -R -m u:client:r-x /home

----------------------------


    - **-R** - recursive for folders and files
    - **-m** - modify
		- u: *At the user level - rules are assigned to specific users*;
		- g: *At the group level - assigned rules to specific groups;*
		- o: *For users who are not included in the group. All Others user* - this rule set general access from the site
    - **client** - username
    - **r-x** - read and execute  (another options rwe - read, write, execute)
    - **/home** - folder for the rules
    

4. Check rules
^^^^^^^^^^^^^^
.. code:: bash

            getfacl *

----------------------------------------------------------

    **result example**	


.. code:: bash

	client@ftpserver:/home/nkrokhmal$ getfacl *
	# file: storage
	# owner: nkrokhmal
	# group: root
	user::rwx
	user:nkrokhmal:rwx
	user:client:r-x
	group::r-x
	mask::rwx
	other::r-x

	# file: teststorage
	# owner: nkrokhmal
	# group: root
	user::rwx
	user:nkrokhmal:rwx
	user:client:r-x
	group::r-x
	mask::rwx
	other::r-x

