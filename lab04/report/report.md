---
author: Владимир Базлов
babel-lang: russian
babel-otherlangs: english
biblatex: true
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other\\*
biblio-style: gost-numeric
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl
documentclass: scrreprt
figureTitle: Рис.
fontsize: 12pt
header-includes:
  - "`\\usepackage{indentfirst}`{=tex}"
  - "`\\usepackage{float}`{=tex}"
  - "`\\floatplacement{figure}{H}`{=tex}"
indent: true
lang: ru-RU
linestretch: 1.5
listingTitle: Листинг
lof: true
lofTitle: Список иллюстраций
lolTitle: Листинги
lot: true
lotTitle: Список таблиц
mainfont: IBM Plex Serif
mainfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
mathfont: STIX Two Math
monofont: IBM Plex Mono
monofontoptions: Scale=MatchLowercase,Scale=0.94,FakeStretch=0.9
papersize: a4
polyglossia-lang:
  name: russian
  options:
    - spelling=modern
    - babelshorthands=true
polyglossia-otherlangs:
  name: english
subtitle: Эмуляция и измерение задержек в глобальных сетях
tableTitle: Таблица
title: Отчёт по лабораторной работе №4
toc: true
toc-depth: 2
toc-title: Содержание
---

# Цель работы

Познакомиться с инструментом NETEM и научиться моделировать задержки и дрожание задержки (jitter) в виртуальной сети Mininet.

# Задание

Создать простую сеть из двух хостов и коммутатора, измерить время передачи пакетов, изменить параметры задержки с помощью NETEM, провести воспроизводимый эксперимент и построить график результатов.

# Выполнение работы

## 1. Запуск сети и проверка соединения

Запустим Mininet с двумя хостами и коммутатором. Проверим соединение между хостами командой `ping` и запишем значения RTT.

![Скриншот 1 — запуск сети и проверка соединения](img/01.png)

## 2. Добавление и изменение задержки

На интерфейсе хоста `h1` зададим задержку 100 мс и проверим результат с помощью `ping`.

```bash
sudo tc qdisc add dev h1-eth0 root netem delay 100ms
```

Затем зададим задержку 100 мс на `h2`, чтобы получить двунаправленную задержку, и повторим измерение. После этого изменим задержку на обоих интерфейсах до 50 мс.

![Скриншот 2 — добавление и изменение задержки](img/02.png)

## 3. Добавление джиттера и корреляции

Для моделирования изменяющейся задержки зададим среднюю задержку 100 мс со случайным отклонением 10 мс. Затем проверим вариант с корреляцией 25%.

![Скриншот 3 — добавление джиттера и корреляции](img/03.png)

## 4. Распределение задержки

Зададим нормальное распределение задержки со средним значением 100 мс и отклонением 20 мс. Проверим результат командой `ping`.

![Скриншот 4 — распределение задержки](img/04.png)

# Выводы

В ходе лабораторной работы была создана виртуальная сеть Mininet и проверено соединение между двумя хостами. С помощью NETEM были исследованы влияние фиксированной задержки, джиттера, корреляции и распределения задержки на время передачи пакетов. Также был рассмотрен способ автоматизации эксперимента и построения графика по результатам `ping`.
