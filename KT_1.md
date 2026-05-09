# 💬 ChatHub — Современный мессенджер

    💭 Личные чаты · Группы · Каналы · Реакции · Редактирование сообщений

📦 Стек технологий

Frontend	⚛️ React 18, TypeScript, React Router v6, Zustand, TailwindCSS, React Hook Form, Axios

Backend	🟢 Node.js, Fastify, JWT, Socket.IO

Database	🐘 PostgreSQL 15 + Redis (сессии, кэш)

Инфраструктура	🐳 Docker, Nginx

Документация	📚 Swagger / OpenAPI

Тесты	🧪 Jest, Supertest (backend), React Testing Library (frontend)

Файлы	☁️ Cloudinary / S3 (изображения, вложения)

📋 Описание проекта

💡 Идея

Современные мессенджеры либо перегружены функциями, либо слишком просты. ChatHub — идеальный баланс: удобный чат с возможностью редактировать сообщения, ставить реакции (👍❤️😂😮😢😡), создавать группы с настраиваемыми ролями и публичные каналы для постов. Всё в реальном времени через WebSocket.

🎯 Целевая аудитория

    👤 Обычные пользователи — хотят общаться в личных и групповых чатах с возможностью исправлять ошибки в сообщениях

    👥 Команды и сообщества — нуждаются в групповых чатах с администрированием

    📢 Блогеры / компании — хотят вести публичные каналы с подписчиками

🖥 Ключевые экраны и функциональность

Для всех пользователей (роль: user)

Экран	Что можно делать

🗂️ Список диалогов	Список всех чатов (личные, группы, каналы). Сортировка по последнему сообщению. Непрочитанные выделены 🔴

💬 Личный чат	Отправка/получение сообщений в realtime. Редактирование ✏️, удаление 🗑️, реакции 😍. Поддержка emoji и вложений 📎

👥 Групповой чат	То же + просмотр участников. Для админов: добавление/удаление, назначение модераторов, смена названия/аватара

📣 Канал	Админ/владелец постит сообщения. Подписчики только читают и ставят реакции (без права писать)

👤 Профиль	Поддержка нескольких аватаров, статус "онлайн/оффлайн" 🟢/⚫, биография, список общих чатов

Для админов группы (роль: group_admin)

Экран	Что можно делать

⚙️ Управление группой	Изменение названия, аватара, описания. Приглашение по ссылке 🔗. Назначение/снятие модераторов. Удаление сообщений любого участника

Для владельца канала (роль: channel_owner)

Экран	Что можно делать

📺 Управление каналом	Постинг. Настройка оформления. Просмотр статистики (просмотры, реакции). Назначение со-админов

Для глобального администратора (роль: admin)

Экран	Что можно делать

🛡️ Admin-панель	Блокировка пользователей 🚫, удаление чатов/каналов, просмотр жалоб, модерация контента

✨ Уникальные фичи

#	Фича	Описание

1	✏️ Редактирование сообщений	Исправляй ошибки даже после отправки

2	😍 Реакции	6+ стандартных реакций

3	🗑️ Удаление везде	"Удалить у всех" в личных чатах (как в Telegram)

4	⚡ Real-time синхронизация	Socket.IO — сообщения приходят мгновенно

5	⌨️ Статус "печатает..."	Видно, когда собеседник набирает текст

6	🔴 Непрочитанные	Счетчики непрочитанных сообщений в списке чатов

7	🔍 Поиск	По сообщениям (в пределах одного чата или глобально)

🗂 Сущности системы

Сущность	Ключевые поля

👤 User	id, email, username, password_hash, bio, last_seen, is_online

👤 Avatar	id, user_id, avatar_url, created_at, is_primary

🔐 Session id, user_id, refresh_token, fingerprint, expires_at, created_at

💬 Chat	id, type (private, group, channel), title, avatar_url, created_at

💬 ChannelSettings, id, chat_id, description, is_private

👥 ChatParticipant	id, chat_id, user_id, role (member, moderator, admin, owner), joined_at, last_read_message_id

💌 Message	id, chat_id, sender_id, text, reply_to_id, edited_at, is_deleted, created_at

😍 MessageReaction	id, message_id, user_id, emoji, created_at

📎 Attachment	id, message_id, file_url, file_type, file_name, file_size

🔗 InviteLink	id, chat_id, token, expires_at

🚨 Report	id, message_id, reporter_id, reason, status (pending, resolved, rejected)

🗃 ER-диаграмма

<img width="1133" height="947" alt="изображение" src="https://github.com/user-attachments/assets/b9750b26-9306-4bbf-9c80-20b5ba70db03" />

🏗 Архитектура системы

<img width="493" height="1080" alt="изображение" src="https://github.com/user-attachments/assets/7fa73cf1-4a46-4152-b3be-a9e868025a94" />

🔄 Real-time события (Socket.IO)

📤 Клиент → Сервер

Событие	Описание

send_message	Отправить сообщение в чат

edit_message	Отредактировать сообщение

delete_message	Удалить сообщение

add_reaction	Поставить реакцию

remove_reaction	Убрать реакцию

typing_start	Пользователь начал печатать

typing_stop	Перестал печатать

mark_read	Отметить сообщения как прочитанные

📥 Сервер → Клиент

Событие	Описание

new_message	Новое сообщение в чате

message_edited	Сообщение отредактировано

message_deleted	Сообщение удалено

reaction_updated	Обновлены реакции

user_typing	Кто-то печатает в чате

read_receipt	Кто-то прочитал сообщения

👤 User Stories

