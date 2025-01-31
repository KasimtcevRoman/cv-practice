# Практическая работа №1. Обработка изображений с использованием библиотеки OpenCV

## Описание структуры программы

Программа позволяет выполнять фильтрацию изображений. Она содержит: 
- `функцию для парсинга аргументов командной строки`
- `функцию для загрузки изображения`
- `функция перевода изображения в оттенки серого`
- `функцию изменения разрешения изображения`
- `функцию применения фотоэффекта сепии к изображению`
- `функцию применения фотоэффекта виньетки к изображению`
- `функцию пикселизации заданной прямоугольной области изображения`
- `функцию, позволяющую выбрать области пикселизации`
- `главную функцию программы`

## Описание алгоритмов

### Функция для парсинга аргументов командной строки

- `"-m" или "--mode": указывает режим работы программы`
- `"-i" или "--image": указывается путь к изображению, которое будет обрабатывваться`
- `"o" или "output": указывается имя выходного изображения`
- `"--value": значение, которое используется в фильтре изменения разрешения изображения`
- `"--x1", "--x2": определяет границы области по оси OX`
- `"--y1", "--y2": определяет границы области по оси OY`
- `"--pix": устанавливает размер пикселя, используется в фильтре пикселезации заданной прямоугольной области изображения`

### Функция для загрузки (чтения) изображения

Данная функция  предназначена для загрузки, отображения изображения с использованием библиотеки OpenCV. 

- `Параметры функции: строка, содержащая путь к изображению, которое нужно загрузить, а также строка, указывающая имя выходного файла, в которое будет сохранено изображение.`
- `Функция cv.imread() используется для чтения изображения из указанного пути, если его невозможно загрузить, то вызывается исключение.`
- `Функция image.shape позволяет получить размеры изображения и количество цветных каналов.`
- `Функция cv.imshow() позволяет открыть окно и показать загруженное изображение с указанным заголовком.`
- `Функция cv.waitKey(0) ожидает нажатия клавиши, что позволяет пользователю увидеть изображение до закрытия окна.`
- `Функция cv.imwrite() позволяет сохранить изображение.`
- `Функция cv.destroyAllWindows() позволяет закрыть все открытые окна.`

### Функция для сохранения изображения

Данная функция  предназначена для сохранения изображения с использованием библиотеки OpenCV. 

- `Параметры функции: строка, содержащая путь к изображению, которое нужно загрузить, а также строка, указывающая имя выходного файла, в которое будет сохранено изображение.`
- `Функция cv.imwrite() позволяет сохранить изображение.`
- `Функция cv.imshow() позволяет открыть окно и показать загруженное изображение с указанным заголовком.`
- `Функция cv.waitKey(0) ожидает нажатия клавиши, что позволяет пользователю увидеть изображение до закрытия окна.`
- `Функция cv.destroyAllWindows() позволяет закрыть все открытые окна.`

### Функция перевода изображения в оттенки серого

Данный фильтр позволяет перевести исходное изображение в оттенки серого с использованием определенного коэффициента для каждого канала. Параметром данной функции является изображение, которое будет обрабатываться. Для каждого пикселя изображения извлекаются значения цветовых каналов RGB и вычисляется коэффициент градации серого по формуле:
```math
coeff = 0.299 * R + 0.587 * G + 0.114 * B
```

### Функция изменения разрешения изображения

Данная функция изменяет разрешение изображения, увеличивая или уменьшая его размер в зависимости от заданного коэффициента. Ее параметрами являются изображение, а также коэффициент изменения разрешения (должен быть больше нуля). Для каждого пикселя в новом изображении вычисляются соответствующие индексы в исходном изображении, значение пикселя из исходного изображения копируется в новое изображение.

### Функция применения фотоэффекта сепии к изображению

Данная функция применяет сепийный фильтр к изображению, изменяя его цветовую палитру для создания эффекта старинной фотографии. Ее параметром является  изображение, которое будет обрабатываться. Для каждого пикселя извлекаются его значения цветовых каналов, затем вычисляются новые значения для каждого канала в соответствии с формулами фильтра сепии.
```math
canalR = 0.393 * R + 0.769 * G + 0.189 * B
```
```math
canalG = 0.349 * R + 0.686 * G + 0.168 * B
```
```math
canalB = 0.272 * R + 0.534 * G + 0.131 * B
```

### Функция применения фотоэффекта виньетки к изображению

Данная функция применяет фильтр виньетки к изображению, создавая плавный переход от полного яркости в центре к затемнению по краям. Ее параметрами являются изображение и радиус. Рассчитывается расстояние от каждого пикселя до центра изображения с помощью формулы: 

```cmath
dist = np.sqrt((x_indices - centr_x) ** 2 + (y_indices - centr_y) ** 2)
 ```
После этого вычисляем коэффициент затемнения по следующей формуле:

```cmath
coef = 1 - np.minimum(1, dist / radius)
```

### Функция пикселизации заданной прямоугольной области изображения

Данная функция предназначена для применения эффекта пикселизации к изображению в заданной области. Ее параметрами являются изображение и размер пикселя. Алгоритм проходит по заданной области изображения, которая выбирается пользователем с помощью мыши, разделяя её на небольшие участки. Для каждого участка вычисляется средний цвет, который представляет собой усредненные значения цветовых каналов. Полученный средний цвет затем заполняет весь участок, создавая эффект пикселизации.

### Функция, позволяющая выбрать область пикселизации 

Данная функция предназначена для отображения изображения и рисования прямоугольника с помощью мыши. При запуске этой функции создается окно, в котором будет показано изображение. В процессе работы устанавливается обработчик событий, который реагирует на действия пользователя мышью.

Когда пользователь нажимает на изображение левой кнопкой мыши, фиксируются начальные координаты. При перемещении мыши обновляются координаты конца прямоугольника, что позволяет видеть его в реальном времени. Когда кнопка мыши отпускается, фиксируются окончательные координаты, и прямоугольник остается на изображении.

Основной цикл функции продолжает работать до тех пор, пока не будет нажата клавиша 'q', после чего окно закрывается. 

### Главная функция программы

В этой функции происходит обработка изображений в зависимости от выбранного режима. Сначала необходимо задать параметры через командную строку, затем, в зависимости от выбранного режима, выполняются вызовы различных функций, описанных выше. Если режим не поддерживается, то программа выдаст ошибку, информируя о том, что такого режима не существует. 

