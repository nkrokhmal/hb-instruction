Заполнить ссылку на диалог в записях про показы рекламы 
=======================================================

1. Работу по заполнению dialogueId в показах производит сервис FillSlideShowDialogueService



2. В базе создана функция fill_dialogue_in_slideshow(DialogueId) которая
    - принимает айди диалога
    - находит в базе показы(SlideShowSessions) во время диалога
    - делает update показов, сохраняя айди диалога для них



3. Если возникла необходимость заполнить dialogueId "в ручном режиме", достаточно обратиться к функции fill_dialogue_in_slideshow(передать dialogueId) средствами PostgreSQL


.. code:: bash
	
	select fill_dialogue_in_slideshow("DialogueId"), "DialogueId" 
        from "Dialogues"
        where "StatusId" = 3 and "BegTime" > '2020-03-10'

-----------------------------

