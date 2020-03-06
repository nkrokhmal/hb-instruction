Merge master and another branch
============================================

1. Merge master into branch
^^^^^^^^^^^^^^^^^^^^^^^^^^^
   - branch name: **devices**
   - master name: **master**

.. code:: bash
	
	git checkout master
	git pull
	git checkout devices
	git fetch origin
	git merge origin/master	

-----------------------------

	- in case you have conflicts
	
	 Open your favorite text editor, such as VS or VS code, and choose "take source" option on git panel.

    	- check appsetting files, and replase all settings, if needed

	- commit your stages

.. code:: bash

        git add .
	git commit -m "merge"
	git push

----------------------------


2. Merge branch into master
^^^^^^^^^^^^^^^^^^^^^^^^^^^

	- first merge all changes from master into devices (look part 1)
	- create new branch from devices

.. code:: bash

	git checkout devices
	git checkout -b fusion_branch
	

----------------------------

	- replase all setting in this branch on master settings (connection strings, ftp settings etc)
	- commit your stages
	
.. code:: bash

	git add .
	git commit -m "settings"
	git push --set-upstream origin fusion_branch

----------------------------

	make pull request to merge your stages into master from fusion_branch

		
