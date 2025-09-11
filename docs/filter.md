# Search API

Описание всех эндпоинтов блока поиска.

---

## GET /search/history

Возвращает историю поисковых запросов пользователя.

### Пример ответа

```json
{
  "history": [
    "Как оформить отпуск?",
    "Отчёт",
    "Договор",
    "Контакты отдела кадров",
    "Как заполнить табель рабочего времени? Как заполнить табель рабочего времени?",
    "Регламент работы с документами",
    "Инструкция по технике безопасности",
    "Календарь праздничных дней 2025",
    "Договор аренды офиса",
    "Образец заявления на командировку",
    "Правила внутреннего трудового распорядка",
    "Отчётность в налоговую"
  ]
}
```

---

## GET /search/suggestions

Возвращает подсказки по введённой строке.

### Параметры запроса

`q` — строка запроса (обязательный параметр).

### Пример запроса

`GET /search/suggestions?q=договор`

### Пример ответа

```json
{
  "suggestions": [
    "договор 1",
    "договор 2",
    "договор 3"
  ]
}
```

---

## GET /search

Поиск документов с фильтрами, сортировкой и пагинацией.

### Параметры запроса

`q` — строка поиска.  
`page` — номер страницы (строка, по умолчанию "1", преобразуется к числу > 0).  
`limit` — размер страницы (строка, по умолчанию "5", преобразуется к числу > 0).  
`type` — тип документа (строка, опционально).  
`option` — дополнительная опция фильтра для некоторых типов (строка, опционально).  
`dateFrom` — нижняя граница даты создания документа в формате YYYY-MM-DD (строка, опционально).  
`dateTo` — верхняя граница даты создания документа в формате YYYY-MM-DD (строка, опционально).  
`sortField` — поле сортировки: `createdAt` (по умолчанию) или `counterparty`.  
`sortDir` — направление сортировки: `asc (по возрастанию)` или `desc (по убыванию)` (по умолчанию `desc`).

### Правила фильтрации и сортировки

По умолчанию берётся копия массива `mockDocuments`.  
Если передан `q`, фильтрация выполняется по вхождению подстроки в `title`, `counterparty` или `content` (регистр игнорируется).  
Если передан `type`, выбираются элементы, где `doc.type` строго равен этому значению.  
Если передан `option`, логика зависит от типа: для `contract` сравнивается `doc.contractType`, для `report` сравнивается `doc.metric`, для остальных типов условие пропускается.  
Если переданы `dateFrom` и/или `dateTo`, фильтрация выполняется по полю `createdAt` (если у элемента оно отсутствует или некорректно, элемент отфильтровывается).  
Сортировка выполняется по `createdAt` (как числовой timestamp) или `counterparty` (лексикографически), направление задаётся `sortDir`.  
Пагинация — по `page` и `limit`.

### Пример ответа

```json
{
  "documents": [
    {
      "id": 1,
      "title": "Типовой договор закупки инертных материалов (Отсрочка) №1",
      "counterparty": "ООО «АБ Ц»",
      "itn": "123456789012",
      "date": "2025-06-16T00:00:00.000Z",
      "deadline": "2025-06-17T00:00:00.000Z",
      "subtask": "Александрова А.А.",
      "status": {
        "filterCode": "inProgress",
        "statusCode": "needRevision",
        "label": "На доработке"
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
    },
    {
      "id": 2,
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
  ],
  "total": 50,
  "page": 1,
  "limit": 5
}
```

### Схема объекта документа

```json
{
  "id": 0,
  "title": "string",
  "counterparty": "string",
  "itn": "string",
  "date": "ISO-8601 string",
  "deadline": "ISO-8601 string",
  "subtask": "string",
  "status": {
    "filterCode": "inProgress | subtask | archived",
    "statusCode": "needRevision | approval | secondParty | approved | signing | signed | archived | rejected | annulled",
    "label": "string"
  },
  "progress": {
    "completed": 0,
    "total": 0,
    "percent": 0
  },
  "executors": [
    {
      "id": 0,
      "name": "string",
      "avatar": "string",
      "role": {
        "code": "initiator | manager | approver | executor | observer",
        "label": "string",
        "important": true
      }
    }
  ]
}
```

Примечание: поля `type`, `contractType`, `metric`, `content`, `createdAt` в мок-данных по умолчанию отсутствуют; они участвуют в фильтрации и сортировке только если будут добавлены к элементам.
  
---

## GET /search/filters

Возвращает возможные типы документов и, опционально, набор фильтров для конкретного типа.

### Параметры запроса

`type` — тип документа, для которого нужно вернуть конфигурацию фильтра (строка, опционально).

## Что делает параметр type:
`Он говорит серверу: «дай мне настройки фильтров именно для такого типа документа»`

Доступные типы: `contract`, `report`, `statement`. (берутся из `documentTypes`)

### Пример запроса

`GET /search/filters?type=contract`

### Пример ответа при переданном type=contract

```json
{
  "filterOptions": {
    "label": "Тип договора",
    "name": "contractType",
    "options": [
      { "value": "default", "label": "Типовой" },
      { "value": "custom",  "label": "Нетиповой" },
      { "value": "any",     "label": "Любой" }
    ]
  },
  "documentTypes": [
    { "value": "contract", "label": "Договор" },
    { "value": "report",   "label": "Отчёт" },
    { "value": "statement","label": "Заявление" }
  ]
}
```

### Пример ответа при отсутствии параметра type или для типа без фильтров

```json
{
  "filterOptions": null,
  "documentTypes": [
    { "value": "contract", "label": "Договор" },
    { "value": "report",   "label": "Отчёт" },
    { "value": "statement","label": "Заявление" }
  ]
}
```
