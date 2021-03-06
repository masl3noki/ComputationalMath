Пытался сделать автовычисление n в проге, но огромное количество ошибок компиляции заставило меня прекратить попытки искать оптимальное n

Аналитически:
* для sin(x): 
   - n=5. Остаточный член = 0.0007 на отрезке [0; 1]
   - n=NAN. Остаточный член = NAN на отрезке [10; 11]

![sin](https://github.com/masl3noki/ComputationalMath/blob/main/1%20sem/Task%201/I.8.19/figure_sin.png)

* для exp(x): 
   - n=6. Остаточный член = 0.0005 на отрезке [0; 1]
   - n=NAN. Остаточный член = NAN на отрезке [10; 11]

![exp](https://github.com/masl3noki/ComputationalMath/blob/main/1%20sem/Task%201/I.8.19/figure_exp.png)

## Функция нахождения оптимального n:

```
def optimal_n(u_, i):
#TODO оценить остаточный член так, чтобы он был меньше точности
    check = ((sp.diff(u_, t) * t**(i+1)) / sp.factorial(i+1)) #остаточный член ряда Маклорена в форме Лагранжа
    if check(1) <= precision:
        return i, True
    else:
        return -1, False
```        

## Модифицированная функция с optimal_n:

```
def mac_series(u): #u = func1 или func2
    u_ = sp.lambdify(t, u) #"готовая" func(t) для numpy.var
    sum = u_(0) #первый член разложения по маклорену
    i = 1

    while True:
        u = sp.diff(u, t)  # очищается из памяти прошлое значение u, теперь u указывает на производную du/dt
        u_ = sp.lambdify(t, u)  # аналогично
        sum = sum + ((u_(0) * (t ** i)) / sp.factorial(i))
        if optimal_n(u_, i)[2]:
            return sum, i
        i += 1
        if i >= STOP:
            break
```
