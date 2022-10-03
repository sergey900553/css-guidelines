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
    <button class="btn__card --is-active"></button>
  </div>
```


### JS хуки обозначаются с приставкой -js
```html
  <div class="card --special">
    <div class="card__photo">Фото</div>
    <card class="card__title --main">Заголовок карточки</card>
    <card class="card__text">Текс карточки</card>
    <button class="btn__card --js-drop-down-menu-trigger"></button>
  </div>
```


# Позиционирование
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








