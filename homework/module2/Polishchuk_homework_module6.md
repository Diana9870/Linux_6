## Варіант A — Скрипт бекапу логів
```
diana@DESKTOP-E5EPJ14:/mnt/c/Users/HP EliteBook/Linux_6$ touch backup.sh
diana@DESKTOP-E5EPJ14:/mnt/c/Users/HP EliteBook/Linux_6$ chmod +x backup.sh
diana@DESKTOP-E5EPJ14:/mnt/c/Users/HP EliteBook/Linux_6$ mkdir logs_test backup_destination
diana@DESKTOP-E5EPJ14:/mnt/c/Users/HP EliteBook/Linux_6$ touch logs_test/file1.log logs_test/file2.log
diana@DESKTOP-E5EPJ14:/mnt/c/Users/HP EliteBook/Linux_6$ ./backup.sh ./logs_test ./backup_destination
Backup created: /mnt/c/Users/HP EliteBook/Linux_6/backup_destination/logs_backup_2026-05-06_10-00.tar.gz
```

## Скрипт backup.sh
```
# 1. Оголошення змінних для аргументів та лок-файлу
LOG_DIR=$1
BACKUP_DIR=$2
LOCK_FILE="/tmp/backup.lock"
TIMESTAMP=$(date +"%Y-%m-%d_%H-%M")
ARCHIVE_NAME="logs_backup_${TIMESTAMP}.tar.gz"

# 2. Перевірка кількості аргументів (перевіряємо, чи передано рівно 2 параметри)
if [ "$#" -ne 2 ]; then
    echo "Usage: ./backup.sh <log_dir> <backup_dir>"
    exit 1
fi

# 3. Перевірка існування каталогів (-d перевіряє, чи є шлях існуючою директорією)
if [ ! -d "$LOG_DIR" ] || [ ! -d "$BACKUP_DIR" ]; then
    echo "Usage: ./backup.sh <log_dir> <backup_dir>"
    exit 1
fi

# 4. Захист від паралельного запуску (якщо lock-файл вже існує, скрипт припиняє роботу)
if [ -f "$LOCK_FILE" ]; then
    echo "Backup already running"
    exit 1
fi

# Створюємо lock-файл перед початком роботи
touch "$LOCK_FILE"

# Функція для видалення lock-файлу при виході (навіть якщо сталася помилка)
trap 'rm -f "$LOCK_FILE"' EXIT

# 5. Створення архіву (tar -czf: c (create), z (gzip), f (file))
tar -czf "${BACKUP_DIR}/${ARCHIVE_NAME}" -C "$LOG_DIR" . 2>/dev/null

# 6. Перевірка результату архівації
if [ $? -eq 0 ]; then
    FULL_PATH=$(realpath "${BACKUP_DIR}/${ARCHIVE_NAME}")
    echo "Backup created: $FULL_PATH"
else
    echo "Backup failed"
    exit 2
fi

# Lock-файл буде видалено автоматично завдяки команді trap '...' EXIT
```


### Коментар
Виконано завдання №6 варіант A — бекап логів. Скрипт backup.sh реалізований згідно з технічним завданням: додано перевірку аргументів, захист від паралельного запуску через лок-файл та архівацію з міткою часу, перевірки на існуючі директорії та лок-файл додані