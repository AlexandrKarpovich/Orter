<div align="center">
  <img src="https://cdn.jsdelivr.net/gh/twitter/twemoji@14.0.2/assets/svg/1f9a6.svg" width="120" height="120" alt="Otter Logo" />
  <h1>Orter 🦦</h1>
  <p><b>Платформа для заказов с чатом поддержки.</b><br>Order + Otter = Выдра в мире микросервисов.</p>
</div>

<p align="center">
  <a href="https://skillicons.dev">
    <img src="https://skillicons.dev/icons?i=laravel,php,nodejs,nuxtjs,ts,postgres,redis,docker&theme=dark" alt="Orter Tech Stack" />
  </a>
</p>
<p align="center">
  <b>Message Brokers:</b> Apache Kafka | RabbitMQ | gRPC
</p>

---

Orter — это легковесная альтернатива монолитному e-commerce для тех, кто любит порядок. Построена на микросервисной архитектуре с gRPC и WebSockets — потому что выдра не терпит, когда заказы теряются, а клиенты ждут ответа.

Проект сфокусирован на бэкенде: микросервисы, gRPC-контракты, Event Streaming и надежные очереди. Фронт на Nuxt.js — просто витрина, чтобы показать, как всё это летает.

## 🏗️ Архитектура

```text
                     ┌───────────────────┐
                     │  Nuxt.js Frontend │
                     └─────────┬─────────┘
                               │
                 REST + JWT    │    WebSockets
              ┌────────────────┴────────────────┐
              │                                 │
              ▼                                 ▼
     ┌─────────────────┐               ┌─────────────────┐
     │  Laravel API    │               │    Node.js WS   │
     │    Gateway      │◀──────────────│     Server      │
     └────────┬────────┘  Redis Sub    └────────▲────────┘
              │                                 │
              │ gRPC                            │
              ▼                                 │ Pub/Sub
     ┌─────────────────┐               ┌────────┴────────┐
     │  Order Service  │──────────────▶│      Redis      │
     │    (Laravel)    │  Publish      └─────────────────┘
     └────────┬────────┘
              │
      ┌───────┴────────┬────────────────┐
      │                │                │
      ▼                ▼                ▼
┌───────────┐    ┌───────────┐    ┌───────────┐
│PostgreSQL │    │ RabbitMQ  │    │   Kafka   │
└───────────┘    └───────────┘    └───────────┘
```

---

- **Laravel API Gateway** — швейцарский нож: маршрутизация, безопасность (JWT RS256), защита.
- **Order Service** — доменная логика (заказы, товары, транзакции).
- **Node.js Server** — чат в реальном времени, слушает Redis Pub/Sub.
- **Kafka** — сбор аналитики (клики, добавления в корзину).
- **RabbitMQ** — очереди на отправку чеков и email (надежная доставка).
- **Redis** — кэш и Pub/Sub для чата.
- **Nuxt.js** — фронт, общается только с Gateway и Node.js.

---

## 🛠️ Стек

| Слой | Технология |
|---|---|
| API Gateway | Laravel 11 + PHP 8.3 |
| Микросервис заказов | Laravel 11 |
| Real-time (чат) | Node.js + Socket.io |
| Брокеры | RabbitMQ, Kafka |
| БД и Кэш | PostgreSQL + Redis |
| Контракты | gRPC + Protocol Buffers |
| Фронт | Nuxt.js (Vue 3) + TypeScript |
| Инфраструктура | Docker + Docker Compose |

---

## ⚙️ Бэкенд: возможности

- 🔐 Регистрация и авторизация (JWT RS256)
- 📦 CRUD товаров и заказов
- 🛒 Управление корзиной
- 💬 Чат поддержки в реальном времени
- ⚡ Real-time синхронизация статуса заказа
- 📨 Асинхронная отправка email и чеков (RabbitMQ)
- 📊 Сбор аналитики кликов (Kafka)
- 🔎 Кэширование тяжелых запросов (Redis)
- 📡 gRPC между сервисами

---

## 📁 Структура репозитория

```text
orter/
├── proto/              # .proto файлы (контракты gRPC)
├── laravel-gateway/    # API Gateway + Auth (JWT)
├── order-service/      # Доменный сервис заказов (Laravel)
├── node-ws/            # WebSocket сервер для чата (Node.js)
├── nuxt-frontend/      # Фронтенд (Nuxt 3 + TypeScript)
├── docker-compose.yml  # Локальный запуск инфраструктуры
└── README.md
```

---

## ⚙️ Локальный запуск

**Требования:**
- Docker + Docker Compose
- PHP 8.3+ и Composer
- Node.js 20+

**1. Клонировать репозиторий**
```bash
git clone https://github.com/AlexandrKarpovich/orter.git
cd orter
```

**2. Поднять инфраструктуру (БД и брокеры)**
```bash
docker compose up -d
```

**3. Запустить Laravel Gateway**
```bash
cd laravel-gateway
composer install
cp .env.example .env
php artisan key:generate
php artisan migrate
php artisan serve --port=8000
```

**4. Запустить Node.js WebSocket сервер**
```bash
cd node-ws
npm install
npm run dev
```

**5. Запустить Nuxt.js фронтенд**
```bash
cd nuxt-frontend
npm install
npm run dev
```
Открыть: [http://localhost:3000](http://localhost:3000)

---

## 🖥️ Фронт: возможности

- 🛍️ Просмотр каталога товаров (SSR для SEO)
- 🛒 Корзина и оформление заказа
- 💬 Встроенный чат с поддержкой (WebSockets)
- 🔔 Живые уведомления о статусе заказа
- 🔐 Авторизация через JWT-токены

---

## 🦦 Почему «Orter»?

Order (Заказ) + Otter (Выдра) = Orter.
Выдра — это юркий и хитрый зверь, который обожает плескаться в потоках данных (Kafka), крепко держит добычу в лапках (наши заказы) и без умолку трещит (чат поддержки).

В общем, выдра не даст заказу утонуть в очереди, а клиенту — остаться без ответа.

---

## 📸 Скриншоты
*(добавить позже)*

## 📝 Лицензия
MIT — делай что хочешь, только выдру не обижай.