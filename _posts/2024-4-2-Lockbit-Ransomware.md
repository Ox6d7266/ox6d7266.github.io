---
layout: post
title: "Lockbit Ransomware"
---


***Про партнерку Lockbit***
До недавнего времени существовало несколько партнерских программ. Каждая заявляла что вот она самая лучшая, на форумах малварщиков можно найти трештолк между локбит и блэккет например. 
Объективно каждая софтина выполняла свой функционал и радовала большими прибылями админов, что расслабляло их и ставило на край пропасти админов и их партнеров. 
Хочу немного распотрошить билдер локбита и посмотреть что там внутри. Заодно рассмотреть типичные ошибки оставленные разрабами. 
Рассматривать буду версию с кряком, но врятли она сильно отличается от купленной на тот момент.

---
 
***Конфиг файл.*** 
Тут тебе и параметры для билда и белый список с файлами и папками, а так же попытки в самораспространение с помощью psexec.

![]({{ site.baseurl }}/images/16.png)

Из неприятного для партнера все хранилось у самих локбитов и кто знает как это все использовалось. 

***Модуль шифрования LB3.***

![]({{ site.baseurl }}/images/17.png)

Особенности модуля шифрования
- модуль запиливается с помощью приватного ключа партнера, который сверяется с открытым ключом партнерки
- шифровальщик особенно эффективно будет работать с 7 по 10 винду.
- шифруются все папки и файлы, кроме белого списка
В целом модуль похож на аналог black cat. Есть и отличие в минимальном функционале самораспрострения через psexec и алгоритме шифрации, однако основной способ распространения через доменный функционал. 

![]({{ site.baseurl }}/images/18.png)

---

***MITRE ATT&CK***
Пора рассказать о тактиках и техниках [MITRE ATT&CK](https://attack.mitre.org/), которые используются оператором рансомы и кодером, для распространения и работы. 
T1027, T1027.002, T1140 – обфускация файлов, инфы и пакинг по
T1007, T1082 – обнаружение системной инфы
T1059 – использование bat и ps1 скриптов
TA0010 – упаковка чувствительных данных организации
T1490 – запрет на восстановление системы
T1485 – уничтожение данных
T1078 – поиск валидных аккаунтов в корпоративной сети
T1486 – шифрование данных
T1202 – использование системных утилит для работы рансомы
T1543.003 – изменение системных процессов, используется для подгрузки различной полезной нагрузки, реализованно с помощью winapi
T1550.002 – использование хешей паролей в процессе атаки

---

***Модуль дешифрации LB3Decryptor.***
 
Тут все очень скучно на основе приватного ключа и публичного формируется пароль шифрации и дешифрации и его использует модуль дешифровки, самописного алгоритма шифрации данных нет, так что сам алгоритм я оставлю за скобками.

![]({{ site.baseurl }}/images/19.png)

---

***Смертельные грехи малварщика локбит.***

![]({{ site.baseurl }}/images/20.png)

- Нулевое самораспространения, с учетом стоимости партнерки, партнер должен сам заботиться о инфецировании сети, т.е. он должен не только найти таргет, ту самую смешную бухгалтершу Нину, но и имея набор инструментов забрать сначала доменного админа, затем провести полноценную атаку на сеть организации. Что делает малварь абсолютно беспомощной без оператора атаки. В 2024 году и это уже успело набить оскомину и стать скучным. Да, защита не стоит на месте, но скука. 
- модуль шифрации не имеет инфы о файле, что может использоваться для детекта со стороны soc.
- трафик не шифруется и его можно задетектить. 
Да малварь очень простая и сама по себе, особенно если выключен psexec не может сделать ничего, как впрочем и blackcat.
Оба образца скучны, так что это где-то 1 из 5 злобных интернет червей. 
Начните писать нормально, блиать.

![]({{ site.baseurl }}/images/21.png)

---
 
[А пока подпишись на канал.](https://t.me/rfmOOx) 