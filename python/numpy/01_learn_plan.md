## 📚 NumPy для джуна: Полный гайд

Вот список того, что **обязательно нужно знать** начинающему разработчику для эффективной работы с NumPy. Это основа для работы с данными в ML, CV и аналитике.

---

## 1️⃣ **СОЗДАНИЕ МАССИВОВ** (основа всего)

```python
import numpy as np

# Базовые способы создания
arr1 = np.array([1, 2, 3, 4, 5])           # из списка
arr2 = np.zeros((3, 4))                    # массив нулей
arr3 = np.ones((2, 3))                     # массив единиц
arr4 = np.eye(3)                           # единичная матрица
arr5 = np.arange(0, 10, 2)                 # [0, 2, 4, 6, 8]
arr6 = np.linspace(0, 1, 5)                # [0, 0.25, 0.5, 0.75, 1]
arr7 = np.random.rand(3, 3)                # случайные (0-1)
arr8 = np.random.randint(0, 10, (2, 3))    # случайные целые
arr9 = np.full((2, 2), 7)                  # заполнить числом

print(f"arange: {arr5}")
print(f"linspace: {arr6}")
```

---

## 2️⃣ **ИНДЕКСАЦИЯ И СРЕЗЫ** (доступ к данным)

```python
arr = np.array([[1, 2, 3, 4],
                [5, 6, 7, 8],
                [9, 10, 11, 12]])

# Доступ к элементам
print(arr[0, 2])        # 3 (строка 0, столбец 2)
print(arr[1][3])        # 8 (альтернативный способ)

# Срезы (как в списках, но для 2D)
print(arr[0:2, 1:3])    # строки 0-1, столбцы 1-2
# [[2 3]
#  [6 7]]

print(arr[:, 2])        # все строки, столбец 2 → [3, 7, 11]
print(arr[1, :])        # строка 1, все столбцы → [5, 6, 7, 8]

# Отрицательные индексы
print(arr[-1, -2])      # 11 (последняя строка, предпоследний столбец)

# Булева индексация (ОЧЕНЬ ВАЖНО!)
mask = arr > 6
print(arr[mask])        # [7, 8, 9, 10, 11, 12] - все элементы > 6

# Фильтрация по условию
filtered = arr[arr > 6]  # то же самое, краткая запись
print(filtered)
```

---

## 3️⃣ **ФОРМА И ИЗМЕНЕНИЕ РАЗМЕРНОСТИ**

```python
arr = np.array([[1, 2, 3],
                [4, 5, 6]])

print(arr.shape)        # (2, 3) - 2 строки, 3 столбца
print(arr.ndim)         # 2 - количество измерений
print(arr.size)         # 6 - общее количество элементов

# Изменение формы
reshaped = arr.reshape(3, 2)  # (3 строки, 2 столбца)
print(reshaped)

flattened = arr.flatten()     # в одномерный массив [1,2,3,4,5,6]
print(flattened)

# Добавление измерения
expanded = arr[np.newaxis, :]  # (1, 2, 3)
print(expanded.shape)

# Транспонирование
transposed = arr.T  # (3, 2)
print(transposed)
```

---

## 4️⃣ **БАЗОВЫЕ ОПЕРАЦИИ** (векторизация)

```python
a = np.array([1, 2, 3, 4])
b = np.array([10, 20, 30, 40])

# Арифметические операции (позлементно!)
print(a + b)        # [11 22 33 44]
print(a * b)        # [10 40 90 160]
print(a / b)        # [0.1 0.1 0.1 0.1]
print(a ** 2)       # [1 4 9 16]
print(np.sqrt(a))   # [1, 1.41, 1.73, 2]

# Скалярные операции
print(a + 10)       # [11 12 13 14]
print(a * 2)        # [2 4 6 8]

# Сравнения
print(a > 2)        # [False False  True  True]
print(a == 3)       # [False False  True False]

# Логические операции
mask = (a > 2) & (a < 4)  # AND
print(mask)         # [False False  True False]

mask = (a > 2) | (a < 2)  # OR
print(mask)
```

---

## 5️⃣ **АГРЕГИРУЮЩИЕ ФУНКЦИИ** (статистика)

