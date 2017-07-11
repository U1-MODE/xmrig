# XMRig
XMRig - высокопроизводительный майнер Minero (XMR) с официальной поддержкой Windows.
На основе cpuminer-multi с большой оптимизацией/переписыванием и удалением большого количества устаревшего кода.

<img src="https://i.imgur.com/OXoB10D.png" width="628" >

#### Список контента
* [Функции](#features)
* [Загрузить](#download)
* [Использование](#usage)
* [Взможные алгоритмы](#algorithm-variations)
* [Создание](#build)
* [Общие вопросы](#common-issues)
* [Прочая информация](#other-information)
* [Пожертвы](#donations)
* [Контакт](#contacts)

## Build
1) Загрузите [MSYS2](http://www.msys2.org/)
2) Откройте MSYS2 MINGW
3) Вставьте:
```
pacman -Sy
pacman -S mingw-w64-x86_64-gcc
pacman -S make
pacman -S mingw-w64-x86_64-cmake
pacman -S mingw-w64-x86_64-pkg-config
```
4) Закройте и откройте MSYS
5) Выполните:
```
cd "путь к папке с проектом"
mkdir build
cd build
cmake .. -G "Unix Makefiles" -DCMAKE_BUILD_TYPE=Release -DUV_INCLUDE_DIR="путь к папке с проектом\libuv\include" -DUV_LIBRARY="c:\путь к папке с проектом\libuv\lib\x64\libuv.a"
make
```

## Функции
* Высокая производительность (290+ H/s on i7 6700).
* Оффициальная поддержка Windows.
* Малый вес файла без зависимостей.
* Поддержка резервного (failover) майнинг сервера.
* Поддержка keepalived.
* Консольные опции
* Поддержка CryptoNight-Lite для AEON.
* Автоматическая конфигурация CPU [CPU configuration](https://github.com/xmrig/xmrig/wiki/Threads).
* Поддержка Nicehash
* Это приложение с открытым исходным кодом.

## Загрузка
* Загрузить релиз: https://github.com/xmrig/xmrig/releases
* Ветка: https://github.com/xmrig/xmrig.git
  * Кслонировать через `git clone https://github.com/xmrig/xmrig.git`

## Использование
### Практический пример
```
xmrig.exe -o xmr-eu.dwarfpool.com:8005 -u YOUR_WALLET -p x -k
```

### Failover
```
xmrig.exe -o pool.supportxmr.com:5555 -u YOUR_WALLET1 -k -o xmr-eu.dwarfpool.com:8005 -u YOUR_WALLET2 -p x -k
```
Для перехода на другой ресурс вы можете добавить несколько пулов, максимальное количество которых не ограничено.

### Опции
```
  -a, --algo=ALGO       cryptonight (по-умолчанию) или cryptonight-lite
  -o, --url=URL         URL пулла для майнинга
  -O, --userpass=U:P    username:password пара для авторизации на сервере
  -u, --user=USERNAME   имя пользователея на сервере
  -p, --pass=PASSWORD   пароль пользователя на сервере
  -t, --threads=N       количество использования ядер
  -v, --av=N            изменение алгоритма, 0 автовыбор
  -k, --keepalive       отправка keepalived для предотвращения таймаута (нужна поддержка пула)
  -r, --retries=N      	количество попыток повтора перед переключением на резервный сервер (по умолчанию: 5)
  -R, --retry-pause=N   время паузы между попытками (по умолчанию: 5)
      --cpu-affinity    установить сродство процесса к ядру ядра процессора, маску 0x3 для ядер 0 и 1
      --no-color        выключить разноцветный лог
      --donate-level=N  уровень пожертвования, по умолчанию 5% (5 минут работы на автора за 100 минут)
  -B, --background      запустить майнер скрыто
  -c, --config=FILE     загрузить файл конфигурации в формате JSON
      --max-cpu-usage=N максимальная нагрузка процессора в автоматическом режиме (default 75)
      --safe            безопасная настройка потоков и параметров av для текущего процессора
      --nicehash        включить поддержку nicehash 
      --print-time=N    видеть статистику хешрейта каждые N секунд
  -h, --help            увидеть этот тект и выйти
  -V, --version         вывести информацию и выйти
```

## Вариации алгоритмов
От версии 0.8.0.
* `--av=1` Для CPUs с поддержкой AES.
* `--av = 2` Режим пониженной мощности (двойной хеш)` 1`.
* `--av = 3` Реализация программного обеспечения AES.
* `--av = 4` Режим пониженной мощности (двойной хэш)` 3`.

## Общие вопросы
### HUGE PAGES недоступны
* Запусти XMRig от имени Администратора
* С версии 0.8.0 XMRig автоматически включает SeLockMemoryPrivilege для текущего пользователя, но нужна перезагрузка. [Manual instruction](https://msdn.microsoft.com/en-gb/library/ms190730.aspx).

## Прочая информация
* Нет поддержки HTTP, только stratum протокол.
* Нет поддержки TLS.
* Пожертвование отключено по-умолчанию


### CPU майнинг таблица
* **i7-6700** - 290+ H/s (4 потока, cpu affinity 0xAA)
* **Dual E5620** - 377 H/s (12 потока, cpu affinity 0xEEEE)

Обратите внимание, что производительность сильно зависит от загрузки системы. Вышеприведенные числа получены на незанятой системе. Задачи, интенсивно использующие кеш процессора, такие как воспроизведение видео, могут значительно ухудшить хэш-скорость. Оптимальное количество потоков зависит от размера кеша L3 процессора, для 1 потока требуется 2 МБ кэша.

### Максимальный контрольный список производительности
* Бездействующая операционная система.
* Не превышайте оптимальное количество потоков.
* Используйте современные процессоры с набором инструкций AES-NI.
* Попробуйте настроить оптимальное слияние.
* Включение быстрой памяти.

## Пожертвы
* Автору
  * XMR: `48edfHu7V9Z84YzzMa6fUueoELZ9ZRXq9VetWzYGzKt52XU5xvqgzYnDK9URnRoJMk1j8nLwEVsaSWJ4fhdUyZijBGUicoD`
  * BTC: `1P7ujsXeX7GxQwHNnJsRMgAdNkFZmNVqJT`
* Мне
  * BTC: ` `

## Связь с автором
* support@xmrig.com
* [reddit](https://www.reddit.com/user/XMRig/)
