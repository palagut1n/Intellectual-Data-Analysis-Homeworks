# Intellectual-Data-Analysis-Homeworks
Мои домашки по курсу ИАДА
[Uplo{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Домашнее задание 1 (5 баллов)\n",
    "\n",
    "Все задания ниже имеют равный вес (5/10)."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### О задании\n",
    "\n",
    "В этом домашнем задании вы попрактикуетесь в работе с библиотекой numpy, которая позволяет сравнительно легко и удобно выполнять разнообразные вычисления, избегая самостоятельной реализации поэлементной обработки."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Во всех задачах необходимо написать код решения внутри функции и убедиться, что она работает, с помощью [assert](https://python-reference.readthedocs.io/en/latest/docs/statements/assert.html) на выражение с использованием этой функции для данных из условия.\n",
    "\n",
    "При решении задач запрещается использовать циклы (`for`, `while`) и оператор `if`. Да, `map`, `reduce`, `filter` и любые другие \"чисто питоновские\" конструкции тоже запрещены. **Используйте только функционал numpy**.\n",
    "\n",
    "Везде, где встречаются массивы или матрицы, подразумевается, что это `numpy.array`.\n",
    "\n",
    "**numpy reference:** https://numpy.org/doc/stable/reference/index.html"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {
    "pycharm": {
     "name": "#%%\n"
    }
   },
   "outputs": [],
   "source": [
    "import numpy as np"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Задание 1\n",
    "\n",
    "Напишите функцию, возвращающую округленную взвешенную сумму оценок по данным оценкам и весам. Можете посчитать свою оценку за курс :) В нашем случае вес экзамена равен 0.3, вес домашек - 0.4, вес контрольной - 0.2, вес самостоятельных - 0.1. Например, если за экзамен у вас 7, за домашки 10, за контрольную 8, а за самостоятельные 6, то вы получите отличную оценку 8!\n",
    "\n",
    "Обратите внимание, что на вход приходит всегда двумерный массив. В нем в каждой строке - окенки одного конкретного студента. От вас требуется вернуть итоговую оценку для всех студентов в массиве."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [],
   "source": [
    "def result_mark(weights: np.array, marks: np.array) -> int:\n",
    "    sum = np.dot(marks,weights)\n",
    "    final = np.round(sum).astype(int)\n",
    "    return(final)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {},
   "outputs": [],
   "source": [
    "weights = np.array([0.3, 0.4, 0.2, 0.1])\n",
    "marks = np.array([[7, 10, 8, 6], [6, 9, 7, 6]])\n",
    "\n",
    "assert np.allclose(result_mark(weights, marks), np.array([8, 7]))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [],
   "source": [
    "weights = np.array([0.3, 0.4, 0.2, 0.1])\n",
    "marks = np.array([[7, 0, 8, 6]])\n",
    "\n",
    "assert np.allclose(result_mark(weights, marks), np.array([4]))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Задание 2\n",
    "\n",
    "Напишите функцию, которая каждое четное число в массиве заменяет на его квадрат (вторую степень), а нечетное - на его синус. Округлите все значения в итоговом массиве до двух знаков после запятой."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "metadata": {},
   "outputs": [],
   "source": [
    "def change_array(array: np.array) -> np.array:\n",
    "    new_array = np.round(np.where(array%2==0, array**2, np.sin(array)),2)\n",
    "    return(new_array)\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "metadata": {},
   "outputs": [],
   "source": [
    "array = np.array([2, 5, 1, 0, -6, 22, 213431])\n",
    "\n",
    "assert np.allclose(change_array(array), np.array([4.0, -0.96, 0.84, 0.0, 36.0, 484.0, -0.58]))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 23,
   "metadata": {},
   "outputs": [],
   "source": [
    "array = np.array([3, 12, 15, -97, 6])\n",
    "\n",
    "assert np.allclose(change_array(array), np.array([0.14, 144.0, 0.65, -0.38, 36.0]))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Задание 3\n",
    "\n",
    "Напишите функцию, которая вычисляет разность каждого элемента с предыдущим (для самого первого считайте, что его разница с \"предыдущим\" нулевая) в виде нового массива и возвращает вектор, в котором каждый новый элемент - сумма всех предыдущих из полученного нового массива."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 31,
   "metadata": {},
   "outputs": [],
   "source": [
    "def cumulative_sum_of_differences(array):\n",
    "    diff = np.diff(array, prepend = array[0])\n",
    "    vec = np.cumsum(diff)\n",
    "    return(vec)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 32,
   "metadata": {},
   "outputs": [],
   "source": [
    "array = np.array([3, 6, 2, 8])\n",
    "\n",
    "assert np.allclose(cumulative_sum_of_differences(array), np.array([0, 3, -1, 5]))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 33,
   "metadata": {},
   "outputs": [],
   "source": [
    "array = np.array([5, 1])\n",
    "\n",
    "assert np.allclose(cumulative_sum_of_differences(array), np.array([0, -4]))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Задание 4\n",
    "\n",
    "Напишите функцию, которая транспонирует двумерный массив, затем делает из него одномерный (reshape), переводит все элементы массива в *int8* и выводит его отсортированным по убыванию."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 46,
   "metadata": {},
   "outputs": [],
   "source": [
    "def flatten_and_sort_transposed(array: np.array) -> np.array:\n",
    "    trans_array = array.T\n",
    "    trans_array_1d = trans_array.flatten() \n",
    "    new_elements= trans_array_1d.astype(np.int8)\n",
    "    final = np.sort(new_elements)[::-1]\n",
    "    return(final)\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 47,
   "metadata": {},
   "outputs": [],
   "source": [
    "array = np.array([[256, -10], [-381, 112]])\n",
    "\n",
    "assert np.allclose(flatten_and_sort_transposed(array), np.array([112, 0, -10, -125]))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 48,
   "metadata": {},
   "outputs": [],
   "source": [
    "array = np.array([[9, 8, 7], [3, 6, 129]])\n",
    "\n",
    "assert np.allclose(flatten_and_sort_transposed(array), np.array([ 9, 8, 7, 6, 3, -127]))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Задание 5\n",
    "\n",
    "Напишите функцию, которая удаляет из массива все строки, среднее значение в которых больше общего среднего значения по всему массиву."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 57,
   "metadata": {},
   "outputs": [],
   "source": [
    "def filter_rows_by_mean(array):\n",
    "    mean = array.mean()\n",
    "    mean_stroki = array.mean(axis=1)\n",
    "    array_new = array[mean_stroki<=mean]\n",
    "    return(array_new)\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 58,
   "metadata": {},
   "outputs": [],
   "source": [
    "array = np.array([[1, 2, 3], [7, 8, 9], [4, 5, 6]])\n",
    "res = filter_rows_by_mean(array)\n",
    "\n",
    "assert res.ndim == 2\n",
    "assert np.allclose(res, np.array([[1, 2, 3], [4, 5, 6]]))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 59,
   "metadata": {},
   "outputs": [],
   "source": [
    "array = np.array([[10, 20], [5, 5], [15, 15]])\n",
    "res = filter_rows_by_mean(array)\n",
    "\n",
    "assert res.ndim == 2\n",
    "assert np.allclose(res, np.array([[5, 5]]))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Задание 6\n",
    "\n",
    "Напишите функцию, которая принимает на вход число - размер массива (квадратная матрица), который заполнен по принипу щахматной доски нулями и единицами. Первой (слева сверху) идет всегда единица. Напомним, что на шахматной доске белые и черные ячейки чередуются (в данном задании чередуются нули и единицы)."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 74,
   "metadata": {},
   "outputs": [],
   "source": [
    "def create_checkerboard(number):\n",
    "    matr=np.zeros(((number),(number)))\n",
    "    matr[::2, ::2] = 1  \n",
    "    matr[1::2, 1::2] = 1\n",
    "    return(matr)\n",
    "\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 75,
   "metadata": {},
   "outputs": [],
   "source": [
    "number = 3\n",
    "\n",
    "assert np.allclose(create_checkerboard(number) - np.array([[1, 0, 1], [0, 1, 0], [1, 0, 1]]), np.zeros((number, number)))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 76,
   "metadata": {},
   "outputs": [],
   "source": [
    "number = 4\n",
    "\n",
    "assert np.allclose(create_checkerboard(number) - np.array([[1, 0, 1, 0], [0, 1, 0, 1], [1, 0, 1, 0], [0, 1, 0, 1]]), np.zeros((number, number)))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Задание 7\n",
    "\n",
    "Напишите функцию, которая соритрует строки двумерного массива по значению в первом (начиная с нуля) столбце (по возрастанию)."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 79,
   "metadata": {},
   "outputs": [],
   "source": [
    "def sort_rows_by_second_column(array):\n",
    "    array_new = array[array[:, 1].argsort()]\n",
    "    return array_new\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 80,
   "metadata": {},
   "outputs": [],
   "source": [
    "array = np.array([[10, 2], [3, 5], [7, 1]])\n",
    "\n",
    "assert np.allclose(sort_rows_by_second_column(array), np.array([[7, 1], [10, 2], [3, 5]]))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 81,
   "metadata": {},
   "outputs": [],
   "source": [
    "array = np.array([[5, 8, 1], [10, 3, 9], [2, 7, 0], [6, -1, 5]])\n",
    "\n",
    "assert np.allclose(sort_rows_by_second_column(array), np.array([[6, -1, 5], [10, 3, 9], [2, 7, 0], [5, 8, 1]]))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Задание 8\n",
    "\n",
    "Напишите функцию, которая вычисляет сумму элементов [побочной](https://ru.wikipedia.org/wiki/Квадратная_матрица) диагонали квадратной матрицы."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 86,
   "metadata": {},
   "outputs": [],
   "source": [
    "def sum_counterdiagonal_elements(array):\n",
    "    return(sum(array[:, ::-1].diagonal()))\n",
    "    "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 87,
   "metadata": {},
   "outputs": [],
   "source": [
    "array = np.array([[5, 8, 1], [10, 3, 9], [2, 7, 0]])\n",
    "\n",
    "assert sum_counterdiagonal_elements(array) == 6"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 88,
   "metadata": {},
   "outputs": [],
   "source": [
    "array = np.array([[-4, -1, -1, 7], [6, -10, 5, -9], [3, -8, 6, 8], [-1, -6, 7, -10]])\n",
    "\n",
    "assert sum_counterdiagonal_elements(array) == 3"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Задание 9\n",
    "\n",
    "Напишите функцию, которая принимает на вход три числа (начало отрезка, конец отрезка, количество точек на отрезке). Данная функция генерирует указанное количество точек равномерно на отрезке с указанными концами (точки равноудалены друг от друга). Затем функция генерирует другой массив - натуральный логарифм от всех точек отрезка плюс единица ($ln(point + 1)$). Возвращает функция массив пар точек вида (точка на отрезке, ее логарифм). Каждое значение в возвращаемом массиве должно быть округлено до двух знаков после запятой."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 105,
   "metadata": {},
   "outputs": [],
   "source": [
    "def generate_logarithm_points(start, end, count):\n",
    "    random_points = np.linspace(start, end, count)\n",
    "    array_1 = np.log(random_points + 1)\n",
    "    array_2 = np.column_stack((random_points, array_1))\n",
    "    return np.round(array_2, 2)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 106,
   "metadata": {},
   "outputs": [],
   "source": [
    "start = 1\n",
    "end = 10\n",
    "count = 5\n",
    "\n",
    "assert np.allclose(generate_logarithm_points(start, end, count), np.array([[1., 0.69], [3.25, 1.45], [5.5 , 1.87], [7.75, 2.17], [10., 2.4]]))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 107,
   "metadata": {},
   "outputs": [],
   "source": [
    "start = 5\n",
    "end = 12\n",
    "count = 4\n",
    "\n",
    "assert np.allclose(generate_logarithm_points(start, end, count), np.array([[5., 1.79], [7.33, 2.12], [9.67, 2.37], [12., 2.56]]))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Задание 10\n",
    "\n",
    "Напишите функцию, которая нормализует входной двумерный массив. \n",
    "\n",
    "Под нормализацией в данном задании подразумевается перевод всех чисел массива на отрезок $[0; 1]$. Очевидно, недостаточно просто вычесть/прибавить лишнее, чтобы остались числа на нужном отрезке. Суть нормализации массива в том, чтобы не потерять информацию о его элементах и их соотношениях. Потому, необходимо сохранить пропорции расстояний между каждым числом - спроецировать все точки на указанный отрезок. Такой метод масштабирования массива называется MinMaxScaling. Само название дает подсказку, как нужно решить задачу. Дробные числа округляйте до двух знаков после запятой.\n",
    "\n",
    "*Будьте осторожны с делением...*"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 114,
   "metadata": {},
   "outputs": [],
   "source": [
    "def min_max_scale(array):\n",
    "    maximum = np.max(array)\n",
    "    minimum = np.min (array)\n",
    "    norm = np.round((array - minimum)/(maximum - minimum),2)\n",
    "    return norm"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 115,
   "metadata": {},
   "outputs": [],
   "source": [
    "array = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])\n",
    "\n",
    "assert np.allclose(min_max_scale(array), np.array([[0. , 0.12, 0.25], [0.38, 0.5 , 0.62], [0.75, 0.88, 1.]]))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 116,
   "metadata": {},
   "outputs": [],
   "source": [
    "array = np.array([[0, 1, 2], [3, 4, 5]])\n",
    "\n",
    "assert np.allclose(min_max_scale(array), np.array([[0., 0.2, 0.4], [0.6, 0.8, 1.]]))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.12.2"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
ading PalagutinDmitriiDz1OMO.ipynb…]()
