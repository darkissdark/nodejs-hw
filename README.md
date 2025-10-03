# Node.js Express Notes API

Простий Express-додаток для роботи з колекцією нотаток, реалізований як частина навчального завдання.

## Функціональність

- HTTP-сервер на Express.js
- Робота з нотатками через REST API
- Логування HTTP-запитів за допомогою pino-http
- Обробка помилок та неіснуючих маршрутів
- CORS підтримка

## API Маршрути

### Отримати всі нотатки

```http
GET /notes
```

**Відповідь:**

```json
{
  "message": "Retrieved all notes"
}
```

### Отримати нотатку за ID

```http
GET /notes/:noteId
```

**Відповідь:**

```json
{
  "message": "Retrieved note with ID: {noteId}"
}
```

### Тестовий маршрут для помилок

```http
GET /test-error
```

**Відповідь:**

```json
{
  "message": "Internal Server Error",
  "error": "Simulated server error"
}
```

## Встановлення та запуск

1. Клонуйте репозиторій:

```bash
git clone <repository-url>
cd nodejs-hw
```

2. Встановіть залежності:

```bash
npm install
```

3. Створіть файл `.env` у корені проєкту:

```env
PORT=3030
NODE_ENV=development
```

4. Запустіть сервер у режимі розробки:

```bash
npm run dev
```

5. Запустіть сервер у продакшені:

```bash
npm start
```

Сервер буде доступний за адресою `http://localhost:3030`

## Технології

- **Node.js** - JavaScript runtime
- **Express.js** - веб-фреймворк
- **pino-http** - логування HTTP-запитів
- **cors** - підтримка CORS
- **dotenv** - змінні оточення
- **nodemon** - автоматичне перезавантаження при розробці

## Структура проєкту

```
nodejs-hw/
├── src/
│   └── server.js          # Основний файл сервера
├── .env                   # Змінні оточення
├── .gitignore            # Git ignore файл
├── .prettierrc           # Конфігурація Prettier
├── eslint.config.mjs     # Конфігурація ESLint
├── package.json          # Залежності та скрипти
└── README.md             # Документація
```

## Middleware

- **express.json()** - обробка JSON даних
- **cors()** - підтримка CORS
- **pino-http** - логування запитів
- **404 handler** - обробка неіснуючих маршрутів
- **500 handler** - обробка помилок сервера

## Приклади використання

### Отримати всі нотатки

```bash
curl http://localhost:3030/notes
```

### Отримати конкретну нотатку

```bash
curl http://localhost:3030/notes/123
```

### Тест помилки

```bash
curl http://localhost:3030/test-error
```

### Тест неіснуючого маршруту

```bash
curl http://localhost:3030/nonexistent
```

## Автор

Створено як частина навчального завдання з Node.js та Express.js.
