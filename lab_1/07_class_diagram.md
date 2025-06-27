# Диаграмма классов

```mermaid
classDiagram
    class User {
        -String id
        -String email
        -String password
        -String fullName
        -String username
        -String phoneNumber
        -Date registrationDate
        -UserStatus status
        -UserRole role
        +register()
        +login()
        +logout()
        +updateProfile()
        +changePassword()
        +uploadAvatar()
        +joinChat(chatId)
        +joinConference(conferenceId)
        +sendMessage(chatId, content)
        +viewMessageHistory(chatId)
        +getConferences()
        +getChats()
    }

    class Administrator {
        -String id
        -String userId
        -String[] permissions
        -Date assignedAt
        +blockUser(userId)
        +unblockUser(userId)
        +assignModerator(userId, entityId)
        +deleteMessage(messageId)
        +deleteChat(chatId)
        +createConference(params)
        +createChat(params)
        +viewLogs()
    }

    class VideoConference {
        -String id
        -String title
        -DateTime startTime
        -DateTime endTime
        -ConferenceStatus status
        -Boolean recordingAvailable
        -String accessCode
        +start()
        +end()
        +inviteUser(userId)
        +enableRecording()
        +disableRecording()
        +enableScreenSharing()
        +disableScreenSharing()
        +getParticipants()
        +getChat()
    }

    class Chat {
        -String id
        -String name
        -ChatType type
        -DateTime createdAt
        -ChatStatus status
        +addParticipant(userId)
        +removeParticipant(userId)
        +sendMessage(userId, content)
        +getMessages()
        +archive()
        +unarchive()
        +rename(name)
    }

    class Message {
        -String id
        -String senderId
        -String chatId
        -String content
        -DateTime timestamp
        -Boolean isEdited
        -Boolean isDeleted
        +edit(content)
        +delete()
        +getMetadata()
        +markAsRead()
        +attachFile(file)
    }

    class Profile {
        -String id
        -String userId
        -String settings
        -Boolean messageHistoryEnabled
        -DateTime lastLogin
        +updateSettings(settings)
        +viewHistory()
        +changeAvatar(image)
        +toggleMessageHistory(enabled)
        +getLastLogin()
    }

    class MessageHistory {
        -String id
        -String messageId
        -String userId
        -DateTime savedAt
        -Boolean isVisible
        +addMessage(messageId)
        +getUserHistory(userId)
        +hideMessage(messageId)
        +clearHistory(chatId)
    }

    class LogEvent {
        -String id
        -String userId
        -EventType eventType
        -DateTime timestamp
        -String description
        -String ipAddress
        +create(eventType, userId)
        +getUserEvents(userId)
        +getByType(type)
        +getByDateRange(start, end)
        +delete(eventId)
    }

    class ScreenSharing {
        -String id
        -String conferenceId
        -String presenterId
        -DateTime startTime
        -DateTime endTime
        -Boolean isActive
        +start(conferenceId, presenterId)
        +stop()
        +isActive()
        +getDuration()
        +changePresenter(newPresenterId)
    }

    class UserStatus {
        <<enumeration>>
        ACTIVE
        BLOCKED
        INACTIVE
    }

    class UserRole {
        <<enumeration>>
        USER
        ADMIN
    }

    class ChatStatus {
        <<enumeration>>
        ACTIVE
        ARCHIVED
    }

    class ConferenceStatus {
        <<enumeration>>
        PLANNED
        ONGOING
        COMPLETED
    }

    class ChatType {
        <<enumeration>>
        PUBLIC
        PRIVATE
        EMBEDDED
    }

    class EventType {
        <<enumeration>>
        LOGIN
        MESSAGE_SENT
        JOINED_CONFERENCE
        BLOCKED_USER
        STARTED_SCREENSHARE
    }

    %% Associations
    User "1" -- "1" Profile : owns
    User "1" -- "0..*" Message : sends
    User "1" -- "0..*" Chat : joins
    User "1" -- "0..*" VideoConference : participates in
    User "1" -- "0..*" LogEvent : creates
    Administrator "1" -- "1" User : extends
    Chat "1" -- "0..*" Message : contains
    Chat "1" -- "0..*" MessageHistory : logs
    VideoConference "1" -- "0..1" Chat : includes
    VideoConference "1" -- "0..*" ScreenSharing : has
```

# Описание классов

### User (Пользователь)
Представляет зарегистрированного пользователя системы. Пользователь может участвовать в чатах и видеоконференциях, отправлять сообщения и настраивать личный профиль.

### Administrator (Администратор)
Является специализированной ролью пользователя. Обладает правами управления чатами, конференциями, пользователями и логами системы.

### VideoConference (Видеоконференция)
Класс, описывающий удалённую видеовстречу. Содержит сведения о времени проведения, участниках, доступе, наличии записи и демонстрации экрана.

### Chat (Чат)
Текстовая группа для общения пользователей. Может существовать независимо или быть встроена в видеоконференцию. Включает список сообщений и участников.

### Message (Сообщение)
Единичное текстовое или мультимедийное сообщение, отправляемое в рамках определённого чата. Содержит данные об авторе, содержимом и времени отправки.

### Profile (Личный кабинет)
Хранит настройки пользователя, включая параметры интерфейса, возможность отображения истории сообщений и дату последнего входа.

### MessageHistory (История сообщений)
Архивная запись сообщений пользователя, может использоваться для просмотра, фильтрации или восстановления сообщений.

### LogEvent (Событие)
Фиксация действий пользователя в системе. Позволяет анализировать действия в рамках сессии: вход, отправка сообщений, модерация и т.п.

### ScreenSharing (Демонстрация экрана)
Сессия отображения экрана одного из участников в рамках видеоконференции. Содержит информацию о времени, статусе и ведущем.

### UserStatus (Статус пользователя)
Перечисление возможных состояний пользователя: `ACTIVE`, `BLOCKED`, `INACTIVE`.

### UserRole (Роль пользователя)
Перечисление ролей: обычный пользователь (`USER`) и администратор (`ADMIN`).

### ChatStatus (Статус чата)
Может быть активным (`ACTIVE`) или архивированным (`ARCHIVED`).

### ConferenceStatus (Статус конференции)
Отражает состояние видеоконференции: `PLANNED`, `ONGOING`, `COMPLETED`.

### ChatType (Тип чата)
Может быть публичным (`PUBLIC`), приватным (`PRIVATE`) или встроенным в конференцию (`EMBEDDED`).

### EventType (Тип события)
Фиксируемые действия в логах: вход, отправка сообщений, участие в конференции, блокировка, запуск демонстрации экрана.
