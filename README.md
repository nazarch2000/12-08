# Домашнее задание к занятию «Резервное копирование баз данных»

---

### Задание 1. Резервное копирование

### Кейс
Финансовая компания решила увеличить надёжность работы баз данных и их резервного копирования. 

Необходимо описать, какие варианты резервного копирования подходят в случаях: 

1.1. Необходимо восстанавливать данные в полном объёме за предыдущий день.
#### Можно делать бэкап каждый день в 22:00

1.2. Необходимо восстанавливать данные за час до предполагаемой поломки.
#### Каждый день в 22:00 делается фулл бэкап, а каждый час инкрементное. Или можно воспользоватся дифференциальным, он более надежный, но медленее чем инкрементное. -->

1.3.* Возможен ли кейс, когда при поломке базы происходило моментальное переключение на работающую или починенную базу данных.
#### master-slave репликация. Slave после поломки берет на себя работу как master.

---

### Задание 2. PostgreSQL

2.1. С помощью официальной документации приведите пример команды резервирования данных и восстановления БД (pgdump/pgrestore).

#### pg_dump <параметры> <имя базы> > <файл для сохранения копии> 

#### pg_restore <параметры> <имя базы> > <файл дампа>

2.1.* Возможно ли автоматизировать этот процесс? Если да, то как?
 
#### Можно настроить репликацию типа, master-slave, восстанавливать при этом ничего не придется, время будет сэкономлено.

#### Можно написать bash скрипт, который будет выполнятся на сервере с бд. Подключить какой-нибудь ftp, на котором будет файл с дампом или проще через ssh передать. 
```bash
#!/bin/sh

PATH=/etc:/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin
read dump_file
ssh_restore=nazar@192.168.0.10
PGPASSWORD=some_password
export PGPASSWORD
pathB=/vstfpd/db_dump
dbUser=nazar
database=sakila

pg_dump -U $dbUser $database > $pathB/$dump_file.sql
ssh $ssh_restore 'pg_restore -U $dbUser $database $pathB/$dump_file'
```

---

### Задание 3. MySQL

3.1. С помощью официальной документации приведите пример команды инкрементного резервного копирования базы данных MySQL. 

3.1.* В каких случаях использование реплики будет давать преимущество по сравнению с обычным резервным копированием?

#### Обеспечение непрерывного доступа к критически важным приложениям и приложениям, ориентированных на клиентов.

---

Задания, помеченные звёздочкой, — дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.
