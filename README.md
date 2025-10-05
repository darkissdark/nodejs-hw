# Node.js Express Notes API with MongoDB

Express-додаток для роботи з нотатками, реалізований з підключенням до MongoDB через Mongoose (03-validation).

## Функціональність

- HTTP-сервер на Express.js
- Підключення до MongoDB через Mongoose
- Повний CRUD для нотаток (створення, читання, оновлення, видалення)
- **Пагінація** колекції нотаток
- **Текстовий пошук** по нотатках
- **Фільтрація** за тегами
- **Валідація** всіх запитів через celebrate
- Логування HTTP-запитів за допомогою pino-http
- Обробка помилок (404, 500) через http-errors
- CORS підтримка
- Модульна архітектура (контролери, роути, middleware, валідації)

## Технології

- **Node.js** - JavaScript runtime
- **Express.js** - веб-фреймворк
- **MongoDB** - NoSQL база даних
- **Mongoose** - ODM для MongoDB
- **celebrate** - валідація запитів
- **Joi** - схеми валідації
- **pino-http** - логування HTTP-запитів
- **http-errors** - обробка HTTP помилок
- **cors** - підтримка CORS
- **dotenv** - змінні оточення

## Структура проєкту

```
nodejs-hw/
├── src/
│   ├── constants/
│   │   └── tags.js               # Константи тегів для нотаток
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
│   ├── validations/
│   │   └── notesValidation.js    # Схеми валідації запитів
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
git checkout 03-validation
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

### Отримати всі нотатки (з пагінацією та фільтрацією)

```http
GET /notes?page=1&perPage=10&tag=Personal&search=hello
```

**Параметри запиту:**

- `page` (опціонально) - номер сторінки (за замовчуванням 1)
- `perPage` (опціонально) - кількість елементів на сторінці (5-20, за замовчуванням 10)
- `tag` (опціонально) - фільтр за тегом (Work, Personal, Meeting, Shopping, Ideas, Travel, Finance, Health, Important, Todo)
- `search` (опціонально) - пошук по title та content

**Відповідь:**

```json
{
  "page": 1,
  "perPage": 10,
  "totalNotes": 150,
  "totalPages": 15,
  "notes": [
    {
      "_id": "...",
      "title": "Назва нотатки",
      "content": "Текст нотатки",
      "tag": "Personal",
      "createdAt": "2024-01-01T00:00:00.000Z",
      "updatedAt": "2024-01-01T00:00:00.000Z"
    }
  ]
}
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
4. **celebrate()** - валідація запитів з детальними повідомленнями про помилки
5. **errors()** - обробка помилок валідації від celebrate
6. **notFoundHandler** - обробка неіснуючих маршрутів (404)
7. **errorHandler** - глобальна обробка помилок (500)

## Валідація запитів

Додаток використовує бібліотеку **celebrate** з **Joi** схемами для валідації:

### Схеми валідації:

- **getAllNotesSchema** - валідація параметрів запиту для GET `/notes`
- **noteIdSchema** - валідація параметра noteId для GET/DELETE/PATCH `/notes/:noteId`
- **createNoteSchema** - валідація тіла запиту для POST `/notes`
- **updateNoteSchema** - валідація тіла запиту для PATCH `/notes/:noteId`

### Приклади помилок валідації:

```json
{
  "message": "Page must be at least 1"
}
```

```json
{
  "message": "Tag must be one of: Work, Personal, Meeting, Shopping, Ideas, Travel, Finance, Health, Important, Todo"
}
```

## Приклади використання

### Отримати всі нотатки з пагінацією та фільтрацією:

```bash
# Базова пагінація
curl "http://localhost:3030/notes?page=2&perPage=5"

# Фільтрація за тегом
curl "http://localhost:3030/notes?tag=Personal"

# Пошук по тексту
curl "http://localhost:3030/notes?search=hello"

# Комбіновані параметри
curl "http://localhost:3030/notes?page=1&perPage=10&tag=Work&search=project"
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
