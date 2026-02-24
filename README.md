
# Проект: Тестирование HTML-формы входа

## Описание проекта

В этом проекте реализована простая веб-страница с формой входа (логин + пароль) на HTML и JavaScript.
Также добавлены **автоматические тесты на Python**, которые проверяют корректность работы формы.

Цель проекта:

* показать работу формы авторизации;
* проверить её с помощью автоматизированного тестирования (например, Selenium + pytest).

---

# Структура проекта

```
project/
│
├── login.html          # HTML страница с формой входа
├── tests/
│   └── test_login.py   # Python тесты для проверки формы
└── README.md           # Документация проекта
```

---

# Объяснение HTML кода

## 1. Объявление типа документа

```html
<!DOCTYPE html>
```

Сообщает браузеру, что используется стандарт **HTML5**.

---

## 2. Корневой элемент HTML

```html
<html lang="ru">
```

Задает язык страницы — русский.

---

## 3. Блок `<head>`

```html
<head>
    <meta charset="UTF-8">
    <title>Форма входа</title>
</head>
```

### Что здесь происходит:

* `meta charset="UTF-8"` — поддержка всех символов (включая русский язык)
* `title` — название страницы во вкладке браузера.

---

## 4. Заголовок страницы

```html
<h2>Вход</h2>
```

Отображает заголовок формы входа.

---

## 5. Форма авторизации

```html
<form id="loginForm">
```

Создается форма с уникальным идентификатором `loginForm`.
Этот id используется JavaScript и тестами.

---

## 6. Поле ввода email

```html
<input type="email" id="email" required>
```

Что происходит:

* `type="email"` — браузер проверяет корректность email
* `id="email"` — используется в JavaScript
* `required` — поле обязательно для заполнения.

---

## 7. Поле ввода пароля

```html
<input type="password" id="password" required>
```

Особенности:

* скрывает введенные символы
* обязательное поле.

---

## 8. Кнопка входа

```html
<button type="submit">Войти</button>
```

Отправляет форму.

---

## 9. Блок для вывода результата

```html
<p id="result"></p>
```

Здесь отображается:

* успешный вход
* ошибка авторизации.

---

# JavaScript логика формы

## Обработчик отправки формы

```javascript
document.getElementById("loginForm").addEventListener("submit", function(event) {
```

Когда пользователь нажимает кнопку **Войти**, запускается этот код.

---

## Отмена перезагрузки страницы

```javascript
event.preventDefault();
```

Форма не отправляется на сервер — всё проверяется на стороне клиента.

---

## Получение данных из формы

```javascript
const email = document.getElementById("email").value.trim();
const password = document.getElementById("password").value.trim();
```

Здесь:

* считываются значения
* убираются пробелы.

---

## Правильные данные для входа

```javascript
const correctEmail = "test@example.com";
const correctPassword = "123456";
```

Тестовый логин:

```
Email: test@example.com
Password: 123456
```

---

## Проверка данных

```javascript
if (email === correctEmail && password === correctPassword)
```

Если данные совпадают — вход успешный.

---

## Успешный вход

```javascript
result.textContent = "Все верно!";
result.style.color = "green";
```

Показывает сообщение:

```
Все верно!
```

---

## Ошибка входа

```javascript
result.textContent = "Неправильный логин или пароль!";
result.style.color = "red";
```

Показывает ошибку.

---

# Python тесты для формы

Для тестирования используется:

* Selenium
* pytest

Установка:

```bash
pip install selenium pytest
```

Также нужен:

* ChromeDriver или другой WebDriver.

---

# Пример тестов (test_login.py)

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
import time


def test_login_success():
    driver = webdriver.Chrome()
    driver.get("file:///path/to/login.html")

    driver.find_element(By.ID, "email").send_keys("test@example.com")
    driver.find_element(By.ID, "password").send_keys("123456")
    driver.find_element(By.TAG_NAME, "button").click()

    time.sleep(1)

    result = driver.find_element(By.ID, "result").text
    assert result == "Все верно!"

    driver.quit()


def test_login_wrong_password():
    driver = webdriver.Chrome()
    driver.get("file:///path/to/login.html")

    driver.find_element(By.ID, "email").send_keys("test@example.com")
    driver.find_element(By.ID, "password").send_keys("wrong")
    driver.find_element(By.TAG_NAME, "button").click()

    time.sleep(1)

    result = driver.find_element(By.ID, "result").text
    assert result == "Неправильный логин или пароль!"

    driver.quit()
```

---

# Что проверяют тесты

## Тест 1 — успешный вход

Проверяет:

* ввод правильного email
* ввод правильного пароля
* появление сообщения успеха.

---

## Тест 2 — неправильный пароль

Проверяет:

* ввод неверного пароля
* появление ошибки.

---

# Как запустить тесты

1. Открой терминал
2. Перейди в папку проекта

```bash
cd project
```

3. Запусти тесты

```bash
pytest
```
