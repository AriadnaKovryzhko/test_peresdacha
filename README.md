# README — Тестирование HTML-формы входа
Описание проекта
В этом проекте реализована простая веб-страница с формой входа (login form) и набор автоматических тестов на Python для проверки её работы.

Страница содержит:

поле ввода email,

поле ввода пароля,

кнопку входа,

сообщение о результате авторизации.

Логика страницы реализована на JavaScript и проверяет правильность введённых данных.

Структура проекта
project/
│
├── index.html        # HTML страница с формой входа
├── tests/
│   └── test_login.py # Python тесты
└── README.md
Объяснение HTML-кода
1. Объявление типа документа
<!DOCTYPE html>
Сообщает браузеру, что используется HTML5.

2. Основная структура страницы
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Форма входа</title>
</head>
<body>
Что здесь происходит
lang="ru" — язык страницы русский.

<meta charset="UTF-8"> — поддержка кириллицы.

<title> — название вкладки браузера.

3. Заголовок страницы
<h2>Вход</h2>
Отображает заголовок формы входа.

4. Форма авторизации
<form id="loginForm">
Форма с уникальным идентификатором.
Она используется JavaScript и тестами для взаимодействия со страницей.

5. Поле Email
<label>Email:</label>
<input type="email" id="email" required>
Объяснение:

type="email" — браузер проверяет корректность email.

id="email" — используется в JavaScript и тестах.

required — поле обязательно.

6. Поле Пароля
<label>Пароль:</label>
<input type="password" id="password" required>
Особенности:

скрывает вводимые символы,

обязательное поле.

7. Кнопка отправки формы
<button type="submit">Войти</button>
При нажатии запускается обработчик формы.

8. Поле для вывода результата
<p id="result"></p>
Здесь отображается сообщение:

успешный вход

ошибка авторизации

JavaScript логика страницы
Обработчик отправки формы
document.getElementById("loginForm").addEventListener("submit", function(event) {
Отслеживает нажатие кнопки входа.

Отмена перезагрузки страницы
event.preventDefault();
Страница не обновляется после отправки формы.

Получение введённых данных
const email = document.getElementById("email").value.trim();
const password = document.getElementById("password").value.trim();
Здесь:

получаем email

получаем пароль

удаляем лишние пробелы.

Правильные данные для входа
const correctEmail = "test@example.com";
const correctPassword = "123456";
Это тестовые данные для входа.

Проверка авторизации
if (email === correctEmail && password === correctPassword)
Если данные совпадают — вход успешен.

Успешный вход
result.textContent = "Все верно!";
result.style.color = "green";
Показывается сообщение зелёного цвета.

Ошибка входа
result.textContent = "Неправильный логин или пароль!";
result.style.color = "red";
Показывается ошибка красным цветом.

Python тесты
Для тестирования используется:

Selenium

PyTest

Установка зависимостей
pip install selenium pytest
Пример теста
from selenium import webdriver
from selenium.webdriver.common.by import By
import time

def test_login_success():
    driver = webdriver.Chrome()
    driver.get("file:///path/to/index.html")

    driver.find_element(By.ID, "email").send_keys("test@example.com")
    driver.find_element(By.ID, "password").send_keys("123456")

    driver.find_element(By.CSS_SELECTOR, "button").click()
    time.sleep(1)

    result = driver.find_element(By.ID, "result").text
    assert result == "Все верно!"

    driver.quit()
Объяснение Python теста
Запуск браузера
driver = webdriver.Chrome()
Открывает Chrome для тестирования.

Открытие страницы
driver.get("file:///path/to/index.html")
Загружает HTML страницу.

Ввод данных
send_keys()
Симулирует ввод пользователя.

Нажатие кнопки
click()
Отправляет форму.

Проверка результата
assert result == "Все верно!"
Проверяет корректность работы авторизации.

Дополнительные тесты
Неправильный пароль
def test_login_wrong_password():
Проверяет, что система показывает ошибку.

Пустые поля
def test_empty_fields():
Проверяет валидацию формы.

Что проверяют тесты
Тесты проверяют:

правильный вход

неправильный пароль

неправильный email

пустые поля

отображение сообщений

работу JavaScript

Как запустить тесты
pytest
Или

pytest -v
