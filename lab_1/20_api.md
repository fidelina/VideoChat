# API системы

## Базовый URL
```
https://api.videoconf-platform.com/v1
```

## Аутентификация
Все защищённые запросы должны содержать заголовок:
```
Authorization: Bearer <token>
```

---

## Пользователи

### Регистрация
```
POST /users/register
```
**Тело запроса:**
```json
{
  "email": "string",
  "password": "string",
  "fullName": "string"
}
```
**Ответ:**
```json
{
  "id": "integer",
  "email": "string",
  "fullName": "string",
  "status": "string"
}
```

### Авторизация
```
POST /users/login
```
**Тело запроса:**
```json
{
  "email": "string",
  "password": "string"
}
```
**Ответ:**
```json
{
  "token": "string",
  "expiresIn": "integer"
}
```

### Получение профиля
```
GET /users/me
```
**Ответ:**
```json
{
  "id": "integer",
  "email": "string",
  "fullName": "string",
  "username": "string",
  "status": "string"
}
```

### Обновление профиля
```
PUT /users/me
```
**Тело запроса:**
```json
{
  "fullName": "string",
  "username": "string"
}
```
**Ответ:** как в `GET /users/me`

---

## Чаты

### Создание чата
```
POST /chats
```
**Тело запроса:**
```json
{
  "name": "string",
  "type": "PUBLIC" | "PRIVATE"
}
```
**Ответ:**
```json
{
  "id": "integer",
  "name": "string",
  "type": "string",
  "status": "string"
}
```

### Отправка сообщения
```
POST /chats/{chatId}/messages
```
**Тело запроса:**
```json
{
  "content": "string"
}
```
**Ответ:**
```json
{
  "id": "integer",
  "chatId": "integer",
  "senderId": "integer",
  "content": "string",
  "timestamp": "string"
}
```

### Получение сообщений
```
GET /chats/{chatId}/messages?page=1&size=20
```
**Ответ:**
```json
{
  "items": [
    {
      "id": "integer",
      "senderId": "integer",
      "content": "string",
      "timestamp": "string"
    }
  ],
  "total": "integer"
}
```

---

## Видеоконференции

### Создание конференции
```
POST /conferences
```
**Тело запроса:**
```json
{
  "title": "string",
  "startTime": "ISO8601",
  "accessCode": "string"
}
```
**Ответ:**
```json
{
  "id": "integer",
  "title": "string",
  "status": "SCHEDULED"
}
```

### Присоединение к конференции
```
POST /conferences/{conferenceId}/join
```
**Тело запроса:**
```json
{
  "accessCode": "string"
}
```
**Ответ:**
```json
{
  "conferenceUrl": "wss://..."
}
```

---

## Демонстрация экрана

### Начать демонстрацию
```
POST /conferences/{conferenceId}/share-screen
```
**Ответ:**
```json
{
  "sessionId": "string",
  "presenterId": "integer",
  "startTime": "string"
}
```

### Остановить демонстрацию
```
POST /conferences/{conferenceId}/stop-screen
```
**Ответ:**
```json
{
  "sessionId": "string",
  "endTime": "string"
}
```

---

## Администрирование

### Блокировка пользователя
```
POST /admin/users/{userId}/block
```
**Ответ:**
```json
{
  "id": "integer",
  "status": "BLOCKED"
}
```

### Получение логов
```
GET /logs?userId=&eventType=&from=&to=&page=1&size=50
```
**Ответ:**
```json
{
  "items": [
    {
      "id": "integer",
      "userId": "integer",
      "eventType": "string",
      "description": "string",
      "timestamp": "string"
    }
  ],
  "total": "integer"
}
```

---

## Коды ошибок

### 400 Bad Request
- Неверный формат данных
- Пропущены обязательные поля

### 401 Unauthorized
- Отсутствует токен
- Неверный/просроченный токен

### 403 Forbidden
- Нет прав доступа
- Доступ к ресурсу запрещен

### 404 Not Found
- Ресурс не найден

### 409 Conflict
- Пользователь или чат уже существует
- Конфликт при присоединении

### 500 Internal Server Error
- Внутренняя ошибка сервера
- Ошибка подключения к БД или WebRTC