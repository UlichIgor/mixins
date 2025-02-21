# ***@mixin*** і ***@include***

**Міксини** дозволяють визначати стилі, які можна повторно використовувати у вашій таблиці стилів. Вони дозволяють легко уникнути використання несемантичних класів, таких як **.float-left**, і поширювати колекції стилів у бібліотеках.

Ім’я міксину може бути будь-яким ідентифікатором Scss або Sass, і воно може містити будь-який оператор, крім операторів верхнього рівня. Їх можна використовувати для інкапсуляції стилів, які можна вставити в одне правило стилю; вони можуть містити власні правила стилю, які можуть бути вкладені в інші правила або включені на верхній рівень таблиці стилів; або вони можуть просто служити для зміни змінних.

Міксини визначаються за допомогою **@mixin** правила, яке записується:
+ `@mixin <name> { ... }`

### ***Приклад:***
створіть один файл ***SASS*** або ***SCSS*** із таким кодом:
```scss
@mixin btnreset() {
  background-color: transparent;
  border: none;
  cursor: pointer;
  button,
  button:active,
  button:focus {
    outline: none;
  }
}

.button {
  @include btnreset();
  padding: 10px 20px;
  font-size: 16px;
  color: #fff;
  background-color: #007bff;
  border-radius: 5px;
}
```
Наведений вище код буде скомпільовано у файл **CSS**, як показано нижче:
```css
.button {
  background-color: transparent;
  border: none;
  cursor: pointer;
  padding: 10px 20px;
  font-size: 16px;
  color: #fff;
  background-color: #007bff;
  border-radius: 5px;
}
.button:active,
.button:focus {
  outline: none;
}
```

### Аргументи Sass, Scss - Mixin
**@mixin name(<arguments...>) { ... }**

Значення **SassScript** можна приймати як аргументи в міксинах, які передаються, коли міксин включено, і доступні як змінні в міксині. Аргумент — це ім’я змінної, яке відокремлюється комою при визначенні міксину. Існує два типи аргументів:
+ Аргументи ключових слів
+ Змінні аргументи

## Аргументи ключових слів
Явний аргумент ключового слова можна використовувати для включення в міксини. Аргументи з іменами можна передавати в будь-якому порядку, а стандартні значення аргументів можна пропускати.

### ***Приклад:***
створіть один файл ***SASS*** або ***SCSS*** із таким кодом:
```scss
@mixin bordered($color, $width: 2px) {
   color: $color;
   border: $width solid black;
   width: 450px;
}

.style {
   @include bordered($color: #77C1EF, $width: 2px);
   padding: 10px;
   margin: 20px;
}
```
Наведений вище код буде скомпільовано у файл **CSS**, як показано нижче:
```css
.style {
   color: #77C1EF;
   border: 2px solid black;
   width: 450px;
   padding: 10px;
   margin: 20px;
}
```

## Змінні аргументи
Змінний аргумент використовується для передачі будь-якої кількості аргументів у mixin. Він містить ключові аргументи, передані функції або міксину. Аргументи ключових слів, передані до mixin, можна отримати за допомогою функції **keywords($args)**, яка повертає значення, зіставлені з String.

### ***Приклад:***
створіть один файл ***SASS*** або ***SCSS*** із таким кодом:
```scss
@mixin colors($background...) {
   @each $color in $background {
      .bg-#{$color} {
         background-color: $color;
      }
   }
}

$values: magenta, red, orange;
.container {
   @include colors($values...);
   padding: 20px;
   border: 1px solid #ccc;
}
```
Наведений вище код буде скомпільовано у файл **CSS**, як показано нижче:
```css
.container {
   padding: 20px;
   border: 1px solid #ccc;
}
.bg-magenta {
   background-color: magenta;
}
.bg-red {
   background-color: red;
}
.bg-orange {
   background-color: orange;
}
```

## Необов'язкові аргументи
Зазвичай кожен аргумент, оголошений міксином, повинен бути переданий, коли цей міксин включено. Однак ви можете зробити аргумент необов’язковим, визначивши значення за умовчанням, яке використовуватиметься, якщо цей аргумент не передано. Стандартні значення використовують той самий синтаксис, що й оголошення змінних: ім’я змінної, двокрапка та вираз SassScript. Це полегшує визначення гнучких API міксинів, які можна використовувати як простими, так і складними способами.

