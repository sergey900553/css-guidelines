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

