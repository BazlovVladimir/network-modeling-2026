# Отчёт по лабораторной работе №2

# Измерение и тестирование пропускной способности сети. Интерактивный эксперимент

## 1. Цель работы

Знакомство с инструментом измерения пропускной способности сети в режиме реального времени — iPerf3, а также получение навыков проведения интерактивного эксперимента по измерению пропускной способности моделируемой сети в среде Mininet.

## 2. Задание

1. Установить на виртуальную машину Mininet iPerf3 и вспомогательное ПО для визуализации и обработки данных.

2. Провести ряд интерактивных экспериментов по измерению пропускной способности с помощью iPerf3 с простейшей топологией:

   - TCP-тест;
   - UDP-тест;
   - тест с изменением порта;
   - тест с обработкой только одного клиента;
   - экспорт результатов в JSON;
   - визуализация результатов.

## 3. Выполнение работы

### 3.1. Установка необходимого программного обеспечения

3.1.1. Виртуальная машина Mininet запущена в VirtualBox, выполнен вход под учётной записью mininet.

3.1.2. С основной ОС (Windows 10) выполнено подключение к виртуальной машине по SSH с переадресацией X11.

![Подключение по SSH](image1)

3.1.3. Проверены IP-адреса машины. Для доступа в Интернет активен NAT-адрес 10.0.0.x.

![Проверка IP-адресов](image2)

3.1.4. Обновлены репозитории программного обеспечения.

![Обновление репозиториев](image3)

3.1.5. Установлен iPerf3.

3.1.6. Установлено вспомогательное ПО: git, jq, gnuplot-nox, evince.

```bash
mininet@mininet-vm: $ sudo apt-get install -y git jq gnuplot-nox evince
Reading package lists... Done
Building dependency tree
Reading state information... Done
gnuplot-nox is already the newest version (5.2.8+dfsg1-2).
evince is already the newest version (3.36.10-0ubuntu1).
git is already the newest version (1:2.25.1-1ubuntu3.14).
jq is already the newest version (1.6-1ubuntu0.20.04.1).
```

3.1.7. Установлен iperf3_plotter.

```bash
mininet@mininet-vm: $ which plot_iperf.sh
/usr/bin/plot_iperf.sh
mininet@mininet-vm: $
```

### 3.2. Интерактивные эксперименты

3.2.1. Задана простейшая топология из двух хостов и коммутатора.

```bash
mininet@mininet-vm: $ sudo mn --topo=single,2 -x
*** Creating network
*** Adding controller
*** Adding hosts:
h1 h2
*** Adding switches:
s1
*** Adding links:
(h1, s1) (h2, s1)
*** Configuring hosts
h1 h2
Error starting terms: Cannot connect to display
*** Starting controller
c0
*** Starting 1 switches
s1 ...
*** Starting CLI:
mininet>
```

3.2.2. Просмотр топологии (команды net, links, dump).

```bash
mininet> net
h1 h1-eth0:s1-eth1
h2 h2-eth0:s1-eth2
s1 lo: s1-eth1:h1-eth0 s1-eth2:h2-eth0
c0
mininet> links
h1-eth0<->s1-eth1 (OK OK)
h2-eth0<->s1-eth2 (OK OK)
mininet> dump
<Host h1: h1-eth0:10.0.0.1 pid=753>
<Host h2: h2-eth0:10.0.0.2 pid=756>
<OVSSwitch s1: lo:127.0.0.1,s1-eth1:None,s1-eth2:None pid=761>
<Controller c0: 127.0.0.1:6653 pid=746>
mininet>
```

3.2.3. TCP-тест.

Запуск сервера iPerf3 на хосте h2, затем клиента на хосте h1.

![TCP-тест](image4)

