# Лабораторная работа: полномочия процессов и механизмы повышения rights
cat > ~/mephi-repo/README.md <<'EOF'
# Лабораторная работа: полномочия процессов и механизмы повышения прав

**Студент:** М265196

## 1. Пользователь user1
Создан пользователь user1 (`useradd`, `passwd`), добавлен в дополнительные группы.
Артефакты: `passwd.out` (getent passwd), `group.out` (getent group).

## 2. Процессы с эффективным UID = root
Запущен `passwd` от user1. Пока он ждал ввода пароля, собран список процессов,
у которых эффективный UID = root, а реальный UID — обычный пользователь:
`ps -eo euser,ruser,pid,args | awk '$1=="root" && $2!="root"'`.
В выводе: `root  user1  <PID>  passwd` — процесс с euid=root, ruid=user1.
Артефакт: `procmon.out`.

## 3. SUID-бит на копии cat
Скопирован /usr/bin/cat в /home/user1/bin/cat, владелец root:root,
права 4755 (SUID-бит): `-rwsr-xr-x`.
Демонстрация: обычный `cat /etc/shadow` от user1 — отказ (exit=1);
SUID-копия — успешное чтение (exit=0), выведены строки /etc/shadow.
Артефакты: `setuid-files.out`, `suid-demo.out`, `stat.out`.

## 4. Capability cap_chown
Скопирован /usr/bin/chown в /home/user1/bin/chown`, назначена capability:
`setcap cap_chown+ep /home/user1/bin/chown` (getcap подтверждает `cap_chown=ep`).
Демонстрация: файл /tmp/rootfile принадлежал root; от имени user1 запущена
копия chown с capability — владелец сменён на user1 (exit=0).
Артефакты: `cap-demo.out`, `getcap.out`.

## 5. sudo-правило для user1
Создано правило /etc/sudoers.d/user1-time (права 0440):
`user1 ALL=(root) NOPASSWD: /usr/bin/date, /usr/bin/timedatectl`
Синтаксис проверен `visudo -c`. От user1 выполнено `sudo -l` — показаны
только разрешённые команды с NOPASSWD; время сменено командой
`timedatectl set-time '20:15:00'` без запроса пароля.
Артефакты: `sudoers.out`, `sudo-demo.out`.

## 6. /etc/shadow
В `shadow.txt` — снимок /etc/shadow с хэшами паролей; сохранён в учебных
целях, репозиторий приватный.

## Скриншот
`mephi-screenshot.png` — содержит вывод демонстраций и идентификатор студента: М265196.
