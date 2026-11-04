# mephi-linux-security-2026

Домашнее задание №1 «Защита в ОС»
Бухгалтерский шифр: 373034
Выполнил: Бахматов Дмитрий Евгеньевич
Среда: виртуальная машина Essora_xlibre_SonicDE-20261021_1930 (Oracle VirtualBox), GNU/Linux (Debian).

## Как выполнено:

1. Создание пользователя. Группа students создана командой groupadd. Пользователь user1 с UID 1234 создан командой useradd -u 1234 -m -G students -s /bin/bash user1. Срок действия пароля ограничен тремя месяцами командой chage -M 90 user1. Проверено командами id user1 и chage -l user1; результаты — в файлах passwd.out, group.out, shadow.txt.

2. Поиск файлов с set-UID. Выполнен поиск командой find / -xdev -type f -perm -4000 -ls, вывод сохранён в файле setuid-files.out.

3. Мониторинг процессов. Вывод команды ps -eo euser,ruser,pid,args отфильтрован условием «эффективный UID равен 0, реальный — обычный пользователь»; продемонстрирована работа утилиты passwd в двух терминалах. Результат — в файле procmon.out.

4. Механизм set-UID. В каталог /home/user1/bin/ скопирована утилита cat, ей установлен set-UID бит командой chmod u+s. Показано, что user1 может прочитать /etc/shadow через ~/bin/cat. Подтверждение — в файле stat.out.

5. Механизм capabilities. В каталог /home/user1/bin/ скопирована утилита chown, ей назначена привилегия командой setcap cap_chown+ep. Показано, что user1 может сменить владельца файла, принадлежащего root. Вывод getcap сохранён в файле getcap.out.

6. Настройка sudo. Через visudo создан файл /etc/sudoers.d/user1-time со строкой: user1 ALL=(root) NOPASSWD: /usr/bin/date, /usr/bin/timedatectl, /usr/sbin/hwclock. Копия файла — в sudoers.out.

7. Артефакты. Сняты скриншот терминала с шифром студента и история команд; всё сохранено в файлах репозитория.

## Содержимое репозитория:

mephi-screenshot.png — скриншот терминала с шифром студента
history.out — история выполненных команд
stat.out — вывод stat для домашней директории и утилит (разделы 1, 3, 4)
getcap.out — вывод getcap для утилит (раздел 4)
passwd.out — содержимое /etc/passwd
group.out — содержимое /etc/group
shadow.txt — содержимое /etc/shadow с обезличенными хэшами паролей
sudoers.out — копия файла /etc/sudoers.d/user1-time (настройка sudo)
setuid-files.out — результат поиска set-UID файлов
procmon.out — вывод мониторинга процессов

Примечание: хэши паролей в shadow.txt обезличены перед публикацией, так как репозиторий публичный.
Все настройки сохраняются после перезагрузки.
