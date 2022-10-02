# css-guidelines


### Не очень

// card и quote - это блоки

// Некоторые компоненты требуют родительского враппера (или контейнера), который задает макет

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

