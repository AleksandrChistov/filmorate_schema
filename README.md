# Filmorate DB schema

![DB tables Shema of the filmorate project](https://github.com/AleksandrChistov/filmorate_schema/blob/main/Снимок%20экрана%202025-04-27%20175628.png)

Данная схема базы данных предназначена для хранения информации о фильмах, жанрах, пользователях и их взаимоотношениях.

**Основные таблицы:**

*   **`user`:** Хранит информацию о зарегистрированных пользователях.
*   **`film`:**  Хранит информацию о фильмах.
*   **`genre`:** Хранит список жанров фильмов.
*   **`film_genre`:**  Связующая таблица, реализующая связь "многие-ко-многим" между фильмами и жанрами.
*   **`film_likes`:** Связующая таблица, реализующая связь "многие-ко-многим" между пользователями и фильмами (кто какие фильмы лайкнул).
*   **`friendship`:** Связующая таблица, реализующая связь "многие-ко-многим" между пользователями (кто с кем дружит).
*   **`mpa`:**  Хранит список рейтингов Ассоциации кинокомпаний (Motion Picture Association, сокращённо МРА). Эта оценка определяет возрастное ограничение для фильма.

**Примеры запросов:**

**1. Получение списка фильмов определенного жанра:**

```sql
SELECT f.id, f.name
FROM film f
JOIN film_genre fg ON f.id = fg.film_id
JOIN genre g ON fg.genre_id = g.id
JOIN mpa m ON f.mpa_id = m.id
WHERE g.name = 'Комедия';
```

**2. Получение списка пользователей, которые лайкнули определенный фильм:**

```sql
SELECT u.id, u.login
FROM user u
JOIN film_likes fl ON u.id = fl.user_id
WHERE fl.film_id = 456;
```

**3. Получение списка друзей пользователя:**

```sql
SELECT u.id, u.login
FROM user u
JOIN friendship f ON (u.id = f.friend_id AND f.user_id = 1) OR (u.id = f.user_id AND f.friend_id = 1)
WHERE f.is_mutual = TRUE;
```
`user_id = 1` - это пример ID пользователя, для которого ищем друзей.
