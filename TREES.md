# Домашнее задание к занятию "Пирамиды и деревья поиска"

## Цель задания

1. Уметь обходить и анализировать древовидные структуры
2. Уметь работать с пирамидальными структурами

------

## Инструкция к заданию

1. Для каждой задачи создайте отдельный реплит, если об обратном не сказано в условии
1. Саму программу пишите в IDEA, реплит используется только для сдачи кода
1. В окне редактора IDEA наберите программный код, решающий поставленную задачу, на основе данной в условии заготовки кода
1. Загружайте файлы из папки src проекта в реплит
1. Отправьте выполненную работу на проверку в личном кабинете Нетологии

------

## Материалы, которые пригодятся для выполнения задания

1. [Структуры данных: двоичное дерево в Java](https://javarush.com/groups/posts/3111-strukturih-dannihkh-dvoichnoe-derevo-v-java)
1. [Структуры данных: пирамида (двоичная куча) в Java](https://javarush.com/groups/posts/3083-strukturih-dannihkh-piramida-dvoichnaja-kucha-v-java)
3. [Как поделиться реплитом для проверки?](https://github.com/netology-code/java-homeworks/blob/java-43/QA_ReplitShare.md)
4. [Как автоотформатировать код?](https://github.com/netology-code/java-homeworks/blob/java-43/QA_Format.md)
5. [Как залить проект из IDEA в реплит?](https://github.com/netology-code/java-homeworks/blob/java-43/QA_ReplitUpload.md)

------

## Задание 1 (обязательное)

Вам дана реализация бинарного дерева поиска на именах.
Имена сравниваются в естественном (лексикографическом) порядке, т.е. как в словаре.
В левом поддереве всегда лежат имена лексикографически меньшие (т.е. которые в словаре лежали бы раньше) чем то что находится в узле, а в правом - лексикографически большие.

Дерево при этом идеально сбалансировано, т.е. по структуре связей между узлами подошло бы и для пирамиды.
Ваша задача как раз и состоит в том, чтобы проверить заданное дерево на min-пирамидальность по длине имён.
Т.е. мы хотим проверить, что это дерево является пирамидой на минимум, если сравнение делать через сравнение длин имён.
Ваш метод не должен перепроверять сбалансированность дерева и он может считать, что она уже гарантируется.

Пример дерева, на которое ваш метод должен сказать ДА:

<img width="1293" alt="image" src="https://user-images.githubusercontent.com/53707586/216814800-519002b9-13a0-4c83-a1c9-3f12756eab25.png">


Пример дерева, на которое ваш метод должен сказать НЕТ (длина имени "Павел" меньше, чем длина имени "Константин", что нарушает пирамидальность):

<img width="1296" alt="image" src="https://user-images.githubusercontent.com/53707586/216814848-5ae11630-3cd1-45d4-adec-65b30dd6fc35.png">



### Заготовка кода
Используйте этот код в качестве заготовки кода вашего проекта. Менять код в `main` нельзя.

```java
import java.io.*;

public class Main {

    public static void main(String[] args) throws IOException {
        // Заполнение дерева
        // Названия переменных указывают на место заполняемого узла
        // например, rl - повернуть на право, затем налево
        // После заполнения дерево выводится на консоль, можете ориентироваться на него

        Tree ll = new Tree("Александа");
        Tree lr = new Tree("Владимир");
        Tree l = new Tree("Борис");
        l.setLeft(ll);
        l.setRight(lr);

        Tree rl = new Tree("Иннокентий");
        Tree rr = new Tree("Пантелеймон");
        Tree r = new Tree("Константин");
        r.setLeft(rl);
        r.setRight(rr);

        Tree root = new Tree("Зоя");
        root.setLeft(l);
        root.setRight(r);

        System.out.println(root); // Выведем дерево в консоль

        System.out.println("Проверка поиска по дереву:");
        System.out.println(root.contains("Иннокентий")); // true
        System.out.println(root.contains("Борис")); // true
        System.out.println(root.contains("Анна")); // false

        /* Ваше задание (нужно раскомментировать)
        System.out.println("Проверка на пирамидальность по длине имени");

        System.out.println(root.isNamePyramid()); // true

        // Меняем имя в одном из узлов на Павел
        // Пирамидальность должна нарушиться
        // А из-за того что имя на ту же букву,
        // в данном случае свойства дерева поиска сохрнаяются
        rr.setName("Павел");
        System.out.println(root.isNamePyramid()); // false

        */
    }

}
```

И код дерева:

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;
import java.util.stream.Collectors;

public class Tree {
    private String name;
    private Tree left;
    private Tree right;

    public Tree(String name) {
        this.name = name;
    }

    public boolean contains(String query) {
        if (query.compareTo(name) < 0) {
            return left != null && left.contains(query);
        } else if (query.compareTo(name) > 0) {
            return right != null && right.contains(query);
        } else {
            return name.equals(query);
        }
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Tree getLeft() {
        return left;
    }

    public void setLeft(Tree left) {
        this.left = left;
    }

    public Tree getRight() {
        return right;
    }

    public void setRight(Tree right) {
        this.right = right;
    }

    /* Раскомментируйте и реализуйте этот метод
    public boolean isNamePyramid() {
        // ВАШ КОД
    } */

    @Override
    public String toString() {
        String[] leftLines = (left != null ? left.toString() : "").split("\n");
        String[] rightLines = (right != null ? right.toString() : "").split("\n");

        int maxLeftSize = Arrays.stream(leftLines)
                .map(String::length)
                .max(Comparator.naturalOrder())
                .orElse(0);

        List<String> lines = new ArrayList<>();
        lines.add(" ".repeat(maxLeftSize + 1) + name);
        for (int i = 0; i < Math.max(leftLines.length, rightLines.length); i++) {
            String prefix = i < leftLines.length ? leftLines[i] : "";
            lines.add(
                    prefix +
                            " ".repeat(maxLeftSize + name.length() + 1 * 2 - prefix.length()) +
                            (i < rightLines.length ? rightLines[i] : "")
            );
        }

        return lines.stream().collect(Collectors.joining("\n"));
    }
}
```

### Алгоритм

Реализуйте проверку рекурсивным образом. Если в текущем узле не нарушается свойство пирамидальности с двумя его детьми (при их наличии), а также если каждый из существующих детей (`left` и `right`) отвечает на вопрос о своей пирамидальности своих поддеревьев ДА, то и ответ будет ДА. Если что-то из этого не соблюдается, то ответ будет НЕТ.

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