```python
arr = np.array([[1, 2, 3],
                [4, 5, 6],
                [7, 8, 9]])

# По всему массиву
print(np.sum(arr))          # 45
print(np.mean(arr))         # 5.0
print(np.median(arr))       # 5.0
print(np.std(arr))          # 2.58
print(np.var(arr))          # 6.67
print(np.min(arr))          # 1
print(np.max(arr))          # 9
print(np.argmin(arr))       # 0 (индекс минимального)
print(np.argmax(arr))       # 8 (индекс максимального)

# По осям (axis=0 - по столбцам, axis=1 - по строкам)
print(np.sum(arr, axis=0))  # [12 15 18] - сумма по столбцам
print(np.sum(arr, axis=1))  # [6 15 24] - сумма по строкам
print(np.mean(arr, axis=0)) # [4. 5. 6.] - среднее по столбцам

# Кумулятивные суммы
print(np.cumsum(arr))       # [1, 3, 6, 10, 15, 21, 28, 36, 45]
```

---

## 6️⃣ **ОПЕРАЦИИ С МАТРИЦАМИ**

```python
A = np.array([[1, 2],
              [3, 4]])
B = np.array([[5, 6],
              [7, 8]])

# Умножение матриц (НЕ поэлементное!)
result = np.dot(A, B)  # или A @ B
print(result)
# [[19 22]
#  [43 50]]

# Поэлементное умножение (просто *)
print(A * B)
# [[5 12]
#  [21 32]]

# Скалярное произведение векторов
v1 = np.array([1, 2, 3])
v2 = np.array([4, 5, 6])
dot_product = np.dot(v1, v2)  # 1*4 + 2*5 + 3*6 = 32

# Норма вектора
norm = np.linalg.norm(v1)  # sqrt(1^2 + 2^2 + 3^2) = 3.74

# Решение систем уравнений
# 2x + y = 5
# x + 3y = 6
coeffs = np.array([[2, 1], [1, 3]])
consts = np.array([5, 6])
solution = np.linalg.solve(coeffs, consts)
print(solution)  # [1.8, 1.4]

# Собственные значения
eigenvalues, eigenvectors = np.linalg.eig(A)
```

---

## 7️⃣ **BROADCASTING** (одна из самых важных концепций!)

```python
# Broadcasting позволяет работать с массивами разных размеров

# Пример 1: Скаляр + массив
arr = np.array([[1, 2, 3],
                [4, 5, 6]])
print(arr + 10)  # 10 прибавляется к каждому элементу

# Пример 2: Вектор-строка + матрица
row = np.array([10, 20, 30])
print(arr + row)  # row добавляется к каждой строке

# Пример 3: Вектор-столбец + матрица
col = np.array([[10],
                [20]])
print(arr + col)  # col добавляется к каждому столбцу

# Пример 4: Скаляр + 3D массив
cube = np.random.rand(2, 3, 4)
print(cube * 2)   # все элементы умножаются на 2

# Правила broadcasting:
# 1. Если размерности разные, добавляются единицы слева
# 2. Размерность = 1 может растягиваться до нужного размера
# 3. Если размерности не совпадают и нет 1 - ошибка!

# Пример ошибки:
a = np.array([[1, 2], [3, 4]])  # (2, 2)
b = np.array([1, 2, 3])          # (3,)
# a + b  # ValueError! (2, 2) и (3,) несовместимы
```

---

## 8️⃣ **ОБЪЕДИНЕНИЕ МАССИВОВ**

```python
a = np.array([[1, 2],
              [3, 4]])
b = np.array([[5, 6],
              [7, 8]])

# Вертикальное объединение (по строкам)
v_stack = np.vstack((a, b))
print(v_stack)
# [[1 2]
#  [3 4]
#  [5 6]
#  [7 8]]

# Горизонтальное объединение (по столбцам)
h_stack = np.hstack((a, b))
print(h_stack)
# [[1 2 5 6]
#  [3 4 7 8]]

# Объединение по определенной оси
concat = np.concatenate((a, b), axis=0)  # axis=0 - вертикально
concat = np.concatenate((a, b), axis=1)  # axis=1 - горизонтально

# Добавление строки/столбца
arr = np.array([[1, 2, 3],
                [4, 5, 6]])
new_row = np.array([[7, 8, 9]])
arr_with_row = np.vstack((arr, new_row))

new_col = np.array([[10], [20]])
arr_with_col = np.hstack((arr, new_col))
```

---

## 9️⃣ **РАЗДЕЛЕНИЕ МАССИВОВ**

