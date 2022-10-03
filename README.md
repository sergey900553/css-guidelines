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

#### Можно попробовать миксины в scss. Проблема такого подхода в раздутии файла из-за повторяюдщихся свойств, но gzip вродн неплохо с этим справляется.
Мы поместили свойства btn в btn-card с помщью миксина, а остальные свойства нужные ддя btn--card просто прописываем как обычно. 
```html
  <div class="card --special">
    <div class="card__photo">Фото</div>
    <card class="card__title --main">Заголовок карточки</card>
    <card class="card__text">Текс карточки</card>
    <button class="btn--card"></button>
  </div>
```























### Не очень

// card и quote - это блоки

// Некоторые компоненты требуют родительского враппера (или контейнера), который задает макет
// l- У этих модулей нет никакой косметики, используются только для позиционирования
// js- Они указывают на то, что поведение JavaScript привязано к компоненту. Стили с ними не ассоциируются; используются только для облегчения манипуляций со скриптами.
// is- Показывают различные состояния, которые могут быть у компонентов

```html
<div class="l-card-container">
  <div class="b-card card--special">
    <div class="card__photo">Фото</div>
    <card class="card__title card__title--main">Заголовок</card>
    <div class="quote">
      <div class="quote__line quote__is-active"></div>
      <div class="quote__text quote__text--red"></div>
    </div>
  </div>
</div>
```



```html

  <div class="card card--special">
    <div class="card__photo">Фото</div>
    <card class="card__title card__title--main">Заголовок</card>
    <div class="quote">
      <div class="quote__line quote__is-active"></div>
      <div class="quote__text quote__text--red"></div>
    </div>
  </div>
```

```scss
.card{
  border:1px solid black;
  .card__title{
    font-size: 30px;
  }
  .card__title--main{
    color: yellowgreen;
  }
}

.card--special{
  border:3px solid red;
}
```



### Лучше

// card и quote - это блоки
```html
  <div class="b-card --special">
    <div class="card__photo">Фото</div>
    <card class="card__title --main">Заголовок</card>
    <div class="b-quote">
      <div class="quote__line --is-active"></div>
      <div class="quote__text --red --cool-border"></div>
    </div>
  </div>
```

```scss
.b-card{
  border:1px solid black;
  &.--special{
    border:3px solid red;
  }
  
  .card__title{
    font-size: 30px;
    &.--main{
      color: yellowgreen;
    }
  }
}
```
или так
```scss
.b-card{
  border:1px solid black;
  &.--special{
    border:3px solid red;
  }
  
  .card__title{
    font-size: 30px;
  }
  .card__title.--main{
      color: yellowgreen;
  }
}
```

