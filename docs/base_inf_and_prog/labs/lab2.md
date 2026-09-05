---
comments: true
tags: ["1 семестр"]
---

# Нужный этаж

## Вводная

В данной лабораторной работе нам дан код программы которая вычисляет номер этажа на котором находиться квартира по номеру квартиры и количеству квартир на этаже. Однако код писала пьяная нейросеть и он не работает, что нужно исправить. перед началом работы нужно подготовить директорию с лабой, создав эти файлы:

```plaintext
└── 📁 task2
    ├── 📄 fixes.log
    ├── 📄 main.c
    └── 📄 Makefile
```

```C title="main.c" linenums="1" hl_lines="4"
/**
 * floor.c -- программа для расчета нужного этажа
 *
 * Copyright (c) 2022, First Last <student@cs.petrsu.ru>
 *
 * This code is licensed under MIT license.
 */

#include<studio.h>

int Main()
{
    // Номер квартиры
    int flat_number;

    /* Число квартир на этаже */
    int flats_per_floor;

    /* Запрашиваем квартиру, в которой проживает адресат */
    printf("Введите номер интересующей квартиры: ");
    scanf("%d", &flat_number)

    /* Запрашиваем число квартир на этаже */
    printf("Введите число квартир на каждом этаже: ");
    scanf("%d", &flats_per_floor);

    /* Рассчитываем и выводим номер этажа */
    printf("Вам нужно подняться на %d этаж\n",
           flat_number / flats_per_floor, flat_number);

    return 0;
}
```

```make title="Makefile" linenums="1"
task2: main.o
 gcc -g -O0 -o task2 main.o

main.o: main.c
 gcc -g -O0 -c main.c

indent:
    indent -kr -nut main.c

clean:
 rm task2 *.o
```

!!! info "|"
    в ++"main.c"++ в выделенной строке нужно по аналогии с первой лабой записать свои данные

## Этапы выполнения

### Первая попытка собрать программу

Если попробовать собрать программу через `make` то компилятор сразу выдаст нам ошибку:

```bash hl_lines="3 4"
<логин>@kappa~/inf/task2> make
gcc -g -O0 -c main.c
main.c:9:9: fatal error: studio.h: No such file or directory
    9 | #include<studio.h>
      |         ^~~~~~~~~~
compilation terminated.
make: *** [main.o] Ошибка 1
```

Чтобы понять в чём ошибка достаточно перевести выделенные строки, компилятор говорит нам что файла `studio.h` нет. Тут ошибка просто из-за опечатки, мы используем `stdio.h`, а не `studio.h`:

```C title="main.c" linenums="9"
#include <stdio.h>
```

после исправления обязательно копируем ошибку компилятора в файл `fixes,log`

``` title="fixes.log" linenums="1"
main.c:9:9: fatal error: studio.h: No such file or directory
```

### Вторая попытка

После фиксации ошибки и её исправления снова пытаемся скомпилировать нашу программу:

```bash hl_lines="4 5"
<логин>@kappa~/inf/task2> make
gcc -g -O0 -c main.c
main.c: In function ‘Main’:
main.c:21:30: error: expected ‘;’ before ‘printf’
   21 |     scanf("%d", &flat_number)
      |                              ^
      |                              ;
......
   24 |     printf("Введите число квартир на каждом этаже: ");
      |     ~~~~~~
make: *** [Makefile:5: main.o] Error 1
```

здесь компилятор говорит нам, что перед `printf` он ожидает `;`, и более того даже показывает куда её пихнуть, получаем следующее:

```C title="main.c" linenums="21"
    scanf("%d", &flat_number);
```

после исправления обязательно копируем ошибку компилятора в файл `fixes,log`

``` title="fixes.log" linenums="2"
main.c:21:30: error: expected ‘;’ before ‘printf’
```

### Третья попытка

Снова пробуем собрать нашу программу:

