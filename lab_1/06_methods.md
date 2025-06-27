# Методы классов

## Пользователь
- `register()`: регистрация нового пользователя  
- `login()`: вход в систему  
- `logout()`: выход из системы  
- `updateProfile()`: обновление данных профиля  
- `changePassword()`: смена пароля  
- `uploadAvatar()`: загрузка фото профиля  
- `joinChat(chatId)`: присоединение к чату  
- `joinConference(conferenceId)`: участие в видеоконференции  
- `sendMessage(chatId, content)`: отправка сообщения в чат  
- `viewMessageHistory(chatId)`: просмотр истории сообщений  
- `getConferences()`: получение списка доступных видеоконференций  
- `getChats()`: получение списка чатов  

## Администратор
- `blockUser(userId)`: блокировка пользователя  
- `unblockUser(userId)`: разблокировка пользователя  
- `assignModerator(userId, entityId)`: назначение модератора на чат или конференцию  
- `deleteMessage(messageId)`: удаление сообщения  
- `deleteChat(chatId)`: удаление чата  
- `createConference(params)`: создание видеоконференции  
- `createChat(params)`: создание нового чата  
- `viewLogs()`: просмотр системных логов  

## Видеоконференция
- `start()`: запуск видеоконференции  
- `end()`: завершение видеоконференции  
- `inviteUser(userId)`: приглашение пользователя  
- `enableRecording()`: включение записи  
- `disableRecording()`: отключение записи  
- `enableScreenSharing()`: включение демонстрации экрана  
- `disableScreenSharing()`: отключение демонстрации экрана  
- `getParticipants()`: получение списка участников  
- `getChat()`: доступ к встроенному чату  

## Чат
- `addParticipant(userId)`: добавление участника в чат  
- `removeParticipant(userId)`: удаление участника из чата  
- `sendMessage(userId, content)`: отправка сообщения от имени пользователя  
- `getMessages()`: получение всех сообщений чата  
- `archive()`: архивирование чата  
- `unarchive()`: восстановление чата из архива  
- `rename(name)`: изменение названия чата  

## Сообщение
- `edit(content)`: редактирование текста сообщения  
- `delete()`: удаление сообщения  
- `getMetadata()`: получение информации о сообщении (время, автор и т.д.)  
- `markAsRead()`: отметить сообщение как прочитанное  
- `attachFile(file)`: прикрепление файла/медиа  

## Личный кабинет
- `updateSettings(settings)`: обновление настроек интерфейса  
- `viewHistory()`: просмотр истории активности и сообщений  
- `changeAvatar(image)`: обновление аватара  
- `toggleMessageHistory(enabled)`: включение/отключение истории сообщений  
- `getLastLogin()`: получение даты последнего входа  

## История сообщений
- `addMessage(messageId)`: добавление сообщения в историю  
- `getUserHistory(userId)`: получение истории сообщений пользователя  
- `hideMessage(messageId)`: скрытие сообщения из отображения  
- `clearHistory(chatId)`: очистка истории чата  

## Событие (лог)
- `create(eventType, userId)`: создание записи о событии  
- `getUserEvents(userId)`: получение событий пользователя  
- `getByType(type)`: фильтрация по типу событий  
- `getByDateRange(start, end)`: фильтрация по дате  
- `delete(eventId)`: удаление события (при необходимости)  

## Демонстрация экрана
- `start(conferenceId, presenterId)`: запуск демонстрации  
- `stop()`: завершение демонстрации  
- `isActive()`: проверка активности демонстрации  
- `getDuration()`: получение длительности сеанса  
- `changePresenter(newPresenterId)`: смена ведущего  
