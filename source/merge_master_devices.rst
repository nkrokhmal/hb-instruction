Выяснение причин и устранение сбоев системы
============================================

1. Зайти на виртуальную машину (на портале Azure адрес) по ssh


   - мастер тестового сервиса: **heedbookadmin@52.174.35.129**
	
2. Получить информацию по слейвам

.. code:: bash
	
	kubectl get nodes

-----------------------------

результат (пример)


.. code:: bash
	
	heedbookadmin@master:~$ kubectl get nodes
	NAME     STATUS     ROLES    AGE    VERSION
	master   Ready      master   113d   v1.14.2
	slave    NotReady   <none>   113d   v1.14.2
	slave2   Ready      <none>   51d    v1.14.2

-----------------------------

	- статус NotReady говорит об ошибке
	- заходим на портал Azure, переходим на соответствующую виртуальную машину, нажимаем Restart


3. Получить информацию по подам

.. code:: bash
	
	kubectl get podes

-----------------------------

результат (пример)


.. code:: bash
	
	heedbookadmin@master:~$ kubectl get pods
	NAME                                             READY   STATUS        RESTARTS   AGE
	audioanalyzeservice-54fd77444d-5x6w4             1/1     Running       6          7d
	default-http-backend-vq6lz                       1/1     Running       0          7d
	detectfaceidextendedscheduler-5449888bf6-cfml8   1/1     Running       0          23m
	detectfaceidextendedscheduler-5449888bf6-fh654   1/1     Terminating   0          144m

-----------------------------

	- статусы Running, Terminating, ContainerCreating, ImagePullBackouff, Error
	- ImagePullBackouff, Error  - говорят об ошибке


		
