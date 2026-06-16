Отлично! Давайте детально разберем, как работает `plot_heatmap` на ваших данных. Это очень наглядный пример!

## 🔍 Детальный разбор работы `plot_heatmap`

### **Исходные данные:**
```python
local_exp = {
    7: [(45, 0.57), (12, 0.34), (5, -0.12), (8, -0.05), (3, 0.02)],
    3: [(12, 0.45), (5, 0.21), (8, -0.03)],
}

# Предположим, мы анализируем класс 7
label = 7
image = X_test[0].reshape(28, 28)  # Изображение 28x28
```

### **Пошаговое выполнение:**

#### **Шаг 1: Получение весов**
```python
weights = dict(explanation.local_exp[label])
# weights = {45: 0.57, 12: 0.34, 5: -0.12, 8: -0.05, 3: 0.02}
```

#### **Шаг 2: Создание маски**
```python
# Создаем пустую матрицу размером с изображение
mask = np.zeros_like(image_display)  # матрица 28x28, все значения = 0

# Для каждого сегмента заполняем его область значением веса
for segment, weight in weights.items():
    # explanation.segments - это матрица 28x28, где каждый пиксель 
    # помечен номером сегмента (0, 1, 2, ..., 49)
    mask[explanation.segments == segment] = weight
```

#### **Визуализация маски:**

Давайте представим, как выглядит `explanation.segments`:

```python
# explanation.segments - это карта сегментов (матрица 28x28)
# Каждое число обозначает номер суперпикселя
segments_map = np.array([
    [0, 0, 0, 1, 1, 1, 2, 2, 2, ...],  # строка 0
    [0, 0, 0, 1, 1, 1, 2, 2, 2, ...],  # строка 1
    [3, 3, 3, 4, 4, 4, 5, 5, 5, ...],  # строка 2
    ...
])  # форма (28, 28)
```

Когда мы делаем:
```python
mask[explanation.segments == 45] = 0.57
# Все пиксели, где segments_map == 45, получают значение 0.57

mask[explanation.segments == 12] = 0.34
# Все пиксели, где segments_map == 12, получают значение 0.34

mask[explanation.segments == 5] = -0.12
# Все пиксели, где segments_map == 5, получают значение -0.12

# И так далее...
```

### **Визуализация результата:**

```python
# Левая часть: оригинальное изображение
plt.subplot(1, 2, 1)
plt.imshow(image_display, cmap='gray')
plt.title('Оригинал')

# Правая часть: тепловая карта
plt.subplot(1, 2, 2)
# Сначала показываем изображение (60% прозрачности)
plt.imshow(image_display, cmap='gray', alpha=0.6)
# Поверх накладываем маску с весами (40% прозрачности)
# Красный = положительный вклад, Синий = отрицательный
plt.imshow(mask, cmap='RdBu', alpha=0.4)
```

## 📊 Визуальный пример

