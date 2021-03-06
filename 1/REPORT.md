# Отчет по лабораторной работе №1
## Работа со списками и реляционным представлением данных
## по курсу "Логическое программирование"

### студент: Шевчук П.В.

## Результат проверки

| Преподаватель     | Дата         |  Оценка       |
|-------------------|--------------|---------------|
| Сошников Д.В. |              |               |
| Левинская М.А.|     5/01/18  |      3        |

> *Комментарии проверяющих (обратите внимание, что более подробные комментарии возможны непосредственно в репозитории по тексту программы)*

В отчете отсутствует реализация предикатов Вашего варинта без использования стандартных (length и т.п.).
## Введение

Любой список в Прологе можно представить как двоичное дерево, в листьях которого находятся элементы списка или пустой список. Элементами списка могут быть любые объекты. То есть он либо пуст, либо состоит из 2х частей: головы и хвоста, который сам является списком.

То, что называется списком в императивных языках, сильно отличается от списка в Прологе. Не во всех императивных языках есть такая структура данных, как список. Обрабатывать элементы списка в Прологе можно только рекурсивно, разделяя список на голову и хвост. В императивных языках, чтобы обратиться к какому-то элементу списка, мы можем использовать итераторы. Список в императивном языке может содержать в себе только элементы одинкового типа, в Прологе списки содержат любые элементы. Логичнее было бы сравнить списки в Прологе с массивами, т.к. и те, и другие чаще других типов используются в программах. Но к элементам массива мы имеем произвольный доступ, чего не скажешь о списках.

Набор стандартных предикатов обработки списков наводит на мысль, что списки можно применять для представления множеств, только в множестве порядок элементов не существенен, а для списка порядок имеет значение, кроме того один и тот же объект может встретиться в списке несколько раз.

## Задание 1.1: Предикат обработки списка

`del(X,Y)` - Удаляет последние элементы списка

Примеры использования:
```del([1,2,3,4,5,6,7,8],3,A).
A = [1, 2, 3, 4, 5]
```

Реализация:
```prolog
del(L,N,[]):-length_(L,N).
del([H|Tail],N,[H|NTail]):-del(Tail,N,NTail).
...
```

Удаляем столько же, сколько в списке, если он N равен длине.
Затем отбрасывается первый элемент и делается это рекурсивно, до тех пор, пока надо будет удалить столько же, соклько в хвосте осталось в переданном списке (пока не сработает первое правило).


## Задание 1.2: Предикат обработки числового списка
`check_order([_]).` - Проверка упорядоченности элементов по возрастанию

Примеры использования:
```
check_order([1,2,3,4,5,6,7,8]).
true .
check_order([1,2,3,4,7,6,7,8]).
false.
```
Реализация:
```
check_order([_]).
check_order([X,Y|T]):-X<Y,!,check_order([Y|T]).
```
С помощью отрицания отката сравниваем элементы

## Задание 2: Реляционное представление данных
Реляционное представление показывает отношения между объектами, а задачей программиста является анализ этих отношений. Результат запроса к таким данным -- это множество ответов, удовлетворяющих внутренней структуре программы. Вся задача сводится к реализации такой структуры, которая обеспечит выдачу ответов. Это вынуждает постоянно проверять выходные данные на правильность и полноту. В ходе отладки своей программы я сталкивалась с ситуацией, когда ответ охватывал не все необходимые объекты. К преимуществам можно отнести относительную простоту разработки: программа разбивается на отдельные компоненты, которые реализуются независимо друг от друга. То есть в ходе написания программы ее можно сразу тестировать, заменяя еще ненаписанные компоненты множеством фактов.

Мое представление условно можно обозначить как списки групп и журнал успеваемости по предметам. Главный недостаток такого представления в том, что все факты об оценках находятся внутри структур, составляющих список. И оценки находятся в общем списке, т.е. нет разделения по группам. Не всегда удобно обращаться со списками, когда нужно получить информацию об оценках. Отношения вида "группа -- предмет" сложно задаются. Возможно, было бы удобно воспользоваться встроенными предикатами assert/asserta/assertz, но, во-первых, мы должны знать, что находится внутри списка со структурами grade, во-вторых, это модификация исходных данных, что, обычно, нежелательно. Факты со списками групп оказались почти бесполезны. Думаю, было бы удобнее, если информация о группе хранилась бы в структуре grade. Из плюсов можно отметить простоту анализа успеваемости по отдельному предмету и сравнение оценок по разным предметам.
1 задание: Напечатать средний балл для каждого предмета.

`average_mark(Sub,Mark)` - печатает средний балл для заданного предмета.

