---
layout: post
title: "Black Cat Ransomware"
---

По BlackCat ransoware уже существует несколько англоязычных отчетов, а вот с русскоязычными, существенный проеб. 
Исправляем проеб и даем оценку шифровальщику.

---

**Инфицирование и атака корпоративной сети**
Инфицирование производится оператором шифровальщика при атаке корпоративной сети жерты или же путем доставки вредоносного файла на компьютер в сети организации. 
Оператор захватывает доменную структуру организации, где создает несколько gpo призванных создать в планировщике задач исполнение пары bat скриптов. 
После распрстранения данных gpo, в том числе и на сервера бекапов, оператор со злобным хохотом и вскриками "да начнется хаос", отключает антивирус и едр компании. 
Где-то тут в компании начинают подозревать о чем-то, однако пароли сменены, доступы закрыты, а эникеям начинают трезвонить женщины бальзаковского возраста с вопросами типа "а вот у нас все не работает, а что делать" и наконец начинается сущий кошмар любой организации. 
С тактикой инфицирования на этом все, в целом если бы они перед тем как прямо себя проявлять забирали бы все с собой, начиная с документов и заканчивая дампами домена, а так же бекапы их атаки были бы более эффективными, однако и в таком варианте, просто, достаточно эффективно, не сказать что круто.  

![]({{ site.baseurl }}/images/11.jpg)

---

**Описание работы модуля**

![]({{ site.baseurl }}/images/12.jpg)

Модуль отвечающий за шифрование имеет набор особенностей, что позволяет выделить его в семейство Black cat
- Похожие учетные данные между собой, используются в том числе и для шифрования, а так же на более старых версиях винды используются для смены пароля. 
- Открытый ключ индетификации, это все же партнерка, а значит как-то денежки платить надо у одного клиента партнерки может быть не один ключ
- шифрация далеко не всех папок и форматов их интересуют только определенные
- Шифровальщик нацелен на более старые версии windows и в этом имеется свой смысл, так как некоторое специфичное по не в состоянии работать на более новых ос, существуют и другие причины почему в организациях остаются устаревшие операционные системы. Да и в целом у большинства свой зоопарк и адок, хотя на 10 версию винды стоило бы перейти всем. 
Авторы знали куда целятся и решили особо не упорствовать. Это дало им возможность не писать свою кастомную библиотеку, а использовать полноценно winapi, что делается не всегда. 
Однако в их случае с учетом целей и захвата ad вполне оправданный подход. Алгоритм работы довольно простой, сначала модуль отсматривает запущенные процессы и кикает их, затем уже ищет необходимые ему директории и файлы и шифрует их.

![]({{ site.baseurl }}/images/13.jpg)

---

Список процессов для закрытия:
- sql svc$ veeam vss
- agntsvc dbeng50 dbsnmp encsvc
- excel firefox infopath isqlplussvc
- msaccess mspub mydesktopqos mydesktopservice
- notepad ocautoupds ocomm ocssd
- onenote oracle outlook powerpnt
- sqbcoreservice sql steam synctime
- tbirdconfig thebat thunderbird visio
- winword wordpad xfssvccon
Список расширений, директорий и файлов для шифрования:
- $recycle.bin $windows.~bt $windows.~ws 386
- adv all users ani appdata
- application data program files program files (x86) programdata
- scr shs spl sys system volume information theme thumbs.db tor browser
- windows windows.old
- desktop.ini exe
- google hlp hta icl
- icns ico iconcache.db ics
- idx intel key ldf
- lnk lock mod mozilla
- mpa msc msi msocache
- msp msstyles msu] nls
- nomedia ntldr ntuser.dat ntuser.dat.log
После шифрации выводит сообщение в котором просит зайти на сайт в онион сети и заплатить бабки. 
Таким нехитрым способом можно создать ад в любой компании, где есть AD и админы с blue team не особо интересуются своей работой, либо в компаниях с определенным специфичным по, которое давно не поддерживается. 

![]({{ site.baseurl }}/images/14.jpg)

---

**MITRE ATT&CK**

![]({{ site.baseurl }}/images/15.jpg)

Пора рассказать о тактиках и техниках [MITRE ATT&CK](https://attack.mitre.org/), которые используются оператором рансомы и кодером, для распространения и работы. 
- T1027, T1027.002, T1140 – обфускация файлов, инфы и пакинг по
- T1007, T1082 – обнаружение системной инфы
- T1059 – использование bat и ps1 скриптов
- TA0010 – упаковка чувствительных данных организации
- T1490 – запрет на восстановление системы
- T1485 – уничтожение данных
- T1078 – поиск валидных аккаунтов в корпоративной сети
- T1486 – шифрование данных
- T1202 – использование системных утилит для работы рансомы
- T1543.003 – изменение системных процессов, используется для подгрузки различной полезной нагрузки, реализованно с помощью winapi
- T1550.002 – использование хешей паролей в процессе атаки