### ***Приклад:***
створіть один файл ***SASS*** або ***SCSS*** із таким кодом:
```scss
@mixin replace-text($image, $x: 50%, $y: 50%) {
  text-indent: -99999em;
  overflow: hidden;
  text-align: left;

  background: {
    image: $image;
    repeat: no-repeat;
    position: $x $y;
  }
}

.mail-icon {
  @include replace-text(url("/images/mail.svg"), 0);
  width: 32px;
  height: 32px;
  display: inline-block;
}
```
Наведений вище код буде скомпільовано у файл **CSS**, як показано нижче:
```css
.mail-icon {
  text-indent: -99999em;
  overflow: hidden;
  text-align: left;
  background-image: url("/images/mail.svg");
  background-repeat: no-repeat;
  background-position: 0 50%;
  width: 32px;
  height: 32px;
  display: inline-block;
}
```

## Аргументи ключових слів
Якщо включено міксин, аргументи можна передавати за іменем на додаток до передачі їх за позицією в списку аргументів. Це особливо корисно для міксинів із декількома необов’язковими аргументами або з логічними аргументами, значення яких неочевидні без назви. Аргументи ключових слів використовують той самий синтаксис, що й оголошення змінних і додаткові аргументи.

### ***Приклад:***
створіть один файл ***SASS*** або ***SCSS*** із таким кодом:
```scss
@mixin square($size, $radius: 0) {
  width: $size;
  height: $size;

  @if $radius != 0 {
    border-radius: $radius;
  }
}

.avatar {
  @include square(100px, $radius: 4px);
  background-color: #eee;
  border: 1px solid #ccc;
}
```
Наведений вище код буде скомпільовано у файл **CSS**, як показано нижче:
```css
.avatar {
  width: 100px;
  height: 100px;
  border-radius: 4px;
  background-color: #eee;
  border: 1px solid #ccc;
}
```

## Логічні значення
Логічні значення – це логічні значення **true** та **false**. Крім літеральних форм, булеві значення повертаються операторами рівності та відношення, а також багатьма вбудованими функціями, такими як **math.comparable()** і **map.has-key()**.

### ***Приклад:***
створіть один файл ***SASS*** або ***SCSS*** із таким кодом:
```scss
@use "sass:math";

@debug 1px == 2px; // false
@debug 1px == 1px; // true
@debug 10px < 3px; // false
@debug math.comparable(100px, 3in); // true
```

Ви можете працювати з логічними значеннями за допомогою логічних операторів. Оператор **and** повертає **true**, якщо обидві сторони є **true**, і **or** оператор повертає **true**, якщо будь-яка сторона є **true**. Оператор **not** повертає значення, протилежне одному логічному значенню.

створіть один файл ***SASS*** або ***SCSS*** із таким кодом:
```scss
@debug true and true; // true
@debug true and false; // false

@debug true or false; // true
@debug false or false; // false

@debug not true; // false
@debug not false; // true
```

## Використання логічних значень
Ви можете використовувати логічні значення, щоб вибрати, чи робити різні речі в Sass. Правило обчислює блок стилів, якщо його **@if** аргумент **true**:

створіть один файл ***SASS*** або ***SCSS*** із таким кодом:
```scss
@mixin avatar($size, $circle: false) {
  width: $size;
  height: $size;

  @if $circle {
    border-radius: $size / 2;
  }
}

.square-av {
  @include avatar(100px, $circle: false);
  background-color: #f0f0f0;
  border: 1px solid #ddd;
}
.circle-av {
  @include avatar(100px, $circle: true);
  background-color: #f0f0f0;
  border: 1px solid #ddd;
}
```
Наведений вище код буде скомпільовано у файл **CSS**, як показано нижче:
```css
.square-av {
  width: 100px;
  height: 100px;
  background-color: #f0f0f0;
  border: 1px solid #ddd;
}

.circle-av {
  width: 100px;
  height: 100px;
  border-radius: 50px;
  background-color: #f0f0f0;
  border: 1px solid #ddd;
}
```

Функція повертає одне значення, якщо її аргумент дорівнює **true**, і інше, якщо її аргумент дорівнює **false**:
створіть один файл ***SASS*** або ***SCSS*** із таким кодом:
```scss
@debug if(true, 10px, 30px); // 10px
@debug if(false, 10px, 30px); // 30px
```

