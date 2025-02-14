# microservices-project  

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![JWT](https://img.shields.io/badge/JWT-000000?style=for-the-badge&logo=jsonwebtoken&logoColor=white)


Этот репозиторий содержит аутентификационный микросервис и связанный с ним API-сервис.  

## Содержание  

### 1. **AuthMicroservice** ([ссылка](https://github.com/RedGradient/AuthMicroservice))  
Аутентификационный микросервис. Подписывает токены приватным ключом. Функционал:  
- JWT-аутентификация  
- Двухфакторная JWT-аутентификация (2FA) с помощью TOTP пароля (например, через Google Authenticator)  
- Подпись токенов асимметричным ключом  
- Ротация ключей  

### 2. **APIMicroservice** ([ссылка](https://github.com/RedGradient/APIMicroservice))
"Микросервис" с эндпоинтом, который аутентифицирует пользователей через JWT Access токен. Верификация токена происходит публичным ключом.

Так как **AuthMicroservice** периодически обновляет ключи, **APIMicroservice** может запрашивать у него актуальный ключ, а также несколько предыдущих ключей. Последние нужны для верификации токенов, которые были подписаны старыми ключами, но время действия которых еще не истекло.
