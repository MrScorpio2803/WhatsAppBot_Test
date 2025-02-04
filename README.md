# Документация к WhatsApp-боту

## Описание
Данный бот реализован для работы с напоминаниями (создание, редактирование, удаление, просмотр)

## Установка
1. Активация виртуального окружения
2. Установка необходимых зависимостей:
   ```sh
   pip install -r requirments.txt
   ```
3. Переход в директорию с ботом
4. Редактирование файла config.ini для настройки подключения к базе данных.
5. Запуск бота командой py main.py.

## Основные команды

### `/create [Время] [Текст] [Регулярность]`
Создает новое напоминание. При создании напоминанию присваивается статус: Активно и определяется принадлежность к группе all_tasks

**Пример:**
```
/create 08:00 Купить молоко day
```

### `/edit <num_reminder> [text=[Текст]] [time=[Время]] [group=[Категория]] [regular=[Регулярность]]`
Изменяет у переданного порядкового номера напоминания один или несколько полей.

**Пример:**
```
/edit text=Текст сообщения time=15:30 group=job regular=day
```

### `/list [Категория]`
Выводит список или всех напоминаний пользователя или напоминаний определенной категории.

### `/cancel [Номер]`
Отменяет напоминание по указанному номеру. Если номер не указан, отменяет последнее напоминание.

### `/help`
Выводит список доступных команд.

## Описание базы данных
Таблица `reminders` хранит напоминания и имеет следующие поля:
- `time` (VARCHAR(5)) — время отправки напоминания.
- `message` (TEXT) — текст напоминания.
- `sender` (VARCHAR(11)) — номер отправителя.
- `num_reminder` (INTEGER) — порядковый номер напоминания.
- `regular` (TEXT) — регулярность (`day`, `week`, `month`, `year`).
- `category` (TEXT) — категория напоминания (опционально).
- `status` (status_type) — статус (`active`, `inactive`).

## Запуск бота
Бот запускается в отдельном потоке вместе с фоновым планировщиком напоминаний. В случае неудачи в консоль выводится сообщение о невозможности подключения к базе данных
```python
if conn:
    scheduler_thread = threading.Thread(target=start_scheduler)
    scheduler_thread.start()
    bot_thread = threading.Thread(target=run_bot)
    bot_thread.start()
    bot_thread.join()
    scheduler_thread.join()
else:
    print("Не удалось подключиться к базе данных.")
```
