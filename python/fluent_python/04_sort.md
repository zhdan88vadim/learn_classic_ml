Да, это **абсолютно правильно**! Отличный пример, который показывает все основные возможности сортировки в Python. Давайте разберем каждый шаг:

## Пошаговый разбор вашего кода:

```python
>>> fruits = ['grape', 'raspberry', 'apple', 'banana']
>>> sorted(fruits)
['apple', 'banana', 'grape', 'raspberry']  # ❶
```
**❶** - `sorted()` возвращает **НОВЫЙ** отсортированный список (по алфавиту). Исходный список не меняется.

```python
>>> fruits
['grape', 'raspberry', 'apple', 'banana']  # ❷
```
**❷** - Исходный список остался неизменным (как и должно быть).

```python
>>> sorted(fruits, reverse=True)
['raspberry', 'grape', 'banana', 'apple']  # ❸
```
**❸** - Сортировка в обратном порядке (по алфавиту, от Я до А).

```python
>>> sorted(fruits, key=len)
['grape', 'apple', 'banana', 'raspberry']  # ❹
```
**❹** - Сортировка по **длине строк** (от самой короткой к самой длинной):
- 'grape' - 5 букв
- 'apple' - 5 букв
- 'banana' - 6 букв
- 'raspberry' - 9 букв

```python
>>> sorted(fruits, key=len, reverse=True)
['raspberry', 'banana', 'grape', 'apple']  # ❺
```
**❺** - Сортировка по длине в обратном порядке (от самой длинной к самой короткой).

```python
>>> fruits
['grape', 'raspberry', 'apple', 'banana']  # ❻
```
**❻** - Исходный список ВСЕ ЕЩЕ не изменился, потому что мы использовали только `sorted()`.

```python
>>> fruits.sort()  # ❼
>>> fruits
['apple', 'banana', 'grape', 'raspberry']  # ❽
```
**❼** - `sort()` - это метод списка, который сортирует **В ПРЯМОМ СМЫСЛЕ** (in-place).
**❽** - Теперь исходный список **ИЗМЕНИЛСЯ** и стал отсортированным.

---

## Важные наблюдения:

### 1. `sorted()` vs `.sort()` - главное отличие
```python
# sorted() - возвращает НОВЫЙ список
new_list = sorted(fruits)  # ✅ возвращает список
print(fruits)  # исходный не изменился

# .sort() - изменяет существующий список
result = fruits.sort()  # ❌ возвращает None
print(fruits)  # исходный ИЗМЕНИЛСЯ
```

### 2. Все варианты `key` (очень мощная возможность)
```python
# Сортировка по длине (как у вас)
sorted(fruits, key=len)

# Другие примеры key:
numbers = ['10', '2', '30', '4']
sorted(numbers, key=int)  # ['2', '4', '10', '30'] - как числа

# Сортировка по последней букве
sorted(fruits, key=lambda x: x[-1])  # ['banana', 'grape', 'apple', 'raspberry']

# Сортировка по второму символу
sorted(fruits, key=lambda x: x[1])  # ['banana', 'raspberry', 'apple', 'grape']
```

### 3. Комбинирование параметров
```python
# Сортировка по длине, а при равной длине - по алфавиту
sorted(fruits, key=lambda x: (len(x), x))
# ['apple', 'grape', 'banana', 'raspberry']
# grape и apple оба по 5 букв, но apple идет первым по алфавиту
```

### 4. Стабильность сортировки
Python использует **стабильную сортировку** (Timsort):
```python
data = [('apple', 2), ('banana', 1), ('grape', 2), ('raspberry', 1)]
sorted(data, key=lambda x: x[1])  
# [('banana', 1), ('raspberry', 1), ('apple', 2), ('grape', 2)]
# Порядок элементов с одинаковым ключом сохраняется!
```

---

## Итоговая таблица для вашего примера:

| Команда | Результат | Исходный список изменился? |
|---------|-----------|---------------------------|
| `sorted(fruits)` | `['apple', 'banana', 'grape', 'raspberry']` | ❌ Нет |
| `sorted(fruits, reverse=True)` | `['raspberry', 'grape', 'banana', 'apple']` | ❌ Нет |
| `sorted(fruits, key=len)` | `['grape', 'apple', 'banana', 'raspberry']` | ❌ Нет |
| `sorted(fruits, key=len, reverse=True)` | `['raspberry', 'banana', 'grape', 'apple']` | ❌ Нет |
| `fruits.sort()` | `None` (но список изменился) | ✅ **ДА** |

**Ваш код безупречен!** 👍 Все работает именно так, как задумано в Python.