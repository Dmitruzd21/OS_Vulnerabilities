# **Инъекции и уязвимости на уровне ОС**

**Ссылка на задание/требования:** https://github.com/netology-code/ibqa-homeworks/blob/main/2.%20Injections%20and%20vulnerabilities/homework_lecture2.md

## **Задание 1**

**План тестирования ПО на предмет переполнения**

- **Для числового поля** - Известно, что поле - двухбайтная величина. Нужно проверить числовое переполнение.

**1. Если в ПО заложен тип данных без знака:**

2 в степени 16 = 65 536

Диапазон чисел без знака: 0 - 65 536

**1.1 Привабить 1 к максимальному значению**

     STR: 65 536 + 1
     Результат при переполнении: 0

**2. Если в ПО заложен тип данных со знаком:**

2 в степени 15 = 32 768

Диапазон чисел со знаком: от -32 768 до 32 768

**2.1. Привабить 1 к максимальному значению**

     STR: 32 768 + 1
     Результат при переполнении: -32 768

**2.2. Отнять 1 из минимального отрицательного значения**

     STR: -32 758 - 1
     Результат при переполнении: 32 768

- **Для символьного поля** - Известно, что строка до 50 символов. Нужно проверить на переполнение буфера и input validation.

**1. Проверка на переполнение буфера (на уровне кода)**

STR: Выполнить следующий код:

    #include <stdio.h>

    int main()
    {
        char text[51] = "abcdefghijabcdefghijabcdefghijabcdefghijabcdefghij";

        char str1[51] = "12345678901234567890123456789012345678901234567890";

        str1[50] = '+';

        printf("Out_of_bounds_write=%s\n", str1);

        return 0;
    }

Результат при переполнении буфера: 12345678901234567890123456789012345678901234567890+abcdefghijabcdefghijabcdefghijabcdefghijabcdefghi

![Replit](/Replit.png)

**2. Input Validation (на уровне фронтенда)**

**2.1 Ввод 49 символов**

    STR: Ввести 49 символов в поле ввода.
    ОР: Успешный ввод

**2.2. Ввод 50 символов**

    STR: Ввести 50 символов в поле ввода.
    ОР: Успешный ввод

**2.3 Ввод 51 символов**

    STR: Ввести 51 символ в поле ввода.
    ОР: Ввод невозможен.

**2.4 Проверки, не связанные с переполнением**

- Пустой ввод
- Ввод пробелов
- Ввод только цифр
- Ввод кириллических/латинских символов
- Ввод специфических символов

**3. Проверка на ввод (на уровне бэкэнда)**

Инструмент тестирование API: Postman

**3.1 Ввод 50 символов в теле запроса**

    STR: 1. Составить POST запрос на сервер в соответсвии с документацией API
         2. Ввести 51 символ в телe запроса (значение поля ввода)
         3. Отправить запрос
    ОР: Ошибка в ответе на запрос

**4. Проверка формирования запросов при помощи анализатора трафика (если данные не должны передаваться в открытом виде)**

Инструменты: OWASP Zap или Charles

**4.1**

    STR: 1. Ввод данных в строку на фронтэнде
         2. Отправка данных на фронтенде
         3. Отслеживание запроса через анализатор трафика
         3. Просмотр тела запроса
    ОР: Тело запроса не содержит в открытом виде данные, которые вводились на фронтенде или данные передаются в зашифрованном виде.

**Дополнительные проверки на уровне фронтенда:**

- XSS - инъекции (фильтрация на фронтенде, проверка экранирования данных в запросе)

- SQL - инъекции (фильтрация на фронтенде, проверка экранирования данных в запросе)

**Дополнительные проверки на уровне бэкэнда:**

- XSS - инъекции

- SQL - инъекции

- Инъекция команда (например вызов командной строки при помощи "&& cmd")

## **Задание 2**

**"Whitelist для символьного поля (известно, что в нём указывается путь, по которому следует получить список файлов):**

- Имена доступных папок
- Имена доступных файлов
- Доступные расширения файлов
- Список конкретных путей
- Доступные имена хостов
- Разрешенные символы
- Допустимые шаблоны формирования путей