## Прийняття довільних аргументів
Іноді корисно, щоб міксин міг приймати будь-яку кількість аргументів. Якщо останній аргумент в @mixin декларації закінчується на ..., тоді всі додаткові аргументи цього міксину передаються цьому аргументу як список. Цей аргумент відомий як список аргументів.

### ***Приклад:***
створіть один файл ***SASS*** або ***SCSS*** із таким кодом:
```scss
@mixin order($height, $selectors...) {
  @for $i from 0 to length($selectors) {
    #{nth($selectors, $i + 1)} {
      position: absolute;
      height: $height;
      margin-top: $i * $height;
      background-color: #f9f9f9;
      border: 1px solid #ccc;
    }
  }
}

@include order(150px, "input.name", "input.address", "input.zip");
```
Наведений вище код буде скомпільовано у файл **CSS**, як показано нижче:
```css
input.name {
  position: absolute;
  height: 150px;
  margin-top: 0px;
  background-color: #f9f9f9;
  border: 1px solid #ccc;
}

input.address {
  position: absolute;
  height: 150px;
  margin-top: 150px;
  background-color: #f9f9f9;
  border: 1px солідний #ccc;
}

input.zip {
  position: absolute;
  height: 150px;
  margin-top: 300px;
  background-color: #f9f9f9;
  border: 1px solid #ccc;
}
```

## Отримання довільних аргументів ключових слів
Списки аргументів також можна використовувати для отримання довільних ключових аргументів. Функція приймає список аргументів і повертає будь-які додаткові ключові слова, які були передані до міксину, як **meta.keywords()** карту від імен аргументів (не включаючи $) до значень цих аргументів.

### ***Приклад:***
створіть один файл ***SASS*** або ***SCSS*** із таким кодом:
```scss
@use "sass:meta";

@mixin syntax-colors($args...) {
  @debug meta.keywords($args);
  // (string: #080, comment: #800, variable: #60b)

  @each $name, $color in meta.keywords($args) {
    pre span.stx-#{$name} {
      color: $color;
      font-weight: bold;
    }
  }
}

@include syntax-colors(
  $string: #080,
  $comment: #800,
  $variable: #60b,
);
```
Наведений вище код буде скомпільовано у файл **CSS**, як показано нижче:
```css
pre span.stx-string {
  color: #080;
  font-weight: bold;
}

pre span.stx-comment {
  color: #800;
  font-weight: bold;
}

pre span.stx-variable {
  color: #60b;
  font-weight: bold;
}
```

## Передача довільних аргументів
Подібно до того, як списки аргументів дозволяють міксинам приймати довільні позиційні або ключові аргументи, той самий синтаксис можна використовувати для передачі позиційних і ключових аргументів у міксини. Якщо ви передаєте список, за яким слідує ...останній аргумент включення, його елементи розглядатимуться як додаткові позиційні аргументи. Подібним чином карта, за якою слідує ..., розглядатиметься як додаткові аргументи ключового слова. Ви навіть можете пройти обидва відразу!

### ***Приклад:***
створіть один файл ***SASS*** або ***SCSS*** із таким кодом:
```scss
$form-selectors: "input.name", "input.address", "input.zip" !default;

@include order(150px, $form-selectors...);
```

## Блоки вмісту
Окрім прийому аргументів, міксин може приймати цілий блок стилів, відомий як блок вмісту. Міксин може оголосити, що він приймає блок вмісту, включивши **@content** правило в своє тіло. Блок вмісту передається за допомогою фігурних дужок, як і будь-який інший блок у **Sass**, і вставляється замість **@content** правила.

### ***Приклад:***
створіть один файл ***SASS*** або ***SCSS*** із таким кодом:
```scss
@mixin hover {
  &:not([disabled]):hover {
    @content;
  }
}

.button {
  border: 1px solid black;
  padding: 10px 20px;
  font-size: 16px;
  color: #fff;
  background-color: #007bff;
  border-radius: 5px;

  @include hover {
    border-width: 2px;
    background-color: #0056b3;
  }
}
```
Наведений вище код буде скомпільовано у файл **CSS**, як показано нижче:
```css
.button {
  border: 1px solid black;
  padding: 10px 20px;
  font-size: 16px;
  color: #fff;
  background-color: #007bff;
  border-radius: 5px;
}
.button:not([disabled]):hover {
  border-width: 2px;
  background-color: #0056b3;
}
```

