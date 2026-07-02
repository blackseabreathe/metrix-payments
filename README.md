# Метрикс, API добавления ожидаемых платежей
Документация описывает возможные ответы от API при попытке добавить платеж. Ответы охватывают случаи ошибок валидации и статус добавления.
Ожидается многомерный массив с любым количеством вложенностей.

## Общая структура ответа
```json
{
  "result": boolean|string|array # true при http_code 200, в других случаях - текст ошибки
}
```

## 🟡 Токен должен быть передан в заголовках запроса в параметре `x-token`

Запрос должен быть отправлен методом `POST`. 

## Формат тела запроса
Валидный JSON


### ❌ Ошибки в запросе (HTTP_CODE: 400, 401, 405)

#### Отправлен запрос методом, отличным от `POST` | Не передан токен | Некорректный токен | Тело запроса пустое
```json
{
  "result": "Текст ошибки",
}
```


### ❌ Ошибки валидации (400)


#### Переданы невалидные параметры в массиве
```json
{
  "result": "Текст ошибки",
}
```




### ⚠️ Платеж успешно добавлен (HTTP_CODE: 200)

```json
{
  "result": true,
}
```




# Curl пример запроса
```diff
- Внимание! Все параметры в запросе обязательны
```

```php
$endpoint = 'xxxxxxxxxxxxx';

// Доступные форматы даты: Y-m-d, d.m.Y
$params = [
    [
        "date" => "2022-02-28",
        "sum" => "555,96",
        "deal" => 333,
    ],
    [
        "date" => "2022-11-01",
        "sum" => "1000.11",
        "deal" => 333,
    ],
];

// Заголовки
$headers = [
    'Accept: application/json',
    'Content-Type: application/json',
    'x-token: xxxxxxxxxxxxxxxx'
];


$curl = curl_init();
curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);
curl_setopt($curl, CURLOPT_POST, 1);
curl_setopt($curl, CURLOPT_POSTFIELDS, json_encode($params));
curl_setopt($curl, CURLOPT_URL, $endpoint);
curl_setopt($curl, CURLOPT_HEADER, 0);
curl_setopt($curl, CURLOPT_HTTPHEADER, $headers);
curl_setopt($curl, CURLOPT_CONNECTTIMEOUT, 5);
curl_setopt($curl, CURLOPT_TIMEOUT, 3);

$result = json_decode(curl_exec($curl), true);
$http_code = curl_getinfo($curl, CURLINFO_HTTP_CODE);
$curl_errno = curl_errno($curl);
curl_close($curl);

echo "http code: $http_code<br><br>";

echo '<pre>';
print_r($result);
```
