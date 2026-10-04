---
title: "Практическая работа №2: Знакомство с языком Java для Android-разработки"
discipline: "Разработка мобильных приложений"
status: "Active"
author: "УПМ 2"
tags: [java, android, oop, mobile-dev, laboratory-work]
---

# Практическая работа: Введение в язык Java в контексте мобильной разработки

> [!NOTE]
> **Цель работы:** Освоить базовый синтаксис языка Java, принципы объектно-ориентированного программирования (ООП), механизмы интерфейсов и стандартные структуры данных, необходимые для разработки нативных компонентов мобильных приложений.

---

## 📋 Содержание

1. [Вводные требования и инструментарий](#0-вводные-требования-и-инструментарий)
2. [Тема 1. Базовый синтаксис, примитивные типы и операторы](#тема-1-базовый-синтаксис-примитивные-типы-и-операторы)
3. [Тема 2. Управляющие конструкции и операторы ветвления](#тема-2-управляющие-конструкции-и-операторы-ветвления)
4. [Тема 3. Массивы, строки и форматирование данных](#тема-3-массивы-строки-и-форматирование-данных)
5. [Тема 4. Методы и модульность](#тема-4-методы-и-модульность)
6. [Тема 5. Классы, объекты и инкапсуляция](#тема-5-классы-объекты-и-инкапсуляция)
7. [Тема 6. Наследование и полиморфизм](#тема-6-наследование-и-полиморфизм)
8. [Тема 7. Абстрактные классы и интерфейсы](#тема-7-абстрактные-классы-и-интерфейсы)
9. [Тема 8. Коллекции и Generics](#тема-8-коллекции-и-generics)
10. [Тема 9. Исключения и безопасность выполнения](#тема-9-исключения-и-безопасность-выполнения)
11. [50 Смешанных практических заданий (Mobile Logic Focus)](#50-смешанных-практических-заданий)
12. [Критерии оценки и регламент сдачи](#критерии-оценки-и-регламент-сдачи)

---

## 0. Вводные требования и инструментарий

Для выполнения заданий рекомендуется использовать актуальное окружение разработки:

- **JDK:** OpenJDK 21 LTS или OpenJDK 25.
- **IDE:** Android Studio (Ladybug / Hedgehog / Jellyfish) или IntelliJ IDEA Community / Ultimate.
- **Система сборки:** Gradle (Kotlin DSL или Groovy DSL).

> [!TIP]
> При написании консольных классов для отладки создавайте классический метод точки входа:
>
> ```java
> public class Main {
>     public static void main(String[] args) {
>         System.out.println("Java for Android is ready!");
>     }
> }
> ```

---

## Тема 1. Базовый синтаксис, примитивные типы и операторы

Язык Java является строго типизированным. Примитивные типы хранят значения непосредственно в стеке памяти, что критично для энергоэффективности мобильных чипов.

### Таблица типов данных

| Тип       | Размер     | Диапазон / Значение         | Мобильный контекст                        |
| :-------- | :--------- | :-------------------------- | :---------------------------------------- |
| `byte`    | 8 бит      | -128 .. 127                 | Буферы сырых сетевых пакетов, аудио       |
| `short`   | 16 бит     | -32 768 .. 32 767           | Обработка датчиков (акселерометр)         |
| `int`     | 32 бит     | \(-2^{31}\) .. \(2^{31}-1\) | ID ресурсов `R.id.*`, индексы списков     |
| `long`    | 64 бит     | \(-2^{63}\) .. \(2^{63}-1\) | Таймстемпы Unix (мс), ID сущностей SQLite |
| `float`   | 32 бит     | IEEE 754                    | Координаты экрана, плотность dp/sp        |
| `double`  | 64 бит     | IEEE 754                    | Геолокация (GPS широта и долгота)         |
| `boolean` | 1 бит/байт | `true` / `false`            | Флаги видимости, состояния переключателей |
| `char`    | 16 бит     | `\u0000` .. `\uffff`        | Одиночные символы ввода                   |

### Пример кода

```java
public class ScreenMetrics 
{
    public static void main(String[] args) 
    {
        final double LATITUDE = 54.0105;
        final double LONGITUDE = 38.2917;

        int screenWidthPx = 1080;
        float density = 2.75f;
        int screenWidthDp = (int) (screenWidthPx / density);

        boolean isLocationEnabled = true;

        System.out.println("Координаты: " + LATITUDE + ", " + LONGITUDE);
        System.out.println("Ширина экрана в dp: " + screenWidthDp);
        System.out.println("Статус GPS: " + (isLocationEnabled ? "Включен" : "Отключен"));
    }
}
```

### Задания для закрепления

1. **Конвертер плотности (dp в px):** Напишите программу, принимающую значение размера в `dp` и коэффициент плотности экрана `dpi` (например, 1.5 для hdpi, 2.0 для xhdpi, 3.0 для xxhdpi), и вычисляющую пиксели.
```java
public class Main 
{
    public static void main(String[] args) 
    {
        float dp = 100;
        float dpi = 2.0f;

        float px = dp * dpi;

        System.out.println(px);
    }
}
```
2. **Парсинг таймстемпа:** Создайте переменную типа `long`, содержащую миллисекунды. Вычислите количество полных минут, секунд и часов без использования сторонних библиотек времени.
```java
public class Main 
{
    public static void main(String[] args) 
    {
        long ms = 7265000;

        long seconds = ms / 1000;
        long minutes = seconds / 60;
        long hours = minutes / 60;

        seconds %= 60;
        minutes %= 60;

        System.out.println(hours + "ч " + minutes + "м " + seconds + "с");
    }
}
```
3. **Расчет расхода батареи:** Дана емкость аккумулятора смартфона (мАч) и среднее потребление модуля связи (мА) и дисплея (мА). Рассчитайте ориентировочное время автономной работы в часах.
```java
public class Main 
{
    public static void main(String[] args) 
    {
        int battery = 5000;
        int connection = 100;
        int display = 300;

        int total = connection + display;

        double time = battery / (double) total;

        System.out.println("Часов: " + time);
    }
}
```
4. **Валидатор диапазона координат:** Реализуйте проверку широты (от -90.0 до 90.0) и долготы (от -180.0 до 180.0) через логические операторы `&&` и `||`.
```java
public class Main {
    public static void main(String[] args) 
    {
        double lat = 50.5;
        double lon = 30.5;

        boolean valid = lat >= -90 && lat <= 90 &&
                        lon >= -180 && lon <= 180;

        System.out.println(valid);
    }
}
```
5. **Побитовые флаги разрешений:** Реализуйте установку, снятие и проверку разрешений приложения (`CAMERA = 1`, `LOCATION = 2`, `STORAGE = 4`) с помощью побитовых операций (`|`, `&`, `~`).
```java
public class Main {
    public static void main(String[] args) 
    {

        int CAMERA = 1;
        int LOCATION = 2;
        int STORAGE = 4;

        int permissions = CAMERA | LOCATION;

        boolean hasCamera = (permissions & CAMERA) != 0;
        boolean hasStorage = (permissions & STORAGE) != 0;

        System.out.println(hasCamera);
        System.out.println(hasStorage);
    }
}
```

---

## Тема 2. Управляющие конструкции и операторы ветвления

Мобильные приложения постоянно реагируют на внешние события: переключение вкладок, изменение интернет-соединения, жизненный цикл Activity/Fragment.

### Пример кода (Modern Switch Expression)

```java
public class NetworkStateEvaluator 
{
    public enum NetworkType { NONE, GPRS, LTE, WIFI, NR_5G }

    public static String getBufferStrategy(NetworkType type) 
    {
        return switch (type) {
            case NONE -> "Оффлайн: показать локальный кэш";
            case GPRS -> "Экономичный режим: низкое разрешение картинок";
            case LTE, WIFI -> "Стандартный режим: прогрессивная загрузка";
            case NR_5G -> "Ультра режим: предварительная загрузка 4K контента";
        };
    }
}
```

### Задания для закрепления

1. **Определение ориентации экрана:** Напишите логику, которая по переданной ширине и высоте окна выводит `"PORTRAIT"`, `"LANDSCAPE"` или `"SQUARE"`.
```java
public class Main 
{
    public static void main(String[] args) 
    {
        int width = 800;
        int height = 1200;
        if (width > height) 
        {
            System.out.println("LANDSCAPE");
        } 
        else if (width < height) 
        {
            System.out.println("PORTRAIT");
        } 
        else 
        {
            System.out.println("SQUARE");
        }
    }
}
```
2. **Классификатор статуса HTTP-ответа:** Используя `switch`, верните категорию ответа сервера по его коду: Информационный (1xx), Успешный (2xx), Перенаправление (3xx), Ошибка клиента (4xx), Ошибка сервера (5xx).
```java
public class Main {
    public static void main(String[] args) 
    {

        int code = 404;
        switch (code / 100) 
        {
            case 1:
                System.out.println("Информационный");
                break;
            case 2:
                System.out.println("Успешный");
                break;
            case 3:
                System.out.println("Перенаправление");
                break;
            case 4:
                System.out.println("Ошибка клиента");
                break;
            case 5:
                System.out.println("Ошибка сервера");
                break;
            default:
                System.out.println("Неизвестный код");
        }
    }
}
```
3. **Симулятор таймера повторных попыток (Backoff):** С помощью цикла `for` смоделируйте 5 попыток подключения к серверу с экспоненциальной задержкой (1с, 2с, 4с, 8с, 16с).
```java
public class Main 
{
    public static void main(String[] args) 
    {

        int delay = 1;
        for (int i = 1; i <= 5; i++) 
        {
            System.out.println("Попытка " + i + 
                    ", задержка " + delay + " сек");
            delay = delay * 2;
        }
    }
}
```
4. **Пропуск поврежденных пакетов:** Дан цикл прохода по массиву идентификаторов сообщений чата. Если ID равен `-1` (ошибка), выполните `continue`. Если ID равен `0` (конец сессии), выполните `break`.
```java
public class Main 
{
    public static void main(String[] args) 
    {

        int[] messages = {10, -1, 25, 0, 30};
        for (int id : messages) 
        {

            if (id == -1) 
            {
                continue;
            }

            if (id == 0) 
            {
                break;
            }
            System.out.println("Сообщение: " + id);
        }
    }
}
```
5. **Контроль ввода пин-кода:** Используя цикл `do-while`, реализуйте логику проверки 4-значного кода с ограничением до 3 попыток.
```java
import java.util.Scanner;

public class Main 
{
    public static void main(String[] args) 
    {

        Scanner sc = new Scanner(System.in);
        int pin = 1234;
        int tries = 0;
        int input;
        do 
        {
            System.out.print("Введите PIN: ");
            input = sc.nextInt();

            tries++;

            if (input == pin) 
            {
                System.out.println("Доступ разрешен");
                break;
            }
            System.out.println("Неверный PIN");
        } 
        while (tries < 3);
        if (tries == 3 && input != pin) 
        {
            System.out.println("Карта заблокирована");
        }
    }
}
```

---

## Тема 3. Массивы, строки и форматирование данных

При рендеринге списков (RecyclerView) и обработке REST API ключевую роль играют массивы и неизменяемые строки `String` вместе с `StringBuilder`.

### Пример кода

```java
public class TextSanitizer 
{
    public static void main(String[] args) 
    {
        String rawInput = "  +7 (999) 123-45-67  ";
        String cleanPhone = rawInput.trim().replaceAll("[^0-9+]", "");

        StringBuilder logBuilder = new StringBuilder();
        logBuilder.append("Пользователь авторизован с номером: ")
                  .append(cleanPhone)
                  .append(" [Время: ")
                  .append(System.currentTimeMillis())
                  .append("]");

        System.out.println(logBuilder.toString());
    }
}
```

### Задания для закрепления

1. **Нормализация поискового запроса:** Напишите метод, очищающий введенный пользователем поисковый запрос от лишних концевых пробелов, приводящий строку к нижнему регистру и заменяющий множественные пробелы на один.
```java
public class Main 
{
    public static void main(String[] args) 
    {
        String text = "  Hello   WORLD  Java  ";
        text = text.trim();
        text = text.toLowerCase();
        text = text.replaceAll("\\s+", " ");
        System.out.println(text);
    }
}
```
2. **Реверс массива кадров анимации:** Дан массив строковых имен кадров анимации. Разверните его задом наперед без создания второго массива.
```java
public class Main 
{
    public static void main(String[] args) 
    {

        String[] frames = {"frame1", "frame2", "frame3"};
        for (int i = 0; i < frames.length / 2; i++) 
        {
            String temp = frames[i];
            frames[i] = frames[frames.length - 1 - i];
            frames[frames.length - 1 - i] = temp;
        }
        for (String s : frames) 
        {
            System.out.println(s);
        }
    }
}
```
3. **Маскирование номера банковской карты:** Примите строку из 16 цифр и верните ее в формате `**** **** **** 1234`.
```java
public class Main 
{
    public static void main(String[] args) 
    {
        String card = "1234567812345678";
        String result = "**** **** **** " 
                + card.substring(12);
        System.out.println(result);
    }
}
```
4. **Поиск пиковых значений акселерометра:** В массиве из 100 значений измерений датчика найдите максимальный всплеск (максимальную разность между соседними элементами).
```java
public class Main 
{
    public static void main(String[] args) 
    {
        int[] values = {5, 10, 3, 20, 2, 15};
        int max = 0;
        for (int i = 1; i < values.length; i++) 
        {
            int diff = Math.abs(values[i] - values[i - 1]);

            if (diff > max) 
            {
                max = diff;
            }
        }

        System.out.println("Максимальный всплеск: " + max);
    }
}
```
5. **Генератор URL-параметров:** Дан строковый массив ключей и массив значений одинаковой длины. Соберите строку GET-запроса вида `?key1=val1&key2=val2` с использованием `StringBuilder`.
```java
public class Main 
{
    public static void main(String[] args) 
    {
        String[] keys = {"key1", "key2"};
        String[] values = {"val1", "val2"};
        StringBuilder url = new StringBuilder("?");
        for (int i = 0; i < keys.length; i++) 
        {
            url.append(keys[i])
               .append("=")
               .append(values[i]);
            if (i < keys.length - 1) 
            {
                url.append("&");
            }
        }

        System.out.println(url);
    }
}
```

---

## Тема 4. Методы и модульность

Методы организуют бизнес-логику презентеров и ViewModel. Java поддерживает перегрузку методов (overloading) и аргументы переменной длины (varargs).

### Пример кода

```java
public class NotificationHelper 
{
    public static void showToast(String message) 
    {
        showToast(message, 2000);
    }

    public static void showToast(String message, int durationMs) 
    {
        System.out.println("[TOAST] " + message + " (Длительность: " + durationMs + "ms)");
    }

    public static void logTags(String category, String... tags) 
    {
        System.out.print("[" + category + "] Теги: ");
        for (String tag : tags) 
        {
            System.out.print("#" + tag + " ");
        }
        System.out.println();
    }
}
```

### Задания для закрепления

1. **Перегрузка валидатора:** Напишите метод `isValid(String email)` и его перегруженную версию `isValid(String email, boolean checkDomain)`.
```java
public class Main 
{

    static boolean isValid(String email) 
    {
        return email.contains("@");
    }

    static boolean isValid(String email, boolean checkDomain) 
    {
        if (!isValid(email)) 
        {
            return false;
        }
        if (checkDomain) 
        {
            return email.endsWith(".com");
        }

        return true;
    }

    public static void main(String[] args) 
    {

        System.out.println(isValid("test@gmail.com"));
        System.out.println(isValid("test@gmail.com", true));
    }
}
```
2. **Форматирование валюты:** Реализуйте метод с параметрами `double amount` и `String currencySymbol`, возвращающий красиво оформленную строку для корзины магазина.
```java
public class Main 
{

    static String format(double amount, String symbol)
    {
        return symbol + String.format("%.2f", amount);
    }

    public static void main(String[] args) 
    {
        System.out.println(format(199.99, "$"));
    }
}
```
3. **Калькулятор суммарного размера кэша:** Создайте метод `calculateCache(long... fileSizesInBytes)`, возвращающий сумму всех файлов в мегабайтах (`double`).
```java
public class Main 
{

    static double calculateCache(long[] files) 
    {
        long sum = 0;
        for (long size : files) 
        {
            sum += size;
        }
        return sum / 1024.0 / 1024.0;
    }

    public static void main(String[] args) 
    {
        long[] files = {1000000, 2000000};
        System.out.println(calculateCache(files));
    }
}
```
4. **Рекурсивный поиск вложений:** Напишите рекурсивный метод для подсчета общего количества элементов во вложенной структуре папок устройства.
```java
public class Main 
{

    static int countFiles(int[] folders, int index) 
    {
        if (index == folders.length) 
        {
            return 0;
        }

        return folders[index] + countFiles(folders, index + 1);
    }

    public static void main(String[] args) 
    {
        int[] files = {3, 5, 2};
        System.out.println(countFiles(files, 0));
    }
}
```
5. **Сравнение версий приложения:** Напишите метод `int compareVersions(String v1, String v2)`, возвращающий `1`, если `v1 > v2`, `-1`, если `v1 < v2`, и `0`, если версии равны (например, "1.12.0" и "1.9.4").
```java
public class Main 
{

    static int compareVersions(String v1, String v2) 
    {
        String[] a = v1.split("\\.");
        String[] b = v2.split("\\.");

        for (int i = 0; i < a.length; i++) 
        {
            int x = Integer.parseInt(a[i]);
            int y = Integer.parseInt(b[i]);

            if (x > y)
                return 1;
            if (x < y)
                return -1;
        }
        return 0;
    }

    public static void main(String[] args) 
    {
        System.out.println(compareVersions("1.12.0", "1.9.4"));
    }
}
```

---

## Тема 5. Классы, объекты и инкапсуляция

Инкапсуляция защищает целостность внутреннего состояния мобильного экрана. Для неизменяемых моделей данных (DTO) в современном Java используются `record`.

### Пример кода

```java
// Традиционный класс с инкапсуляцией
public class UserProfile {
    private final long id;
    private String username;
    private int loyaltyPoints;

    public UserProfile(long id, String username) 
    {
        this.id = id;
        setUsername(username);
        this.loyaltyPoints = 0;
    }

    public long getId() { return id; }

    public String getUsername() { return username; }

    public void setUsername(String username) 
    {
        if (username == null || username.trim().isEmpty()) 
        {
            throw new IllegalArgumentException("Имя пользователя не может быть пустым");
        }
        this.username = username.trim();
    }

    public void addPoints(int points) 
    {
        if (points > 0) {
            this.loyaltyPoints += points;
        }
    }
}

// Современный DTO в виде record (начиная с Java 16+)
record PushNotificationDto(String title, String body, long timestamp, boolean isRead) {}
```

### Задания для закрепления

1. **Модель экрана настроек (SettingsModel):** Спроектируйте класс с приватными полями `isDarkMode`, `volumeLevel` (от 0 до 100) и `appLanguage`. Обеспечьте валидацию уровня громкости в сеттере.
```java
public class Main 
{
    static class SettingsModel 
    {
        private boolean isDarkMode;
        private int volumeLevel;
        private String appLanguage;

        public void setVolume(int volume) {
            if (volume >= 0 && volume <= 100) 
            {
                volumeLevel = volume;
            }
        }

        public void show() 
        {
            System.out.println(isDarkMode);
            System.out.println(volumeLevel);
            System.out.println(appLanguage);
        }
    }


    public static void main(String[] args) 
    {
        SettingsModel s = new SettingsModel();
        s.setVolume(50);
        s.show();
    }
}
```
2. **DTO корзины интернет-магазина:** Создайте `record CartItem(String id, String title, double price, int count)`. Добавьте в него метод вычисления общей стоимости позиции.
```java
public class Main
{
    record CartItem(String id, String title, double price, int count) 
    {
        double total() 
        {
            return price * count;
        }
    }

    public static void main(String[] args) 
    {
        CartItem item = new CartItem("1", "Phone", 500, 2);
        System.out.println(item.total());
    }
}
```
3. **Счетчик непрочитанных пушей:** Разработайте класс `BadgeCounter`, в котором значение счетчика нельзя установить в отрицательное число, а инкремент и декремент происходят через отдельные методы.
```java
public class Main 
{

    static class BadgeCounter 
    {
        private int count = 0;
        void increment() 
        {
            count++;
        }

        void decrement() 
        {
            if (count > 0) 
            {
                count--;
            }
        }

        void show() 
        {
            System.out.println(count);
        }
    }

    public static void main(String[] args) 
    {
        BadgeCounter b = new BadgeCounter();
        b.increment();
        b.increment();
        b.decrement();
        b.show();
    }
}
```
4. **Инкапсулированный таймер сессии:** Создайте класс `SessionTracker` с приватными полями времени входа и последнего действия. Напишите метод проверки, истекла ли сессия (timeout = 15 минут).
```java
public class Main 
{
    static class SessionTracker 
    {
        private long loginTime;
        private long lastAction;

        SessionTracker() 
        {
            loginTime = System.currentTimeMillis();
            lastAction = loginTime;
        }

        boolean isExpired() 
        {
            long time = System.currentTimeMillis() - lastAction;
            return time > 15 * 60 * 1000;
        }
    }

    public static void main(String[] args) 
    {
        SessionTracker s = new SessionTracker();
        System.out.println(s.isExpired());
    }
}
```
5. **Модель геопозиции:** Создайте класс `GeoPoint` с неизменяемыми полями широты и долготы, валидируемыми в конструкторе.
```java
public class Main 
{
    static class GeoPoint 
    {
        private final double latitude;
        private final double longitude;
        GeoPoint(double lat, double lon) 
        {
            if (lat < -90 || lat > 90 ||
                lon < -180 || lon > 180) 
                {

                throw new IllegalArgumentException();
            }
            latitude = lat;
            longitude = lon;
        }

        void show() 
        {
            System.out.println(latitude + " " + longitude);
        }
    }

    public static void main(String[] args) 
    {
        GeoPoint point = new GeoPoint(50, 30);
        point.show();
    }
}
```

---

## Тема 6. Наследование и полиморфизм

Наследование позволяет переиспользовать базовое поведение виджетов, а полиморфизм — единообразно отрисовывать разнородные элементы списков (списки карточек, баннеров и кнопок).

### Пример кода

```java
// Базовый элемент UI
public abstract class UiComponent {
    protected int id;
    protected boolean isVisible;

    public UiComponent(int id) {
        this.id = id;
        this.isVisible = true;
    }

    public abstract void render();
}

// Дочерний компонент кнопки
public class ButtonComponent extends UiComponent 
{
    private final String text;

    public ButtonComponent(int id, String text) 
    {
        super(id);
        this.text = text;
    }

    @Override
    public void render() 
    {
        System.out.println("Рендер кнопки [" + id + "]: '" + text + "'");
    }
}

// Дочерний компонент изображения
public class ImageComponent extends UiComponent {
    private final String imageUrl;

    public ImageComponent(int id, String imageUrl) {
        super(id);
        this.imageUrl = imageUrl;
    }

    @Override
    public void render() {
        System.out.println("Рендер изображения [" + id + "] по адресу: " + imageUrl);
    }
}
```

### Задания для закрепления

1. **Иерархия экранов приложения:** Создайте базовый класс `BaseScreen` (с методами `onOpen()`, `onClose()`) и наследников: `LoginScreen`, `HomeScreen`, `SettingsScreen`.
```java
public class Main 
{
    static class BaseScreen 
    {
        void onOpen() 
        {
            System.out.println("Экран открыт");
        }

        void onClose() 
        {
            System.out.println("Экран закрыт");
        }
    }

    static class LoginScreen extends BaseScreen 
    {
        void login() 
        {
            System.out.println("Вход");
        }
    }

    static class HomeScreen extends BaseScreen 
    {
        void showHome() {
            System.out.println("Главная");
        }
    }

    static class SettingsScreen extends BaseScreen 
    {
        void settings() 
        {
            System.out.println("Настройки");
        }
    }

    public static void main(String[] args) 
    {
        LoginScreen l = new LoginScreen();
        l.onOpen();
        l.login();
        l.onClose();
    }
}
```
2. **Полиморфный обработчик аналитики:** Реализуйте базовый класс `AnalyticsEvent` и подклассы `ClickEvent`, `PurchaseEvent`, `ScreenViewEvent`. Напишите сервис, принимающий `AnalyticsEvent` и выводящий разную логику логирования.
```java
public class Main 
{
    static class AnalyticsEvent 
    {
        void send() 
        {
            System.out.println("Событие");
        }
    }


    static class ClickEvent extends AnalyticsEvent
    {
        void send() 
        {
            System.out.println("Клик");
        }
    }

    static class PurchaseEvent extends AnalyticsEvent 
    {
        void send() 
        {
            System.out.println("Покупка");
        }
    }

    static class ScreenViewEvent extends AnalyticsEvent 
    {
        void send() 
        {
            System.out.println("Просмотр экрана");
        }
    }

    public static void main(String[] args) 
    {
        AnalyticsEvent e = new PurchaseEvent();
        e.send();
    }
}
```
3. **Модели сенсоров смартфона:** Создайте класс `DeviceSensor` и наследников `GyroscopeSensor` и `LightSensor`. Реализуйте полиморфный метод `readData()`.
```java
public class Main {

    static class DeviceSensor {

        void readData() {
            System.out.println("Данные сенсора");
        }
    }


    static class GyroscopeSensor extends DeviceSensor {

        void readData() {
            System.out.println("Гироскоп");
        }
    }


    static class LightSensor extends DeviceSensor {

        void readData() {
            System.out.println("Освещенность");
        }
    }


    public static void main(String[] args) {

        DeviceSensor s = new GyroscopeSensor();

        s.readData();
    }
}
```
4. **Виджеты с кастомной отрисовкой:** Напишите метод `drawScreen(List<UiComponent> components)`, который итерируется по коллекции и вызывает метод `render()` для каждого элемента независимо от его типа.
```java
import java.util.*;

public class Main {

    interface UiComponent {

        void render();
    }


    static class Button implements UiComponent {

        public void render() {
            System.out.println("Кнопка");
        }
    }


    static class Text implements UiComponent {

        public void render() {
            System.out.println("Текст");
        }
    }


    static void drawScreen(List<UiComponent> list) {

        for (UiComponent c : list) {
            c.render();
        }
    }


    public static void main(String[] args) {

        List<UiComponent> items = new ArrayList<>();

        items.add(new Button());
        items.add(new Text());

        drawScreen(items);
    }
}
```
5. **Тарифные планы подписки:** Создайте базовый класс `Subscription` с расчетом стоимости и подклассы: `MonthlySubscription`, `FamilySubscription` (с учетом количества пользователей), `AnnualDiscountSubscription`.
```java
public class Main {

    static class Subscription {

        double price;

        Subscription(double price) {
            this.price = price;
        }


        double getPrice() {
            return price;
        }
    }


    static class MonthlySubscription extends Subscription {

        MonthlySubscription() {
            super(10);
        }
    }


    static class FamilySubscription extends Subscription {

        FamilySubscription() {
            super(20);
        }
    }


    static class AnnualDiscountSubscription extends Subscription {

        AnnualDiscountSubscription() {
            super(100);
        }
    }


    public static void main(String[] args) {

        Subscription s = new FamilySubscription();

        System.out.println(s.getPrice());
    }
}
```

---

## Тема 7. Абстрактные классы и интерфейсы

Интерфейсы определяют контракты взаимодействия. В Android через интерфейсы традиционно реализуются обработчики кликов (`OnClickListener`), колбэки сетевых вызовов и сервис-локаторы.

### Пример кода

```java
public interface OnItemClickListener<T> {
    void onItemClick(T item, int position);

    // Default-метод (Java 8+)
    default void onItemLongClick(T item, int position) {
        System.out.println("Долгое нажатие на элемент: " + position);
    }
}

public interface NetworkSyncable {
    void syncWithCloud();
}

public class NoteItem implements NetworkSyncable {
    private final String content;

    public NoteItem(String content) {
        this.content = content;
    }

    @Override
    public void syncWithCloud() {
        System.out.println("Синхронизация заметки: " + content);
    }
}
```

### Задания для закрепления

1. **Контракт хранилища данных:** Создайте интерфейс `KeyValueStorage` с методами `save(String key, String value)`, `get(String key)`, `clear()`. Реализуйте класс `MemoryStorage`.
```java
import java.util.*;
public class Main {

    static class KeyValueStorage {

        private Map<String, String> map = new HashMap<>();

        void put(String key, String value) {
            map.put(key, value);
        }

        String get(String key) {
            return map.get(key);
        }

        void remove(String key) {
            map.remove(key);
        }
    }


    public static void main(String[] args) {
        KeyValueStorage s = new KeyValueStorage();
        s.put("name", "Alex");
        System.out.println(s.get("name"));
        s.remove("name");
    }
}
```
2. **Колбэк загрузки изображения:** Напишите интерфейс `ImageLoadCallback` с методами `onSuccess(String bitmapRef)` и `onError(Throwable error)`.
```java
public class Main {

    interface ImageLoadCallback {

        void onSuccess(String image);

        void onError(String error);
    }


    static class Loader {

        void load(ImageLoadCallback callback) {

            boolean ok = true;

            if (ok) {
                callback.onSuccess("image.png");
            } else {
                callback.onError("Ошибка");
            }
        }
    }


    public static void main(String[] args) {

        Loader loader = new Loader();

        loader.load(new ImageLoadCallback() {

            public void onSuccess(String image) {
                System.out.println(image);
            }

            public void onError(String error) {
                System.out.println(error);
            }
        });
    }
}
```
3. **Слушатель жизненного цикла фоновой задачи:** Спроектируйте интерфейс `BackgroundTaskListener` со стандартным методом `onProgress(int percentage)`.
```java
public class Main {

    interface TaskListener {

        void onStart();
        void onFinish();
    }


    static class DownloadTask {
        void run(TaskListener listener) {
            
            listener.onStart();
            System.out.println("Загрузка...");
            listener.onFinish();
        }
    }


    public static void main(String[] args) {

        DownloadTask task = new DownloadTask();
        task.run(new TaskListener() {

            public void onStart() {
                System.out.println("Начало");
            }


            public void onFinish() {
                System.out.println("Готово");
            }
        });
    }
}
```
4. **Множественная реализация:** Создайте класс `MediaFile`, реализующий два интерфейса: `Playable` (метод `play()`, `stop()`) и `Shareable` (метод `shareViaBluetooth()`).
```java
public class Main {

    interface Camera {

        void takePhoto();
    }

    interface Gps {

        void getLocation();
    }

    static class Phone implements Camera, Gps {

        public void takePhoto() {
            System.out.println("Фото");
        }

        public void getLocation() {
            System.out.println("GPS");
        }
    }

    public static void main(String[] args) {

        Phone p = new Phone();
        p.takePhoto();
        p.getLocation();
    }
}
```
5. **Функциональный интерфейс для фильтрации:** Напишите аннотированный `@FunctionalInterface` `PredicateValidator<T>` с методом `boolean validate(T data)` и протестируйте его через лямбда-выражение.
```java
public class Main {

    interface PredicateValidator<T> {
        
        boolean check(T value);
    }

    public static void main(String[] args) {
        PredicateValidator<String> validator = text ->
                text.length() >= 5;
                System.out.println(
                validator.check("Hello")
        );
    }
}
```

---

## Тема 8. Коллекции и Generics

Коллекции (`List`, `Set`, `Map`) составляют основу передачи списков в адаптеры UI и сопоставления связей типа "Ключ-Значение".

### Пример кода

```java
import java.util.*;

public class ChatRepository {
    private final Map<String, List<String>> userDialogs = new HashMap<>();

    public void addMessage(String userId, String message) {
        userDialogs.computeIfAbsent(userId, k -> new ArrayList<>()).add(message);
    }

    public List<String> getDialog(String userId) {
        return userDialogs.getOrDefault(userId, Collections.emptyList());
    }

    public static void main(String[] args) {
        ChatRepository repo = new ChatRepository();
        repo.addMessage("user_42", "Привет, заказ доставлен?");
        repo.addMessage("user_42", "Да, спасибо!");

        System.out.println("Сообщения пользователя 42: " + repo.getDialog("user_42"));
    }
}
```

### Задания для закрепления

1. **Удаление дубликатов контактов:** Дан список телефонных номеров с дубликатами. Используя `Set`, верните очищенную от повторов коллекцию с сохранением исходного порядка добавления (`LinkedHashSet`).
```java
import java.util.*;
public class Main {
    public static void main(String[] args) {

        List<String> phones = Arrays.asList(
                "111", "222", "111", "333", "222"
        );

        Set<String> result = new LinkedHashSet<>(phones);
        System.out.println(result);
    }
}
```
2. **Очередь сетевых запросов:** Реализуйте имитацию очереди синхронизации данных `Queue<String>` (FIFO), обрабатывая элементы по мере поступления.
```java
import java.util.*;

public class Main {
    public static void main(String[] args) {

        Queue<String> queue = new LinkedList<>();
        queue.add("Запрос 1");
        queue.add("Запрос 2");
        queue.add("Запрос 3");
        while (!queue.isEmpty()) {
            System.out.println("Обработка: " + queue.poll());
        }
    }
}
```
3. **Обобщенный ответ API (Generic ApiResponse):** Создайте класс `ApiResponse<T>` с полями `int statusCode`, `T data`, `String errorMessage` и булевым геттером `isSuccessful()`.
```java
public class Main {

    static class ApiResponse<T> {
        int statusCode;
        T data;
        String errorMessage;
        boolean isSuccessful;
        
        ApiResponse(int code, T data, String error, boolean success) {
            statusCode = code;
            this.data = data;
            errorMessage = error;
            isSuccessful = success;
        }
    }

    public static void main(String[] args) {

        ApiResponse<String> response =
                new ApiResponse<>(200, "Hello", "", true);
        System.out.println(response.data);
    }
}
```
4. **Сортировка товаров по цене и популярности:** Дан `List<Product>`. Отсортируйте коллекцию с помощью `Comparator` сначала по возрастанию цены, а при равной цене — по рейтингу.
```java
import java.util.*;
public class Main {

    static class Product {
        String name;
        double price;
        int rating;

        Product(String name, double price, int rating) {
            this.name = name;
            this.price = price;
            this.rating = rating;
        }
    }

    public static void main(String[] args) {

        List<Product> products = new ArrayList<>();

        products.add(new Product("Phone", 500, 4));
        products.add(new Product("Laptop", 1000, 5));
        products.add(new Product("Tablet", 500, 5));

        products.sort((a, b) -> {
            if (a.price != b.price)
                return Double.compare(a.price, b.price);

            return Integer.compare(a.rating, b.rating);
        });

        for (Product p : products) {
            System.out.println(p.name);
        }
    }
}
```
5. **Кэш экранов (LRU Cache концепт):** Спроектируйте простейший механизм кэширования последних открытых фрагментов с ограничением максимальной емкости в 5 элементов.
```java
import java.util.*;
public class Main {

    static class LRUCache extends LinkedHashMap<String, String> {

        private final int maxSize = 5;

        LRUCache() {
            super(5, 0.75f, true);
        }

        protected boolean removeEldestEntry(
                Map.Entry<String, String> e) {
            return size() > maxSize;
        }
    }

    public static void main(String[] args) {

        LRUCache cache = new LRUCache();

        cache.put("Screen1", "Home");
        cache.put("Screen2", "Settings");
        cache.put("Screen3", "Profile");

        System.out.println(cache);
    }
}
```

---

## Тема 9. Исключения и безопасность выполнения

Падение мобильного приложения (Crash) недопустимо. Грамотная обработка исключений гарантирует стабильность приложения при потере сети или повреждении входных данных.

### Пример кода

```java
public class ConfigParser {
    public static int parsePort(String portString) {
        try {
            return Integer.parseInt(portString);
        } catch (NumberFormatException e) {
            System.err.println("Ошибка преобразования порта. Установлен порт по умолчанию 8080: " + e.getMessage());
            return 8080;
        } finally {
            System.out.println("Проверка порта завершена.");
        }
    }
}
```

### Задания для закрепления

1. **Пользовательское исключение отсутствия сети:** Создайте проверяемое исключение `NoInternetException` и напишите метод имитации запроса, выбрасывающий его при флаге `hasConnection == false`.
```java
public class Main {

    static class NoInternetException extends Exception {
        NoInternetException(String message) {
            super(message);
        }
    }

    static void request(boolean hasConnection)
            throws NoInternetException {

        if (!hasConnection) {
            throw new NoInternetException("Нет интернета");
        }

        System.out.println("Запрос выполнен");
    }

    public static void main(String[] args) {
        try {
            request(false);
        } catch (NoInternetException e) {
            System.out.println(e.getMessage());
        }
    }
}
```
2. **Конструкция try-with-resources:** Напишите метод чтения файла локального конфига с автоматическим закрытием потока ввода (`BufferedReader`).
```java
import java.io.*;
public class Main {

    static void readFile(String file) {

        try (BufferedReader br =
                     new BufferedReader(new FileReader(file))) {

            String line;

            while ((line = br.readLine()) != null) {
                System.out.println(line);
            }

        } catch (IOException e) {
            System.out.println("Ошибка чтения");
        }
    }

    public static void main(String[] args) {
        readFile("config.txt");
    }
}
```
3. **Парсинг JSON-поля возраста:** Реализуйте метод `int parseAge(String ageStr)`, выбрасывающий непроверяемое исключение `InvalidUserDataException`, если возраст отрицательный или превышает 130.
```java
public class Main {

    static class InvalidUserDataException
            extends RuntimeException {

        InvalidUserDataException(String message) {
            super(message);
        }
    }

    static int parseAge(String ageStr) {

        int age = Integer.parseInt(ageStr);

        if (age < 0 || age > 130) {
            throw new InvalidUserDataException("Неверный возраст");
        }

        return age;
    }

    public static void main(String[] args) {

        try {
            System.out.println(parseAge("25"));
        } catch (InvalidUserDataException e) {
            System.out.println(e.getMessage());
        }
    }
}
```
4. **Множественные блоки catch:** Напишите блок обработки `try-catch`, раздельно обрабатывающий `NullPointerException`, `IndexOutOfBoundsException` и общий `Exception`.
```java
public class Main {

    public static void main(String[] args) {

        try {
            String text = null;
            System.out.println(text.length());

            int[] numbers = {1, 2};
            System.out.println(numbers[5]);

        } catch (NullPointerException e) {
            System.out.println("Объект равен null");

        } catch (IndexOutOfBoundsException e) {
            System.out.println("Неверный индекс");

        } catch (Exception e) {
            System.out.println("Другая ошибка");
        }
    }
}
```
5. **Безопасное извлечение значения из Bundle:** Напишите утилитный метод, безопасно читающий строковое значение по ключу, возвращающий значение по умолчанию при возникновении любой ошибки.
```java
import java.util.*;
public class Main {

    static String getValue(
            Map<String, String> bundle,
            String key) {

        try {
            return bundle.getOrDefault(key, "default");
        } catch (Exception e) {
            return "default";
        }
    }

    public static void main(String[] args) {

        Map<String, String> bundle = new HashMap<>();

        bundle.put("name", "Alex");

        System.out.println(getValue(bundle, "name"));
        System.out.println(getValue(bundle, "age"));
    }
}
```

---

## Практические задания:

Каждое задание представляет собой фрагмент реальной логики мобильного приложения (Android UI-стейт, работа с хранилищем, сетью, сенсорами и валидацией).

### Блок 1: Авторизация, безопасность и валидация ввода

1. **Валидатор надежности пароля:** Проверьте пароль на соответствие критериям: минимум 8 символов, хотя бы одна заглавная буква, одна цифра и один специальный знак (`!@#$%^&*`).
```java
public class Main {

    static boolean checkPassword(String pass) {

        if (pass.length() < 8)
            return false;

        boolean big = false;
        boolean digit = false;
        boolean symbol = false;

        for (char c : pass.toCharArray()) {

            if (Character.isUpperCase(c))
                big = true;

            if (Character.isDigit(c))
                digit = true;

            if ("!@#$%^&*".contains("" + c))
                symbol = true;
        }

        return big && digit && symbol;
    }


    public static void main(String[] args) {
        System.out.println(checkPassword("Hello1!"));
    }
}
```
2. **Нормализатор телефонных номеров:** Принимайте строку в произвольном формате (например, `8 (999) 000-11-22` или `+7 999 000 11 22`) и приводите её к строгому стандарту E.164 (`+79990001122`).
```java
public class Main {

    static String normalizePhone(String phone) {

        phone = phone.replaceAll("[^0-9]", "");
        if (phone.startsWith("8")) {
            phone = "7" + phone.substring(1);
        }
        return "+" + phone;
    }


    public static void main(String[] args) {

        System.out.println(
            normalizePhone("8 (999) 000-11-22")
        );
    }
}
```
3. **Генератор одноразового SMS-кода:** Напишите метод генерации 6-значного числового OTP-кода.
```java
import java.util.*;
public class Main {

    static String generateCode() {

        Random r = new Random();
        int code = 100000 + r.nextInt(900000);
        return "" + code;
    }

    public static void main(String[] args) {

        System.out.println(generateCode());
    }
}
```
4. **Проверка срока действия JWT-токена:** Дан Unix-таймстемп истечения токена в секундах. Определите, активен ли токен относительно текущего системного времени.
```java
public class Main {

    static boolean isTokenActive(long expireTime) {

        long now = System.currentTimeMillis();
        return now < expireTime;
    }

    public static void main(String[] args) {

        long token = System.currentTimeMillis() + 10000;
        System.out.println(
                isTokenActive(token)
        );
    }
}
```
5. **Маскировка персональных данных (PII):** Замаскируйте адрес электронной почты: `alexander.ivanov@mail.ru` -> `a***********v@mail.ru`.
```java
public class Main {

    static String maskEmail(String email) {

        int index = email.indexOf("@");
        String name = email.substring(0, index);
        String result = name.charAt(0)
                + "**********"
                + email.substring(index - 1);
        return result;
    }


    public static void main(String[] args) {

        System.out.println(
                maskEmail("alexander.ivanov@mail.ru")
        );
    }
}
```
6. **Блокировщик брутфорса:** Реализуйте класс `LoginThrottler`, который после 5 неудачных попыток ввода пароля блокирует ввод на 60 секунд.
```java
public class Main {

    static class LoginThrottler {
        int attempts = 0;
        long blockedUntil = 0;

        boolean login(boolean correct) {
            if (System.currentTimeMillis() < blockedUntil)
                return false;

            if (correct) {
                attempts = 0;
                return true;
            }

            attempts++;

            if (attempts >= 5) {
                blockedUntil = System.currentTimeMillis() + 60000;
            }
            return false;
        }
    }

    public static void main(String[] args) {
        LoginThrottler l = new LoginThrottler();
        System.out.println(l.login(false));
    }
}
```
7. **Шифратор перестановкой для локальных заметок:** Напишите простейший обратимый шифратор строк для сокрытия черновиков в локальной базе.
```java
public class Main {

    static String encrypt(String text) {
        String result = "";

        for (char c : text.toCharArray()) {
            result += (char)(c + 1);
        }

        return result;
    }

    static String decrypt(String text) {
        String result = "";
        for (char c : text.toCharArray()) {
            result += (char)(c - 1);
        }

        return result;
    }

    public static void main(String[] args) {
        String text = "Hello";

        String encrypted = encrypt(text);
        System.out.println(encrypted);
        System.out.println(decrypt(encrypted));
    }
}
```
8. **Проверка биометрической готовности:** Напишите логику оценки возможности аутентификации по отпечатку пальца на основе набора булевых флагов оборудования и разрешений.
```java
public class Main {

    static boolean isReady(boolean hardware, boolean permission) {
        return hardware && permission;
    }

    public static void main(String[] args) {

        boolean hardware = true;
        boolean permission = true;
        System.out.println(
                isReady(hardware, permission)
        );
    }
}
```
9. **Контроль сессии по тайм-ауту:** Реализуйте сброс состояния экрана в случае неактивности пользователя более 3 минут.
```java
public class Main {

    static boolean isExpired(long lastAction) {

        long now = System.currentTimeMillis();
        return now - lastAction > 3 * 60 * 1000;
    }

    public static void main(String[] args) {

        long lastAction = System.currentTimeMillis();
        System.out.println(isExpired(lastAction));
    }
}
```
10. **Валидатор промокода:** Проверьте правильность промокода по маске: 4 заглавные латинские буквы, дефис, 4 цифры (например, `SALE-2026`).
```java
public class Main {

    static boolean checkPromo(String code) {

        return code.matches("[A-Z]{4}-[0-9]{4}");
    }

    public static void main(String[] args) {

        System.out.println(
                checkPromo("SALE-2026")
        );
    }
}
```

### Блок 2: Работа со списками, каталогами и кэшем (RecyclerView Logic)

11. **DiffUtil-компаратор элементов списка:** Напишите метод, принимающий старый и новый списки элементов `NewsItem` и возвращающий список ID измененных и удаленных позиций.
```java
import java.util.*;

public class Main {

    static List<Integer> compare(
            List<String> oldList,
            List<String> newList) {

        List<Integer> result = new ArrayList<>();

        int size = Math.max(oldList.size(), newList.size());

        for (int i = 0; i < size; i++) {

            if (i >= oldList.size() ||
                i >= newList.size() ||
                !oldList.get(i).equals(newList.get(i))) {

                result.add(i);
            }
        }

        return result;
    }


    public static void main(String[] args) {

        List<String> oldList =
                Arrays.asList("A", "B", "C");

        List<String> newList =
                Arrays.asList("A", "X", "C");

        System.out.println(
                compare(oldList, newList)
        );
    }
}
```
12. **Пагинация ленты новостей:** Напишите класс `PaginationHelper`, который по номеру страницы `page` и размеру страницы `pageSize = 20` извлекает нужный срез из общего массива новостей.
```java
import java.util.*;
public class Main {

    static Map<Character, List<String>> group(
            List<String> names) {

        Map<Character, List<String>> map =
                new HashMap<>();

        for (String name : names) {

            char c = name.charAt(0);

            map.putIfAbsent(c, new ArrayList<>());

            map.get(c).add(name);
        }

        return map;
    }


    public static void main(String[] args) {

        List<String> names =
                Arrays.asList(
                "Alex", "Anna", "Bob"
                );

        System.out.println(group(names));
    }
}
```
13. **Группировка контактов по первой букве:** Дан список имен. Сгруппируйте их в `Map<Character, List<String>>` для отображения заголовков секций.
```java
import java.util.*;

public class Main {

    static Map<Character, List<String>> group(
            List<String> names) {

        Map<Character, List<String>> map =
                new HashMap<>();

        for (String name : names) {

            char c = name.charAt(0);

            map.putIfAbsent(c, new ArrayList<>());

            map.get(c).add(name);
        }

        return map;
    }


    public static void main(String[] args) {

        List<String> names =
                Arrays.asList(
                "Alex", "Anna", "Bob"
                );

        System.out.println(group(names));
    }
}
```
14. **Полнотекстовый фильтр списка:** Реализуйте метод фильтрации каталога товаров по вхождению подстроки в наименование или артикул без учета регистра.
```java
import java.util.*;

public class Main {

    static List<String> search(
            List<String> products,
            String text) {

        List<String> result =
                new ArrayList<>();

        for (String p : products) {

            if (p.toLowerCase()
                    .contains(text.toLowerCase())) {

                result.add(p);
            }
        }

        return result;
    }


    public static void main(String[] args) {

        List<String> products =
                Arrays.asList(
                "Phone",
                "Laptop",
                "Tablet"
                );

        System.out.println(
                search(products, "top")
        );
    }
}
```
15. **Карусель промо-баннеров:** Реализуйте циклическое получение следующего элемента баннера по индексу текущего клика.
```java
public class Main {

    static String nextBanner(
            String[] banners,
            int index) {

        return banners[index % banners.length];
    }


    public static void main(String[] args) {

        String[] banners = {
                "Sale",
                "New",
                "Gift"
        };

        System.out.println(
                nextBanner(banners, 4)
        );
    }
}
```
16. **Подсчет суммарной стоимости корзины с учетом промокода:** Рассчитайте сумму позиций с учетом скидок на отдельные категории товаров.
```java
import java.util.*;

public class Main {

    static double calculate(
            double[] prices,
            String promo) {

        double sum = 0;

        for (double p : prices) {
            sum += p;
        }

        if (promo.equals("SALE")) {
            sum *= 0.9;
        }

        return sum;
    }


    public static void main(String[] args) {

        double[] cart = {100, 200, 300};

        System.out.println(
                calculate(cart, "SALE")
        );
    }
}
```
17. **Удаление свайпом с возможностью отмены (Undo):** Спроектируйте логику временного буфера удаленного элемента списка с таймером фиксации удаления.
```java
import java.util.*;

public class Main {

    static String deleted;


    static void delete(
            List<String> list,
            int index) {

        deleted = list.remove(index);
    }


    static void undo(
            List<String> list) {

        if (deleted != null) {
            list.add(deleted);
            deleted = null;
        }
    }


    public static void main(String[] args) {

        List<String> chats =
                new ArrayList<>(
                Arrays.asList("A", "B", "C"));

        delete(chats, 1);

        undo(chats);

        System.out.println(chats);
    }
}
```
18. **Сортировка чатов по времени последнего сообщения:** Отсортируйте список диалогов так, чтобы сверху оказались диалоги с самыми свежими сообщениями.
```java
import java.util.*;
public class Main {

    static class Chat {

        String name;
        long time;


        Chat(String name, long time) {
            this.name = name;
            this.time = time;
        }
    }


    public static void main(String[] args) {

        List<Chat> chats = new ArrayList<>();

        chats.add(new Chat("Alex", 100));
        chats.add(new Chat("Bob", 300));
        chats.add(new Chat("Tom", 200));


        chats.sort((a,b) ->
                Long.compare(b.time, a.time));


        for (Chat c : chats) {
            System.out.println(c.name);
        }
    }
}
```
19. **Поиск дубликатов в галерее по контрольной сумме:** Напишите алгоритм поиска повторяющихся файлов по размеру и имени.
```java
import java.util.*;
public class Main {

    public static void main(String[] args) {

        List<String> files =
                Arrays.asList(
                "a.jpg",
                "b.jpg",
                "a.jpg",
                "c.png"
                );


        Set<String> set = new HashSet<>();

        for (String file : files) {

            if (!set.add(file)) {
                System.out.println(
                        "Дубликат: " + file
                );
            }
        }
    }
}
```
20. **Ограничитель емкости кэша картинок:** Реализуйте стратегию вытеснения самого старого файла (FIFO), если суммарный объем картинок превысил 100 МБ.
```java
import java.util.*;
public class Main {

    static class ImageCache {

        Queue<String> files =
                new LinkedList<>();

        int max = 3;


        void add(String file) {

            if (files.size() >= max) {
                files.poll();
            }

            files.add(file);
        }


        void show() {
            System.out.println(files);
        }
    }


    public static void main(String[] args) {

        ImageCache c = new ImageCache();

        c.add("1.png");
        c.add("2.png");
        c.add("3.png");
        c.add("4.png");

        c.show();
    }
}
```

### Блок 3: Сеть, парсинг данных и офлайн-синхронизация

21. **Парсер параметров диплинка (Deep Link):** Извлеките параметры маршрутизации из URL вида `app://shop/product?id=452&source=push`.
```java
import java.util.*;
public class Main {

    static Map<String,String> parse(String url) {

        Map<String,String> map = new HashMap<>();

        String query = url.substring(
                url.indexOf("?") + 1
        );

        String[] parts = query.split("&");

        for (String p : parts) {

            String[] data = p.split("=");

            map.put(data[0], data[1]);
        }

        return map;
    }


    public static void main(String[] args) {

        String url =
        "app://shop/product?id=452&source=push";

        System.out.println(parse(url));
    }
}
```
22. **Симулятор Retry-политики запроса:** Смоделируйте выполнение сетевого вызова с 3 повторами при возникновении `SocketTimeoutException`.
```java
public class Main {

    static void request() {

        int delay = 1;

        for (int i = 1; i <= 3; i++) {

            try {

                System.out.println(
                    "Попытка " + i
                );

                throw new Exception();

            } catch (Exception e) {

                System.out.println(
                    "Ошибка, ждать "
                    + delay + " сек"
                );

                delay *= 2;
            }
        }
    }


    public static void main(String[] args) {

        request();
    }
}
```
23. **Очередь отложенных офлайн-действий:** Создайте класс `OfflineActionQueue`, сохраняющий лайки и комментарии при отсутствии сети и отправляющий их пачкой при подключении.
```java
import java.util.*;
public class Main {

    static class OfflineActionQueue {

        Queue<String> queue =
                new LinkedList<>();


        void add(String action) {
            queue.add(action);
        }


        void send() {

            while (!queue.isEmpty()) {

                System.out.println(
                    "Отправлено: "
                    + queue.poll()
                );
            }
        }
    }


    public static void main(String[] args) {

        OfflineActionQueue q =
                new OfflineActionQueue();

        q.add("Like");
        q.add("Comment");

        q.send();
    }
}
```
24. **Слияние локальных данных с сервером (Conflict Resolver):** Напишите резолвер конфликта версии заметки: если серверная версия новее локальной, обновлять локальную, иначе отправлять запрос на перезапись.
```java
public class Main {

    static String resolve(
            int localVersion,
            int serverVersion) {


        if (serverVersion > localVersion) {
            return "Обновить локальные данные";
        }

        return "Отправить данные на сервер";
    }


    public static void main(String[] args) {

        System.out.println(
                resolve(2, 3)
        );
    }
}
```
25. **Оценка скорости скачивания файла:** По количеству байт и времени в миллисекундах рассчитайте скорость передачи в КБ/с и Мбит/с.
```java
public class Main {

    public static void main(String[] args) {

        long bytes = 5000000;
        long time = 10000; // мс


        double kb =
                bytes / 1024.0 /
                (time / 1000.0);


        double mb =
                bytes * 8 /
                1024 / 1024 /
                (time / 1000.0);


        System.out.println(
                kb + " KB/s"
        );

        System.out.println(
                mb + " Mbit/s"
        );
    }
}
```
26. **Парсинг заголовков пагинации сервера:** Извлеките из заголовка `Link: <https://api.com/items?page=3>; rel="next"` номер следующей страницы.
```java
import java.util.*;
public class Main {

    static List<String> parse(String link) {

        List<String> result =
                new ArrayList<>();

        String[] parts = link.split(",");

        for (String p : parts) {

            int start = p.indexOf("<") + 1;
            int end = p.indexOf(">");

            result.add(
                p.substring(start, end)
            );
        }

        return result;
    }


    public static void main(String[] args) {

        String link =
        "<page1>,<page2>,<page3>";

        System.out.println(parse(link));
    }
}
```
27. **Проверка актуальности кэша по ETag:** Реализуйте логику: если переданный `ETag` совпадает с сохраненным, возвращать ответ `304 Not Modified` без загрузки тела.
```java
public class Main {

    static boolean checkCache(
            String oldTag,
            String newTag) {

        return oldTag.equals(newTag);
    }


    public static void main(String[] args) {

        System.out.println(
                checkCache("abc", "abc")
        );
    }
}
```
28. **Форматирование байтов в читаемый вид:** Переведите число байт в строку: `1024` -> `"1.0 KB"`, `1048576` -> `"1.0 MB"`, `1536` -> `"1.5 KB"`.
```java
public class Main {

    static String format(long bytes) {

        if (bytes < 1024)
            return bytes + " B";

        if (bytes < 1024 * 1024)
            return bytes / 1024 + " KB";

        return bytes / 1024 / 1024 + " MB";
    }


    public static void main(String[] args) {

        System.out.println(
                format(5000000)
        );
    }
}
```
29. **Имитация веб-сокета для биржевого виджета:** Напишите интерфейс слушателя котировок валют и генератор событий с интервалом.
```java
public class Main {

    static class WebSocket {

        void connect() {
            System.out.println("Подключено");
        }


        void send(String msg) {
            System.out.println(msg);
        }


        void close() {
            System.out.println("Закрыто");
        }
    }


    public static void main(String[] args) {

        WebSocket socket =
                new WebSocket();

        socket.connect();

        socket.send("Hello");

        socket.close();
    }
}
```
30. **Валидатор ответа API:** Напишите метод проверки обязательных полей JSON-объекта профиля на `null`.
```java
public class Main {

    static boolean isSuccess(int code) {

        return code >= 200 &&
               code < 300;
    }


    public static void main(String[] args) {

        System.out.println(
                isSuccess(200)
        );
    }
}
```

### Блок 4: Управление состоянием экрана и UI-архитектура

31. **Конечный автомат экрана загрузки (UI State MVI):** Реализуйте переключение состояний экрана через `sealed`-иерархию или `enum`: `Loading`, `Success(data)`, `Empty`, `Error(message)`.
```java
public class Main {

    enum State {
        LOADING,
        SUCCESS,
        EMPTY,
        ERROR
    }


    static void show(State state) {

        switch (state) {

            case LOADING:
                System.out.println("Загрузка...");
                break;

            case SUCCESS:
                System.out.println("Успешно");
                break;

            case EMPTY:
                System.out.println("Пусто");
                break;

            case ERROR:
                System.out.println("Ошибка");
                break;
        }
    }


    public static void main(String[] args) {

        show(State.SUCCESS);
    }
}
```
32. **Дебаунсер кликов (Anti-Double Click):** Напишите класс-обертку над кликом, предотвращающий повторный вызов действия, если с момента прошлого клика прошло менее 500 мс.
```java
public class Main {

    static class ClickDebouncer {

        long lastClick = 0;


        boolean click() {

            long now = System.currentTimeMillis();

            if (now - lastClick < 500) {
                return false;
            }

            lastClick = now;

            return true;
        }
    }


    public static void main(String[] args) {

        ClickDebouncer c =
                new ClickDebouncer();

        System.out.println(c.click());
        System.out.println(c.click());
    }
}
```
33. **Стек навигации экранов (BackStack):** Реализуйте собственную структуру данных стека экранов с поддержкой операций `push(Screen)`, `pop()` и `popToRoot()`.
```java
import java.util.*;
public class Main {

    static class ScreenStack {

        Stack<String> screens =
                new Stack<>();


        void push(String screen) {
            screens.push(screen);
        }


        String pop() {
            return screens.pop();
        }


        void popToRoot() {

            while (screens.size() > 1) {
                screens.pop();
            }
        }
    }


    public static void main(String[] args) {

        ScreenStack stack =
                new ScreenStack();

        stack.push("Home");
        stack.push("Profile");
        stack.push("Settings");

        stack.popToRoot();

        System.out.println(stack.screens);
    }
}
```
34. **Моделирование темной и светлой темы:** Спроектируйте класс `ThemePalette`, возвращающий шестнадцатеричные коды цветов (HEX) в зависимости от выбранного режима оформления.
```java
public class Main {

    static class ThemePalette {


        String background(boolean dark) {

            if (dark)
                return "#000000";

            return "#FFFFFF";
        }


        String text(boolean dark) {

            if (dark)
                return "#FFFFFF";

            return "#000000";
        }
    }


    public static void main(String[] args) {

        ThemePalette t =
                new ThemePalette();

        System.out.println(
                t.background(true)
        );

        System.out.println(
                t.text(true)
        );
    }
}
```
35. **Расчет прогресса заполнения профиля:** Определите процент заполненности аккаунта пользователя (аватар, био, телефон, почта, город).
```java
public class Main {

    static int progress(
            String avatar,
            String bio,
            String phone,
            String email,
            String city) {

        int count = 0;

        if (avatar != null) count++;
        if (bio != null) count++;
        if (phone != null) count++;
        if (email != null) count++;
        if (city != null) count++;


        return count * 100 / 5;
    }


    public static void main(String[] args) {

        System.out.println(
                progress(
                "img",
                "text",
                null,
                "mail",
                "Berlin")
        );
    }
}
```
36. **Инвертор цвета текста для контрастности:** По заданному цвету фона (RGB) вычислите, какой цвет текста отображать для читаемости — белый или черный (по формуле яркости YIQ).
```java
public class Main {

    static String getTextColor(
            int r, int g, int b) {


        int yiq =
        (r * 299 + g * 587 + b * 114) / 1000;


        if (yiq >= 128)
            return "BLACK";

        return "WHITE";
    }


    public static void main(String[] args) {

        System.out.println(
                getTextColor(20,20,20)
        );
    }
}
```
37. **Форматирование счетчика лайков:** Переведите большие числа в сокращения: `950` -> `"950"`, `1200` -> `"1.2K"`, `1500000` -> `"1.5M"`.
```java
public class Main {

    static String formatLikes(int likes) {

        if (likes >= 1000000)
            return likes / 1000000 + "M";

        if (likes >= 1000)
            return likes / 1000 + "K";


        return "" + likes;
    }


    public static void main(String[] args) {

        System.out.println(
                formatLikes(1500000)
        );
    }
}
```
38. **Менеджер системных диалогов:** Реализуйте класс очереди показа диалоговых окон, исключающий перекрытие одного всплывающего окна другим.
```java
import java.util.*;

public class Main {

    static class DialogManager {

        Queue<String> dialogs =
                new LinkedList<>();


        void add(String dialog) {
            dialogs.add(dialog);
        }


        void showNext() {

            if (!dialogs.isEmpty()) {

                System.out.println(
                    dialogs.poll()
                );
            }
        }
    }


    public static void main(String[] args) {

        DialogManager d =
                new DialogManager();

        d.add("Ошибка сети");
        d.add("Обновление");

        d.showNext();
        d.showNext();
    }
}
```
39. **Валидатор состояния кнопки "Оплатить":** Кнопка активна только при условии: корзина не пуста, выбран способ оплаты, адрес доставки подтвержден.
```java
public class Main {

    static boolean canPay(
            boolean cartEmpty,
            boolean payment,
            boolean address) {


        return !cartEmpty &&
                payment &&
                address;
    }


    public static void main(String[] args) {

        System.out.println(
                canPay(false,true,true)
        );
    }
}
```
40. **Транслятор ошибок для пользователя:** Напишите метод, переводящий системные исключения (`TimeoutException`, `UnknownHostException`) в понятные человекочитаемые подсказки на русском языке.
```java
public class Main {

    static String translate(String error) {


        switch(error) {

            case "TimeoutException":
                return "Превышено время ожидания";

            case "UnknownHostException":
                return "Нет подключения к серверу";

            default:
                return "Неизвестная ошибка";
        }
    }


    public static void main(String[] args) {

        System.out.println(
                translate("TimeoutException")
        );
    }
}
```

### Блок 5: Аппаратные функции, геолокация и фоновые задачи

41. **Расчет расстояния между двумя GPS-точками:** Реализуйте формулу гаверсинусов (Haversine formula) для определения дистанции в метрах между координатами пользователя и курьера.
```java
public class Main {

    static double distance(
            double lat1, double lon1,
            double lat2, double lon2) {


        double r = 6371000;

        double dLat = Math.toRadians(lat2 - lat1);
        double dLon = Math.toRadians(lon2 - lon1);


        double a = Math.sin(dLat / 2) *
                Math.sin(dLat / 2) +
                Math.cos(Math.toRadians(lat1)) *
                Math.cos(Math.toRadians(lat2)) *
                Math.sin(dLon / 2) *
                Math.sin(dLon / 2);


        double c = 2 * Math.atan2(
                Math.sqrt(a),
                Math.sqrt(1 - a)
        );


        return r * c;
    }


    public static void main(String[] args) {

        System.out.println(
                distance(50,30,51,31)
        );
    }
}
```
42. **Определитель вхождения в геозону (Geofencing):** Напишите метод, определяющий, находится ли точка с координатами `(lat, lon)` внутри окружности радиуса `R` с центром в `(centerLat, centerLon)`.
```java
public class Main {

    static boolean inside(
            double lat,
            double lon,
            double centerLat,
            double centerLon,
            double radius) {


        double d =
        Math.sqrt(
            Math.pow(lat-centerLat,2) +
            Math.pow(lon-centerLon,2)
        );


        return d <= radius;
    }


    public static void main(String[] args) {

        System.out.println(
                inside(10,10,10,11,2)
        );
    }
}
```
43. **Энергосберегающий планировщик геолокации:** Изменяйте частоту опроса датчика GPS в зависимости от уровня заряда батареи: > 50% (каждые 5 сек), 15-50% (каждые 30 сек), < 15% (раз в 5 минут).
```java
public class Main {

    static int getInterval(int battery) {

        if (battery > 50)
            return 5;

        if (battery >= 15)
            return 30;

        return 300;
    }


    public static void main(String[] args) {

        System.out.println(
                getInterval(20) + " сек"
        );
    }
}
```
44. **Детектор падения смартфона (акселерометр):** Напишите алгоритм, определяющий состояние невесомости / резкого ускорения по 3 осям `(X, Y, Z)` при превышении критического порога.
```java
public class Main {

    static boolean fall(
            double x,
            double y,
            double z) {


        double force =
                Math.sqrt(
                x*x + y*y + z*z);


        return force > 15;
    }


    public static void main(String[] args) {

        System.out.println(
                fall(10,10,10)
        );
    }
}
```
45. **Шагомер на основе пиковых амплитуд:** Дан массив показаний вертикального ускорения. Подсчитайте количество совершенных шагов по локальным максимумам выше заданного барьера.
```java
public class Main {

    static int countSteps(double[] data) {

        int steps = 0;

        for(int i = 1; i < data.length-1; i++) {

            if(data[i] > data[i-1] &&
               data[i] > data[i+1] &&
               data[i] > 10) {

                steps++;
            }
        }


        return steps;
    }

    public static void main(String[] args) {

        double[] a = {5,12,4,15,3};

        System.out.println(
                countSteps(a)
        );
    }
}
```
46. **Контроллер яркости по датчику освещенности:** Преобразуйте люксы (Lux) внешнего освещения в процент яркости экрана (0-100%) по логарифмической шкале.
```java
public class Main {

    static int brightness(double lux) {

        double value =
                Math.log10(lux + 1) * 25;

        if(value > 100)
            value = 100;

        return (int)value;
    }

    public static void main(String[] args) {

        System.out.println(
                brightness(300)
        );
    }
}
```
47. **Планировщик фоновой синхронизации данных:** Проверьте совокупность системных условий для старта тяжелой синхронизации: подключение к Wi-Fi + устройство подключено к зарядному устройству.
```java
public class Main {

    static boolean canSync(
            boolean wifi,
            boolean charging) {

        return wifi && charging;
    }


    public static void main(String[] args) {

        System.out.println(
                canSync(true,true)
        );
    }
}
```
48. **Монитор расхода мобильного трафика:** Спроектируйте класс, суммирующий трафик раздельно для мобильной сети и Wi-Fi с предупреждением при достижении лимита в 5 ГБ.
```java
public class Main {

    static class Traffic {

        long mobile;
        long wifi;

        void addMobile(long mb) {
            mobile += mb;
        }

        void addWifi(long mb) {
            wifi += mb;
        }

        void show() {
            System.out.println(
                    "Mobile: " + mobile +
                    " WiFi: " + wifi
            );
        }
    }

    public static void main(String[] args) {

        Traffic t = new Traffic();
        t.addMobile(100);
        t.addWifi(500);
        t.show();
    }
}
```
49. **Плеер аудиофайлов (State Machine):** Смоделируйте жизненный цикл аудиоплеера: `IDLE` -> `INITIALIZED` -> `PREPARED` -> `PLAYING` -> `PAUSED` -> `STOPPED`. Блокируйте недопустимые переходы.
```java
public class Main {

    enum State {
        IDLE,
        PLAYING,
        PAUSED,
        STOPPED
    }

    public static void main(String[] args) {

        State state = State.IDLE;
        state = State.PLAYING;
        System.out.println(state);
    }
}
```
50. **Логгер крашей приложения:** Реализуйте запись стектрейса ошибки, версии Android, модели смартфона и свободного места на накопителе в форматированный отчет об аварийном завершении.
```java
public class Main {

    static String createReport(
            String error,
            String android,
            String phone) {

        return "Ошибка: " + error +
                "\nAndroid: " + android +
                "\nТелефон: " + phone;
    }

    public static void main(String[] args) {

        System.out.println(
                createReport(
                "Crash",
                "14",
                "Samsung")
        );
    }
}
```

---