```python
arr = np.array([1, 2, 3, 4, 5, 6])

# Разделение на части
split1 = np.split(arr, 2)     # [array([1,2,3]), array([4,5,6])]
split2 = np.split(arr, 3)     # [array([1,2]), array([3,4]), array([5,6])]

# Разделение 2D массива
matrix = np.array([[1, 2, 3, 4],
                   [5, 6, 7, 8],
                   [9, 10, 11, 12]])

# Вертикальное разделение
v_split = np.vsplit(matrix, 3)  # 3 части по строкам

# Горизонтальное разделение
h_split = np.hsplit(matrix, 2)  # 2 части по столбцам
```

---

## 🔟 **UNIQUE И СОРТИРОВКА**

```python
arr = np.array([1, 2, 2, 3, 3, 3, 4, 5, 5])

# Уникальные значения
unique = np.unique(arr)
print(unique)  # [1 2 3 4 5]

# Количество уникальных значений
counts = np.unique(arr, return_counts=True)
print(counts)  # (array([1,2,3,4,5]), array([1,2,3,1,2]))

# Поиск дубликатов
unique, counts = np.unique(arr, return_counts=True)
duplicates = unique[counts > 1]
print(f"Дубликаты: {duplicates}")  # [2 3 5]

# Сортировка
unsorted = np.array([3, 1, 4, 1, 5, 9, 2])
sorted_arr = np.sort(unsorted)      # [1 1 2 3 4 5 9]
sorted_indices = np.argsort(unsorted)  # [1 3 6 0 2 4 5]

# Сортировка 2D массива
matrix = np.array([[3, 1, 2],
                   [6, 5, 4]])
sorted_rows = np.sort(matrix, axis=1)  # сортировка по строкам
sorted_cols = np.sort(matrix, axis=0)  # сортировка по столбцам
```

---

## 1️⃣1️⃣ **ФУНКЦИИ ДЛЯ МАШИННОГО ОБУЧЕНИЯ**

```python
# Train/test split
from sklearn.model_selection import train_test_split

X = np.random.rand(100, 10)  # 100 образцов, 10 признаков
y = np.random.randint(0, 2, 100)  # 100 меток

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

print(f"X_train: {X_train.shape}")  # (80, 10)
print(f"X_test: {X_test.shape}")    # (20, 10)

# Нормализация
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
X_normalized = scaler.fit_transform(X_train)  # среднее=0, std=1

# Кросс-валидация (простой пример)
from sklearn.model_selection import KFold

kf = KFold(n_splits=5, shuffle=True, random_state=42)
for train_idx, val_idx in kf.split(X):
    X_train_fold = X[train_idx]
    X_val_fold = X[val_idx]
```

---

## 1️⃣2️⃣ **ПОЛЕЗНЫЕ ТРЮКИ**

```python
# 1. Клиппинг значений
arr = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
clipped = np.clip(arr, 3, 7)  # все значения в [3, 7]
print(clipped)  # [3 3 3 4 5 6 7 7 7 7]

# 2. Замена значений по условию
arr = np.array([1, 2, 3, 4, 5])
arr[arr > 3] = 0  # все > 3 заменяем на 0
print(arr)  # [1 2 3 0 0]

# 3. np.where (тройной оператор)
arr = np.array([1, 2, 3, 4, 5])
result = np.where(arr > 3, 100, 0)
print(result)  # [0 0 0 100 100]

# 4. np.any / np.all
arr = np.array([1, 2, 3, 4, 5])
print(np.any(arr > 4))   # True (есть элемент > 4)
print(np.all(arr > 0))   # True (все элементы > 0)

# 5. Умное индексирование
arr = np.array([10, 20, 30, 40, 50])
indices = np.array([0, 2, 4])
print(arr[indices])  # [10 30 50]

# 6. np.tile (повторение массива)
arr = np.array([1, 2, 3])
tiled = np.tile(arr, 3)
print(tiled)  # [1 2 3 1 2 3 1 2 3]

# 7. np.roll (сдвиг элементов)
arr = np.array([1, 2, 3, 4, 5])
rolled = np.roll(arr, 2)  # сдвиг на 2 вправо
print(rolled)  # [4 5 1 2 3]

# 8. Удаление измерений с размером 1
arr = np.array([[[1, 2, 3]]])  # (1, 1, 3)
squeezed = np.squeeze(arr)  # (3,)
print(squeezed.shape)  # (3,)

# 9. Добавление измерения
arr = np.array([1, 2, 3])  # (3,)
expanded = arr[:, np.newaxis]  # (3, 1)
print(expanded.shape)

# 10. Уникальные значения с позициями
arr = np.array([1, 2, 3, 2, 1, 4, 2])
unique, indices, inverse = np.unique(
    arr, return_index=True, return_inverse=True
)
print(f"Уникальные: {unique}")  # [1 2 3 4]
print(f"Индексы: {indices}")    # [0 1 2 5]
print(f"Inverse: {inverse}")    # [0 1 2 1 0 3 1]
```