📌 Как пользователь, я хочу редактировать отправленные сообщения,

   чтобы исправлять опечатки.

📌 Как пользователь, я хочу ставить реакции на сообщения (❤️😂😮),

   чтобы быстро выражать эмоции без лишних сообщений.

📌 Как пользователь, я хочу удалить сообщение "у всех" в личном чате,

   если отправил не туда.

📌 Как администратор группы, я хочу удалить неуместное сообщение

   любого участника.

📌 Как владелец канала, я хочу публиковать посты и видеть,

   сколько реакций они собрали.

📌 Как пользователь, я хочу видеть, когда собеседник набирает текст,

   чтобы понимать, что меня слышат.

🗺 План действий

📅 Общая диаграмма (Gantt-стиль)

<img width="636" height="299" alt="изображение" src="https://github.com/user-attachments/assets/d31cb7f5-fa75-4cea-9917-1d2d6dc249d0" />

📋 Детальный план по блокам

1. 📐 Проектирование (до 5 мая)

    Описание идеи и целевой аудитории

    User stories для каждой роли

    Список сущностей и связей

    ER-диаграмма (черновик)

    Список экранов

    🎨 Wireframes в Figma (список чатов, комната чата, профиль)

    📡 Список эндпоинтов API + Socket.IO событий

    📁 Структура папок фронта и бэка

2. 🎨 Frontend — базовая структура (до 14 мая)

    Инициализация проекта (Vite + React + TypeScript)

    Настройка роутинга (React Router v6)

    Layout: сайдбар с чатами, профиль

    Базовые UI-компоненты: Button, Input, Modal, MessageBubble

    Настройка Zustand (authStore, chatStore)

    Страницы: Login, Register, ChatList (заглушки)

3. ⚙️ Backend — ядро (до 26 мая)

    Инициализация проекта (Node.js + Express + TypeScript)

    Подключение Prisma + PostgreSQL

    Схема БД и первые миграции

    Auth: регистрация, логин, JWT, refresh token

    Middleware: authMiddleware, roleGuard, валидация (zod)

    CRUD: чаты, участники, сообщения

    Swagger-документация (автогенерация)

4. 🔌 WebSocket — realtime (до 9 июня)

    Настройка Socket.IO сервера

    Комнаты (join/leave по chatId)

    Обработка: send_message, edit_message, delete_message

    Обработка: add_reaction, remove_reaction

    Обработка: typing_start, typing_stop, mark_read

    Подключение Socket.IO клиента на фронте

5. 🎨 Frontend — основные экраны (до 20 июня)

    Список диалогов с превью последнего сообщения

    Комната чата: сообщения, отправка, редактирование, удаление

    Компонент реакций (выбор эмодзи)

    Отображение статуса "печатает..."

    Профиль пользователя + аватар

    Группы: создание, добавление участников, настройки

    Каналы: создание, постинг, подписка

    Поиск по сообщениям

6. 📎 Файлы и полировка (до 28 июня)

    Загрузка файлов/изображений (Cloudinary/S3)

    Пригласительные ссылки в группы/каналы

    Система жалоб на сообщения

    Admin-панель: блокировка пользователей, модерация

    Обработка ошибок и валидация — фронт и бэк

    Пагинация + бесконечный скролл сообщений

7. 🧪 Тестирование и инфраструктура (до 5 июля)

    Unit-тесты: AuthService, MessageService (Jest + Supertest)

    Тесты фронта: компонент MessageBubble, форма отправки

    Docker Compose: фронт + бэк + PostgreSQL + Redis

    README: финальная версия с инструкцией запуска

    Подготовка демо: сценарий показа, тестовые данные (seed)

    🎤 Презентация: структура выступления, слайды

🚀 Как запустить локально

bash

# 1️⃣ Клонировать репозиторий

git clone https://github.com/kowaldiv/chathub.git

cd chathub

# 2️⃣ Создать .env файлы (примеры ниже)

cp backend/.env.example backend/.env

cp frontend/.env.example frontend/.env

# 3️⃣ Запустить через Docker Compose

docker compose up --build

🌐 Приложение будет доступно:

Сервис	URL

Frontend	http://localhost:5173

Backend API	http://localhost:3000

Swagger Docs	http://localhost:3000/api-docs

WebSocket	ws://localhost:3000

🔐 Переменные окружения (backend)

env

# Database

DATABASE_URL=postgresql://user:password@postgres:5432/chathub

# Redis

REDIS_URL=redis://redis:6379

# JWT

JWT_SECRET=your_jwt_secret_key

JWT_REFRESH_SECRET=your_refresh_secret

# Cloudinary (для файлов)

CLOUDINARY_CLOUD_NAME=your_cloud_name

CLOUDINARY_API_KEY=your_api_key

CLOUDINARY_API_SECRET=your_api_secret

# Email (Nodemailer)

MAIL_HOST=smtp.gmail.com

MAIL_USER=your@gmail.com

MAIL_PASS=your_app_password

📁 Структура проекта

<img width="454" height="979" alt="изображение" src="https://github.com/user-attachments/assets/a907495b-18df-44e8-b6b6-2973fd6b633c" />

📎 Полезные ссылки

Ресурс	Ссылка

📋 Доска задач	Linear / Notion

🎨 Figma (UI/UX)	Wireframes

📡 API Docs	http://localhost:3000/api-docs

🐙 GitHub	github.com/kowaldiv/chathub

    🚀 ChatHub — общайся с комфортом. Редактируй, реагируй, создавай сообщества.
    
    Сделано kowaldiv с ❤️ и TypeScript
