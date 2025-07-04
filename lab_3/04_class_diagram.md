
# Диаграмма классов

## Описание диаграммы

Диаграмма классов представляет собой визуальное отображение архитектуры системы видеосвязи и чатов. На диаграмме показаны основные сущности системы, их атрибуты, методы (если применимо), связи между классами, а также ограничения и обобщения.

## Границы системы

Система охватывает следующие функциональные области:
1. Управление пользователями
2. Обмен сообщениями в чатах
3. Видеоконференции и демонстрация экрана
4. Личный кабинет пользователя
5. Логирование действий и событий

## Классы и их атрибуты

### Пользователь
```
+ id: Integer
+ email: String
+ пароль: String
+ полное имя: String
+ никнейм: String
+ статус: Enum
+ роль: Enum
+ дата регистрации: Date
```

### Администратор (наследует Пользователь)
```
(расширяет возможности по управлению системой)
```

### Чат
```
+ id: Integer
+ название: String
+ тип: Enum
+ статус: Enum
+ дата создания: Date
```

### Сообщение
```
+ id: Integer
+ текст: String
+ дата и время отправки: DateTime
+ отредактировано: Boolean
+ удалено: Boolean
```

### Видеоконференция
```
+ id: Integer
+ тема: String
+ дата и время начала: DateTime
+ дата и время окончания: DateTime
+ статус: Enum
+ код доступа: String
```

### Демонстрация экрана
```
+ id: Integer
+ conferenceId: Integer
+ presenterId: Integer
+ активна: Boolean
+ начало: DateTime
+ окончание: DateTime
```

### Личный кабинет
```
+ id: Integer
+ настройки уведомлений: JSON
+ история сообщений включена: Boolean
+ дата последнего входа: DateTime
```

### Событие (лог)
```
+ id: Integer
+ пользователь: Integer
+ тип события: String
+ описание: String
+ дата и время: DateTime
+ IP-адрес: String
```

## Связи между классами

1. Пользователь (1) --- (1) Личный кабинет
2. Пользователь (1) --- (0..*) Чат (участие в чатах)
3. Чат (1) --- (0..*) Сообщение
4. Пользователь (1) --- (0..*) Сообщение (отправитель)
5. Пользователь (1) --- (0..*) Видеоконференция (участие)
6. Видеоконференция (1) --- (0..*) Демонстрация экрана
7. Пользователь (1) --- (0..*) Событие
8. Пользователь <|-- Администратор

## Ограничения

### Ограничения атрибутов
1. Email должен быть уникальным и валидным
2. Пароль — не менее 6 символов, хранится в хэшированном виде
3. Никнейм и полное имя не должны быть пустыми
4. Дата окончания конференции должна быть позже даты начала

### Ограничения связей
1. Чат не существует без создателя/участников
2. Сообщение не может существовать без чата и отправителя
3. Конференция требует хотя бы одного участника
4. Демонстрация экрана не может существовать без конференции
5. Личный кабинет не может существовать без пользователя
6. Событие (лог) должно быть связано с конкретным пользователем