Примеры использования:
```prolog
| ?- average_mark('Логическое программирование',X).
X = 4.1071428571428568
| ?- average_mark('Математический анализ',X).
X = 4.0357142857142856
| ?- average_mark('Психология',X).
X = 3.8571428571428572
```
Реализация:
```prolog
% Предикат отрицания(negation as failure)
not(X):- \+ X.
%предикат суммы
sum([],0).
sum([X|T],R):-
	sum(T,R1), R is R1+X.
%предикат среднего значения
average(T,R):-
	sum(T,N), length(T,X), R is N/X. 
  
  % task2.1)Напечатать средний балл для каждого предмета

% Сумма оценок по предмету
% (список оценок, сумма оценок)
sum_marks([],0).
sum_marks([grade(X,Y)|T],N):- sum_marks(T,M), N is Y + M.

% Средний балл для предмета
% (название предмета, средняя оценка)
average_mark(Sub,Mark):-
    subject(Sub,Y),
    sum_marks(Y, Sum),
    length(Y, Len),
    Mark is Sum / Len.
```
Сначала получаем список всех оценок по предмету, затем подсчитываем сумму всех баллов по этому предмету, потом считаем срадний балл (сумму делим на количество оценок).
2 задание: Для каждой группы, найти количество несдавших студентов
`do_not_pass_group(Gr,Count)` - находит количество несдавших студентов в заданной группе

Примеры использования:
```prolog
| ?- do_not_pass_group(101,X).
X = 3
| ?- do_not_pass_group(102,X).
X = 3
| ?- do_not_pass_group(103,X).
X = 2
| ?- do_not_pass_group(104,X).
X = 2
```

Реализация:
```prolog
% task2.2 Для каждой группы, найти количество не сдавших студентов

% Список всех оценок по всем предметам
% (список предметов, список оценок)
all_marks([],L).
all_marks([H|T], List_pass):-subject(H,X), all_marks(T, New_list), append(X, New_list, List_pass).

% Удаление повторяющихся оценок
delete_all(_,[],[]).
delete_all(X,[X|L],L1):-delete_all(X,L,L1).
delete_all(X,[Y|L],[Y|L1]):- X \= Y, delete_all(X,L,L1).

remove_same([],[]).
remove_same([H|T],[H|T1]):-delete_all(H,T,T2), remove_same(T2,T1).

% Проверяем, сколько студентов, получивших 2, имеются в нужной группе
% (список всех оценок, список группы, количество несдавших студентов из группы)
check([],L,0).
check([grade(X,Y)|T],L,N):- Y < 3, my_member(X, L), !, check(T,L,M), N is M + 1.
check([_|T],L,N):-check(T,L,N).

% Количество несдавших студентов в группе
% (номер группы, число несдавших)
do_not_pass_group(Gr,Count):-
    group(Gr, Lgroup),
    findall(Sub, subject(Sub,_), Subs),
    all_marks(Subs, List_pass),
    remove_same(List_pass, New),
    check(New, Lgroup, Count).
 ```
   Получаем список студентов нужной группы, получаем список всех возможных предметов, получаем все оценки по всем предметам, удаляем повторяющиеся оценки, чтобы если один и тот же студент не сдал больше 1 предмета, не считать его дважды, проверяем сколько из несдавших студентов являются студентами заданной группы.
3 задание: Найти количество несдавших студентов для каждого из предметов

`do_not_pass_sub(Sub,Count)` - находит количество несдавших студентов для заданного предмета
Примеры использования:
```prolog
| ?- do_not_pass_sub('Логическое программирование',X).
X = 3
| ?- do_not_pass_sub('Математический анализ',X).
X = 1
| ?- do_not_pass_sub('Психология',X).
X = 2
```

Реализация:
```prolog


% task2.3 nНайти количество несдавших студентов для каждого из предметов

%(список оценок, число несдавших)
count_do_not_pass([],0).
count_do_not_pass([grade(X,Y)|T],N):- Y < 3, !, count_do_not_pass(T,M), N is M + 1.
count_do_not_pass([_|T],N):-count_do_not_pass(T,N).

% (название предмета, число студентов)
do_not_pass_sub(Sub,Count):-
    subject(Sub,Y),
    count_do_not_pass(Y,Count).
```
Получаем список оценок по нужному предмету, считаем сколько из них двойки.

## Выводы

В императивных языках мы указываем как что-либо сделать, в Прологе -- что необходимо сделать. Мы сообщаем системе, что нам известно и задаем вопросы. Программы на Прологе выглядят простыми, но за ними скрывается мощный логический бэкграунд. Неудивительно, что такое программирование называется логическим.орошо, что в этой работе у нас была возможность на простых задачах в первом приближении узнать о логическом программировании и о Прологе в частности.
