# Домашнее задание к занятию "Ориентированные графы"

## Цель задания

1. Попрактиковаться в обходах ориентированных графов

------

## Инструкция к заданию

1. Для каждой задачи создайте отдельный реплит, если об обратном не сказано в условии
1. Саму программу пишите в IDEA, реплит используется только для сдачи кода
1. В окне редактора IDEA наберите программный код, решающий поставленную задачу, на основе данной в условии заготовки кода
1. Загружайте файлы из папки src проекта в реплит
1. Отправьте выполненную работу на проверку в личном кабинете Нетологии

------

## Материалы, которые пригодятся для выполнения задания

3. [Как поделиться реплитом для проверки?](https://github.com/netology-code/java-homeworks/blob/java-43/QA_ReplitShare.md)
4. [Как автоотформатировать код?](https://github.com/netology-code/java-homeworks/blob/java-43/QA_Format.md)
5. [Как залить проект из IDEA в реплит?](https://github.com/netology-code/java-homeworks/blob/java-43/QA_ReplitUpload.md)

------

## Задание 1 (обязательное)

Информацию о наличии прямых авиарейсов между городами можно представить в виде ориентированного графа.
Вершинами в нём будут города, дугами - наличие прямого рейса.

Например, рассмотрим следующий граф:

<img width="569" alt="image" src="https://user-images.githubusercontent.com/53707586/216820655-10a58abd-1ec6-48c2-a7b8-a93e3223742c.png">

В нём отображается наличие прямого рейса из Рима в Берлин, но не из Парижа в Лондон или из Парижа в Пекин.

Вашей задачей будет написать метод, который определяет за какое минимальное количество перелётов человек может добраться из одного города в другой.
Если добраться невозможно, то метод должен вернуть -1.

Например, из Рима до Пекина можно добраться за 2 перелёта (алгоритм не должен выбрать более длинный путь Рим - Париж - Берлин - Пекин):

<img width="561" alt="image" src="https://user-images.githubusercontent.com/53707586/216820791-af19cea8-7187-405d-8784-2bb8c140fd1e.png">


### Алгоритм

Для этого достаточно запустить поиск в ширину из стартовой вершины. Порядок обхода такой, что мы гарантированно обходим вершины по возрастанию их расстояния от исходной вершины (в отличие от обхода в глубину).

Во время обхода будем запоминать расстояния до каждой вершины в мапе, чтобы была информация для ответа.

Как только посещаем вершину города-назначения, возвращаем результат из мапы.

### Заготовка кода
Используйте этот код в качестве заготовки кода вашего проекта. Менять код в `main` нельзя.

```java
import java.io.*;

public class Main {

    public static void main(String[] args) throws IOException {
        DAG<String> flights = new DAG<>();

        Vertex<String> rome = flights.createVertex("Рим");
        Vertex<String> paris = flights.createVertex("Париж");
        Vertex<String> berlin = flights.createVertex("Берлин");
        Vertex<String> london = flights.createVertex("Лондон");
        Vertex<String> beijing = flights.createVertex("Пекин");

        flights.createEdge(rome, paris);
        flights.createEdge(london, paris);
        flights.createEdge(berlin, paris);
        flights.createEdge(rome, berlin);

        flights.createEdge(paris, berlin);
        flights.createEdge(berlin, beijing);

        System.out.println(flights.path(london, paris)); // 1
        System.out.println(flights.path(london, berlin)); // 2
        System.out.println(flights.path(rome, beijing)); // 2
        System.out.println(flights.path(rome, london)); // -1
    }

}
```

Заготовки классов `Vertex` и `Graph` (менять можно только методы поиска):

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Objects;

public class Vertex<T> {
    private T value;
    private List<Vertex> adjacent = new ArrayList<>(); // список смешности

    public Vertex(T value) {
        this.value = value;
    }

    public List<Vertex> getAdjacent() {
        return adjacent;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Vertex<?> vertex = (Vertex<?>) o;
        return value.equals(vertex.value);
    }

    @Override
    public int hashCode() {
        return Objects.hash(value);
    }
}
```

```java
import java.util.*;

public class DAG<T> {
    private List<Vertex<T>> vertices = new ArrayList<>();

    public Vertex<T> createVertex(T value) {
        Vertex<T> v = new Vertex<>(value);
        vertices.add(v);
        return v;
    }

    public void createEdge(Vertex<T> from, Vertex<T> to) {
        // добавляем в список смежности только в одном направлении
        from.getAdjacent().add(to);
    }

    public int path(Vertex<T> from, Vertex<T> to) {
        Map<Vertex<T>, Integer> paths = new HashMap<>(); // по городу даёт количество полётов чтобы до него добраться из from
        paths.put(from, 0); // добраться из пункта отправления до него же самого можно вообще не летая

        Queue<Vertex<T>> queue = new ArrayDeque<>(); // очередь обхода вершин
        Set<Vertex<T>> added = new HashSet<>(); // запоминаем все когда-либо побывавшие в очереди обхода вершины чтобы не дублироваться
        queue.add(from); // начинать будем из города отправления

        while (!queue.isEmpty()) { // пока очередь не пуста
            Vertex<T> v = ... // вынимаем следующий элемент из очереди
           
            // если v это город назначения, то возвращаем из paths готовый ответ для этой вершины

            // перебираем вершины по дугам из v
            // если такую вершину уже добавляли в очередь (воспользуйтесь коллекцией added), то пропускаем её.
            // иначе, добавляем её в очередь, added и paths (подумайте, сколько перелётов будет до этого города, если мы знаем сколько перелётов до v)
        }

        return -1;
    }

}
```


------


## Правила приема работы

Прикреплён реплит с решением задачи

------

## Критерии оценки

1. Программа оформлена на основе заготовки кода, предоставленной в условии
1. Программа запускается и отрабатывает без ошибок
2. Программа соответствует всем требованиям из условия задачи
3. Программа работает правильно на всех примерах из условия
4. Программный код соответствует пройденным требованиям к качеству кода
5. Программа спроектирована достаточно логично и правильно, не противоречит общепринятым в производстве практикам и традициям
6. Программа реализует достаточный для зачёта по эффективности алгоритм
7. При наличии недочётов, в зависимости от их серьёзности и количества, работа может быть отправлена на доработку или принята; решение принимается на основе экспертной оценки работы.