---

## 1️⃣3️⃣ **ПРИМЕР ИЗ РЕАЛЬНОЙ ЖИЗНИ (распознавание букв)**

```python
# Ваш случай с HOG + SVM

# 1. Загрузка данных
X_train = np.random.rand(1000, 1764)  # 1000 изображений, 1764 HOG-признаков
y_train = np.random.randint(0, 33, 1000)  # 33 класса (буквы)

# 2. Нормализация
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)

# 3. Создание меток ошибок
test_labels = np.random.randint(0, 33, 100)
y_test_pred = np.random.randint(0, 33, 100)

# 4. Поиск ошибок (ваш код!)
misclassified_idx = np.where((test_labels != y_test_pred))[0]

print(f"Найдено {len(misclassified_idx)} ошибок")
print(f"Индексы ошибок: {misclassified_idx[:5]}")

# 5. Анализ ошибок
for idx in misclassified_idx[:5]:
    true_class = test_labels[idx]
    pred_class = y_test_pred[idx]
    print(f"Образец {idx}: истинный={true_class}, предсказанный={pred_class}")

# 6. Вычисление метрик
accuracy = np.mean(test_labels == y_test_pred)
print(f"Точность: {accuracy*100:.2f}%")

# 7. Матрица ошибок
from sklearn.metrics import confusion_matrix
cm = confusion_matrix(test_labels, y_test_pred)
print(f"Размер матрицы ошибок: {cm.shape}")
```

---

## 📋 **Краткий чек-лист для джуна**

### ✅ **Обязательно знать:**
- [x] Создание массивов (`array`, `zeros`, `ones`, `arange`, `random`)
- [x] Индексация и срезы (включая булеву маску)
- [x] `.shape`, `.reshape()`, `.flatten()`, `.T`
- [x] Поэлементные операции (`+`, `-`, `*`, `/`, `**`)
- [x] Умножение матриц (`@` или `np.dot`)
- [x] Агрегации (`sum`, `mean`, `std`, `min`, `max`, `argmax`)
- [x] Broadcasting (как работают операции с разными размерами)
- [x] `np.where()` для поиска индексов
- [x] Фильтрация по условию (`arr[arr > threshold]`)
- [x] `np.unique()` для уникальных значений
- [x] Объединение (`vstack`, `hstack`, `concatenate`)

### 📚 **Полезно знать:**
- [ ] `np.einsum` (для сложных матричных операций)
- [ ] `np.fft` (преобразование Фурье)
- [ ] `np.polynomial` (полиномы)
- [ ] Структурированные массивы
- [ ] `np.memmap` (работа с большими данными)

---

## 🎯 **Самые частые ошибки джунов**

```python
# ❌ ОШИБКА 1: Забывают про копии
arr = np.array([1, 2, 3])
arr_copy = arr  # НЕ копия! это ссылка
arr_copy[0] = 100
print(arr)  # [100 2 3] - изменился исходный!

# ✅ Правильно:
arr_copy = arr.copy()
arr_copy[0] = 100
print(arr)  # [1 2 3] - исходный не изменился

# ❌ ОШИБКА 2: Путают поэлементное и матричное умножение
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])
wrong = A * B  # поэлементное
correct = A @ B  # матричное

# ❌ ОШИБКА 3: Забывают про оси
arr = np.array([[1, 2, 3], [4, 5, 6]])
print(np.mean(arr))      # среднее по всем
print(np.mean(arr, axis=0))  # среднее по столбцам
print(np.mean(arr, axis=1))  # среднее по строкам

# ❌ ОШИБКА 4: Используют циклы вместо векторизации
# ПЛОХО:
result = []
for x in arr:
    result.append(x * 2)
# ХОРОШО:
result = arr * 2
```

---

## 🚀 **Советы для быстрого обучения**

1. **Практикуйтесь на реальных данных** - возьмите ваш датасет с буквами
2. **Изучайте чужие решения** - смотрите, как используют numpy в проектах ML
3. **Используйте Jupyter Notebook** - видите результат сразу
4. **Запомните важные функции** - составьте шпаргалку
5. **Изучайте broadcasting** - это экономит 90% циклов