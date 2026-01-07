# otus_less8
Администратор Linux. Professional

Задание

1) Включить отображение меню Grub.

2) Попасть в систему без пароля несколькими способами.

3) Установить систему с LVM, после чего переименовать VG.

1. Включить отображение меню Grub


Для отображения меню нужно отредактировать конфигурационный файл.

```
nano /etc/default/grub
```
Комментируем строку, скрывающую меню и ставим задержку для выбора пункта меню в 10 секунд.

<img width="850" height="253" alt="image" src="https://github.com/user-attachments/assets/9b0db37f-c0a1-45e8-bb25-69b693d710d8" />

Обновляем конфигурацию загрузчика и перезагружаемся для проверки.

```
update-grub
reboot
```
При загрузке в окне виртуальной машины мы должны увидеть меню загрузчика.

Не получилось с данными надстройками пришлось изменить на такие:

GRUB_TIMEOUT_STYLE=menu

После меню появилось:


<img width="629" height="477" alt="image" src="https://github.com/user-attachments/assets/3a99ea78-d023-4123-bc3e-e1727599f701" />

2. Попасть в систему без пароля несколькими способами

Для получения доступа необходимо открыть GUI VirtualBox (или другой системы виртуализации), запустить виртуальную машину и при выборе ядра для загрузки нажать e - в данном контексте edit. Попадаем в окно, где мы можем изменить параметры загрузки:


<img width="636" height="524" alt="image" src="https://github.com/user-attachments/assets/02480f47-152a-4e29-a2f3-a2017d5ab416" />


Способ 1. init=/bin/bash

В конце строки, начинающейся с linux, добавляем init=/bin/bash и нажимаем сtrl-x для загрузки в систему
В целом на этом все, Вы попали в систему. Но есть один нюанс. Рутовая файловая
система при этом монтируется в режиме Read-Only. Если вы хотите перемонтировать ее в режим Read-Write, можно воспользоваться командой:
root@ubuntu22:~# mount -o remount,rw /

После чего можно убедиться, записав данные в любой файл или прочитав вывод
команды:
root@ubuntu22:~# mount | grep root


<img width="725" height="438" alt="image" src="https://github.com/user-attachments/assets/35aa7dec-edb0-497a-9a9f-6e212fdcdf7c" />


Способ 2. Recovery mode

В меню загрузчика на первом уровне выбрать второй пункт (Advanced options…), далее загрузить пункт меню с указанием recovery mode в названии. 
Получим меню режима восстановления.

<img width="613" height="398" alt="image" src="https://github.com/user-attachments/assets/810f8924-ac9c-4153-afe3-b5a325ee7573" />

В этом меню сначала включаем поддержку сети (network) для того, чтобы файловая система перемонтировалась в режим read/write (либо это можно сделать вручную).
Далее выбираем пункт root и попадаем в консоль с пользователем root. Если вы ранее устанавливали пароль для пользователя root (по умолчанию его нет), то необходимо его ввести. 
В этой консоли можно производить любые манипуляции с системой.

<img width="701" height="411" alt="image" src="https://github.com/user-attachments/assets/19c322b8-ca1a-48f2-b970-99f520233eef" />


<img width="943" height="485" alt="image" src="https://github.com/user-attachments/assets/73beae26-6b7c-4be8-8941-2cdd4f7b4d62" />

3. Установить систему с LVM, после чего переименовать VG

Мы установили систему Ubuntu 22.04 со стандартной разбивкой диска с использованием  LVM.
Первым делом посмотрим текущее состояние системы (список Volume Group):

```
vgs
```
<img width="431" height="82" alt="image" src="https://github.com/user-attachments/assets/08e48280-cc12-443f-adc9-4080c806073a" />

Нас интересует вторая строка с именем Volume Group. Приступим к переименованию:

```
vgrename ubuntu-vg ubuntu-otus
```

<img width="617" height="173" alt="image" src="https://github.com/user-attachments/assets/d74e6d5c-d773-40e2-911c-9ef0b63b9b9a" />

Далее правим /boot/grub/grub.cfg. Везде заменяем старое название VG на новое (в файле дефис меняется на два дефиса ubuntu--vg ubuntu--otus).


<img width="1013" height="529" alt="image" src="https://github.com/user-attachments/assets/e8bf16a9-4ecf-401e-9607-5c7c5a6cc2c7" />

После чего можем перезагружаться и, если все сделано правильно, успешно грузимся с новым именем Volume Group и проверяем:


<img width="485" height="82" alt="image" src="https://github.com/user-attachments/assets/569d8440-5350-4345-bda4-de9e4a32518a" />


При желании можно так же заменить название Logical Volume.

