# css-guidelines


Главное как можно меньше вложенностей в css. Нахуй вложенности.

Даже такой простой пример кода уже создает коллизии. Даже, если card с помощью scss сделать родителем, то коллизия классов text останется. 
```html
  <div class="card">
    <div class="photo">Фото</div>
    <card class="title">Заголовок карточки</card>
    <card class="text">Текс карточки</card>
    <div class="quote">
      <div class="line"></div>
      <div class="text">Текст цитаты</div>
    </div>
  </div>
```
### Так лучше
```html
  <div class="card">
    <div class="card__photo">Фото</div>
    <card class="card__title">Заголовок карточки</card>
    <card class="card__text">Текс карточки</card>
    <div class="quote">
      <div class="quote__line"></div>
      <div class="quote__text">Текст цитаты</div>
    </div>
  </div>
```

Модификаторы с наследованием сильно удлиняют классы.
```html
  <div class="card card--special">
    <div class="card__photo">Фото</div>
    <card class="card__title card__title--main">Заголовок карточки</card>
    <card class="card__text">Текс карточки</card>
    <div class="quote">
      <div class="quote__line"></div>
      <div class="quote__text quote__text--red">Текст цитаты</div>
    </div>
  </div>
  ```
  ### Попытка сократить модификаторы
  ```html
  <div class="card --special">
    <div class="card__photo">Фото</div>
    <card class="card__title --main">Заголовок карточки</card>
    <card class="card__text">Текс карточки</card>
    <div class="quote">
      <div class="quote__line"></div>
      <div class="quote__text --red">Текст цитаты</div>
    </div>
  </div>
  ```
##  "Кросскомпонентные… Компоненты?" Aka Миксы Разве смысл не в том, что мы может модульными и независимым? Каждый компонент должен быть изолирован и не должен знать о других компонентах. 

### Предположим у нас есть стандартная кнопка с классом btn, которая подходит в компонент без изменений, тогда ничего менять не нужно
```html
  <div class="card --special">
    <div class="card__photo">Фото</div>
    <card class="card__title --main">Заголовок карточки</card>
    <card class="card__text">Текс карточки</card>
    <button class="btn"></button>
  </div>
```
### Так вышло, что нам нужна кнопка чуть меньше, чем стандатрная, что делать? 

#### Можно использовать модификаторы в файле с кнопкой, так как добавление класса из другого компонента для задания стиля, как это делает способ с "миксами", нарушает открытые/закрытые принципы компонентно-ориентированного дизайна. В принципе, это неплохо подходит для консистентного дизайна. Но если он не консистентный, то этих модификаций может стать слишком много.
```html
  <div class="card --special">
    <div class="card__photo">Фото</div>
    <card class="card__title --main">Заголовок карточки</card>
    <card class="card__text">Текс карточки</card>
    <button class="btn --small --round --blue"></button>
  </div>
```

#### Можно попробовать миксины в scss. Проблема такого подхода в раздутии файла из-за повторяюдщихся свойств, но gzip вродн неплохо с этим справляется. Также нормальный сборщик может объеденить классы уже на этапе сборки.
Мы поместили свойства btn в btn-card с помщью миксина, а остальные свойства нужные ддя btn--card просто прописываем как обычно. 
```html
  <div class="card --special">
    <div class="card__photo">Фото</div>
    <card class="card__title --main">Заголовок карточки</card>
    <card class="card__text">Текс карточки</card>
    <button class="btn__card"></button>
  </div>
```

### Можно использовать базовый класс вместо миксинов, когда базовые стили для кнопки определяются в базовом классе.
```html
  <div class="card --special">
    <div class="card__photo">Фото</div>
    <card class="card__title --main">Заголовок карточки</card>
    <card class="card__text">Текс карточки</card>
    <button class="btn btn__card"></button>
  </div>
```


### Состояние обозначается с приставкой is
```html
  <div class="card --special">
    <div class="card__photo">Фото</div>
    <card class="card__title --main">Заголовок карточки</card>
    <card class="card__text">Текс карточки</card>
    <button class="btn__card is-active"></button>
  </div>
```


### JS хуки обозначаются с приставкой -js
```html
  <div class="card --special">
    <div class="card__photo">Фото</div>
    <card class="card__title --main">Заголовок карточки</card>
    <card class="card__text">Текс карточки</card>
    <button class="btn__card js-btn__card"></button>
  </div>
```


# Позиционирование
Чтобы расположить один блок относительно другого или позиционировать блоки внутри другого блока в БЭМ принято использовать миксы. Если решить эти задачи с помощью миксов невозможно, применяются HTML-обертки.


## Для позиционирования блока исползуется класс, привязынный к родителю. В btn базовые стили, в btn__card позиционирование.

```html
  <div class="card ">
    <div class="card__photo">Фото</div>
    <card class="card__title">Заголовок карточки</card>
    <card class="card__text">Текс карточки</card>
    <button class="btn btn__card"></button>
  </div>
```
## Либо миксин в css, когда в класс btn__card примикосваны базовые стили кнопки, а позиционирование уже делает как облычно в btn__card.
```html
  <div class="card ">
    <div class="card__photo">Фото</div>
    <card class="card__title">Заголовок карточки</card>
    <card class="card__text">Текс карточки</card>
    <button class="btn__card"></button>
  </div>
```

