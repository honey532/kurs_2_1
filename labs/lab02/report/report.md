---
## Front matter
title: "Управление пользователями и группами"
subtitle: "Лабораторная работа № 2"
author: "Глобин Никита Анатольевич"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: true # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: IBM Plex Serif
romanfont: IBM Plex Serif
sansfont: IBM Plex Sans
monofont: IBM Plex Mono
mathfont: STIX Two Math
mainfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
romanfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
sansfontoptions: Ligatures=Common,Ligatures=TeX,Scale=MatchLowercase,Scale=0.94
monofontoptions: Scale=MatchLowercase,Scale=0.94,FakeStretch=0.9
mathfontoptions:
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы

Получить представление о работе с учётными записями пользователей и группами пользователей в операционной системе типа Linux.

# Задание

1. Переключение учётных записей пользователей  
2. Создание учётных записей пользователей  
3. Работа с группами  
4. Контрольные вопросы  

# Теоретическое введение


# Выполнение лабораторной работы


## Переключение учётных записей пользователей



2. Войшли в систему как обычный пользователь и откроем терминал, узнаем какую учётную запись пользователя вы используете, введя команду whoami (рис. [-@fig:001]).

![001](image/1.png){#fig:001 width=70%}

Выведем на экран более подробную информацию, используя команду id (рис. [-@fig:002])

![002](image/2.png){#fig:002 width=70%}

3. Используем команду su для переключения к учётной записи root. При запросе пароля введем пароль пользователя root.(рис. [-@fig:003])

![003](image/3.png){#fig:003 width=70%}

мы видем что id пользователя root 0 потому что это зарезервированый номер. то же и с группой. так же мы видем что срок для пороля безграничный (рис. [-@fig:004])

![004](image/4.png){#fig:004 width=70%}


4. вернем к учётной записи своего пользователя: su имя_пользователя (рис. [-@fig:005])

![005](image/5.png){#fig:005 width=70%}


5. просмотрим в безопасном режиме файл /etc/sudoers, используя, например, sudo -i visudo (рис. [-@fig:006])

![006](image/6.png){#fig:006 width=70%}

6. Убедимся, что в открытом с помощью visudo файле присутствует строка %wheel ALL=(ALL) ALL (рис. [-@fig:007])

![007](image/7.png){#fig:007 width=70%}

Группа wheel используется для:
* Централизованного управления доступом к sudo: только участники группы могут выполнять команды с повышенными правами.
* Повышения безопасности: можно легко ограничить или предоставить доступ к административным правам.
* Соответствия практике безопасного администрирования: root-доступ предоставляется не напрямую, а через sudo, с логированием действий.

7. СоздаЕМ пользователя alice, входящего в группу wheel: sudo -i useradd -G wheel alice (рис. [-@fig:008])

![008](image/8.png){#fig:008 width=70%}


8. Проверяем, что пользователь alice добавлен в группу wheel, введя
id alice (рис. [-@fig:009])

![009](image/9.png){#fig:009 width=70%}


9. Задаем пароль для пользователя alice, набрав sudo -i passwd alice (рис. [-@fig:010])


![010](image/10.png){#fig:010 width=70%}

10. Переключимся на учётную запись пользователя alice: su alice (рис. [-@fig:011])

![011](image/11.png){#fig:011 width=70%}

11. Создаем пользователя bob: sudo useradd bob (рис. [-@fig:012])

![012](image/12.png){#fig:012 width=70%}


12. Установим пароль для пользователя bob: sudo passwd bob (рис. [-@fig:013])

![013](image/13.png){#fig:013 width=70%}


13. Просмотрим, в какие группы входит пользователь bob: id bob (рис. [-@fig:014])

![014](image/14.png){#fig:014 width=70%}

![015](image/15.png){#fig:015 width=70%}


## Создание учётных записей пользователей

1. Переключимся в терминале на учётную запись пользователя root: su (рис. [-@fig:016])

![016](image/16.png){#fig:016 width=70%}

2. Откроем файл конфигурации /etc/login.defs для редактирования, используя, например, vim (не забудьте, что требуются полномочия пользователя root): sudo vim /etc/login.defs
Измените несколько параметров. Например, найдите параметр
CREATE_HOME
и убедитесь, что он установлен в значение yes. Также установите параметр
USERGROUPS_ENAB no
Это позволит не добавлять нового пользователя в группу с тем же именем, что и поль-
зователь, а использовать группу users.(рис. [-@fig:017]) (рис. [-@fig:018]) (рис. [-@fig:019])

![017](image/17.png){#fig:017 width=70%}

![018](image/18.png){#fig:18 width=70%}

![019](image/19.png){#fig:019 width=70%}


3. Перейдем в каталог /etc/skel:
cd /etc/skel
Создайте каталоги Pictures и Documents:
mkdir Pictures
и
mkdir Documents
Это позволит добавить эти каталоги по умолчанию во все домашние каталоги пользо-
вателей.(рис. [-@fig:022])

![022](image/22.png){#fig:022 width=70%}


4. Изменим содержимое файла .bashrc, добавив строку export EDITOR=/usr/bin/mceditor (рис. [-@fig:023]) и (рис. [-@fig:024])

![023](image/23.png){#fig:023 width=70%}

![024](image/24.png){#fig:024 width=70%}

5. Переключем в терминале на учётную запись пользователя alice: su alice (рис. [-@fig:025])

![025](image/25.png){#fig:025 width=70%}


6. Используя утилиту useradd, создадим пользователя carol:
sudo -i useradd carol (рис. [-@fig:026])

![026](image/26.png){#fig:026 width=70%}


7. Установим пароль для пользователя carol:
sudo passwd carol (рис. [-@fig:027])

![027](image/27.png){#fig:027 width=70%}


8. совершим ряд действий (рис. [-@fig:028]) 

* Пользователь carol имеет UID 1002 и входит в группу с GID 100 под именем users. Это соответствует тому, что параметр USERGROUPS_ENAB был установлен в no — в этом случае не создаётся отдельная группа carol, а пользователь добавляется в общую группу users.

* Каталоги Documents и Pictures присутствуют в домашнем каталоге пользователя carol. Это подтверждает, что шаблонные директории, созданные в /etc/skel, автоматически копируются при создании новой учётной записи. Это удобно для стандартизации структуры домашнего каталога пользователей.

![028](image/28.png){#fig:028 width=70%}


9. Переключимся в терминале на учётную запись пользователя alice:
su alice (рис. [-@fig:029])

![029](image/29.png){#fig:029 width=70%}


10. Файл /etc/shadow содержит зашифрованные пароли и параметры политики паролей для пользователей системы. Каждая строка содержит 9 полей, разделённых двоеточиями (рис. [-@fig:030])

![030](image/30.png){#fig:030 width=70%}

Мы видем зашефрованый пароль, полшьзователя, время которое ещё будет действоват пароль, время с момента создания пароля и время за сколько всплывет напоминани о смене пророля.

11. Изменим свойства пароля пользователя carol следующим образом:
sudo passwd -n 30 -w 3 -x 90 carol (рис. [-@fig:031])

![031](image/31.png){#fig:031 width=70%}


12. Убедимся в изменении в строке с данными о пароле пользователя carol в файле
/etc/shadow:
sudo cat /etc/shadow | grep carol (рис. [-@fig:032]) (рис. [-@fig:033]) (рис. [-@fig:034])

![032](image/32.png){#fig:032 width=70%}

проверяем, что идентификатор alice существует во всех трёх файлах

![033](image/33.png){#fig:033 width=70%}

Убедимся, что идентификатор carol существует не во всех трёх файлах

![034](image/34.png){#fig:034 width=70%}

## Работа с группами

1. Находясь под учётной записью пользователя alice, создаем группы main и third:
sudo groupadd main
sudo groupadd third (рис. [-@fig:035])

![035](image/35.png){#fig:035 width=70%}


2. Используем usermod для добавления пользователей alice и bob в группу main,
а carol, dan, dave и david — в группу third:
sudo usermod -aG main alice
sudo usermod -aG main bob
sudo usermod -aG third caro (рис. [-@fig:036])

![036](image/36.png){#fig:036 width=70%}

3. Убедимся, что пользователь carol правильно добавлен в группу third:
id carol
Пользователю carol должна быть назначена основная группа с идентификатором
gid = 100 (users). Определите, в какие вторичные группы входит carol
(рис. [-@fig:037])

![037](image/37.png){#fig:037 width=70%}


4. Определимм, участниками каких групп являются другие созданные вами пользователи. (рис. [-@fig:037])

* bob в bob, third, main
* alice в alice, main, wheel
* carol в users, third


## Контрольные вопросы

1. При помощи каких команд можно получить информацию о номере (идентификаторе), назначенном пользователю Linux, и о группах, в которые он включён?

* Команда id — показывает UID, GID и список групп пользователя
* Команда groups — показывает, в какие группы входит пользователь


2. Какой UID имеет пользователь root? Как узнать UID пользователя?

Пользователь root всегда имеет UID 0.

Чтобы узнать UID текущего пользователя: whoami


3. В чём состоит различие между командами su и sudo?

* su Переключает на другого пользователя, запрашивая его пароль Полностью меняет окружение на другого пользователя

* sudo Выполняет одну команду от имени другого пользователя, но запрашивает ваш собственный пароль


4. В каком конфигурационном файле определяются параметры sudo?

Основной конфигурационный файл sudo — это:/etc/sudoers


5. Какую команду следует использовать для безопасного изменения конфигурации sudo?

Для безопасного редактирования файла /etc/sudoers следует использовать команду: visudo


6. Если вы хотите предоставить пользователю доступ ко всем командам администрирования системы через sudo, членом какой группы он должен быть?

Пользователь должен быть добавлен в группу wheel 


## Выводы 

мы научились создовать и упровлять пользователями. так же научились создовать группы и добовлять в них пользователей.

































# Выводы

Здесь кратко описываются итоги проделанной работы.

# Список литературы{.unnumbered}

::: {#refs}
:::
