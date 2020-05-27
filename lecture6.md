# Лекция 4

Можно посмотреть пример в записи вебинара: [https://geekbrains.ru/lessons/66069](https://geekbrains.ru/lessons/66071)

- [Вступление](https://github.com/Geekbrains-Frontend-Level-2/lectures/blob/master/lecture6.md#вступление)
- [Начало работы](https://github.com/Geekbrains-Frontend-Level-2/lectures/blob/master/lecture6.md#начало-работы)
- [Methods](https://github.com/Geekbrains-Frontend-Level-2/lectures/blob/master/lecture6.md#methods)
- [Computed](https://github.com/Geekbrains-Frontend-Level-2/lectures/blob/master/lecture6.md#computed)
- [Жизненный цикл](https://github.com/Geekbrains-Frontend-Level-2/lectures/blob/master/lecture6.md#жизненный-цикл)
- [Связывание данных](https://github.com/Geekbrains-Frontend-Level-2/lectures/blob/master/lecture6.md#связывание-данных)
- [Single File Component](https://github.com/Geekbrains-Frontend-Level-2/lectures/blob/master/lecture6.md#single-file-component)
- [Полезные ссылки](https://github.com/Geekbrains-Frontend-Level-2/lectures/blob/master/lecture6.md#полезные-ссылки)

## Вступление
Сегодня речь пойдет о замечательном фреймворке ``Vue.js``. ``Vue`` отлично подходит для создания сложных ``SPA`` приложений.
> ``SPA`` – расшифровывается оно как Single Page Application, то есть одностраничное приложение.
Когда пользователь ходит по ссылкам внутри такого сайта, у него не происходит перезагрузка страниц.
Сейчас уже сложно найти более-менее сложный сайт, который бы не использовал данный концепт. 
vk.com, facebook.com, mail.ru – все это ``SPA`` приложения.

``Vue`` нам предоставляет отличный функционал для создания таких сайтов. Разумеется, существуют аналоги в виде [Англяра](https://angular.io/) или [Реакта](https://ru.reactjs.org/), но мое мнение, что ``VUE`` намного удобнее :)

У Vue сейчас уже достаточно большое сообщество, а также [прекрасная документация](https://vuejs.org/v2/guide/). Есть также [на русском языке](https://ru.vuejs.org/v2/guide/index.html), и что характерно, она тоже вполне себе приличная.

## Начало работы

Итак, предлагаю перейти непосредственно к изучению Vue. Для начала мы рассмотрим с вами простейший пример подключения на страницу через ``CDN``. 
В рамках этого мы рассмотрим базовые возможности фреймворка.

Сделаем заглушку, чтобы в ней продемонстрировать работу ``Vue``.

``index.html``
```HTML
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf8" />
    <title>VUE</title>
  </head>
  <body>
    <main>
    </main>
    <script>
    </script>
  </body>
</html>
```

Давайте посмотрим в документации, что нам предлагается сделать, чтобы начать использовать ``VUE``. Заходим в [Getting Started](https://vuejs.org/v2/guide/#Getting-Started),
и тут видим, что нам предлагается использовать ``Vue``, установив его из хранилища ``CDN``. Давайте рассмотрим этот вариант.

Тут есть 2 версии – для разработки и для продакшена. Подключим для разработки, чтобы было удобнее.
Поместим в ``<head></head>`` скрипт:
```HTML
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

Фреймворк мы подключили, если обновим страницу, увидим, соответсвующее сообщение в консоли:
> You are running Vue in development mode.
Make sure to turn on production mode when deploying for production.
See more tips at https://vuejs.org/guide/deployment.html

Теперь, давайте инициализируем наше ``Vue`` приложение. Делается это следующим образом.

```HTML
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf8" />
    <title>VUE</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <main>
    </main>
    <script>
      new Vue({
        el: 'main',
        data: {
        }
      })
    </script>
  </body>
</html>
```

Мы создали новый объект ``Vue``, в который передаем поля: 
- ``el`` – сюда отрендерится наше приложение
- ``data`` – какие-то данные.

Согласитесь, это несколько похоже на то, как мы с вами создавали список товаров и корзину в наших интернет магазинах?

Если перезагрузим страницу, то визуально ничего не изменится, однако в элементе ``main`` расположилость полноценное ``Vue`` приложение.
Теперь, внутри ``main`` мы можем использовать почти все возможности фреймворка. Например, давайте передадим в данные ``Vue`` какое-то сообщение. 

```HTML
<script>
  new Vue({
    el: 'main',
    data: {
      message: 'Hello Vue!'
    }
  })
</script>
```

Теперь я хочу вывести это сообщение на странице. Делается это очень простым способом:
```HTML
<main>
  <h1>{{ message }}</h1>
</main>
```
Перезагрузив страницу мы увидим наше сообщение.
> Если в верстке прописать синтаксис двойных фигурных скобочек (как мы сделали выше), то внутри них можно написать полноценное JS выражение.
Данные берутся из переменных/функций/вычислямых значений скрипта ``Vue``

Давайте рассмотрим, какими еще инструментами обладает ``Vue``?

## Methods

Например, объект ``Vue`` обладает таким полем как ``methods``:

```HTML
<script>
  new Vue({
    el: 'main',
    data: {
      message: 'Hello Vue!'
    },
    methods: {
      // some methods
    },
  })
</script>
```

Здесь мы можем писать функции, которые будут взаимодействовать с нашим компонентом. Такие функции могут изменять данные, отображение, делать запросы на сервер и многое другое. 
Также, здесь мы можем писать обработчики на какие-то действия.

Например. Я хочу, чтобы по клику на надпись (Hello Vue), всплывал ``alert`` с этой надписью.

Давайте напишем такую функцию:

```HTML
<script>
  new Vue({
    el: 'main',
    data: {
      message: 'Hello Vue!'
    },
    methods: {
      onHeaderClick () {
        alert(this.message)
      }
    },
  })
</script>
```

Заметьте, как мы обратились к данным – через this, тоже напоминает работу в классах в наших магазинах, да?

Теперь нам надо навесить обработчик клика. Раньше мы должны были найти требуемый элемент в верстке, и сделать  нечто такое:
```JavaScript
element.addEventListener(‘click’, this.onHeaderClick.bind(this))
```

Средствами ``Vue`` это делается намного проще. Достаточно написать такую конструкцию прямо в верстке:

```HTML
<h1 v-on:click="onHeaderClick">{{ message }}</h1>
```

Теперь, если мы теперь кликнем на нашу надпись, то сработает обработчик.

И даже такую запись можно упростить:

```HTML
<h1 @click="onHeaderClick">{{ message }}</h1>
```

Если мы хотим передать параметры, то достаточно прокинуть их в скобки:

```HTML
<h1 @click="onHeaderClick(message)">{{ message }}</h1>
```

```JavaScript
methods: {
  onHeaderClick (text) {
    alert(text)
  }
},
```

И опять же заметье - нет никакх bind. Здесь ``Vue`` автоматически прокидывает необходимый контекст.

## Computed

Далее, у нас есть такое поле как ``computed``.

```HTML
<script>
  new Vue({
    el: 'main',
    data: {
      message: 'Hello Vue!'
    },
    methods: {
      onHeaderClick (text) {
        alert(text)
      }
    },
    computed: {
      // some computed methods
    },
  })
</script>
```

Здесь также мы будем размещать функции. 
Однако, данные функции обязаны возвращать какое-то значение.
Более того. Данные функции выролняются ``ТОЛЬКО ПРИ ПЕРВОМ ОБРАЩЕНИИ К ЭТОЙ ПЕРЕМЕННОЙ``, либо когда ``МЕНЯЮТСЯ РЕАКТИВНЫЕ ДАННЫЕ, ОТ КОТОРЫХ ЗАВИСЯТ ЭТИ ФУНКЦИИ``.

Что это значит? Можно привести наглядный пример.

Пусть у меня будет такая функция, клоторая будет возвращать текущее количество секунд в часе:

```JavaScript
computed: {
  getComputedDate () {
    console.log(‘CALLED’)
    return new Date().getSeconds();
  }
}
```

Если мы сейчас обновим страницу, то никакого вывода в консоли не увидим. 

Однако, если мы обратимся к данной функции в верстке, то сразу увидим вывод в консоли (``CALLED``):
```HTML
<h1>{{ getComputedDate }}</h1>
```

Далее, попробуем, например выводить результат этой функции в ``alert`` (воспользуемся нажатием на заголовок, которое мы реализовали ранее).
Мы увидим, что сработала эта функция только один раз, даже если мы нажмем раз 20 (окошко вызовется 20 раз, однако в консоли будет лишь один вывод ``CALLED``).

Однако, как я уже сказал, что функция может перерассчитать свой вывод, если она зависит от реактивных  данных и эти данные изменились.

Представим, что у нас есть переменная ``counter``:
```JavaScript
data: {
  message: 'Hello Vue!',
  counter: 0,
},
```

Используем эту переменную в выводе, в нашей вычисляемой функции:

```JavaScript
computed: {
  getComputedDate () {
    console.log('CALLED ', this.counter, 'TIMES')
    return new Date().getSeconds();
  }
}
```

И при клике на заголовок будем менять данную переменную:

```JavaScript
methods: {
  onHeaderClick (text) {
    alert(text)
    this.counter++
  },
}
```

Теперь, при каждом обращении функция будет возвращать всегда разное значение (количество секунд же меняется?), и в консоли мы будем видеть выводы ``CALLED``.

## Жизненный цикл
[Советую прочитать прямо из документации :)](https://ru.vuejs.org/v2/guide/instance.html#%D0%A5%D1%83%D0%BA%D0%B8-%D0%B6%D0%B8%D0%B7%D0%BD%D0%B5%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE-%D1%86%D0%B8%D0%BA%D0%BB%D0%B0-%D1%8D%D0%BA%D0%B7%D0%B5%D0%BC%D0%BF%D0%BB%D1%8F%D1%80%D0%B0)

## Связывание данных

``Связывание данных`` – ключевая особенность ``Vue.js``. Оно позволяет синхронизировать данные и их отображение. Рассмотрим его на примере.
Немного изменим html и добавим поле ввода для указания имени:

```JavaScript
<main>
  <input type="text" />
  <h1>Введенные данные: {{ dataFromInput }}</h1>
</main>
```

Теперь привяжем значение ``dataFromInput`` к полю ввода. Для этого в Vue используется атрибут ``v-model``:

```JavaScript
<main>
  <input type="text" v-model="dataFromInput" />
  <h1>Введенные данные: {{ dataFromInput }}</h1>
</main>
```

Теперь при изменении значения в поле ввода будет меняться значение ``dataFromInput`` и, как следствие, содержимое тэга ``<h1></h1>``, причём изменение происходит в реальном времени. Эта особенность упрощает работу с данными и помогает избавиться от лишних обработчиков событий.
 
## Single File Component
Теперь предлагаю рассмотреть такую вещь как ``Single File Component``.

Что это такое?

Это некоторый файл, с расширением ``.vue``, который имеет в себе верстку компонента, скрипты, относящиеся к этой верстке и стили, которые также относятся к этой верстке.

Таким образом это максимальная инкапсуляция какой-то сущности. Пример:

```VUE
<template>
  <div :class="[#style.wrapper]">Hello!</div>
</template>

<script>
export default {
  data () {
    someString: '',
    someInteger: 0,
  },
  methods: {
    log () {
      console.log('Hello!')
    }
  },
  computed: {
    getSomething () {
      return 'Some value'
    }
  },
  mounted () {
    console.log('Component mounted')
  }
}
</script>

<style module>
.wrapper {
  color: red;
}
</style>
```

Давайте переведем нашу страницу с товарами на такие вот однофайловые компоненты.

### Настройка Webpack

Для начала, нам надо донастроить наш вебпак, чтобы он мог спокойно обрабатывать файлы ``.vue``

Для этого нам надо установить следующие зависимости (в скобках укажу версии, которые использовались в рамках вебинара): 
- ``vue`` (2.6.11)
- ``vue-loader`` (15.9.2)
- ``vue-template-compiler`` (2.6.11 - должна быть как у vue)
- ``vue-style-loader`` (4.1.2)

```bash
npm i vue vue-loader vue-template-compiler vue-style-loader
```

Теперь надо прорписать ``.vue`` правила в ``webpack.config.js``

```JavaScript
rules: [
  {
    test: /\.vue$/,
    loader: 'vue-loader',
  }
]
```

Заимпортим плагин для компиляции:

```JavaScript
const VueLoaderPlugin = require('vue-loader/lib/plugin')

...
resolve: { alias: { vue: 'vue/dist/vue.esm.js' } },
...

plugins: [
  new VueLoaderPlugin()
]
```
 

Также, давайте изменим наш загрузчик стилей, чтобы, мы смогли загружать ``css`` из однофайловых компонентов:

```JavaScript
rules: [
  {
    test: /\.css$/,
    use: [
      'vue-style-loader',
      {
        loader: 'css-loader',
        options: {
          modules: true,
        }
      }
    ]
  },
  ...
]

Используя такую конфигурацию, мы можем начать пользоваться однофайловыми компонентами!
Пример полного конфига можно взять [здесь](https://github.com/Geekbrains-Frontend-Level-2/ZaitsevDmitry/blob/lesson6/webpack.config.js)
 
## Полезные ссылки
- [Документация](https://ru.vuejs.org/)
