# Реализовать API для идемпотентной генерации псевдослучайных значений

## Частный пример
Есть сайт, на сайте есть пользователи. Пользователи могут загружать 
аватары. Если пользователь не загрузил аватар, на аватарке должна 
отображаться первая буква имени пользователя, а фон должен должен 
окрашиваться в псевдослучайный цвет. При этом у одного и того же 
пользователя цвет фона всегда должен быть одинаковым, т.е. идемпотентным. 
Но хранить дефолтный цвет фона в БД нельзя.

## Общая постановка
Необходимо реализовать API для псевдослучайной генерации чисел и цветов с 
помощью хэш-функции MD5 по идентификатору пользователя

Интерфейс необходим следующий
```java
public interface HashGenerator {

    /**
     * Возвращает int от Integer.MIN_VALUE до Integer.MAX_VALUE
     * @return
     */
    int randomInt();

    int randomInt(int min, int max);

    /**
     * Случайная строка из цифр и латинских букв
     * @param maxLength
     * @return
     */
    String randomAlphanumeric(int maxLength);

    /**
     * Случайная строка из латинских букв
     * @param maxLength
     * @return
     */
    String randomAlphabetic(int maxLength);

    /**
     * Необходимо вернуть случайный цвет. Возвращаемое значение должно 
формироваться так: 0xAABBCCDD,
     * где AA, BB, CC, DD – уровни каналов соответствующие порядку RGBA 
(red, green, blue, alpha). Канал прозрачности можно принять константным и 
всегда равным 255
     * @return
     */
    int randomColor();

    /**
     * Меняет значение сида в хэш-функции так, чтобы в дельнейшем 
генерировались новые значения
     */
    void shake();
}
```

Пример использования решения:
```java
HashGenerator hg = HashGeneratorMd5.create(userId);
String userLogin = hg.randomAlphanumeric(64);
hg.shake();
String userName = hg.randomAlphabetic(20);
int avatarBackground = hg.randomColor();
hg.shake();
int avatarTextColor = hg.randomColor();
int age = hg.randomInt(1, 100);
System.out.println(String.format("UserProfile: { login: %s, name: %s, age: 
%d, avatarBackgroundColor: %d, avatarTextColor: %d }", userLogin, 
userName, age, avatarBackground, avatarTextColor));
```

## Подсказки
- Прочитать всю главу про классы и интерфейсы, статические методы
- Прочитать про хэш функции и разобраться чем отличается сид от ключа 
(seed, key)
- Можно использовать готовую библиотеку с md5, подключённую через maven, 
либо найти реализацию md5 на джаве и скопировать код эз ис


