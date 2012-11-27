temp
====

В putty неверно отображается псевдографика Midnight Commander
------------------------------------------------------------
https://github.com/alba2001/temp

Если вместо рамок в mc отрисовываются различные символы, то измените настройки putty:
*Terminal > Keyboard > "The Function keys and keypad" = linux
*Window > Translation > Character set - выставляем правильную кодировку
*Connection > Data > "Terminal-type string" пишем linux
Сохраняем сессию, и после подключения mc будет отображать псевдографику корректно.