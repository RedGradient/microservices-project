# microservices-project  

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![JWT](https://img.shields.io/badge/JWT-000000?style=for-the-badge&logo=jsonwebtoken&logoColor=white)
![KrakenD](https://img.shields.io/badge/KrakenD-Gateway-blue?style=for-the-badge)


Этот репозиторий содержит аутентификационный микросервис и связанный с ним API-сервис.
Для централизованного доступа к API задействован шлюз **KrakenD**.

## Содержание  

### 1. **AuthMicroservice** ([ссылка](https://github.com/RedGradient/AuthMicroservice))  
Аутентификационный микросервис. Создает и подписывает Access/Refresh токены приватным ключом.
#### Функционал  
- JWT-аутентификация  
- Двухфакторная JWT-аутентификация (2FA) с помощью TOTP пароля (например, через Google Authenticator)
#### Детали реализации
- Подпись токенов с использованием асимметричного алгоритма RSA   
- Ротация ключей. В проекте есть специальная [функция](https://github.com/RedGradient/AuthMicroservice/blob/master/rotate_keys.py#L55-L61), которая создает новую пару ключей (private и public) и вводит их в работу.

### 2. **APIMicroservice** ([ссылка](https://github.com/RedGradient/APIMicroservice))
Макет API сервиса, содержащего бизнес-логику. Есть защищенные эндпоинты, требующие JWT авторизации (1 шт).

Так как **AuthMicroservice** периодически обновляет ключи, 
**APIMicroservice** может запрашивать у него актуальный публичный ключ, а также несколько предыдущих ключей. 
Последние нужны для верификации токенов, подписанных старыми ключами, время действия которых еще не истекло.

## Запуск
Проект докеризирован, поэтому для запуска достаточно выполнить ```docker-compose up --build``` в папке проекта.
API будет доступен по адресу ```http://localhost:8080/```.

### API
- **`/users/{user-id}`**:  
  Возвращает пользователя по его id  
  **Метод**: ``GET``
- **`/orders/{user-id}`**:  
  Возвращает все заказы пользователя по его id  
  **Метод**: ``GET``
- **`/user-data/{user-id}`**:  
  Этот эндпоинт существует только в KrakenD, он агрегирует результаты с `/users/{user-id}` и `/orders/{user-id}`
  **Метод**: ``GET``
