## Типы селекторов
### Классы
~~~
.class_name{}
~~~
### id
~~~
#id{}
~~~
### Элементное обращение
~~~
h3{}
~~~
# Способы центрования элемента
1: использование display flex вместе со свойствами justify-content:center и align-items:center
2: использование margin: auto
3: использование grid (такой способ может не поддерживаться браузером)
4: использование position absolute со свойствами top: 50%, left: 50% (Бывают ситуации, когда ширина и высота элемента фиксированные)
5: использование position:absolute со свойствами top:50%, left: 50% и указанием translate(-50%,-50%) (Если есть случаи, когда высота и ширина элемента неизвестны)