```python
import numpy as np
import matplotlib.pyplot as plt
from skimage.segmentation import slic, mark_boundaries

# Создаем тестовые данные
np.random.seed(42)

# 1. Создаем тестовое изображение (простая цифра)
image = np.zeros((28, 28))
# Рисуем простую цифру 7
image[5:12, 10:15] = 1  # вертикальная палка
image[5:10, 15:20] = 1  # горизонтальная палка

# 2. Создаем сегментацию
segments = slic(image, n_segments=10, compactness=10, sigma=1, channel_axis=None)

# 3. Создаем explanation (имитируем)
class ExplanationMock:
    def __init__(self):
        self.segments = segments
        self.local_exp = {
            7: [(1, 0.57), (2, 0.34), (3, -0.12), (4, -0.05), (5, 0.02)],
            3: [(2, 0.45), (3, 0.21), (4, -0.03)],
        }

explanation = ExplanationMock()
label = 7

# 4. Функция plot_heatmap
def plot_heatmap(explanation, label, image):
    # Если изображение RGB, берем первый канал
    if len(image.shape) == 3 and image.shape[2] == 3:
        image_display = image[:, :, 0]
    else:
        image_display = image
    
    weights = dict(explanation.local_exp[label])
    print(f"Веса для класса {label}: {weights}")
    
    mask = np.zeros_like(image_display)
    for segment, weight in weights.items():
        mask[explanation.segments == segment] = weight
    
    print(f"Уникальные значения в маске: {np.unique(mask)}")
    print(f"Количество ненулевых пикселей: {np.count_nonzero(mask)}")
    
    plt.figure(figsize=(15, 5))
    
    # Показываем сегментацию
    plt.subplot(1, 3, 1)
    plt.imshow(mark_boundaries(image_display, explanation.segments), cmap='gray')
    plt.title('Сегментация')
    plt.axis('off')
    
    # Оригинал
    plt.subplot(1, 3, 2)
    plt.imshow(image_display, cmap='gray')
    plt.title('Оригинал')
    plt.axis('off')
    
    # Тепловая карта
    plt.subplot(1, 3, 3)
    plt.imshow(image_display, cmap='gray', alpha=0.6)
    plt.imshow(mask, cmap='RdBu', alpha=0.5)
    plt.title(f'Тепловая карта (класс {label})')
    plt.axis('off')
    plt.colorbar()
    plt.show()
    
    # Дополнительно: показываем маску отдельно
    plt.figure(figsize=(10, 5))
    plt.subplot(1, 2, 1)
    plt.imshow(mask, cmap='RdBu')
    plt.title('Маска с весами')
    plt.colorbar()
    plt.axis('off')
    
    plt.subplot(1, 2, 2)
    # Показываем, какие сегменты положительные (красные), какие отрицательные (синие)
    plt.imshow(mask > 0, cmap='Reds', alpha=0.5)
    plt.imshow(mask < 0, cmap='Blues', alpha=0.5)
    plt.title('Положительные (красный) vs Отрицательные (синий)')
    plt.axis('off')
    plt.show()

# Запускаем
plot_heatmap(explanation, label, image)
```

## 🎯 Что происходит с каждым сегментом

```python
# Давайте разберем каждый сегмент отдельно
weights = {45: 0.57, 12: 0.34, 5: -0.12, 8: -0.05, 3: 0.02}

print("Детальный разбор:")
print("="*60)

for segment, weight in weights.items():
    # Находим все пиксели этого сегмента
    segment_mask = (explanation.segments == segment)
    n_pixels = np.sum(segment_mask)
    
    # Находим их координаты
    if n_pixels > 0:
        y_coords, x_coords = np.where(segment_mask)
        center_y = y_coords.mean()
        center_x = x_coords.mean()
        
        color = "🟥 ПОЛОЖИТЕЛЬНЫЙ" if weight > 0 else "🟦 ОТРИЦАТЕЛЬНЫЙ"
        strength = "СИЛЬНЫЙ" if abs(weight) > 0.3 else "СРЕДНИЙ" if abs(weight) > 0.1 else "СЛАБЫЙ"
        
        print(f"Сегмент {segment}:")
        print(f"  Вес: {weight:+.4f} ({color}, {strength})")
        print(f"  Размер: {n_pixels} пикселей")
        print(f"  Центр: ({center_y:.1f}, {center_x:.1f})")
        print(f"  Вклад: {'увеличивает' if weight > 0 else 'уменьшает'} вероятность класса")
        print()
```

## 📊 Визуализация процесса

```python
# Создаем пошаговую визуализацию создания маски
def visualize_mask_creation(explanation, label, image):
    weights = dict(explanation.local_exp[label])
    
    fig, axes = plt.subplots(2, 3, figsize=(15, 10))
    
    # 1. Оригинал
    axes[0, 0].imshow(image, cmap='gray')
    axes[0, 0].set_title("1. Оригинальное изображение")
    axes[0, 0].axis('off')
    
    # 2. Сегментация
    axes[0, 1].imshow(mark_boundaries(image, explanation.segments), cmap='gray')
    axes[0, 1].set_title("2. Сегментация на суперпиксели")
    axes[0, 1].axis('off')
    
    # 3. Маска с весами
    mask = np.zeros_like(image)
    for segment, weight in weights.items():
        mask[explanation.segments == segment] = weight
    
    axes[0, 2].imshow(mask, cmap='RdBu')
    axes[0, 2].set_title("3. Маска с весами")
    axes[0, 2].axis('off')
    
    # 4-6. Показываем каждый сегмент по отдельности
    for i, (segment, weight) in enumerate(list(weights.items())[:3]):
        seg_mask = (explanation.segments == segment)
        seg_image = np.zeros_like(image)
        seg_image[seg_mask] = image[seg_mask]
        
        axes[1, i].imshow(mark_boundaries(seg_image, seg_mask), cmap='gray')
        axes[1, i].set_title(f"Сегмент {segment}\nВес: {weight:+.4f}")
        axes[1, i].axis('off')
    
    plt.tight_layout()
    plt.show()

# Использование
visualize_mask_creation(explanation, label, image)
```