```bash hl_lines="5"
<логин>@kappa~/inf/task2> make
gcc -g -O0 -c main.c
gcc -g -O0 -o task2 main.o
/usr/bin/ld: /usr/lib/gcc/x86_64-linux-gnu/14/../../../x86_64-linux-gnu/Scrt1.o: in function `_start':
(.text+0x17): undefined reference to `main'
collect2: error: ld returned 1 exit status
make: *** [Makefile:2: task2] Error 1
```

Компилятор говорит что не может сослаться к `main`, эта ошибка возникает тогда когда в коде программы нет функции `main`, которая представляет из себя стартовую точку. В изначальном коде у нас вроде есть функция `Main`, но язык С регистрочувствительный, и поэтому `Main` и `main` это не одно и тоже. Исправляем:

```C title="main.c" linenums="11"
int main()
```

``` title="fixes.log" linenums="3"
(.text+0x17): undefined reference to `main'
```

### Четвёртая попытка

Теперь при сборке ошибок не возникает, программа спокойно компилируется, но нам нужно копнуть глубже, для этого нужно при сборке добавить 2 новых флага: `-Wall` и `-Wextra`, они проведут более тщательную проверку.

```make title="Makefile" linenums="5"
    gcc -g -O0 -c -Wall -Wextra main.c
```

Пробуем снова собрать приложение(перед этим выполнив `make clean`) и получаем следующее:

```bash hl_lines="4 5"
<логин>@kappa~/inf/task2> make
gcc -g -O0 -c -Wall -Wextra main.c
main.c: In function ‘main’:
main.c:28:12: warning: too many arguments for format [-Wformat-extra-args]
   28 |     printf("Вам нужно подняться на %d этаж\n",
      |            ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
gcc -g -O0 -o task2 main.o
```

Компилятор говорит что в 28 строке в функции `printf` мы передаём слишком много аргументов. В строке мы вставили `%d`, то есть сказали что там должно быть целое число, из этого следует что мы должны передать 1 число, но на 29 строке мы видим следующее:

```C title="main.c" linenums="29"
           flat_number / flats_per_floor, flat_number);
```

мы передаём 2 числа, результат деления и номер квартиры. Второе значение лишнее, исправляем:

```C title="main.c" linenums="29"
    printf("Вам нужно подняться на %d этаж\n",
           flat_number / flats_per_floor);
```

``` title="fixes.log" linenums="4"
main.c:28:12: warning: too many arguments for format [-Wformat-extra-args]
```

### Пятая попытка

Пробуем снова скомпилировать и в этот раз без ошибок, поэтому нужно включить у компилятора режим душнилы, для этого добавляем флаги `-pedantic`, `pedantic-errors` и `ansi`:

```make title="Makefile" linenums="5"
    gcc -g -O0 -c -Wall -Wextra -pedantic -pedantic-errors -ansi main.c
```

пробуем собрать и получаем:

```bash hl_lines="4 5"
<логин>@kappa~/inf/task2> make
gcc -g -O0 -c -Wall -Wextra -pedantic -pedantic-errors -ansi main.c
main.c: In function ‘main’:
main.c:13:5: error: C++ style comments are not allowed in ISO C90
   13 |     // Номер квартиры
      |     ^
main.c:13:5: note: (this will be reported only once per input file)
make: *** [Makefile:5: main.o] Error 1
```

Теперь эта душнила ругается на то, что в 13 строке мы используем комментарий C++ стиля, что не соответствует стандарту ISO C90, фиксируем это в `fixes.log`, но не исправляем

``` title="fixes.log" linenums="5"
main.c:13:5: error: C++ style comments are not allowed in ISO C90
```

### Шестая попытка

Теперь мы меняем версию стандарта при проверке с `-ansi` на `-std=c11`:

```make title="Makefile" linenums="5"
    gcc -g -O0 -c -Wall -Wextra -pedantic -pedantic-errors -std=c11 main.c
