---
title: "Практическая работа №4: Создание первых Android-приложений на Java"
discipline: "Разработка мобильных приложений"
status: "Active"
author: "УПМ 2"
tags: [android, beginner, java, layout, intent, view-binding, tutorial]
---
## 📋 Содержание

1. [Анатомия Android Studio и первый запуск](#1-анатомия-android-studio-и-первый-запуск)
2. [Структура проекта: куда смотреть и что где лежит](#2-структура-проекта-куда-смотреть-и-что-где-лежит)
3. [Разметка экрана (XML): создаем визуальный интерфейс](#3-разметка-экрана-xml-создаем-визуальный-интерфейс)
4. [Оживление экрана: связываем XML с кодом на Java](#4-оживление-экрана-связываем-xml-с-кодом-на-java)
5. [Ресурсы: почему нельзя писать текст прямо в коде](#5-ресурсы-почему-нельзя-писать-текст-прямо-в-коде)
6. [Второй экран и переходы между ними (Intent)](#6-второй-экран-и-переходы-между-ними-intent)
7. [3 Готовых учебных проекта с построчным разбором](#7-3-готовых-учебных-проекта-с-построчным-разбором)
8. [Топ-12 критических ошибок новичков и их решение](#8-топ-12-критических-ошибок-новичков-и-их-решение)
9. [30 Практических заданий на самостоятельную разработку](#9-30-практических-заданий-на-самостоятельную-разработку)
10. [Чек-лист студента перед показом работы](#10-чек-лист-студента-перед-показом-работы)

---

## 1. Анатомия Android Studio и первый запуск

### Шаг 1. Создание нового проекта

1. Запустите **Android Studio**.
2. В стартовом окне нажмите **New Project** (или выберите в меню: `File -> New -> New Project...`).
3. В списке шаблонов выберите категорию **Phone and Tablet**, а затем шаблон **Empty Views Activity**.

> [!IMPORTANT]
> Обратите внимание: выбирайте именно **Empty Views Activity** (с синей иконкой классической разметки), а **НЕ** _Empty Activity_. Шаблон _Empty Activity_ в современных версиях создает проект на базе декларативного фреймворка Jetpack Compose без использования XML и Java.

4. Нажмите **Next** и заполните параметры проекта:
   - **Name:** Имя приложения (например, `MyFirstApp`).
   - **Package name:** Идентификатор пакета строчными буквами (например, `com.example.myfirstapp`).
   - **Save location:** Путь к папке на диске (крайне желательно без пробелов и русских букв в пути).
   - **Language:** Обязательно переключите на **Java** (по умолчанию часто стоит Kotlin).
   - **Minimum SDK:** Выберите `API 26: Android 8.0 (Oreo)` или `API 28`. Это обеспечит работу на 95%+ реальных устройств.
   - **Build configuration language:** Рекомендуется оставить дефолтный `Kotlin DSL (build.gradle.kts)` или `Groovy DSL`.
5. Нажмите **Finish**. Дождитесь, пока внизу в строке состояния завершится процесс **Gradle Sync** (при первом запуске это может занять 2–5 минут, среда скачивает необходимые компоненты).

### Шаг 2. Подготовка устройства для запуска

#### Вариант А: Запуск на реальном смартфоне (Рекомендуется для слабых ПК)

1. Откройте на смартфоне **Настройки -> О телефоне**.
2. Быстро нажмите 7 раз подряд на пункт **Номер сборки** (Build number), пока не появится надпись _"Вы стали разработчиком!"_.
3. Вернитесь в общее меню настроек -> **Система (или Для разработчиков)** -> включите тумблер **Отладка по USB (USB Debugging)**.
4. Подключите смартфон к компьютеру через USB-кабель. На экране смартфона появится всплывающий запрос _"Разрешить отладку по USB?"_ — поставьте галочку _"Всегда разрешать с этого компьютера"_ и нажмите **ОК**.
5. В верхней панели Android Studio в выпадающем списке устройств появится модель вашего смартфона.

#### Вариант Б: Виртуальный эмулятор (AVD)

1. На верхней панели справа нажмите на значок **Device Manager** (иконка смартфона с роботом).
2. Нажмите кнопку **Create Device** (или знак `+`).
3. Выберите модель (например, `Pixel 8` или `Pixel 7`).
4. Нажмите **Next**, выберите образ системы (например, `VanillaIceCream` или `UpsideDownCake`) и скачайте его по ссылке **Download**.
5. Нажмите **Finish**. Теперь устройство можно запустить кнопкой Play.

---

## 2. Структура проекта: куда смотреть и что где лежит

В левой панели переключите режим отображения проекта в положение **Android** (в верхнем выпадающем меню проводника). Новичку нужны только три папки:

```
app/
├── manifests/
│   └── AndroidManifest.xml       <-- Паспорт приложения (разрешения, список экранов)
├── java/
│   └── com.example.myfirstapp/
│       └── MainActivity.java     <-- Логика экрана: кнопки, расчеты, действия
└── res/
    ├── layout/
    │   └── activity_main.xml     <-- Внешний вид экрана: кнопки, поля ввода, текст
    ├── values/
    │   ├── strings.xml           <-- Все текстовые надписи приложения
    │   ├── colors.xml            <-- Палитра цветов
    │   └── themes.xml            <-- Шрифты, стили кнопок и тема (темная/светлая)
    └── mipmap/ (или drawable/)    <-- Иконки и картинки
```

---

## 3. Разметка экрана (XML): создаем визуальный интерфейс

Откройте файл `res/layout/activity_main.xml`. В правом верхнем углу редактора есть три режима:

- **Code:** Только XML-текст.
- **Design:** Только визуальный редактор с перетаскиванием элементов.
- **Split:** Разделенный экран (слева код, справа живой предпросмотр — **самый удобный режим!**).

### Базовые элементы интерфейса (Виджеты)

1. `TextView` — текстовая метка для отображения надписей, заголовков и результатов.
2. `EditText` — поле ввода, куда пользователь вводит текст, пароль или числа.
3. `Button` — кнопка, на которую можно нажимать.
4. `ImageView` — контейнер для показа картинок.

### Размеры: wrap_content, match_parent и dp

- `wrap_content` — элемент занимает ровно столько места, сколько нужно его содержимому (тексту внутри).
- `match_parent` — элемент растягивается на всю доступную ширину или высоту родительского контейнера.
- `dp` (density-independent pixels) — независимые от плотности пиксели для задания отступов и размеров кнопок (например, `16dp`).
- `sp` (scale-independent pixels) — специальные единицы **только для размера шрифтов** (например, `18sp`), учитывающие системные настройки слабовидящих.

### Контейнер LinearLayout: простой и предсказуемый

Для новичков самым простым является `LinearLayout`. Он выстраивает все элементы строго друг за другом: вертикально (сверху вниз) или горизонтально (слева направо).

### Пример понятной разметки (activity_main.xml)

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="24dp"
    android:gravity="center_horizontal">

    <!-- Заголовок -->
    <TextView
        android:id="@+id/textViewTitle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Добро пожаловать!"
        android:textSize="24sp"
        android:textStyle="bold"
        android:layout_marginBottom="16dp" />

    <!-- Поле для ввода имени -->
    <EditText
        android:id="@+id/editTextName"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Введите ваше имя"
        android:inputType="textPersonName"
        android:layout_marginBottom="16dp" />

    <!-- Кнопка действия -->
    <Button
        android:id="@+id/buttonGreet"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Поздороваться"
        android:layout_marginBottom="24dp" />

    <!-- Поле для вывода результата -->
    <TextView
        android:id="@+id/textViewResult"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Здесь появится ответ"
        android:textSize="18sp"
        android:textColor="#333333" />

</LinearLayout>
```

### Задания для закрепления темы

1. Измените атрибут `android:orientation` с `vertical` на `horizontal` и посмотрите в режиме Split, что произошло с кнопками.
2. Добавьте второе поле ввода `EditText` для ввода возраста с атрибутом `android:inputType="number"`.
3. Установите для текста заголовка синий цвет с помощью атрибута `android:textColor="#1976D2"`.
4. Сделайте отступ между элементами с помощью `android:layout_marginTop="20dp"`.
5. Добавьте кнопку «Очистить всё» с текстом красного цвета.

---

## 4. Оживление экрана: связываем XML с кодом на Java

Теперь откроем файл `MainActivity.java`. Чтобы программа могла реагировать на нажатия и менять текст, нужно выполнить 3 шага:

1. **Объявить переменные** нужных типов (`TextView`, `EditText`, `Button`).
2. **Связать переменные с ID** из разметки через команду `findViewById()`.
3. **Назначить слушатель нажатия** (`OnClickListener`).

### Полный и подробный код MainActivity.java

```java
package com.example.myfirstapp;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    // Шаг 1: Объявляем ссылки на наши элементы экрана
    private TextView textViewTitle;
    private EditText editTextName;
    private Button buttonGreet;
    private TextView textViewResult;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // Эта строка загружает XML-разметку на экран устройства.
        // До нее вызывать findViewById НЕЛЬЗЯ!
        setContentView(R.layout.activity_main);

        // Шаг 2: Находим каждый элемент по его ID из activity_main.xml
        textViewTitle = findViewById(R.id.textViewTitle);
        editTextName = findViewById(R.id.editTextName);
        buttonGreet = findViewById(R.id.buttonGreet);
        textViewResult = findViewById(R.id.textViewResult);

        // Шаг 3: Вешаем слушатель клика на кнопку
        buttonGreet.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                // Считываем текст, который пользователь напечатал в поле ввода
                String enteredName = editTextName.getText().toString().trim();

                // Проверяем: не пустое ли поле?
                if (enteredName.isEmpty()) {
                    // Показываем всплывающее уведомление (Toast)
                    Toast.makeText(MainActivity.this, "Пожалуйста, введите имя!", Toast.LENGTH_SHORT).show();
                } else {
                    // Формируем приветствие и выводим его в TextView
                    String greeting = "Привет, " + enteredName + "! Рады тебя видеть.";
                    textViewResult.setText(greeting);

                    // Очищаем поле ввода для следующего раза
                    editTextName.setText("");
                }
            }
        });
    }
}
```

> [!TIP]
> **Лямбда-выражение (современный стиль Java):**
> Вместо длинной конструкции `new View.OnClickListener() { ... }` в современной Java можно писать компактно:
>
> ```java
> buttonGreet.setOnClickListener(v -> {
>     // действия при клике
> });
> ```

### Задания для закрепления темы

1. Добавьте всплывающее сообщение `Toast` при успешном выводе приветствия.
2. Добавьте проверку: если пользователь ввел имя длиной менее 2 символов, выводите ошибку: _"Слишком короткое имя"_.
3. Реализуйте кнопку, которая при клике меняет текст заголовка на _"Текст успешно изменен!"_.
4. Реализуйте кнопку, которая скрывает надпись с результатом с помощью команды `textViewResult.setVisibility(View.GONE);`.
5. Сделайте счетчик: при каждом нажатии на кнопку увеличивайте число в `TextView` на единицу.

---

## 5. Ресурсы: почему нельзя писать текст прямо в коде

Если вы напишете в XML `android:text="Привет"`, среда разработки подчеркнет строку желтым цветом и выдаст предупреждение: _Hardcoded string "Привет", should use `@string` resource_.

### Почему это важно:

1. **Мультиязычность:** Вы можете создать перевод на английский, немецкий или китайский язык без изменения единой строчки Java-кода.
2. **Единый центр правок:** Если название компании или слоган встречается на 10 экранах, вы меняете его в одном месте в файле `strings.xml`.

### Как правильно работать со строками:

Откройте `res/values/strings.xml`:

```xml
<resources>
    <string name="app_name">Мое Первое Приложение</string>
    <string name="title_welcome">Добро пожаловать в сервис!</string>
    <string name="hint_enter_name">Введите ваше имя</string>
    <string name="btn_submit">Отправить данные</string>
    <string name="error_empty_field">Это поле не может быть пустым</string>
</resources>
```

В файле разметки XML используйте ссылку через собачку:

```xml
<Button
    android:id="@+id/btnSubmit"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:text="@string/btn_submit" />
```

В коде Java получайте строку так:

```java
String errorMsg = getString(R.string.error_empty_field);
```

### Задания для закрепления темы

1. Вынесите все хардкод-строки из первого проекта в файл `res/values/strings.xml`.
2. Создайте в `res/values/colors.xml` три кастомных цвета: `brand_blue`, `brand_green`, `brand_gray` и примените их к фону и кнопкам.
3. Добавьте векторную иконку через меню `res -> New -> Vector Asset`, выбрав иконку из стандартной базы Material Design, и выведите ее в `ImageView`.
4. Создайте строковый ресурс с параметром подстановки: `<string name="welcome_user">Привет, %1$s!</string>` и выведите его через `getString(R.string.welcome_user, name)`.
5. Создайте альтернативный файл `strings.xml (en)` для английской локализации и проверьте автоматический перевод при смене языка в эмуляторе.

---

## 6. Второй экран и переходы между ними (Intent)

Любое реальное приложение состоит из нескольких экранов. В Android экран называется **Activity**.

### Шаг 1. Создание второй Activity

1. В левой панели нажмите правой кнопкой мыши по папке с пакетом (где лежит `MainActivity`).
2. Выберите: **New -> Activity -> Empty Views Activity**.
3. Назовите экран `SecondActivity` и нажмите **Finish**.
4. Студия автоматически создаст два файла: `SecondActivity.java` и `activity_second.xml`, а также зарегистрирует экран в `AndroidManifest.xml`!

### Шаг 2. Разметка второго экрана (activity_second.xml)

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="24dp"
    android:gravity="center">

    <TextView
        android:id="@+id/textViewReceivedData"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="20sp"
        android:text="Ожидание данных..."
        android:layout_marginBottom="20dp" />

    <Button
        android:id="@+id/buttonClose"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Вернуться назад" />

</LinearLayout>
```

### Шаг 3. Переход с передачей данных из MainActivity.java

```java
// Создаем намерение перейти на SecondActivity
Intent intent = new Intent(MainActivity.this, SecondActivity.class);

// Кладем данные по ключу (ключ-значение)
intent.putExtra("EXTRA_USERNAME", enteredName);
intent.putExtra("EXTRA_AGE", 20);

// Запускаем переход!
startActivity(intent);
```

### Шаг 4. Прием данных в SecondActivity.java

```java
package com.example.myfirstapp;

import android.content.Intent;
import android.os.Bundle;
import android.widget.Button;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;

public class SecondActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_second);

        TextView textViewReceived = findViewById(R.id.textViewReceivedData);
        Button buttonClose = findViewById(R.id.buttonClose);

        // Получаем объект Intent, который открыл этот экран
        Intent incomingIntent = getIntent();

        // Извлекаем данные по тому же ключу, что отправляли
        String userName = incomingIntent.getStringExtra("EXTRA_USERNAME");
        int userAge = incomingIntent.getIntExtra("EXTRA_AGE", 0);

        if (userName != null) {
            textViewReceived.setText("Пользователь: " + userName + "\nВозраст: " + userAge);
        }

        // Закрываем текущий экран и возвращаемся назад
        buttonClose.setOnClickListener(v -> finish());
    }
}
```

### Задания для закрепления темы

1. Создайте третий экран `AboutActivity` с информацией об авторе приложения и кнопкой выхода.
2. Передайте со второго экрана на третий логическое значение `boolean` (например, флаг согласия с правилами).
3. Добавьте проверку: если переданная строка `null`, отображайте надпись: _"Гость"_.
4. Реализуйте передачу дробного числа `double` (например, баланс счета или температура).
5. Создайте кнопку, открывающую системный веб-браузер по ссылке с помощью неявного интента: `new Intent(Intent.ACTION_VIEW, Uri.parse("https://google.com"))`.

---

## 7. 3 Готовых учебных проекта с построчным разбором

---

### Проект 1: «Тап-Кликер» (Счетчик кликов со сменой цвета)

**Идея:** Простая игра, где пользователь нажимает кнопку и зарабатывает очки. При достижении круглых чисел меняется цвет фона, а кнопка сброса возвращает счетчик в ноль.

#### Разметка (`activity_main.xml`)

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/rootLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center"
    android:padding="32dp"
    android:backgroundColor="#F5F5F5">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Счетчик кликов"
        android:textSize="22sp"
        android:layout_marginBottom="16dp" />

    <TextView
        android:id="@+id/tvCounter"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="0"
        android:textSize="64sp"
        android:textStyle="bold"
        android:textColor="#212121"
        android:layout_marginBottom="32dp" />

    <Button
        android:id="@+id/btnClickMe"
        android:layout_width="200dp"
        android:layout_height="60dp"
        android:text="Кликни меня!"
        android:textSize="18sp"
        android:layout_marginBottom="16dp" />

    <Button
        android:id="@+id/btnReset"
        android:layout_width="200dp"
        android:layout_height="wrap_content"
        android:text="Сбросить"
        style="@style/Widget.Material3.Button.OutlinedButton" />

</LinearLayout>
```

#### Исходный код (`MainActivity.java`)

```java
package com.example.clicker;

import android.graphics.Color;
import android.os.Bundle;
import android.widget.Button;
import android.widget.LinearLayout;
import android.widget.TextView;
import android.widget.Toast;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    private int score = 0;
    private TextView tvCounter;
    private LinearLayout rootLayout;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        tvCounter = findViewById(R.id.tvCounter);
        Button btnClickMe = findViewById(R.id.btnClickMe);
        Button btnReset = findViewById(R.id.btnReset);
        rootLayout = findViewById(R.id.rootLayout);

        btnClickMe.setOnClickListener(v -> {
            score++;
            tvCounter.setText(String.valueOf(score));

            // Логика поощрения игрока
            if (score == 10) {
                rootLayout.setBackgroundColor(Color.parseColor("#E8F5E9")); // Светло-зеленый
                Toast.makeText(this, "Отличный старт! 10 очков!", Toast.LENGTH_SHORT).show();
            } else if (score == 50) {
                rootLayout.setBackgroundColor(Color.parseColor("#FFF9C4")); // Золотистый
                Toast.makeText(this, "Половина сотни! Ты мастер клика!", Toast.LENGTH_SHORT).show();
            }
        });

        btnReset.setOnClickListener(v -> {
            score = 0;
            tvCounter.setText("0");
            rootLayout.setBackgroundColor(Color.parseColor("#F5F5F5"));
            Toast.makeText(this, "Счет сброшен", Toast.LENGTH_SHORT).show();
        });
    }
}
```

---

### Проект 2: «Калькулятор чаевых и счета»

**Идея:** Пользователь вводит сумму чека в ресторане и количество гостей. Приложение рассчитывает 10% чаевых и итоговую сумму с человека с защитой от вылета при пустых полях.

#### Разметка (`activity_main.xml`)

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="24dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Калькулятор счета"
        android:textSize="26sp"
        android:textStyle="bold"
        android:layout_marginBottom="24dp" />

    <EditText
        android:id="@+id/etTotalBill"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Сумма чека (руб.)"
        android:inputType="numberDecimal"
        android:layout_marginBottom="12dp" />

    <EditText
        android:id="@+id/etPersonsCount"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Количество человек"
        android:inputType="number"
        android:layout_marginBottom="24dp" />

    <Button
        android:id="@+id/btnCalculate"
        android:layout_width="match_parent"
        android:layout_height="56dp"
        android:text="Рассчитать итог"
        android:layout_marginBottom="24dp" />

    <TextView
        android:id="@+id/tvTipAmount"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Чаевые (10%): 0.00 руб."
        android:textSize="18sp"
        android:layout_marginBottom="8dp" />

    <TextView
        android:id="@+id/tvPerPerson"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="С каждого гостя: 0.00 руб."
        android:textSize="20sp"
        android:textStyle="bold"
        android:textColor="#00796B" />

</LinearLayout>
```

#### Исходный код (`MainActivity.java`)

```java
package com.example.tipcalculator;

import android.os.Bundle;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;
import androidx.appcompat.app.AppCompatActivity;
import java.util.Locale;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        EditText etTotalBill = findViewById(R.id.etTotalBill);
        EditText etPersonsCount = findViewById(R.id.etPersonsCount);
        Button btnCalculate = findViewById(R.id.btnCalculate);
        TextView tvTipAmount = findViewById(R.id.tvTipAmount);
        TextView tvPerPerson = findViewById(R.id.tvPerPerson);

        btnCalculate.setOnClickListener(v -> {
            String billStr = etTotalBill.getText().toString().trim();
            String personsStr = etPersonsCount.getText().toString().trim();

            // Валидация: защита от пустых строк
            if (billStr.isEmpty() || personsStr.isEmpty()) {
                Toast.makeText(this, "Заполните оба поля!", Toast.LENGTH_SHORT).show();
                return;
            }

            try {
                double bill = Double.parseDouble(billStr);
                int persons = Integer.parseInt(personsStr);

                if (bill <= 0) {
                    Toast.makeText(this, "Сумма чека должна быть больше нуля", Toast.LENGTH_SHORT).show();
                    return;
                }

                if (persons <= 0) {
                    Toast.makeText(this, "Количество гостей должно быть не менее 1", Toast.LENGTH_SHORT).show();
                    return;
                }

                // Расчет 10% чаевых
                double tip = bill * 0.10;
                double total = bill + tip;
                double perPerson = total / persons;

                // Вывод результатов с форматированием до двух знаков после запятой
                tvTipAmount.setText(String.format(Locale.getDefault(), "Чаевые (10%%): %.2f руб.", tip));
                tvPerPerson.setText(String.format(Locale.getDefault(), "С каждого гостя: %.2f руб.", perPerson));

            } catch (NumberFormatException e) {
                Toast.makeText(this, "Ошибка ввода чисел!", Toast.LENGTH_SHORT).show();
            }
        });
    }
}
```

---

### Проект 3: «Визитная карточка студента» (Двухэкранное приложение)

**Идея:** На первом экране студент заполняет данные своего профиля (ФИО, группа, специализация). При нажатии кнопки данные передаются на второй экран, оформленный в виде красивой стилизованной ID-карты.

#### Разметка экрана ввода (`activity_main.xml`)

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="24dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Создание визитки"
        android:textSize="24sp"
        android:textStyle="bold"
        android:layout_marginBottom="24dp" />

    <EditText
        android:id="@+id/etFullName"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Фамилия и Имя"
        android:layout_marginBottom="16dp" />

    <EditText
        android:id="@+id/etGroup"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Учебная группа (например, ИВТ-22)"
        android:layout_marginBottom="16dp" />

    <EditText
        android:id="@+id/etSkill"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Любимая технология (Java, SQL, React)"
        android:layout_marginBottom="24dp" />

    <Button
        android:id="@+id/btnGenerateCard"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Сгенерировать карту" />

</LinearLayout>
```

#### Код отправки (`MainActivity.java`)

```java
package com.example.studentcard;

import android.content.Intent;
import android.os.Bundle;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        EditText etFullName = findViewById(R.id.etFullName);
        EditText etGroup = findViewById(R.id.etGroup);
        EditText etSkill = findViewById(R.id.etSkill);
        Button btnGenerate = findViewById(R.id.btnGenerateCard);

        btnGenerate.setOnClickListener(v -> {
            String name = etFullName.getText().toString().trim();
            String group = etGroup.getText().toString().trim();
            String skill = etSkill.getText().toString().trim();

            if (name.isEmpty() || group.isEmpty() || skill.isEmpty()) {
                Toast.makeText(this, "Заполните абсолютно все поля анкеты!", Toast.LENGTH_SHORT).show();
                return;
            }

            Intent intent = new Intent(MainActivity.this, CardActivity.class);
            intent.putExtra("KEY_NAME", name);
            intent.putExtra("KEY_GROUP", group);
            intent.putExtra("KEY_SKILL", skill);
            startActivity(intent);
        });
    }
}
```

#### Разметка карты (`activity_card.xml`)

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center"
    android:padding="24dp"
    android:backgroundColor="#ECEFF1">

    <!-- Карточка студента -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:padding="24dp"
        android:background="#FFFFFF"
        android:elevation="8dp"
        android:layout_marginBottom="32dp">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="СТУДЕНЧЕСКИЙ БИЛЕТ"
            android:textSize="14sp"
            android:letterSpacing="0.1"
            android:textColor="#78909C"
            android:layout_marginBottom="16dp" />

        <TextView
            android:id="@+id/tvCardName"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Иван Иванов"
            android:textSize="22sp"
            android:textStyle="bold"
            android:textColor="#263238"
            android:layout_marginBottom="8dp" />

        <TextView
            android:id="@+id/tvCardGroup"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Группа: ПИ-202"
            android:textSize="16sp"
            android:layout_marginBottom="4dp" />

        <TextView
            android:id="@+id/tvCardSkill"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Направление: Android Development"
            android:textSize="16sp"
            android:textColor="#0288D1" />

    </LinearLayout>

    <Button
        android:id="@+id/btnBack"
        android:layout_width="200dp"
        android:layout_height="wrap_content"
        android:text="Назад к анкете" />

</LinearLayout>
```

#### Код приема (`CardActivity.java`)

```java
package com.example.studentcard;

import android.os.Bundle;
import android.widget.Button;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;

public class CardActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_card);

        TextView tvCardName = findViewById(R.id.tvCardName);
        TextView tvCardGroup = findViewById(R.id.tvCardGroup);
        TextView tvCardSkill = findViewById(R.id.tvCardSkill);
        Button btnBack = findViewById(R.id.btnBack);

        // Считываем значения из Intent
        String name = getIntent().getStringExtra("KEY_NAME");
        String group = getIntent().getStringExtra("KEY_GROUP");
        String skill = getIntent().getStringExtra("KEY_SKILL");

        tvCardName.setText(name);
        tvCardGroup.setText("Группа: " + group);
        tvCardSkill.setText("Стек: " + skill);

        btnBack.setOnClickListener(v -> finish());
    }
}
```

---

## 8. Топ-12 критических ошибок новичков и их решение

| №   | Симптом / Ошибка                                               | Причина                                                                                     | Как исправить                                                                                                                     |
| :-- | :------------------------------------------------------------- | :------------------------------------------------------------------------------------------ | :-------------------------------------------------------------------------------------------------------------------------------- |
| 1   | **Приложение моментально вылетает при запуске**                | `NullPointerException` при попытке вызвать метод у элемента UI.                             | Вы вызвали `findViewById()` **до** строчки `setContentView(R.layout.activity_main)` или опечатались в ID элемента.                |
| 2   | **Вылет при нажатии на кнопку расчета**                        | `NumberFormatException: For input string: ""`                                               | Вы попытались вызвать `Integer.parseInt(text)`, когда пользователь ничего не ввел. Всегда делайте проверку `if (text.isEmpty())`. |
| 3   | **Всплывающее сообщение Toast не показывается**                | Написали `Toast.makeText(...)`, но забыли `.show()`.                                        | Всегда дописывайте в конце `.show()`!                                                                                             |
| 4   | **Красный класс `R` в коде (`Cannot resolve symbol R`)**       | Синтаксическая ошибка в одном из XML-файлов в папке `res/`.                                 | Откройте панель **Build** внизу, найдите ошибку в XML (незакрытый тег или спецсимвол) и исправьте её.                             |
| 5   | **Текст налезает друг на друга в левом верхнем углу**          | Использован `ConstraintLayout`, но элементам не заданы привязки (Constraints).              | Либо добавьте привязки со всех 4 сторон, либо замените контейнер на `LinearLayout`.                                               |
| 6   | **Второй экран не открывается (вылет при startActivity)**      | Забыли зарегистрировать новую Activity в `AndroidManifest.xml`.                             | Добавьте `<activity android:name=".SecondActivity" android:exported="false" />` внутрь тега `<application>`.                      |
| 7   | **Приложение вылетает при попытке скачивания из сети**         | В `AndroidManifest.xml` не запрошено разрешение на доступ к интернету.                      | Добавьте `<uses-permission android:name="android.permission.INTERNET" />`.                                                        |
| 8   | **При повороте экрана введенные данные пропадают**             | При повороте смартфона Android по умолчанию полностью уничтожает и создает Activity заново. | Для начала зафиксируйте ориентацию в манифесте: `android:screenOrientation="portrait"`.                                           |
| 9   | **В Android Studio подчеркивает всё красным, хотя код верный** | Сбой внутренних кэшей среды разработки.                                                     | Выберите в верхнем меню: `File -> Invalidate Caches... -> Invalidate and Restart`.                                                |
| 10  | **Текст в TextView не обновляется**                            | Вы забыли вызвать `.toString()` при конкатенации или вызвали метод у не той переменной.     | Проверьте: `tvResult.setText(String.valueOf(result));`.                                                                           |
| 11  | **Кнопка не реагирует на нажатия**                             | Слушатель `setOnClickListener` написан, но не прикреплен к кнопке.                          | Убедитесь, что метод вызван именно у объекта кнопки внутри `onCreate()`.                                                          |
| 12  | **Gradle Sync Failed (нет интернета)**                         | Android Studio не смогла скачать плагины сборщика.                                          | Проверьте подключение к сети и отключите прокси/VPN, если они блокируют репозиторий Google Maven.                                 |

---

## 9. Практические задания

Задания распределены по трем уровням сложности для постепенного освоения платформы.

### Уровень 1: Простые одноэкранные утилиты

1. **Светофор:** Экран с тремя кнопками («Красный», «Желтый», «Зеленый»). При нажатии на каждую кнопка меняет фоновый цвет всего экрана на соответствующий.
2. **Генератор случайных чисел (Dice Roller):** Кнопка «Бросить кубик», которая выводит в крупном `TextView` случайное число от 1 до 6.
3. **Мини-тест с одной кнопкой:** Задан вопрос и поле ввода ответа. По кнопке «Проверить» выводится надпись «Верно!» (зеленым) или «Ошибка!» (красным).
4. **Переключатель видимости (Toggle):** Кнопка «Показать секрет / Скрыть секрет», которая поочередно показывает и скрывает текст на экране (`View.VISIBLE` / `View.GONE`).
5. **Инвертор текста:** Поле ввода, куда вводится слово, и кнопка, выводящая его задом наперед (с помощью `StringBuilder.reverse()`).
6. **Калькулятор возраста питомца:** Перевод человеческих лет собаки в «собачьи» (умножение введенного числа на 7).
7. **Счетчик символов:** Поле ввода текста, под которым в реальном времени или по нажатию кнопки отображается общее количество введенных букв.
8. **Симулятор фонарика:** Кнопка переключения экрана из черного в чисто белый цвет на максимальной яркости.
9. **Конвертер сантиметров в дюймы:** Поле ввода числа и перевод сантиметров в дюймы (`дюймы = см / 2.54`).
10. **Определитель четности:** Программа принимает целое число и сообщает, является ли оно четным или нечетным.

### Уровень 2: Калькуляторы и логические экраны

11. **Индекс массы тела (ИМТ / BMI):** Ввод веса (кг) и роста (см). Расчет по формуле: `вес / (рост/100)^2` с текстовым вердиктом («Дефицит», «Норма», «Избыток»).
12. **Калькулятор расхода топлива:** Ввод пройденного расстояния (км) и потраченных литров бензина. Расчет среднего расхода на 100 км пути.
13. **Конвертер температур:** Поле ввода градусов Цельсия и кнопки для пересчета в Фаренгейты (`F = C * 1.8 + 32`) и Кельвины.
14. **Простой калькулятор (4 действия):** Два поля для чисел и четыре отдельные кнопки: `+`, `-`, `*`, `/` с защитой от деления на ноль.
15. **Таймер скидки магазина:** Ввод исходной цены и процента скидки. Расчет суммы скидки и итоговой цены товара.
16. **Конвертер валют (Рубли в Доллары/Евро):** Ввод суммы в рублях и фиксированный курс с выводом результата с округлением.
17. **Генератор надежного PIN-кода:** Кнопка генерации 4-значного или 6-значного кода без повторяющихся подряд цифр.
18. **Тест на знание столиц:** Экран показывает название страны и 3 кнопки с вариантами столиц. Подсчет очков за правильные ответы.
19. **Калькулятор времени в пути:** Ввод расстояния (км) и средней скорости (км/ч). Расчет времени в часах и минутах.
20. **Оценщик надежности пароля:** Проверка введенного пароля по длине: меньше 6 знаков — слабый, от 6 до 10 — средний, более 10 — надежный.

### Уровень 3: Двухэкранные сценарии и сложные формы

21. **Экран входа и Личный кабинет:** На первом экране ввод логина и пароля (хардкод `admin`/`1234`). При успехе — переход на экран кабинета с отображением имени пользователя.
22. **Оформление заказа пиццы:** Экран выбора размера пиццы и адреса доставки. На втором экране — чек с деталями заказа и итоговой стоимостью.
23. **Квиз из двух вопросов:** Вопрос 1 на первом экране, по нажатию «Далее» — переход на экран с вопросом 2, а затем вывод суммарного балла.
24. **Конструктор визитки мастера:** Ввод номера телефона, профессии и имени. На втором экране — стилизованная карточка с возможностью нажать на кнопку и перейти в системную звонилку (`ACTION_DIAL`).
25. **Дневник заметок (Передача текста):** Экран ввода длинного текста заметки. На втором экране — режим чтения крупным шрифтом с кнопкой «Редактировать» (возврат назад).
26. **Калькулятор автокредита:** Расчет ежемесячного платежа по формуле на первом экране и показ графика выплат на втором.
27. **Электронный билет на поезд:** Ввод станций отправления и прибытия. На втором экране — генерация карточки посадочного талона с текущей датой.
28. **Анкета спортивного трекера:** Ввод целевого количества шагов и пройденных за день. На втором экране — прогресс-бар и процент выполнения нормы.
29. **Мини-словарь терминов:** Список из 3 терминов (кнопки). При клике на любую открывается экран с подробным описанием термина.
30. **Итоговый проект «Портфолио студента»:** Главный экран с фотографией, кнопками «Обо мне», «Мои навыки», «Контакты», открывающими соответствующие детальные экраны.

---


---

## 11. Выполнение практических заданий (30 решений)

> **Важно:** изображения ниже — учебные макеты экранов в стиле Android-превью, а не реальные скриншоты запущенной Android Studio. Кодовые фрагменты предназначены для вставки в `onCreate()` соответствующей `Activity` после `setContentView(...)`. Создайте в XML элементы с указанными `id`; добавьте нужные импорты (`android.view.View`, `android.graphics.Color`, `android.widget.*`, `android.content.Intent`, `android.widget.Toast`). Для двухэкранных работ создайте дополнительные Activity и зарегистрируйте их в `AndroidManifest.xml`. Названия классов и ID в примерах должны совпадать с вашим проектом.


### Задание 1. Светофор

**Решение.** В `activity_main.xml` добавьте `rootLayout` и три кнопки `btnRed`, `btnYellow`, `btnGreen`.

![Макет экрана Android для задания 1: Светофор](task4_screenshots/task_01.png)

```java
LinearLayout root = findViewById(R.id.rootLayout);
findViewById(R.id.btnRed).setOnClickListener(v -> root.setBackgroundColor(Color.RED));
findViewById(R.id.btnYellow).setOnClickListener(v -> root.setBackgroundColor(Color.YELLOW));
findViewById(R.id.btnGreen).setOnClickListener(v -> root.setBackgroundColor(Color.GREEN));
```

**XML:** добавьте элементы управления с ID, указанными в коде (`EditText`, `Button`, `TextView`); для задания 1 используйте корневой `LinearLayout` с `android:id="@+id/rootLayout"`.

### Задание 2. Генератор кубика

**Решение.** Добавьте `TextView tvResult` и `Button btnRoll`.

![Макет экрана Android для задания 2: Генератор кубика](task4_screenshots/task_02.png)

```java
TextView result = findViewById(R.id.tvResult);
findViewById(R.id.btnRoll).setOnClickListener(v ->
    result.setText(String.valueOf(new java.util.Random().nextInt(6) + 1)));
```

**XML:** добавьте элементы управления с ID, указанными в коде (`EditText`, `Button`, `TextView`); для задания 1 используйте корневой `LinearLayout` с `android:id="@+id/rootLayout"`.

### Задание 3. Мини-тест

**Решение.** Поле ответа — `etAnswer`, кнопка — `btnCheck`, результат — `tvResult`.

![Макет экрана Android для задания 3: Мини-тест](task4_screenshots/task_03.png)

```java
EditText answer = findViewById(R.id.etAnswer);
TextView result = findViewById(R.id.tvResult);
findViewById(R.id.btnCheck).setOnClickListener(v -> {
    boolean correct = answer.getText().toString().trim().equalsIgnoreCase("Париж");
    result.setText(correct ? "Верно!" : "Ошибка!");
    result.setTextColor(correct ? Color.GREEN : Color.RED);
});
```

**XML:** добавьте элементы управления с ID, указанными в коде (`EditText`, `Button`, `TextView`); для задания 1 используйте корневой `LinearLayout` с `android:id="@+id/rootLayout"`.

### Задание 4. Переключатель видимости

**Решение.** `tvSecret` изначально скрыт, кнопка `btnToggle` переключает состояние.

![Макет экрана Android для задания 4: Переключатель видимости](task4_screenshots/task_04.png)

```java
TextView secret = findViewById(R.id.tvSecret);
Button toggle = findViewById(R.id.btnToggle);
secret.setVisibility(View.GONE);
toggle.setOnClickListener(v -> {
    boolean visible = secret.getVisibility() != View.VISIBLE;
    secret.setVisibility(visible ? View.VISIBLE : View.GONE);
    toggle.setText(visible ? "Скрыть секрет" : "Показать секрет");
});
```

**XML:** добавьте элементы управления с ID, указанными в коде (`EditText`, `Button`, `TextView`); для задания 1 используйте корневой `LinearLayout` с `android:id="@+id/rootLayout"`.

### Задание 5. Инвертор текста

**Решение.** Используйте `EditText etInput`, `Button btnReverse`, `TextView tvResult`.

![Макет экрана Android для задания 5: Инвертор текста](task4_screenshots/task_05.png)

```java
EditText input = findViewById(R.id.etInput);
TextView result = findViewById(R.id.tvResult);
findViewById(R.id.btnReverse).setOnClickListener(v ->
    result.setText(new StringBuilder(input.getText().toString()).reverse().toString()));
```

**XML:** добавьте элементы управления с ID, указанными в коде (`EditText`, `Button`, `TextView`); для задания 1 используйте корневой `LinearLayout` с `android:id="@+id/rootLayout"`.

### Задание 6. Возраст питомца

**Решение.** Возраст собаки в условных годах равен человеческому возрасту, умноженному на 7.

![Макет экрана Android для задания 6: Возраст питомца](task4_screenshots/task_06.png)

```java
EditText input = findViewById(R.id.etYears);
TextView result = findViewById(R.id.tvResult);
findViewById(R.id.btnCalculate).setOnClickListener(v -> {
    try { int years = Integer.parseInt(input.getText().toString().trim());
        if (years < 0) throw new NumberFormatException();
        result.setText("Возраст собаки: " + (years * 7));
    } catch (NumberFormatException e) { input.setError("Введите целое число от 0"); }
});
```

**XML:** добавьте элементы управления с ID, указанными в коде (`EditText`, `Button`, `TextView`); для задания 1 используйте корневой `LinearLayout` с `android:id="@+id/rootLayout"`.

### Задание 7. Счетчик символов

**Решение.** При нажатии на кнопку посчитайте длину строки.

![Макет экрана Android для задания 7: Счетчик символов](task4_screenshots/task_07.png)

```java
EditText input = findViewById(R.id.etInput);
TextView result = findViewById(R.id.tvResult);
findViewById(R.id.btnCount).setOnClickListener(v ->
    result.setText("Количество символов: " + input.getText().length()));
```

**XML:** добавьте элементы управления с ID, указанными в коде (`EditText`, `Button`, `TextView`); для задания 1 используйте корневой `LinearLayout` с `android:id="@+id/rootLayout"`.

### Задание 8. Симулятор фонарика

**Решение.** Это имитация фонарика изменением экрана; для управления реальной вспышкой нужна CameraManager и разрешения.

![Макет экрана Android для задания 8: Симулятор фонарика](task4_screenshots/task_08.png)

```java
View root = findViewById(R.id.rootLayout);
Button toggle = findViewById(R.id.btnToggle);
final boolean[] on = {false};
toggle.setOnClickListener(v -> {
    on[0] = !on[0];
    root.setBackgroundColor(on[0] ? Color.WHITE : Color.BLACK);
    toggle.setText(on[0] ? "Выключить" : "Включить");
});
```

**XML:** добавьте элементы управления с ID, указанными в коде (`EditText`, `Button`, `TextView`); для задания 1 используйте корневой `LinearLayout` с `android:id="@+id/rootLayout"`.

### Задание 9. Сантиметры в дюймы

**Решение.** Один дюйм равен 2,54 см.

![Макет экрана Android для задания 9: Сантиметры в дюймы](task4_screenshots/task_09.png)

```java
EditText input = findViewById(R.id.etCm);
TextView result = findViewById(R.id.tvResult);
findViewById(R.id.btnConvert).setOnClickListener(v -> {
    try { double cm = Double.parseDouble(input.getText().toString().trim());
        result.setText(String.format(java.util.Locale.getDefault(), "%.2f дюйма", cm / 2.54));
    } catch (NumberFormatException e) { input.setError("Введите число"); }
});
```

**XML:** добавьте элементы управления с ID, указанными в коде (`EditText`, `Button`, `TextView`); для задания 1 используйте корневой `LinearLayout` с `android:id="@+id/rootLayout"`.

### Задание 10. Четность числа

**Решение.** Остаток от деления на 2 определяет четность.

![Макет экрана Android для задания 10: Четность числа](task4_screenshots/task_10.png)

```java
EditText input = findViewById(R.id.etNumber);
TextView result = findViewById(R.id.tvResult);
findViewById(R.id.btnCheck).setOnClickListener(v -> {
    try { int n = Integer.parseInt(input.getText().toString().trim());
        result.setText(n % 2 == 0 ? "Четное число" : "Нечетное число");
    } catch (NumberFormatException e) { input.setError("Введите целое число"); }
});
```

**XML:** добавьте элементы управления с ID, указанными в коде (`EditText`, `Button`, `TextView`); для задания 1 используйте корневой `LinearLayout` с `android:id="@+id/rootLayout"`.

### Задание 11. Индекс массы тела

**Решение.** Рост вводится в сантиметрах, вес — в килограммах.

![Макет экрана Android для задания 11: Индекс массы тела](task4_screenshots/task_11.png)

```java
EditText weight = findViewById(R.id.etWeight), height = findViewById(R.id.etHeight);
TextView result = findViewById(R.id.tvResult);
findViewById(R.id.btnCalculate).setOnClickListener(v -> {
    try { double w = Double.parseDouble(weight.getText().toString());
        double h = Double.parseDouble(height.getText().toString()) / 100.0;
        if (w <= 0 || h <= 0) throw new NumberFormatException();
        double bmi = w / (h * h);
        result.setText(String.format(java.util.Locale.getDefault(), "ИМТ: %.1f — %s", bmi,
            bmi < 18.5 ? "Дефицит" : bmi < 25 ? "Норма" : "Избыток"));
    } catch (NumberFormatException e) { result.setText("Проверьте вес и рост"); }
});
```

**XML:** добавьте элементы управления с ID, указанными в коде (`EditText`, `Button`, `TextView`); для задания 1 используйте корневой `LinearLayout` с `android:id="@+id/rootLayout"`.

### Задание 12. Расход топлива

**Решение.** Расход = литры / километры × 100.

![Макет экрана Android для задания 12: Расход топлива](task4_screenshots/task_12.png)

```java
EditText km = findViewById(R.id.etKm), liters = findViewById(R.id.etLiters);
TextView result = findViewById(R.id.tvResult);
findViewById(R.id.btnCalculate).setOnClickListener(v -> {
    try { double distance = Double.parseDouble(km.getText().toString());
        double fuel = Double.parseDouble(liters.getText().toString());
        if (distance <= 0 || fuel < 0) throw new NumberFormatException();
        result.setText(String.format(java.util.Locale.getDefault(), "Расход: %.2f л/100 км", fuel / distance * 100));
    } catch (NumberFormatException e) { result.setText("Введите корректные значения"); }
});
```

**XML:** добавьте элементы управления с ID, указанными в коде (`EditText`, `Button`, `TextView`); для задания 1 используйте корневой `LinearLayout` с `android:id="@+id/rootLayout"`.

### Задание 13. Конвертер температур

**Решение.** Формула Фаренгейта: `F = C * 1.8 + 32`; Кельвина: `K = C + 273.15`.

![Макет экрана Android для задания 13: Конвертер температур](task4_screenshots/task_13.png)

```java
EditText input = findViewById(R.id.etCelsius);
TextView result = findViewById(R.id.tvResult);
findViewById(R.id.btnConvert).setOnClickListener(v -> {
    try { double c = Double.parseDouble(input.getText().toString());
        result.setText(String.format(java.util.Locale.getDefault(), "%.1f °F\n%.2f K", c * 1.8 + 32, c + 273.15));
    } catch (NumberFormatException e) { input.setError("Введите температуру"); }
});
```

**XML:** добавьте элементы управления с ID, указанными в коде (`EditText`, `Button`, `TextView`); для задания 1 используйте корневой `LinearLayout` с `android:id="@+id/rootLayout"`.

### Задание 14. Калькулятор

**Решение.** Создайте два поля `etA`, `etB`, четыре кнопки и `tvResult`. Общий метод защищает от деления на ноль.

![Макет экрана Android для задания 14: Калькулятор](task4_screenshots/task_14.png)

```java
EditText a = findViewById(R.id.etA), b = findViewById(R.id.etB);
TextView result = findViewById(R.id.tvResult);
java.util.function.BiConsumer<String, Double> show = (op, ignored) -> {};
View.OnClickListener calculate = v -> {
    try { double x = Double.parseDouble(a.getText().toString()), y = Double.parseDouble(b.getText().toString());
        double r;
        int id = v.getId();
        if (id == R.id.btnAdd) r = x + y;
        else if (id == R.id.btnSub) r = x - y;
        else if (id == R.id.btnMul) r = x * y;
        else { if (y == 0) { result.setText("На ноль делить нельзя"); return; } r = x / y; }
        result.setText(String.valueOf(r));
    } catch (NumberFormatException e) { result.setText("Введите два числа"); }
};
findViewById(R.id.btnAdd).setOnClickListener(calculate);
findViewById(R.id.btnSub).setOnClickListener(calculate);
findViewById(R.id.btnMul).setOnClickListener(calculate);
findViewById(R.id.btnDiv).setOnClickListener(calculate);
```

**XML:** добавьте элементы управления с ID, указанными в коде (`EditText`, `Button`, `TextView`); для задания 1 используйте корневой `LinearLayout` с `android:id="@+id/rootLayout"`.

### Задание 15. Калькулятор скидки

**Решение.** Итоговая цена равна цене минус процент скидки.

![Макет экрана Android для задания 15: Калькулятор скидки](task4_screenshots/task_15.png)

```java
EditText price = findViewById(R.id.etPrice), discount = findViewById(R.id.etDiscount);
TextView result = findViewById(R.id.tvResult);
findViewById(R.id.btnCalculate).setOnClickListener(v -> {
    try { double p = Double.parseDouble(price.getText().toString()), d = Double.parseDouble(discount.getText().toString());
        if (p < 0 || d < 0 || d > 100) throw new NumberFormatException();
        result.setText(String.format(java.util.Locale.getDefault(), "Скидка: %.2f\nИтого: %.2f", p*d/100, p*(1-d/100)));
    } catch (NumberFormatException e) { result.setText("Цена ≥ 0, скидка от 0 до 100"); }
});
```

**XML:** добавьте элементы управления с ID, указанными в коде (`EditText`, `Button`, `TextView`); для задания 1 используйте корневой `LinearLayout` с `android:id="@+id/rootLayout"`.

### Задание 16. Конвертер валют

**Решение.** Для учебного примера используются фиксированные курсы; перед реальным использованием замените их актуальными.

![Макет экрана Android для задания 16: Конвертер валют](task4_screenshots/task_16.png)

```java
EditText rub = findViewById(R.id.etRubles);
TextView result = findViewById(R.id.tvResult);
findViewById(R.id.btnConvert).setOnClickListener(v -> {
    try { double r = Double.parseDouble(rub.getText().toString());
        result.setText(String.format(java.util.Locale.getDefault(),
            "USD (курс 90): %.2f\nEUR (курс 100): %.2f", r/90.0, r/100.0));
    } catch (NumberFormatException e) { rub.setError("Введите сумму"); }
});
```

**XML:** добавьте элементы управления с ID, указанными в коде (`EditText`, `Button`, `TextView`); для задания 1 используйте корневой `LinearLayout` с `android:id="@+id/rootLayout"`.

### Задание 17. Генератор PIN

**Решение.** Генерация PIN длиной 4 или 6 цифр без одинаковых соседних цифр.

![Макет экрана Android для задания 17: Генератор PIN](task4_screenshots/task_17.png)

```java
TextView result = findViewById(R.id.tvPin);
findViewById(R.id.btnGenerate).setOnClickListener(v -> {
    java.util.Random random = new java.util.Random();
    int length = 4; // для 6 цифр замените на 6
    StringBuilder pin = new StringBuilder();
    while (pin.length() < length) {
        int digit = random.nextInt(10);
        if (pin.length() == 0 || digit != pin.charAt(pin.length()-1) - '0') pin.append(digit);
    }
    if (pin.charAt(0) == '0') pin.setCharAt(0, (char)('1' + random.nextInt(9)));
    result.setText(pin.toString());
});
```

**XML:** добавьте элементы управления с ID, указанными в коде (`EditText`, `Button`, `TextView`); для задания 1 используйте корневой `LinearLayout` с `android:id="@+id/rootLayout"`.

### Задание 18. Тест столиц

**Решение.** Для трёх вариантов создайте кнопки `btnParis`, `btnRome`, `btnBerlin`, а также `tvScore`.

![Макет экрана Android для задания 18: Тест столиц](task4_screenshots/task_18.png)

```java
TextView scoreView = findViewById(R.id.tvScore);
final int[] score = {0};
View.OnClickListener answer = v -> {
    if (v.getId() == R.id.btnParis) score[0]++;
    scoreView.setText(v.getId() == R.id.btnParis ? "Верно! Очки: " + score[0] : "Неверно. Очки: " + score[0]);
};
findViewById(R.id.btnParis).setOnClickListener(answer);
findViewById(R.id.btnRome).setOnClickListener(answer);
findViewById(R.id.btnBerlin).setOnClickListener(answer);
```

**XML:** добавьте элементы управления с ID, указанными в коде (`EditText`, `Button`, `TextView`); для задания 1 используйте корневой `LinearLayout` с `android:id="@+id/rootLayout"`.

### Задание 19. Время в пути

**Решение.** Время в часах = расстояние / скорость; отдельно выводятся часы и минуты.

![Макет экрана Android для задания 19: Время в пути](task4_screenshots/task_19.png)

```java
EditText distance = findViewById(R.id.etDistance), speed = findViewById(R.id.etSpeed);
TextView result = findViewById(R.id.tvResult);
findViewById(R.id.btnCalculate).setOnClickListener(v -> {
    try { double km = Double.parseDouble(distance.getText().toString()), kmh = Double.parseDouble(speed.getText().toString());
        if (km < 0 || kmh <= 0) throw new NumberFormatException();
        int minutes = (int)Math.round(km / kmh * 60);
        result.setText((minutes/60) + " ч " + (minutes%60) + " мин");
    } catch (NumberFormatException e) { result.setText("Проверьте расстояние и скорость"); }
});
```

**XML:** добавьте элементы управления с ID, указанными в коде (`EditText`, `Button`, `TextView`); для задания 1 используйте корневой `LinearLayout` с `android:id="@+id/rootLayout"`.

### Задание 20. Надежность пароля

**Решение.** Длина меньше 6 — слабый, 6–10 — средний, больше 10 — надежный.

![Макет экрана Android для задания 20: Надежность пароля](task4_screenshots/task_20.png)

```java
EditText password = findViewById(R.id.etPassword);
TextView result = findViewById(R.id.tvResult);
findViewById(R.id.btnCheck).setOnClickListener(v -> {
    int n = password.getText().length();
    result.setText(n < 6 ? "Слабый пароль" : n <= 10 ? "Средний пароль" : "Надежный пароль");
});
```

**XML:** добавьте элементы управления с ID, указанными в коде (`EditText`, `Button`, `TextView`); для задания 1 используйте корневой `LinearLayout` с `android:id="@+id/rootLayout"`.

### Задание 21. Вход и личный кабинет

**Решение.** Добавьте `LoginActivity` и `CabinetActivity` (или используйте `MainActivity` как экран входа). Зарегистрируйте второй экран в манифесте.

![Макет экрана Android для задания 21: Вход и личный кабинет](task4_screenshots/task_21.png)

```java
EditText login = findViewById(R.id.etLogin), password = findViewById(R.id.etPassword);
findViewById(R.id.btnLogin).setOnClickListener(v -> {
    if (login.getText().toString().equals("admin") && password.getText().toString().equals("1234")) {
        Intent i = new Intent(this, CabinetActivity.class);
        i.putExtra("USER_NAME", login.getText().toString()); startActivity(i);
    } else Toast.makeText(this, "Неверный логин или пароль", Toast.LENGTH_SHORT).show();
});
// В CabinetActivity: tvWelcome.setText("Добро пожаловать, " + getIntent().getStringExtra("USER_NAME"));
```

**XML и переходы:** создайте соответствующие поля и кнопки с указанными ID, а для второго экрана — отдельный XML-файл и Activity. Передавайте данные через `Intent.putExtra()` и читайте их через `getIntent().get...Extra()`.

### Задание 22. Заказ пиццы

**Решение.** На первом экране выберите размер и адрес. На втором экране покажите детали заказа и стоимость.

![Макет экрана Android для задания 22: Заказ пиццы](task4_screenshots/task_22.png)

```java
String size = ((RadioButton)findViewById(radioGroup.getCheckedRadioButtonId())).getText().toString();
String address = ((EditText)findViewById(R.id.etAddress)).getText().toString().trim();
if (address.isEmpty()) { ((EditText)findViewById(R.id.etAddress)).setError("Введите адрес"); return; }
int price = size.equals("Большая") ? 900 : size.equals("Средняя") ? 650 : 450;
Intent i = new Intent(this, OrderActivity.class);
i.putExtra("SIZE", size); i.putExtra("ADDRESS", address); i.putExtra("PRICE", price); startActivity(i);
// OrderActivity получает extras и выводит чек через getIntent().getStringExtra("SIZE") и getIntExtra("PRICE", 0).
```

**XML и переходы:** создайте соответствующие поля и кнопки с указанными ID, а для второго экрана — отдельный XML-файл и Activity. Передавайте данные через `Intent.putExtra()` и читайте их через `getIntent().get...Extra()`.

### Задание 23. Квиз из двух вопросов

**Решение.** Передавайте ответ первого вопроса во вторую Activity, там проверяйте второй ответ и выводите сумму баллов.

![Макет экрана Android для задания 23: Квиз из двух вопросов](task4_screenshots/task_23.png)

```java
// На первом экране после проверки вопроса 1:
Intent i = new Intent(this, QuestionTwoActivity.class);
i.putExtra("SCORE", answer1.equalsIgnoreCase("Париж") ? 1 : 0);
startActivity(i);
// На втором экране при проверке вопроса 2:
int score = getIntent().getIntExtra("SCORE", 0);
if (answer2.equalsIgnoreCase("Берлин")) score++;
tvResult.setText("Результат: " + score + " из 2");
```

**XML и переходы:** создайте соответствующие поля и кнопки с указанными ID, а для второго экрана — отдельный XML-файл и Activity. Передавайте данные через `Intent.putExtra()` и читайте их через `getIntent().get...Extra()`.

### Задание 24. Визитка мастера

**Решение.** Передайте имя, профессию и телефон на экран визитки; кнопка звонка открывает системную звонилку.

![Макет экрана Android для задания 24: Визитка мастера](task4_screenshots/task_24.png)

```java
Intent i = new Intent(this, BusinessCardActivity.class);
i.putExtra("NAME", name); i.putExtra("JOB", job); i.putExtra("PHONE", phone); startActivity(i);
// В BusinessCardActivity:
String phone = getIntent().getStringExtra("PHONE");
findViewById(R.id.btnCall).setOnClickListener(v ->
    startActivity(new Intent(Intent.ACTION_DIAL, android.net.Uri.parse("tel:" + phone))));
```

**XML и переходы:** создайте соответствующие поля и кнопки с указанными ID, а для второго экрана — отдельный XML-файл и Activity. Передавайте данные через `Intent.putExtra()` и читайте их через `getIntent().get...Extra()`.

### Задание 25. Дневник заметок

**Решение.** Передайте заметку через Intent, отобразите её крупным шрифтом; кнопка редактирования вызывает `finish()`.

![Макет экрана Android для задания 25: Дневник заметок](task4_screenshots/task_25.png)

```java
// Экран ввода:
Intent i = new Intent(this, NoteViewActivity.class);
i.putExtra("NOTE_TEXT", ((EditText)findViewById(R.id.etNote)).getText().toString());
startActivity(i);
// NoteViewActivity:
TextView note = findViewById(R.id.tvNote);
note.setTextSize(22);
note.setText(getIntent().getStringExtra("NOTE_TEXT"));
findViewById(R.id.btnEdit).setOnClickListener(v -> finish());
```

**XML и переходы:** создайте соответствующие поля и кнопки с указанными ID, а для второго экрана — отдельный XML-файл и Activity. Передавайте данные через `Intent.putExtra()` и читайте их через `getIntent().get...Extra()`.

### Задание 26. Автокредит

**Решение.** Для фиксированной ставки используйте аннуитетную формулу; результат показывайте на втором экране.

![Макет экрана Android для задания 26: Автокредит](task4_screenshots/task_26.png)

```java
double principal = Double.parseDouble(etPrincipal.getText().toString());
double annualRate = Double.parseDouble(etRate.getText().toString());
int months = Integer.parseInt(etMonths.getText().toString());
double r = annualRate / 12.0 / 100.0;
double payment = r == 0 ? principal / months :
    principal * r / (1 - Math.pow(1 + r, -months));
Intent i = new Intent(this, CreditDetailsActivity.class);
i.putExtra("PAYMENT", payment); i.putExtra("MONTHS", months); startActivity(i);
// Проверяйте principal > 0, months > 0 и rate >= 0 до вычисления.
```

**XML и переходы:** создайте соответствующие поля и кнопки с указанными ID, а для второго экрана — отдельный XML-файл и Activity. Передавайте данные через `Intent.putExtra()` и читайте их через `getIntent().get...Extra()`.

### Задание 27. Билет на поезд

**Решение.** Создайте экран билета и передайте станции и текущую дату.

![Макет экрана Android для задания 27: Билет на поезд](task4_screenshots/task_27.png)

```java
String from = etFrom.getText().toString().trim(), to = etTo.getText().toString().trim();
if (from.isEmpty() || to.isEmpty()) { Toast.makeText(this, "Укажите обе станции", Toast.LENGTH_SHORT).show(); return; }
Intent i = new Intent(this, TicketActivity.class);
i.putExtra("FROM", from); i.putExtra("TO", to);
i.putExtra("DATE", new java.text.SimpleDateFormat("dd.MM.yyyy", java.util.Locale.getDefault()).format(new java.util.Date()));
startActivity(i);
```

**XML и переходы:** создайте соответствующие поля и кнопки с указанными ID, а для второго экрана — отдельный XML-файл и Activity. Передавайте данные через `Intent.putExtra()` и читайте их через `getIntent().get...Extra()`.

### Задание 28. Спортивный трекер

**Решение.** Рассчитайте процент выполнения цели и установите прогресс в `ProgressBar`.

![Макет экрана Android для задания 28: Спортивный трекер](task4_screenshots/task_28.png)

```java
int goal = Integer.parseInt(etGoal.getText().toString());
int steps = Integer.parseInt(etSteps.getText().toString());
if (goal <= 0 || steps < 0) { tvResult.setText("Проверьте значения"); return; }
int percent = (int)Math.min(100, Math.round(steps * 100.0 / goal));
progressBar.setMax(100); progressBar.setProgress(percent);
tvResult.setText("Выполнено: " + percent + "%");
```

**XML и переходы:** создайте соответствующие поля и кнопки с указанными ID, а для второго экрана — отдельный XML-файл и Activity. Передавайте данные через `Intent.putExtra()` и читайте их через `getIntent().get...Extra()`.

### Задание 29. Мини-словарь

**Решение.** Создайте три кнопки терминов; по нажатию показывайте описание в `TextView` или открывайте DetailActivity.

![Макет экрана Android для задания 29: Мини-словарь](task4_screenshots/task_29.png)

```java
TextView definition = findViewById(R.id.tvDefinition);
findViewById(R.id.btnJava).setOnClickListener(v -> definition.setText("Java — объектно-ориентированный язык программирования."));
findViewById(R.id.btnXml).setOnClickListener(v -> definition.setText("XML — формат разметки интерфейса Android Views."));
findViewById(R.id.btnIntent).setOnClickListener(v -> definition.setText("Intent — сообщение для запуска Activity или действия системы."));
```

**XML и переходы:** создайте соответствующие поля и кнопки с указанными ID, а для второго экрана — отдельный XML-файл и Activity. Передавайте данные через `Intent.putExtra()` и читайте их через `getIntent().get...Extra()`.

### Задание 30. Портфолио студента

**Решение.** Главный экран содержит фото и три кнопки; каждая открывает свою Activity с подробностями.

![Макет экрана Android для задания 30: Портфолио студента](task4_screenshots/task_30.png)

```java
findViewById(R.id.btnAbout).setOnClickListener(v -> startActivity(new Intent(this, AboutActivity.class)));
findViewById(R.id.btnSkills).setOnClickListener(v -> startActivity(new Intent(this, SkillsActivity.class)));
findViewById(R.id.btnContacts).setOnClickListener(v -> startActivity(new Intent(this, ContactsActivity.class)));
// Добавьте AboutActivity, SkillsActivity и ContactsActivity в проект и AndroidManifest.xml.
// Фото поместите в res/drawable и задайте его в ImageView через android:src="@drawable/student_photo".
```

**XML и переходы:** создайте соответствующие поля и кнопки с указанными ID, а для второго экрана — отдельный XML-файл и Activity. Передавайте данные через `Intent.putExtra()` и читайте их через `getIntent().get...Extra()`.


### Общие настройки для запуска

1. Создайте проект через **Empty Views Activity** и выберите **Java**.
2. В `MainActivity.java` поместите обработчики внутрь `onCreate()` после `setContentView(R.layout.activity_main)`.
3. Для каждого задания используйте отдельную копию проекта либо заменяйте содержимое экрана и соответствующий Java-код: у разных заданий разные наборы `id`.
4. Для всех числовых полей проверяйте пустой ввод и некорректные значения, прежде чем выполнять вычисления.
5. Для переходов между экранами добавьте Activity через **New → Activity → Empty Views Activity**. Если Android Studio не зарегистрировала Activity автоматически, внесите её в `<application>` манифеста.
6. Макеты скриншотов в этом документе иллюстрируют ожидаемый внешний вид; фактический результат может отличаться в зависимости от темы, версии Android и оформления XML.