## 💡 Ключевые моменты

1. **`mask`** — это матрица того же размера, что и изображение
2. **Каждый пиксель** получает значение веса своего сегмента
3. **Положительные веса** → красные оттенки (увеличивают вероятность)
4. **Отрицательные веса** → синие оттенки (уменьшают вероятность)
5. **Прозрачность** позволяет видеть оригинальное изображение под маской

## 🎨 Цветовая схема `RdBu`

```python
# RdBu - Red-Blue colormap
# Красный: положительные значения
# Белый: ноль
# Синий: отрицательные значения

# Пример:
weights_example = [-0.5, -0.25, 0, 0.25, 0.5]
# -0.5 → темно-синий
# -0.25 → светло-синий
# 0 → белый
# 0.25 → светло-красный
# 0.5 → темно-красный
```

Теперь вы полностью понимаете, как работает тепловая карта! Это очень мощный инструмент для визуализации важности пикселей. 🚀





























































Отличный вопрос! Это ключевой момент понимания LIME. Давайте детально разберем, как работает маппинг суперпикселя на сегмент.

## 🎯 Суть маппинга

Суперпиксель (сегмент) — это **группа пикселей**, которые объединены в один сегмент. Маппинг работает так:

```python
# explanation.segments - это матрица (28, 28), где КАЖДЫЙ пиксель 
# содержит номер сегмента, к которому он принадлежит

segments = explanation.segments
print(segments.shape)  # (28, 28)

# Пример матрицы сегментов (упрощенно 5x5 вместо 28x28):
segments_example = np.array([
    [0, 0, 0, 1, 1],
    [0, 0, 0, 1, 1],
    [0, 0, 0, 2, 2],
    [3, 3, 3, 2, 2],
    [3, 3, 3, 2, 2]
])
```

## 📊 Пошаговый разбор

### **1. Создание сегментов (SLIC)**

```python
from skimage.segmentation import slic

# Сегментация создает карту, где каждый пиксель получает номер сегмента
segments = slic(image, n_segments=50, compactness=10)

# segments - это матрица (28, 28) с числами от 0 до 49
# Например:
# [[0, 0, 0, 1, 1, 1, ...],
#  [0, 0, 0, 1, 1, 1, ...],
#  [2, 2, 2, 3, 3, 3, ...],
#  ...]
```

### **2. Получение весов из LIME**

```python
# LIME возвращает веса для КАЖДОГО сегмента
weights = {
    45: 0.57,   # Сегмент 45 имеет вес 0.57
    12: 0.34,   # Сегмент 12 имеет вес 0.34
    5: -0.12,   # Сегмент 5 имеет вес -0.12
}
```

### **3. Маппинг весов на пиксели**

```python
# Создаем пустую маску
mask = np.zeros_like(image)  # (28, 28) все нули

# Для каждого сегмента:
for segment, weight in weights.items():
    # Находим ВСЕ пиксели, которые принадлежат этому сегменту
    pixel_positions = (explanation.segments == segment)
    
    # ВСЕМ этим пикселям присваиваем ОДИН и тот же вес
    mask[pixel_positions] = weight
```

## 🔍 Детальный пример

Давайте создадим полностью рабочий пример:

