# Домашнее задание к занятию "Графы"

## Цель задания

1. Попрактиковаться в обходах графов

------

## Инструкция к заданию

1. Для каждой задачи создайте отдельный реплит, если об обратном не сказано в условии
1. Саму программу вы пишете в IDEA, реплит используется только для сдачи кода
1. В окне редактора IDEA наберите программный код, решающий поставленную задачу, на основе данной в условии заготовки кода
1. Загружаете файлы из папки src проекта в реплит
1. Отправьте выполненную работу на проверку в личном кабинете Нетологии

------

## Материалы, которые пригодятся для выполнения задания

1. [Реализуем алгоритм поиска в глубину](https://habr.com/ru/company/otus/blog/660725/)
3. [Как поделиться реплитом для проверки?](https://github.com/netology-code/java-homeworks/blob/java-43/QA_ReplitShare.md)
4. [Как автоотформатировать код?](https://github.com/netology-code/java-homeworks/blob/java-43/QA_Format.md)
5. [Как залить проект из IDEA в реплит?](https://github.com/netology-code/java-homeworks/blob/java-43/QA_ReplitUpload.md)

------

## Задание 1 (обязательное)

Социальную сеть можно представить себе в виде графа, где вершинами будут аккаунты пользователей, а наличие ребра будет показывать наличие между двумя пользователями дружбы (что они друг друга добавили в друзья).

Например, рассмотрим такой граф:

<img width="440" alt="image" src="https://user-images.githubusercontent.com/53707586/216818760-c1bedd77-dad9-497d-9724-257404ca911c.png">

Здесь мы видим, например, что Петя дружит с Дашей, Даша с Катей, а Петя с Катей не дружат, как и Катя с Пашей.

Вам нужно написать метод в графе, который позволяет определить, можно ли гуляя по спискам друзей с одной страницы дойти до второй.

Например, от Пети можно дойти до Оли (они дружат), до Кати (через страницу Даши), но не до Паши.

### Заготовка кода
Используйте этот код в качестве заготовки кода вашего проекта. Менять код в `main` нельзя.

```java
import java.io.*;

public class Main {

    public static void main(String[] args) throws IOException {
        Graph<String> socialNetwork = new Graph<>(); // создание графа

        // создание вершин-страниц социальной сети
        Vertex<String> petya = socialNetwork.createVertex("Петя");
        Vertex<String> olya = socialNetwork.createVertex("Оля");
        Vertex<String> dasha = socialNetwork.createVertex("Даша");
        Vertex<String> katya = socialNetwork.createVertex("Катя");

        // создание рёбер - добавления в друзья
        socialNetwork.createEdge(petya, olya);
        socialNetwork.createEdge(olya, dasha);
        socialNetwork.createEdge(dasha, petya);
        socialNetwork.createEdge(dasha, katya);

        Vertex<String> pasha = socialNetwork.createVertex("Паша");
        Vertex<String> kostya = socialNetwork.createVertex("Костя");

        socialNetwork.createEdge(pasha, kostya);

        // поиск достижимости между анкетами
        System.out.println(socialNetwork.isConnected(petya, olya)); // true
        System.out.println(socialNetwork.isConnected(petya, katya)); // true
        System.out.println(socialNetwork.isConnected(pasha, kostya)); // true
        System.out.println(socialNetwork.isConnected(dasha, kostya)); // false

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
import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

public class Graph<T> {
    private List<Vertex<T>> vertices = new ArrayList<>();

    public Vertex<T> createVertex(T value) {
        Vertex<T> v = new Vertex<>(value);
        vertices.add(v);
        return v;
    }

    public void createEdge(Vertex<T> a, Vertex<T> b) {
        // добавляем их друг друга в их списки смежности
        a.getAdjacent().add(b);
        b.getAdjacent().add(a);
    }

    public boolean isConnected(Vertex<T> a, Vertex<T> b) {
        return dfsFind(a, b, new HashSet<>()); // рекурсивный обход в глубину
    }

    // метод отвечает на вопрос, нашли ли мы обходом из v вершину target с учётом
    // посещённых вершин, которые записаны в memory
    public boolean dfsFind(Vertex<T> v, Vertex<T> target, Set<Vertex<T>> memory) {
        // если вершина в которую зашли (v) это та которую мы искали (target), то поиск закончен
        if (v.equals(target)) {
            return true; // нашли
        }
        memory.add(v); // запоминаем вершину которую посетили
        
        // ВАШ КОД
        // перебираем все смежные вершины у v
        // если такую вершину ещё не посещали, заходим рекурсивно в неё
        // если такой заход завершился нахождением target-а - выходим из метода с true

        return false; // ничего не нашли
    }

}

```

### Алгоритм

Для того чтобы определить можно ли дойти из анкеты `a` до анкеты `b`, запустим обход в глубину из вершины `a`.
Если мы в его процессе наткнёмся на `b`, то дойти можно; иначе - нет.

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
