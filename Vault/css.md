## Типы селекторов
### Классы
~~~ css
.class_name{}
~~~
### id
~~~ css
#id{}
~~~
### Элементное обращение
~~~ css
h3{}
~~~
# Способы центрования элемента
1: использование display flex вместе со свойствами justify-content:center и align-items:center
2: использование margin: auto
3: использование grid (такой способ может не поддерживаться браузером)
4: использование position absolute со свойствами top: 50%, left: 50% (Бывают ситуации, когда ширина и высота элемента фиксированные)
5: использование position:absolute со свойствами top:50%, left: 50% и указанием translate(-50%,-50%) (Если есть случаи, когда высота и ширина элемента неизвестны)
# Анимация
Пример того как сделать крутящийся круг
~~~ css
@keyframes rotate {

  from{

    transform: rotate(0deg) scale(1);

  }

  to{

    transform: rotate(360deg ) scale(1.2);

  }

}
~~~
- Далее нужно добавить эту анимацию классу
~~~ css
 animation: rotate 1s infinite linear;
~~~