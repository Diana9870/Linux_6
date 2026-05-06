## Варіант A — Скрипт бекапу логів
```
diana@DESKTOP-E5EPJ14:/mnt/c/Users/HP EliteBook/Linux_6$ touch backup.sh
diana@DESKTOP-E5EPJ14:/mnt/c/Users/HP EliteBook/Linux_6$ chmod +x backup.sh
diana@DESKTOP-E5EPJ14:/mnt/c/Users/HP EliteBook/Linux_6$ mkdir logs_test backup_destination
diana@DESKTOP-E5EPJ14:/mnt/c/Users/HP EliteBook/Linux_6$ touch logs_test/file1.log logs_test/file2.log
diana@DESKTOP-E5EPJ14:/mnt/c/Users/HP EliteBook/Linux_6$ ./backup.sh ./logs_test ./backup_destination
Backup created: /mnt/c/Users/HP EliteBook/Linux_6/backup_destination/logs_backup_2026-05-06_10-00.tar.gz
```
### Коментар
Виконано завдання №6 варіант A — бекап логів. Скрипт backup.sh реалізований згідно з технічним завданням: додано перевірку аргументів, захист від паралельного запуску через лок-файл та архівацію з міткою часу, перевірки на існуючі директорії та лок-файл додані