## Для позиционирования вложенных HTML-элементов, в большинстве случаев, создается дополнительный элемент блока
```html
  <div class="card">
    <div class="card__container">
      <div class="card__photo">Фото</div>
      <card class="card__title">Заголовок карточки</card>
      <card class="card__text">Текс карточки</card>
      <button class="btn__card"></button>
    </div>
  </div>
```

## Куда вкладывать компоненты
```html
  <div class="card ">
    <div class="card__photo">Фото</div>
    <card class="card__title">Заголовок карточки</card>
    <card class="card__text">Текс карточки</card>
    <div class="card__some-group">
      <button class="btn__card"></button>
      <button class="btn__card"></button>
      <a class="base-href" href="#"></a>
    </div>
  </div>
```

## Позиционирование элементов внутри блока
Для позиционирования вложенных HTML-элементов, в большинстве случаев, создается дополнительный элемент блока (например, block__inner).
```html
<body class="page"> 
  <div class="page__inner"> 
    <!-- верхний колонтитул и навигация --> 
    <header class="header">...</header> 
    <!-- нижний колонтитул --> 
    <footer class="footer">...</footer> 
  </div> 
</body>
```
```css
.page__inner { 
  margin-right: auto; 
  margin-left: auto; 
  width: 960px; 
}
```
## Еще раз про позиционирование
В идеале каждый блок ничего не должен знать о своем местоположении, т.е если сказать просто, у него не должно быть никаких margin, top, left и т.п. За его расположение должен отвечать контейнер внутри которого он размещается, зачастую этот контейнер можно смиксовать с самим блоком. 

так НЕ желательно:
```html
<div class="block-1-style-and-it-position"></div>
```
Лучше так: в блоке стили, в block-positio - позиция
```html
<div class="block block-position"></div>
```
Ну в крайнем случае так, если второй вариант не подходит по каким-то причинам:
```html
<div class="element-of-block-2-for-set-position-to-block-1">
    <div class="block-1-style"></div>
</div>
```
## Aka 
Так
```html
<div class="inner__pseudo-quote">
    <blockquote class="pseudo-quote">
        <p>!!В123!аsdшыыыыывыввыв23ыыа работа заполнит большую часть жизни и единственный способ быть полностью довольным— делать
            то,
            что по-вашему является великим делом. И&nbsp;единственный способ делать великие дела&nbsp;— любить&nbsp;то,
            что вы&nbsp;делаете.</p>
    </blockquote>
</div>
```
Или так
```html
<blockquote class="pseudo-quote inner__pseudo-quote">
    <p>!!В123!аsdшыыыыывыввыв23ыыа работа заполнит большую часть жизни и единственный способ быть полностью довольным— делать
        то,
        что по-вашему является великим делом. И&nbsp;единственный способ делать великие дела&nbsp;— любить&nbsp;то,
        что вы&nbsp;делаете.</p>
</blockquote>
```


## Может data-state?
![image](https://user-images.githubusercontent.com/42143402/193599092-401341c6-acc4-4868-ab0a-9d3598600c0c.png)

## Зачем нужен микс блока и модификатора? Может просто использовать sccs? Когда базовые стили уже добавляются к модификатору. В block_red уже золожены базовые стили с помощью миксина в scss.
```html
<div class="block_red">
```
Но модификатор может добавляться/убираться динамически, в этом случае нам всё равно нужны стили на block.
```html
<div class="block block_red">
```

## Наследование
![image](https://user-images.githubusercontent.com/42143402/193815845-4990612a-55dd-4fcc-b059-24776228d6f8.png)
# Принцип единственной ответственности
Используя принцип единственной ответственности в CSS, можно быть уверенным, что CSS-классы являются легко расширяемыми и переопределяемыми. Давайте рассмотрим следующий пример
```css
.button {
  padding: 1rem 2rem;
  font-size: 2rem;
  border-radius: 0.2rem;
  background-color: #eb4934;
  color: #fff;
  font-weight: bold;
}
 
.button--secondary {
  border-radius: 0;
  font-size: 1rem;
  background-color: #888;
}
```
Видно, что если потребуется расширить класс .button с помощью модификатора .button--secondary, придётся выполнить много переопределений, хотя мы хотим всего лишь поменять цвет фона, сохранив стили по умолчанию.

Проблема в том, что наш класс .button имеет несколько ролей:
* Определяет блочную модель (padding)
* Определяет типографику (font-size, font-weight)
* Определяет внешний вид (color, background-color, border-radius)

Так лучше 
```css
/* Общие стили */
.button {
  padding: 1rem 2rem;
  font-weight: bold;
  color: #fff;
}
 
/* Расширения стиля */
.button--radialBorder {
  border-radius: 0.2rem;
}
 
.button--large {
  font-size: 2rem;
}
 
.button--primary {
  background-color: #eb4934;
}
 
.button--secondary {
  background-color: #888;
}
```