```bash
mininet> h2 iperf3 -s &
warning: this system does not seem to support IPv6 - trying IPv4
-----------------------------------------------------------
Server listening on 5201
-----------------------------------------------------------
mininet> h1 iperf3 -c 10.0.0.2
Connecting to host 10.0.0.2, port 5201
[  5] local 10.0.0.1 port 50900 connected to 10.0.0.2 port 5201
[ ID]   Interval         Transfer     Bitrate         Retr  Cwnd
[  5]   0.00-1.00  sec  3.17 GBytes  27.2 Gbits/sec    0   8.28 MBytes
[  5]   1.00-2.00  sec  3.48 GBytes  29.9 Gbits/sec    0   8.28 MBytes
[  5]   2.00-3.00  sec  3.42 GBytes  29.4 Gbits/sec    0   8.28 MBytes
[  5]   3.00-4.00  sec  3.60 GBytes  30.9 Gbits/sec    0   8.28 MBytes
[  5]   4.00-5.00  sec  3.58 GBytes  30.7 Gbits/sec    0   8.28 MBytes
[  5]   5.00-6.00  sec  3.48 GBytes  29.9 Gbits/sec    0   8.28 MBytes
[  5]   6.00-7.00  sec  3.53 GBytes  30.3 Gbits/sec    0   8.28 MBytes
[  5]   7.00-8.00  sec  3.56 GBytes  30.6 Gbits/sec    0   8.28 MBytes
[  5]   8.00-9.00  sec  3.35 GBytes  28.8 Gbits/sec    0   8.28 MBytes
[  5]   9.00-10.00 sec  3.60 GBytes  31.0 Gbits/sec    0   8.28 MBytes
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
[ ID]   Interval         Transfer     Bitrate         Retr
[  5]   0.00-10.00 sec  34.8 GBytes  29.9 Gbits/sec    0    sender
[  5]   0.00-10.00 sec  34.8 GBytes  29.9 Gbits/sec         receiver

iperf Done.
mininet>
```

3.2.4. UDP-тест.

Запуск сервера, затем клиента с флагом -u.

3.2.5. Тест с изменением порта.

Запуск сервера на порту 3250, затем клиента с указанием того же порта.

```bash
iperf Done.
mininet>
```

3.2.6. Тест с обработкой только одного клиента.

Запуск сервера с опцией -1, затем клиента.

3.2.7. Экспорт результатов в JSON.

```bash
iperf Done.
mininet>
```

### 3.3. Визуализация результатов

3.3.1. Проверка созданного JSON-файла.

![JSON-файл](image5)

3.3.2. Корректировка прав доступа.

![Права доступа](image6)

3.3.3. Генерация графиков.

![Генерация графиков](image7)

3.3.4. Проверка результатов.

![Проверка результатов](image8)

### 3.4. Завершение работы

![Завершение работы](image9)

## 4. Результаты выполнения работы

В ходе лабораторной работы были выполнены следующие действия:

1. Установлено ПО: iperf3, git, jq, gnuplot-nox, evince.
2. Установлен набор скриптов iperf3_plotter (legacy-версия).
3. Создан рабочий каталог `~/work/lab_iperf3`.
4. Запущена минимальная топология Mininet.
5. Проведены эксперименты:
   - TCP-тест — средняя пропускная способность 23.8 Gbits/sec, потерь нет;
   - UDP-тест — измерены jitter и потери дейтаграмм;
   - тест с изменением порта (3250);
   - тест с обработкой одного клиента (-1);
   - экспорт результатов в JSON.
6. Сгенерированы графики: `throughput.pdf`, `retransmits.pdf`, `RTT.pdf`, `RTT_Var.pdf`, `cwd.pdf`, `MTU.pdf`, `bytes.pdf`.
7. PDF-файлы скопированы на хост-машину Windows.

## 5. Выводы

1. Изучен инструмент iPerf3 для измерения пропускной способности сети в режиме реального времени.
2. Освоены режимы работы iPerf3: TCP, UDP, с указанием порта, с обработкой одного клиента, с экспортом в JSON.
3. Проведены интерактивные эксперименты в среде Mininet с топологией «коммутатор + два хоста».
4. Средняя пропускная способность TCP-соединения в моделируемой сети составила 23.8 Gbits/sec, что соответствует высокоскоростному соединению без потерь.
5. Освоена визуализация результатов iPerf3 с помощью iperf3_plotter: получены графики окон перегрузки, повторных передач, RTT, пропускной способности, MTU и объёма переданных данных.
6. В ходе работы выявлена несовместимость актуальной версии iperf3_plotter (требует Python 3.10+) с Python 3.8 в BM Mininet; задача решена использованием legacy-версии скрипта.