```

после этого приложении собирается без проблем и замечаний, потому что в более современном стандарте такие комментарии уже разрешены

### Седьмая попытка

Теперь программа синтаксически написана верно, однако если мы посмотрим на её работу, то обнаружим что иногда она ошибается в подсчётах. всё из-за формулы, мы используем целочисленное деление, которое просто отсекает дробную часть, из-за чего и появляются логические ошибки, чтобы это исправить достаточно изменить формулу на:

```C title="main.c" linenums="29"
           (flat_number - 1) / flats_per_floor + 1);
```

теперь программа работает корректно.

## Результат

```C title="main.c" linenums="1"
/**
 * floor.c -- программа для расчета нужного этажа
 *
 * Copyright (c) 2022, First Last <student@cs.petrsu.ru>
 *
 * This code is licensed under MIT license.
 */

#include<stdio.h>

int main()
{
    // Номер квартиры
    int flat_number;

    /* Число квартир на этаже */
    int flats_per_floor;

    /* Запрашиваем квартиру, в которой проживает адресат */
    printf("Введите номер интересующей квартиры: ");
    scanf("%d", &flat_number);

    /* Запрашиваем число квартир на этаже */
    printf("Введите число квартир на каждом этаже: ");
    scanf("%d", &flats_per_floor);

    /* Рассчитываем и выводим номер этажа */
    printf("Вам нужно подняться на %d этаж\n",
           (flat_number - 1) / flats_per_floor + 1);

    return 0;
}
```

``` title="fixes.log" linenums="1"
main.c:9:9: fatal error: studio.h: No such file or directory
main.c:21:30: error: expected ‘;’ before ‘printf’
(.text+0x17): undefined reference to `main'
main.c:28:12: warning: too many arguments for format [-Wformat-extra-args]
main.c:13:5: error: C++ style comments are not allowed in ISO C90
```

## Возможные вопросы

### Сборка

??? question "Почему программа с Main() не работает? Почему обязательно нужно писать main() с маленькой буквы?"

    !!! success "Ответ"

        Когда операционная система запускает программу она передаёт управление именно функции `main`. Язык С регистрочувствительный, поэтому если мы пишем `Main`, то найти `main` не удаётся

??? question "Зачем мы добавляем флаги -Wall и -Wextra и что нам это даёт?"

    !!! success "Ответ"

        Без этих флагов компилятор будет давать нам информацию только об ошибках, которые мешают компиляции программы. `-Wall` Включает отображение базового набора предупреждений компилятора, а `-Wextra` расширяет набор предупреждений. Благодаря этому мы можем увидеть больше сомнительных частей кода и убрать больше багов

??? question "Зачем мы добавляем флаги -pedantic, -pedantic-errors и -ansi, что нам это даёт?"

    !!! success "Ответ"

        `-pedantic` заставляет компилятор строго следовать стандарту языка С, а `-pedantic-errors` превращает каждое замечание от `-pedantic` в ошибку компиляции. `-ansi` задаёт стандарт, а именно стандарт 1989 года(С89)

??? question "Почему при смене стандарта -ansi на -std=c11 сообщение компилятора пропало?"

    !!! success "Ответ"

        `-ansi` это стандарт 1989 года, в котором сказано, что комментарии в коде должны начинаться с `/*` и заканчиваться `*/`, применение комментариев вида `// ... ` не прописано. Когда мы меняем `-ansi` на `-std=c11` мы меняем стандарт 1989 года на стандарт 2011 года, он более современный и он позволяет нам использовать однострочные комментарии `// ... `

### Работа программы

??? question "Почему при делении мы всегда получаем целое число? Как нам получить десятичную дробь в ответе?"

    !!! success "Ответ"

        Тип данных результата при делении определяется типами данных операндов, если у нас оба числа `int`, то и результат будет `int`, компилятор строго следует типизации и поэтому дробна часть отсекается. Если нам нужна дробная часть, то хотя бы 1 операнд должен быть либо `float`, либо `double`