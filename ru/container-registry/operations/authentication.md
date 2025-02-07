# Аутентифицироваться в [!KEYREF container-registry-short-name]

Перед тем как начать работу с [!KEYREF container-registry-short-name] через Docker CLI, необходимо пройти аутентификацию.

Вы можете аутентифицироваться как пользователь или как сервисный аккаунт. Ознакомьтесь со [способами аутентификации](#method) и выберите подходящий.

> [!NOTE]
>
> Необходима роль на каталог — `viewer`. Подробнее про роли читайте в разделе [[!TITLE]](../security/index.md).

## Способы аутентификации {#method}

Вы можете аутентифицироваться следующими способами: 

- Как пользователь: 
    - [С помощью OAuth-токена](#oauth) (срок жизни **год**).
    - [С помощью IAM-токена](#iam) (срок жизни **12 часов**).
- Как сервисный аккаунт: 
    - [С помощью авторизованных ключей](#sa-json) (неограниченный срок жизни).
    - [C помощью IAM-токена](#sa-iam) (срок жизни **12 часов**).

Если вы не используете хранилище ключей, то можете положить конфигурационный файл на виртуальную машину. В таком случае, 
при работе с Docker CLI с этой машины, вам не придется проходить аутентификацию.

> [!IMPORTANT]
>
> Внимание! Стоит с осторожностью относиться к распространению файлов с ключами — любой, кто имеет доступ на вашу виртуальную
> машину, сможет выполнять команды в Docker CLI от вашего имени.

Команда для аутентификации выглядит следующим образом:

```
$ docker login \
--username <тип токена> \
--password <токен> \
cr.yandex
```

- В параметр `username` передается тип токена `<тип токена>`. Допустимые значения: `oauth`, `iam` или `json_key`. 
- В параметр `password` передается сам токен.
- После всех параметров указывается адрес для аутентификации `cr.yandex`. Если его не указать запрос пойдет в сервис по умолчанию — [Docker Hub](https://hub.docker.com).

## Аутентифицироваться как пользователь {#user}

### Аутентификация с помощью OAuth-токена {#oauth}

> [!NOTE]
> 
> Срок жизни OAuth-токена 1 год. После этого необходимо [получить новый OAuth-токен](https://oauth.yandex.ru/authorize?response_type=token&client_id=1a6990aa636648e9b2ef855fa7bec2fb) и повторить процедуру аутентификации.

1. Если у вас еще нет OAuth-токена, получите его по [ссылке](https://oauth.yandex.ru/authorize?response_type=token&client_id=1a6990aa636648e9b2ef855fa7bec2fb).

1. Выполните команду: 
    
    ```
    $ docker login \
    --username oauth \
    --password <OAuth-токен> \
    cr.yandex
    ```

### Аутентификация с помощью IAM-токена {#iam}

> [!NOTE]
> 
> Срок жизни IAM-токена 12 часов. После этого необходимо получить новый IAM-токен и повторить процедуру аутентификации.

1. Если у вас еще нет IAM-токена, получите его командой:
    
    ```
    $ yc iam create-token
    ```
    
1. Выполните команду: 
    
    ```
    $ docker login \
    --username iam \
    --password <IAM-токен> \
    cr.yandex
    ```

## Аутентифицироваться как сервисный аккаунт {#sa}

### Аутентификация с помощью авторизованных ключей {#sa-json}

> [!NOTE]
> 
> Срок жизни авторизованных ключей неограничен, но вы всегда можете получить новые авторизованные ключи и повторить процедуру аутентификации, если что-то пошло не так.

Используя [сервисный аккаунт](../../iam/concepts/users/service-accounts.md), ваши программы могут получать доступ к ресурсам Яндекс.Облака. Получите файл с авторизованными ключами для вашего сервисного аккаунта, используя CLI. 

1. Получите [авторизованные ключи](../../iam/concepts/users/service-accounts.md#sa-key) для вашего сервисного аккаунта: 
    
    ```
    $ yc iam key create --service-account-name default-sa -o key.json
    id: aje8a87g4e...
    service_account_id: aje3932acd...
    created_at: "2019-05-31T16:56:47Z"
    key_algorithm: RSA_2048
    ```
    
1. Выполните команду: 

    ```
    $ cat key.json | docker login \
    --username json_key \ 
    --password-stdin \ 
    cr.yandex 
    
    Login Succeeded
    ```
    
    - Команда `cat key.json` записывает содержимое файла с ключом в поток ввода.
    - Флаг `--password-stdin` позволяет читать пароль из потока ввода.
    
    
### Аутентификация с помощью IAM-токена {#sa-iam}

> [!NOTE]
> 
> Срок жизни IAM-токена 12 часов. После этого необходимо получить новый IAM-токен и повторить процедуру аутентификации.

1. Получите [авторизованные ключи](../../iam/concepts/users/service-accounts.md#sa-key) для вашего сервисного аккаунта: 
    
    ```
    $ yc iam key create --service-account-name default-sa -o key.json
    id: aje8a87g4e...
    service_account_id: aje3932acd...
    created_at: "2019-05-31T16:56:47Z"
    key_algorithm: RSA_2048
    ```

1. Добавьте файл с авторизованным ключом в ваш профиль CLI:  

   ```
   $ yc config set service-account-key key.json
   ```
   
   При установке параметра `service-account-key` обнуляется параметр `token`. Подробнее о параметрах, читайте в разделе [Конфигурация CLI](../../cli/concepts/core-propreties.md).

1. Получите IAM-токен: 

    ```
    $ yc iam create-token
    ```

1. Выполните команду: 
    
    ```
    $ docker login \
    --username iam \
    --password <IAM-токен> \
    cr.yandex
    ```
