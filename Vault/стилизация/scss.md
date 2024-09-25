# Что такое ?
это метаязык (язык для описания другого языка), который упрощает создание css кода.
SCSS - диалект языка SASS. Синтаксис данного языка очень гибок, он учитывает множество мелочей так желанных в css. Более того в нем даже есть логика, (@if,@each), математика(можно складывать, как числа так и строки и цвета ). 
# Отличие scss от sass
Заключается оно в том, что scss больше похож на обычный css.
## Пример sass кода
~~~ sss
$blue: #3bbfce
$margin: 16px
.content-navigation
  border-color: $blue
  color: darken($blue, 9%)
.border
  padding: $margin / 2
  margin: $margin / 2
  border-color: $blue
~~~
## Пример scss кода
~~~ scss
$blue: #3bbfce;
$margin: 16px;

.content-navigation {
  border-color: $blue;
  color: darken($blue, 9%);
}

.border {
  padding: $margin / 2;
  margin: $margin / 2;
  border-color: $blue;
}
~~~
# Установка и использование
- Для начала нужно установить [ruby](https://www.ruby-lang.org/en/downloads/) . После чего 
- 