```python
import numpy as np
import matplotlib.pyplot as plt
from skimage.segmentation import slic
from skimage.segmentation import mark_boundaries

# ============================================
# 1. СОЗДАЕМ ТЕСТОВОЕ ИЗОБРАЖЕНИЕ (10x10)
# ============================================
image = np.zeros((10, 10))
# Рисуем квадрат
image[2:8, 2:8] = 1

print("Исходное изображение:")
print(image)
print()

# ============================================
# 2. СОЗДАЕМ СЕГМЕНТАЦИЮ (4 сегмента)
# ============================================
# Вручную создаем сегментацию для примера
segments = np.array([
    [0, 0, 0, 0, 1, 1, 1, 1, 1, 1],
    [0, 0, 0, 0, 1, 1, 1, 1, 1, 1],
    [0, 0, 0, 0, 1, 1, 1, 1, 1, 1],
    [0, 0, 0, 0, 1, 1, 1, 1, 1, 1],
    [2, 2, 2, 2, 3, 3, 3, 3, 3, 3],
    [2, 2, 2, 2, 3, 3, 3, 3, 3, 3],
    [2, 2, 2, 2, 3, 3, 3, 3, 3, 3],
    [2, 2, 2, 2, 3, 3, 3, 3, 3, 3],
    [2, 2, 2, 2, 3, 3, 3, 3, 3, 3],
    [2, 2, 2, 2, 3, 3, 3, 3, 3, 3]
])

print("Карта сегментов (каждый пиксель помечен номером сегмента):")
print(segments)
print()

# ============================================
# 3. ВЕСА ОТ LIME
# ============================================
# LIME определил важность каждого сегмента
weights = {
    0: 0.8,   # Сегмент 0 очень важен (положительный)
    1: 0.3,   # Сегмент 1 важен (положительный)
    2: -0.5,  # Сегмент 2 мешает (отрицательный)
    3: 0.1    # Сегмент 3 слабо важен (положительный)
}

print("Веса сегментов от LIME:")
for seg, w in weights.items():
    print(f"  Сегмент {seg}: {w:+.2f}")
print()

# ============================================
# 4. МАППИНГ: СОЗДАНИЕ МАСКИ
# ============================================
mask = np.zeros_like(image, dtype=float)

print("ШАГ ЗА ШАГОМ: КАК ЗАПОЛНЯЕТСЯ МАСКА")
print("="*50)

for segment, weight in weights.items():
    # Находим все пиксели этого сегмента
    pixel_positions = (segments == segment)
    n_pixels = np.sum(pixel_positions)
    
    # Присваиваем вес всем пикселям сегмента
    mask[pixel_positions] = weight
    
    print(f"\nСегмент {segment} (вес: {weight:+.2f}):")
    print(f"  Количество пикселей: {n_pixels}")
    print(f"  Позиции: {np.argwhere(pixel_positions).tolist()[:5]}...")  # первые 5
    print(f"  Маска после присвоения:")
    print(mask)

# ============================================
# 5. ВИЗУАЛИЗАЦИЯ
# ============================================
fig, axes = plt.subplots(2, 3, figsize=(15, 10))

# 1. Оригинальное изображение
axes[0, 0].imshow(image, cmap='gray')
axes[0, 0].set_title("1. Оригинальное изображение")
axes[0, 0].axis('off')

# 2. Карта сегментов
axes[0, 1].imshow(segments, cmap='tab10')
axes[0, 1].set_title("2. Карта сегментов\n(каждый цвет = один сегмент)")
axes[0, 1].axis('off')

# 3. Маска с весами
axes[0, 2].imshow(mask, cmap='RdBu', vmin=-1, vmax=1)
axes[0, 2].set_title("3. Маска с весами\n(красный=+, синий=-)")
axes[0, 2].axis('off')
plt.colorbar(axes[0, 2].imshow(mask, cmap='RdBu', vmin=-1, vmax=1), ax=axes[0, 2])

# 4. Сегмент 0 (вес: +0.8)
seg0_mask = (segments == 0)
seg0_image = np.zeros_like(image)
seg0_image[seg0_mask] = image[seg0_mask]
axes[1, 0].imshow(mark_boundaries(seg0_image, seg0_mask), cmap='gray')
axes[1, 0].set_title(f"4. Сегмент 0\nВес: +0.8")
axes[1, 0].axis('off')

# 5. Сегмент 2 (вес: -0.5)
seg2_mask = (segments == 2)
seg2_image = np.zeros_like(image)
seg2_image[seg2_mask] = image[seg2_mask]
axes[1, 1].imshow(mark_boundaries(seg2_image, seg2_mask), cmap='gray')
axes[1, 1].set_title(f"5. Сегмент 2\nВес: -0.5")
axes[1, 1].axis('off')

# 6. Финальная тепловая карта
axes[1, 2].imshow(image, cmap='gray', alpha=0.6)
axes[1, 2].imshow(mask, cmap='RdBu', alpha=0.5, vmin=-1, vmax=1)
axes[1, 2].set_title("6. Тепловая карта\n(наложена на изображение)")
axes[1, 2].axis('off')

plt.tight_layout()
plt.show()

# ============================================
# 6. ДЕТАЛЬНАЯ СТАТИСТИКА
# ============================================
print("\n" + "="*50)
print("ДЕТАЛЬНАЯ СТАТИСТИКА")
print("="*50)

for segment in np.unique(segments):
    seg_mask = (segments == segment)
    n_pixels = np.sum(seg_mask)
    weight = weights.get(segment, 0)
    avg_value = image[seg_mask].mean() if n_pixels > 0 else 0
    
    color = "🟥 ПОЛОЖИТЕЛЬНЫЙ" if weight > 0 else "🟦 ОТРИЦАТЕЛЬНЫЙ" if weight < 0 else "⬜ НЕЙТРАЛЬНЫЙ"
    
    print(f"Сегмент {segment}:")
    print(f"  Вес: {weight:+.2f} ({color})")
    print(f"  Пикселей: {n_pixels} ({n_pixels/100*100:.1f}% изображения)")
    print(f"  Средняя яркость: {avg_value:.3f}")
    print()
```

