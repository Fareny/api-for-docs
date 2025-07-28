### 1 Получение списка документов

Метод: GET /documents

Что приходит (пример)

```JSON
{
  "status": "inProgress",
  "type": "contract",
  "template": "typical",
  "search": "договор",
  "page": 2,
  "limit": 10
}
```

Что уходит (пример)

```JSON
{
  "documents": [
    {
      "id": 1,
      "title": "Нетиповой договор поставки №1",
      "organization": "ООО «АБ Ц»",
      "date": "2025-06-16T00:00:00.000Z",
      "deadline": "2025-06-16T00:00:00.000Z",
      "status": {
        "filterCode": "inProgress",
        "statusCode": "needRevision",
        "label": "На доработке"
      },
      "type": "contract",
      "template": "typical",
      "progress": {
        "completed": 5,
        "total": 23,
        "percent": 20
      },
      "executors": [
        { "id": 1, "name": "Иван", "avatar": "..." },
        { "id": 2, "name": "Анна", "avatar": "..." },
        { "id": 3, "name": "Петр", "avatar": "..." }
      ]
    }
  ],
  "total": 50,
  "page": 2,
  "limit": 10
}
```
