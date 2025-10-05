# Node.js Express Notes API with MongoDB

Express-додаток для роботи з нотатками, реалізований з підключенням до MongoDB через Mongoose (02-mongodb).

## Функціональність

- HTTP-сервер на Express.js
- Підключення до MongoDB через Mongoose
- Повний CRUD для нотаток (створення, читання, оновлення, видалення)
- Логування HTTP-запитів за допомогою pino-http
- Обробка помилок (404, 500) через http-errors
- CORS підтримка
- Модульна архітектура (контролери, роути, middleware)

## Технології

- **Node.js** - JavaScript runtime
- **Express.js** - веб-фреймворк
- **MongoDB** - NoSQL база даних
- **Mongoose** - ODM для MongoDB
- **pino-http** - логування HTTP-запитів
- **http-errors** - обробка HTTP помилок
- **cors** - підтримка CORS
- **dotenv** - змінні оточення

## Структура проєкту

```
nodejs-hw/
├── src/
│   ├── controllers/
│   │   └── notesController.js    # Контролери для обробки запитів
│   ├── db/
│   │   └── connectMongoDB.js     # Підключення до MongoDB
│   ├── middleware/
│   │   ├── errorHandler.js       # Обробка помилок
│   │   ├── logger.js             # HTTP логування
│   │   └── notFoundHandler.js    # Обробка 404
│   ├── models/
│   │   └── note.js               # Mongoose модель Note
│   ├── routes/
│   │   └── notesRoutes.js        # Маршрути для нотаток
│   └── server.js                 # Головний файл сервера
├── .env                          # Змінні оточення
├── package.json                  # Залежності та скрипти
└── README.md                     # Документація
```

## Встановлення та запуск

### 1. Клонування репозиторію:

```bash
git clone <repository-url>
cd nodejs-hw
git checkout 02-mongodb
```

### 2. Встановлення залежностей:

```bash
npm install
```

### 3. Налаштування MongoDB:

Створіть файл `.env` у корені проєкту:

```env
PORT=3030
NODE_ENV=development
MONGO_URL=mongodb+srv://<username>:<password>@cluster.mongodb.net/<dbname>
```

### 4. Запуск сервера:

**Режим розробки:**

```bash
npm run dev
```

**Продакшн:**

```bash
npm start
```

Сервер буде доступний за адресою `http://localhost:3030`

## API Маршрути

### Отримати всі нотатки

```http
GET /notes
```

### Отримати нотатку за ID

```http
GET /notes/:noteId
```

### Створити нову нотатку

```http
POST /notes
Content-Type: application/json

{
  "title": "Назва нотатки",
  "content": "Текст нотатки",
  "tag": "Personal"
}
```

### Оновити нотатку

```http
PATCH /notes/:noteId
Content-Type: application/json

{
  "title": "Оновлена назва",
  "content": "Новий текст",
  "tag": "Important"
}
```

### Видалити нотатку

```http
DELETE /notes/:noteId
```

## Модель Note

| Поле        | Тип    | Обов'язкове | За замовчуванням | Значення                                                                           |
| ----------- | ------ | ----------- | ---------------- | ---------------------------------------------------------------------------------- |
| `title`     | String | Так         | -                | Назва нотатки (з trim)                                                             |
| `content`   | String | Ні          | ""               | Текст нотатки (з trim)                                                             |
| `tag`       | String | Ні          | "Todo"           | Work, Personal, Meeting, Shopping, Ideas, Travel, Finance, Health, Important, Todo |
| `createdAt` | Date   | Авто        | -                | Дата створення                                                                     |
| `updatedAt` | Date   | Авто        | -                | Дата оновлення                                                                     |

## Middleware

Додаток використовує наступні middleware:

1. **logger** (pino-http) - логування всіх HTTP-запитів
2. **express.json()** - парсинг JSON-тіла запитів
3. **cors()** - дозвіл CORS запитів
4. **notFoundHandler** - обробка неіснуючих маршрутів (404)
5. **errorHandler** - глобальна обробка помилок (500)

## Приклади використання

### Отримати всі нотатки:

```bash
curl http://localhost:3030/notes
```

### Створити нову нотатку:

```bash
curl -X POST http://localhost:3030/notes \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Нова нотатка",
    "content": "Текст нотатки",
    "tag": "Personal"
  }'
```

### Оновити нотатку:

```bash
curl -X PATCH http://localhost:3030/notes/<noteId> \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Оновлена нотатка",
    "tag": "Important"
  }'
```

### Видалити нотатку:

```bash
curl -X DELETE http://localhost:3030/notes/<noteId>
```
