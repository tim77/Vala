# Основы

## Исходный код и компиляция

Код на Vala сохраняется в файлы с расширением .vala. Vala не принуждает к такой же структурированности, как в языках вроде Java - здесь нет принципа пакетов или class-файлов. Вместо этого структура определяется текстом в каждом файле, характеризуя логическое расположение кода с помощью конструкций вроде пространств имён. Для компиляции Вы передаёте компилятору список необходимых файлов, а Vala разберётся как их связать вместе.

В результате вы можете объявить столько классов или функций в файле, сколько захотите; можно даже комбинировать части различных пространств имен, но это не очень хорошая идея. Существуют определённые соглашения, которым вы, вероятно, захотите следовать. Хорошим примером того, как организовать структуру проекта на Vala, является сам [проект Vala](https://gitlab.gnome.org/GNOME/vala).

Все файлы исходного кода одного пакета вместе с флагами компилятора передаются valac в виде параметров. Это похоже на компиляцию исходного кода в Java. Например:

```bash
$ valac compiler.vala --pkg libvala
```

создаст исполняемый файл с именем compiler, связанный с пакетом libvala. Кстати говоря, именно так и создается компилятор valac!

Если вы хотите, чтобы исполняемый файл имел другое имя, или вы передали компилятору сразу несколько файлов с исходным кодом, вы можете указать имя файла после флага -o:

`$ valac source1.vala source2.vala -o myprogram`

`$ ./myprogram`

Если вы запустите компилятор valac с флагом -C, то не получите исполняемый файл. Вместо этого компилятор создаст промежуточный код на C для каждого из исходных файлов Vala, в данном случае это source1.c и source2.c. Если вы посмотрите на содержимое этих файлов, вы увидите, что программирование класса в Vala эквивалентно той же задаче в C, но при этом менее трудоёмко. Также заметьте, что этот класс зарегистрирован динамически в запущенной системе. Это хороший пример возможностей платформы GNOME, но, как я уже сказал ранее, вам не требуется вдаваться в эти подробности для использования Vala.

Если вы хотите получить заголовочный файл C, для вашего проекта, используйте флаг -H:

`$ valac hello.vala -C -H hello.h`

## Обзор синтаксиса

Синтаксис Vala основан на C\#. В результате большинство из этого будет знакомо программистам, которые знают любой C-подобный язык, поэтому я буду краток.

Область видимости определяется с помощью фигурных скобок. Объект или ссылка действительны только между `{` и `}`. Также есть разделители, служащие для определения классов, методов, блоков кода и т. д., поэтому у них есть своя область определения. Vala не имеет строгих ограничений к месту объявления переменных.

Идентификатор определяется типом и именем, например, int d означает целое число с именем d. Для типов значений также создается объект заданного типа. Для ссылочных типов просто определяется новая ссылка, которая изначально ни на что не указывает.

Для идентификаторов используются те же правила, что и в языке С: первая буква должна быть из диапазонов \[a-z\], \[A-Z\] или символом нижнего подчеркивания, последующие символы также могут быть цифрами \[0-9\]. Другие символы Unicode не разрешены. Возможно использование зарезервированного слова в качестве идентификатора, если поставить символ @ перед ним. Этот символ не является частью имени. Например, вы можете назвать метод foreach, объявив его как `@foreach`, даже несмотря на то, что `foreach` - это зарезервированное слово Vala.

Экземпляры ссылочных типов создаются с помощью оператора new и имени метода-конструктора, который обычно совпадает с именем объекта, например: `Object h = new Object()` создает новый объект типа `Object` и ссылку на него с именем `h`.

## Комментарии

В Vala есть различные способы комментирования исходного кода.

```text
// Однострочный комментарий
/* Многострочный комментарий */
/**
* Документационный комментарий
*/
```

Они обрабатываются так же, как и в большинстве других языков, и нуждаются в небольшом пояснении. Документационный комментарий на самом деле не является специальным, но инструмент генерации документации, такой как Valadoc, распознаёт их.