## 📊 Реальный пример с MNIST

```python
# ============================================
# РЕАЛЬНЫЙ ПРИМЕР С MNIST
# ============================================

# Берем реальное изображение
idx = 0
image = X_test[idx].reshape(28, 28)
image_rgb = np.stack([image, image, image], axis=2)

# Получаем объяснение
explanation = explainer.explain_instance(
    image_rgb,
    predict_proba,
    top_labels=5,
    hide_color=0,
    num_samples=500,
    random_seed=42,
    segmentation_fn=segmentation_fn
)

top_label = explanation.top_labels[0]
weights = dict(explanation.local_exp[top_label])

# Смотрим, как выглядит сегментация
print("Карта сегментов (первые 5x5 пикселей):")
print(explanation.segments[:5, :5])
print()

# Создаем маску
mask = np.zeros_like(image)
for segment, weight in weights.items():
    mask[explanation.segments == segment] = weight

# Проверяем, что получилось
print(f"Размер маски: {mask.shape}")
print(f"Уникальные значения в маске: {np.unique(mask)}")
print()

# Смотрим на конкретный сегмент (например, самый важный)
top_segment = sorted(weights.items(), key=lambda x: abs(x[1]), reverse=True)[0][0]
print(f"Самый важный сегмент: {top_segment}")

# Находим все пиксели этого сегмента
segment_pixels = (explanation.segments == top_segment)
print(f"Количество пикселей в сегменте: {np.sum(segment_pixels)}")

# Показываем позиции этих пикселей
positions = np.argwhere(segment_pixels)
print(f"Первые 10 позиций: {positions[:10]}")
print()

# Визуализация
fig, axes = plt.subplots(1, 4, figsize=(16, 4))

# 1. Оригинал
axes[0].imshow(image, cmap='gray')
axes[0].set_title("Оригинал")
axes[0].axis('off')

# 2. Карта сегментов
axes[1].imshow(explanation.segments, cmap='tab20')
axes[1].set_title("Карта сегментов")
axes[1].axis('off')

# 3. Только самый важный сегмент
seg_image = np.zeros_like(image)
seg_image[segment_pixels] = image[segment_pixels]
axes[2].imshow(mark_boundaries(seg_image, segment_pixels), cmap='gray')
axes[2].set_title(f"Самый важный сегмент\n{top_segment}")
axes[2].axis('off')

# 4. Тепловая карта
axes[3].imshow(image, cmap='gray', alpha=0.6)
axes[3].imshow(mask, cmap='RdBu', alpha=0.5)
axes[3].set_title("Тепловая карта")
axes[3].axis('off')

plt.tight_layout()
plt.show()
```

## 🎯 Ключевые моменты

1. **Суперпиксель = группа пикселей**
   - Сегмент может содержать от десятков до сотен пикселей
   - Все пиксели в одном сегменте получают ОДИН вес

2. **Маппинг = присвоение веса всем пикселям сегмента**
   ```python
   mask[explanation.segments == segment] = weight
   ```
   - `explanation.segments == segment` — создает булеву маску
   - Все `True` позиции получают значение `weight`

3. **Результат** — тепловая карта, где:
   - Красные области = положительный вклад
   - Синие области = отрицательный вклад
   - Интенсивность цвета = сила вклада

Это очень эффективный способ визуализации, потому что он показывает не только **какие** области важны, но и **как именно** они влияют на предсказание! 🚀