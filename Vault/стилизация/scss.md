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
- Для начала нужно установить [ruby](https://www.ruby-lang.org/en/downloads/) . После чего нужно установить sass-gem (gem install sass) Если всё прошло гладко, то теперь вам доступна консольная программа sass. Всех нюансах её использования можно прочитать здесь - sass -- help
- Если запустить sass с ключом --watch, то программа будет следить за указанными вами файлами. В случае их изменения, она автоматически пересоберет все необходимые css-файлы 
- Если вам нужно единожды обновить все css файлы, то вместо --watch, запускаем с              --update . Никакой слежки проводится не будет, так же как и проверок на необходимость обновления.
# Практика
## @Вложенность
Одна из самых желанных фич для css является вложенность селекторов
~~~ scss
#some {
  border: 1px solid red;
  .some { background: white; }
}

/* => */

#some { border: 1px solid red; }
#some .some { background: white; }
~~~
Символ `&` равносилен родительскому селектору.
~~~ scss
$IE_7: 'body.ie_7';
//...
.some {
  display: inline-block;
  #{$IE_7} & { zoom: 1; display: inline; }
}

/* => */

.some { display: inline-block; }
body.ie_7 .some { zoom: 1; display: inline; }
~~~
## $variables 
Переменные определяются так
~~~ scss
$some: red;
~~~
## @математика
Математические вычисления в scss vможно производить без функции calc(),
так же математику можно разделить на математику чисел и цветов.
~~~ scss
.block {
  $block_width: 500px;
  padding: 5px;
  border: 1px solid black;
  width: $block_width - ( 5px * 2 ) - ( 1px * 2 );
}
~~~
## @строки
SCSS умеет складывать строки, а также поддерживает конструкцию #{}
~~~ scss
$image_dir: 'images/web/';
$page: 12;

.some 
{ 
  background-image: url( $image_dir + 'pen.gif' ); 
  &:before { content: "Страница #{ $page }!"; }
}

/* => */

.some { background-image: url("images/web/pen.gif"); }
.some:before { content: "Страница 12!"; }
~~~