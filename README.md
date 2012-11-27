temp
====

В putty неверно отображается псевдографика Midnight Commander
------------------------------------------------------------
http://www.gentoo.ru/content/v-putty-neverno-otobrazhaetsya-psevdografika-midnight-commander

Если вместо рамок в mc отрисовываются различные символы, то измените настройки putty:
   
* Terminal > Keyboard > "The Function keys and keypad" = linux
* Window > Translation > Character set - выставляем правильную кодировку
* Connection > Data > "Terminal-type string" пишем linux
    
Сохраняем сессию, и после подключения mc будет отображать псевдографику корректно.