Отличный пример! Давайте я покажу, как использовать множества в Python для более декларативного кода на конкретных примерах.

## 1. **Объединение, пересечение и разность**

```python
# ❌ Без множеств (императивный стиль)
def find_common_friends(user1_friends, user2_friends):
    common = []
    for friend in user1_friends:
        if friend in user2_friends:
            common.append(friend)
    return common

# ✅ С множествами (декларативный стиль)
def find_common_friends(user1_friends, user2_friends):
    return list(set(user1_friends) & set(user2_friends))  # Пересечение

# Пример
friends_alice = {"Bob", "Charlie", "David", "Eva"}
friends_bob = {"Charlie", "Eva", "Frank", "Grace"}

# Общие друзья
common = friends_alice & friends_bob
print(f"Общие друзья: {common}")  # {'Charlie', 'Eva'}

# Все друзья (объединение)
all_friends = friends_alice | friends_bob
print(f"Все друзья: {all_friends}")  # {'Bob', 'Charlie', 'David', 'Eva', 'Frank', 'Grace'}

# Только у Алисы (разность)
only_alice = friends_alice - friends_bob
print(f"Только у Алисы: {only_alice}")  # {'Bob', 'David'}
```

## 2. **Проверка на подмножество**

```python
# ❌ Без множеств
def is_subset(small, big):
    for item in small:
        if item not in big:
            return False
    return True

# ✅ С множествами
def is_subset(small, big):
    return small.issubset(big)  # или small <= big

# Пример
required_permissions = {"read", "write"}
user_permissions = {"read", "write", "delete", "admin"}

if required_permissions <= user_permissions:
    print("У пользователя есть все необходимые права")
else:
    print("Недостаточно прав")
```

## 3. **Поиск уникальных элементов**

```python
# ❌ Без множеств (сложно и медленно)
def get_unique_words(text):
    words = text.lower().split()
    unique = []
    for word in words:
        if word not in unique:
            unique.append(word)
    return unique

# ✅ С множествами (просто и быстро)
def get_unique_words(text):
    return set(text.lower().split())

text = "hello world hello python world"
unique_words = get_unique_words(text)
print(f"Уникальные слова: {unique_words}")  # {'hello', 'world', 'python'}
```

## 4. **Проверка дубликатов**

```python
# ❌ Без множеств
def has_duplicates(items):
    seen = []
    for item in items:
        if item in seen:
            return True
        seen.append(item)
    return False

# ✅ С множествами
def has_duplicates(items):
    return len(items) != len(set(items))

numbers = [1, 2, 3, 4, 5, 3, 6]
print(f"Есть дубликаты? {has_duplicates(numbers)}")  # True
```

## 5. **Фильтрация по нескольким условиям**

```python
# Представьте, что у вас есть список товаров
products = [
    {"id": 1, "name": "Laptop", "category": "electronics", "in_stock": True},
    {"id": 2, "name": "Book", "category": "books", "in_stock": True},
    {"id": 3, "name": "Phone", "category": "electronics", "in_stock": False},
    {"id": 4, "name": "Pen", "category": "office", "in_stock": True},
    {"id": 5, "name": "Tablet", "category": "electronics", "in_stock": True},
]

# ✅ С множествами - элегантное решение
allowed_categories = {"electronics", "books"}
in_stock_products = {p["id"] for p in products if p["in_stock"]}
electronics_products = {p["id"] for p in products if p["category"] in allowed_categories}

# Товары, которые есть в наличии И относятся к разрешенным категориям
available_allowed = in_stock_products & electronics_products

print(f"Доступные товары из разрешенных категорий: {available_allowed}")
# {1, 5} - Laptop и Tablet
```

## 6. **Анализ данных (реальный пример)**

```python
# Анализ логов
logs = [
    "user1 accessed page /home",
    "user2 accessed page /profile",
    "user1 accessed page /settings",
    "user3 accessed page /home",
    "user1 accessed page /profile",
    "user2 accessed page /logout",
]

# ❌ Без множеств (сложно)
def get_unique_users(logs):
    users = []
    for log in logs:
        user = log.split()[0]
        if user not in users:
            users.append(user)
    return users

# ✅ С множествами (просто)
def get_unique_users(logs):
    return {log.split()[0] for log in logs}

def get_visited_pages(logs):
    return {log.split()[-1] for log in logs}

unique_users = get_unique_users(logs)
pages = get_visited_pages(logs)

print(f"Уникальные пользователи: {unique_users}")  # {'user1', 'user2', 'user3'}
print(f"Посещенные страницы: {pages}")  # {'/home', '/profile', '/settings', '/logout'}

# Пользователи, которые посетили страницу /home
home_users = {log.split()[0] for log in logs if "/home" in log}
print(f"Пользователи на /home: {home_users}")  # {'user1', 'user3'}
```

## 7. **Сравнение производительности**

```python
import time

# Создаем большой список
big_list = list(range(10000))
search_items = list(range(5000, 15000))

# ❌ Поиск в списке O(n)
start = time.time()
found_list = [x for x in search_items if x in big_list]
print(f"Поиск в списке: {time.time() - start:.4f} сек")

# ✅ Поиск в множестве O(1)
big_set = set(big_list)
start = time.time()
found_set = [x for x in search_items if x in big_set]
print(f"Поиск в множестве: {time.time() - start:.4f} сек")
```

## 8. **Симметрическая разность (элементы только в одном множестве)**

```python
# Навыки двух разработчиков
alice_skills = {"Python", "Java", "SQL", "Docker"}
bob_skills = {"Python", "JavaScript", "SQL", "AWS"}

# ❌ Без множеств
def get_different_skills(skills1, skills2):
    diff = []
    for skill in skills1:
        if skill not in skills2:
            diff.append(skill)
    for skill in skills2:
        if skill not in skills1:
            diff.append(skill)
    return diff

# ✅ С множествами
def get_different_skills(skills1, skills2):
    return skills1 ^ skills2  # Симметрическая разность

different = alice_skills ^ bob_skills
print(f"Разные навыки: {different}")  # {'Java', 'Docker', 'JavaScript', 'AWS'}
```

## Вывод

Использование множеств позволяет:
1. **Писать более читаемый код** без вложенных циклов
2. **Выражать алгоритмы декларативно** (что нужно сделать, а не как)
3. **Улучшить производительность** (поиск O(1) вместо O(n))
4. **Использовать готовые теоретико-множественные операции** (∩, ∪, ⊂)

Это именно то, что имеется в виду под "более декларативным способом, избегая вложенных циклов и условий"! 🚀