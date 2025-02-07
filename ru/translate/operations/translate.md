# Перевести текст

Чтобы перевести текст, воспользуйтесь методом [translate](../api-ref/Translation/translate).

## Примеры

[!INCLUDE [ai-before-beginning](../../_includes/ai-before-beginning.md)]

### Hello, world

В этом примере мы переведем на русский язык две строки с текстом: <q>Hello</q> и <q>World</q>.

1. Узнайте код языка, на который вы хотите перевести текст, с помощью метода [listLanguages](../api-ref/Translation/listLanguages):

    ```bash
    $ export IAM_TOKEN=CggaATEVAgA...
    $ curl -X POST \
        -H "Content-Type: application/json" \
        -H "Authorization: Bearer ${IAM_TOKEN}" \
        -d "{\"folder_id\": \"${FOLDER_ID}\"}" \
        "https://translate.api.cloud.yandex.net/translate/v2/languages"
    ```

    Ответ будет содержать список языков с названиями на соответствующем языке:

    ```json
    {
        "languages": [
            ...
            {
            "code": "ru",
            "name": "русский"
            },
            ...
        ]
    }
    ```
2. Создайте файл с телом запроса, например `body.json`.
    Строки текста для перевода перечислите в поле `texts`. В поле `targetLanguageCode` укажите язык, на который необходимо перевести текст.

    ```json
    {
        "folder_id": "b1gvmob95yysaplct532",
        "texts": ["Hello", "World"],
        "targetLanguageCode": "ru"
    }
    ```
3. Передайте файл на перевод с помощью метода [translate](../api-ref/Translation/translate):
    ```bash
    $ export IAM_TOKEN=CggaATEVAgA...
    $ curl -X POST \
        -H "Content-Type: application/json" \
        -H "Authorization: Bearer ${IAM_TOKEN}" \
        -d @body.json \
        "https://translate.api.cloud.yandex.net/translate/v2/translate"
    ```

    В ответе сервис вернет переведенные строки текста:
    ```json
    {
        "translations": [
            {
            "text": "Привет",
            "detectedLanguageCode": "en"
            },
            {
            "text": "Мир",
            "detectedLanguageCode": "en"
            }
        ]
    }
    ```

### Укажите язык исходного текста

Есть слова, которые пишутся одинаково в разных языках, но переводятся по-разному. Например, слово <q>angel</q> в английском языке означает духовное существо, а в немецком — удочку. Если переданный текст состоит из таких слов, то SpeechKit может ошибиться при определении языка текста.

Чтобы избежать ошибки, укажите в поле `sourceLanguageCode` язык, с которого необходимо перевести текст:

```json
{
    "folder_id": "b1gvmob95yysaplct532",
    "texts": ["angel"],
    "targetLanguageCode": "ru",
    "sourceLanguageCode": "de"
}
```

Сохраните тело запроса в файле, например `body.json`, и передайте его с помощью метода [translate](../api-ref/Translation/translate):

```bash
$ export IAM_TOKEN=CggaATEVAgA...
$ curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer ${IAM_TOKEN}" \
    -d @body.json \
    "https://translate.api.cloud.yandex.net/translate/v2/translate"

{
    "translations": [
        {
            "text": "удочка"
        }
    ]
}
```