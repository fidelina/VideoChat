# База данных

## Схема базы данных

```sql
-- Таблица пользователей
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    full_name VARCHAR(255),
    username VARCHAR(100) UNIQUE,
    phone_number VARCHAR(20),
    registration_date TIMESTAMP NOT NULL,
    status VARCHAR(20) NOT NULL,
    role VARCHAR(20) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Таблица личных кабинетов
CREATE TABLE profiles (
    id SERIAL PRIMARY KEY,
    user_id INTEGER NOT NULL REFERENCES users(id),
    settings JSONB,
    message_history_enabled BOOLEAN DEFAULT TRUE,
    last_login TIMESTAMP,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Таблица чатов
CREATE TABLE chats (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255),
    type VARCHAR(20) NOT NULL,
    status VARCHAR(20) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Таблица сообщений
CREATE TABLE messages (
    id SERIAL PRIMARY KEY,
    sender_id INTEGER NOT NULL REFERENCES users(id),
    chat_id INTEGER NOT NULL REFERENCES chats(id),
    content TEXT,
    timestamp TIMESTAMP NOT NULL,
    is_edited BOOLEAN DEFAULT FALSE,
    is_deleted BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Таблица видеоконференций
CREATE TABLE conferences (
    id SERIAL PRIMARY KEY,
    title VARCHAR(255),
    start_time TIMESTAMP,
    end_time TIMESTAMP,
    status VARCHAR(20),
    recording_available BOOLEAN DEFAULT FALSE,
    access_code VARCHAR(50),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Таблица демонстраций экрана
CREATE TABLE screen_shares (
    id SERIAL PRIMARY KEY,
    conference_id INTEGER NOT NULL REFERENCES conferences(id),
    presenter_id INTEGER NOT NULL REFERENCES users(id),
    start_time TIMESTAMP,
    end_time TIMESTAMP,
    is_active BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Таблица логов событий
CREATE TABLE logs (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    event_type VARCHAR(50),
    description TEXT,
    timestamp TIMESTAMP NOT NULL,
    ip_address VARCHAR(45),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

## Индексы
```sql
-- Индексы пользователей
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_status ON users(status);
CREATE INDEX idx_users_role ON users(role);

-- Индексы чатов
CREATE INDEX idx_chats_type ON chats(type);
CREATE INDEX idx_chats_status ON chats(status);

-- Индексы сообщений
CREATE INDEX idx_messages_chat_id ON messages(chat_id);
CREATE INDEX idx_messages_sender_id ON messages(sender_id);
CREATE INDEX idx_messages_timestamp ON messages(timestamp);

-- Индексы конференций
CREATE INDEX idx_conferences_status ON conferences(status);
CREATE INDEX idx_conferences_start_time ON conferences(start_time);

-- Индексы логов
CREATE INDEX idx_logs_user_id ON logs(user_id);
CREATE INDEX idx_logs_event_type ON logs(event_type);
```

## Ограничения
```sql
-- Ограничения пользователей
ALTER TABLE users
    ADD CONSTRAINT chk_users_status CHECK (status IN ('ACTIVE', 'BLOCKED', 'INACTIVE')),
    ADD CONSTRAINT chk_users_role CHECK (role IN ('USER', 'ADMIN'));

-- Ограничения чатов
ALTER TABLE chats
    ADD CONSTRAINT chk_chats_type CHECK (type IN ('PUBLIC', 'PRIVATE', 'EMBEDDED')),
    ADD CONSTRAINT chk_chats_status CHECK (status IN ('ACTIVE', 'ARCHIVED'));

-- Ограничения конференций
ALTER TABLE conferences
    ADD CONSTRAINT chk_conferences_status CHECK (status IN ('SCHEDULED', 'IN_PROGRESS', 'COMPLETED'));

-- Ограничения сообщений
ALTER TABLE messages
    ADD CONSTRAINT chk_messages_deleted CHECK (is_deleted IN (TRUE, FALSE));

-- Ограничения логов
ALTER TABLE logs
    ADD CONSTRAINT chk_logs_event_type CHECK (event_type IN (
        'LOGIN', 'LOGOUT', 'MESSAGE_SENT', 'CONFERENCE_JOINED', 'USER_BLOCKED', 'SCREEN_SHARED'
    ));

```

## Триггеры
```sql
-- Функция обновления поля updated_at
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = CURRENT_TIMESTAMP;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Триггеры
CREATE TRIGGER trg_users_updated_at
    BEFORE UPDATE ON users
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER trg_profiles_updated_at
    BEFORE UPDATE ON profiles
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER trg_chats_updated_at
    BEFORE UPDATE ON chats
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER trg_messages_updated_at
    BEFORE UPDATE ON messages
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER trg_conferences_updated_at
    BEFORE UPDATE ON conferences
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER trg_screen_shares_updated_at
    BEFORE UPDATE ON screen_shares
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

```