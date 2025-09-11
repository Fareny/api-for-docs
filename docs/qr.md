# POST /qr/validate

Метод для валидации QR-кода по `id` документа.

## Входные данные (Request Body)

```json
{
  "id": "string"
}
```

### Пояснения к полям:
- `id` — строковый идентификатор документа. Обязателен.

## Логика обработки

1. Если `id` отсутствует или не является строкой → возвращается статус `invalid`.
2. Если документа с таким `id` нет в системе → возвращается статус `not_found`.
3. Если документ найден:
   - Если документ соответствует `same`.
   - Если документ не соответстует `diff`.

## Выходные данные (Response)

Успешный ответ всегда возвращается со статусом `200 OK`.

### Варианты:

**1. Некорректный `id`**
```json
{
  "status": "invalid"
}
```

**2. Документ не найден**
```json
{
  "status": "not_found"
}
```

**3. Документ найден, результат проверки**
```json
{
  "status": "same",        // или "diff"
  "document": {
    "id": "2",
    "title": "Типовой договор закупки инертных материалов (Отсрочка) №2",
    "counterparty": "ООО «АБ Ц»",
    "itn": "123456789012",
    "date": "2025-06-16T00:00:00.000Z",
    "deadline": "2025-06-17T00:00:00.000Z",
    "subtask": "Александрова А.А.",
    "status": {
      "filterCode": "inProgress",
      "statusCode": "approval",
      "label": "Согласование"
    },
    "progress": { 
      "completed": 5, 
      "total": 23, 
      "percent": 20 
    },
    "executors": [
      {
        "id": 1,
        "name": "Иван Иванов",
        "avatar": "https://avatars.mds.yandex.net/i?id=ade0dc0d02a6be992c751d279913bb8a_l-5303121-images-thumbs&n=13",
        "role": {
          "code": "initiator",
          "label": "Инициатор",
          "important": true
        }
      },
      {
        "id": 2,
        "name": "Анна Ананасова",
        "avatar": "https://i.pinimg.com/originals/fc/67/03/fc6703e79d41363832817cbdf297beaa.jpg",
        "role": {
          "code": "manager",
          "label": "Менеджер",
          "important": true
        }
      },
      {
        "id": 3,
        "name": "Петр Петров",
        "avatar": "https://avatars.mds.yandex.net/get-yapic/65952/WjsBnhtGBT6kAknZY3xjyYe2pCc-1/orig",
        "role": {
          "code": "approver",
          "label": "Согласующий"
        }
      },
      {
        "id": 4,
        "name": "Олег Олегов",
        "avatar": "https://avatars.mds.yandex.net/get-yapic/65952/WjsBnhtGBT6kAknZY3xjyYe2pCc-1/orig",
        "role": {
          "code": "executor",
          "label": "Исполнитель"
        }
      },
      {
        "id": 5,
        "name": "Светлана Светлова",
        "avatar": "https://avatars.mds.yandex.net/get-yapic/65952/WjsBnhtGBT6kAknZY3xjyYe2pCc-1/orig",
        "role": {
          "code": "observer",
          "label": "Наблюдатель"
        }
      }
    ]
  }
}
```
