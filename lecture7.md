# Лекция 7

Можно посмотреть пример в записи вебинара: [https://geekbrains.ru/lessons/66072](https://geekbrains.ru/lessons/66072)

- [Вступление](https://github.com/Geekbrains-Frontend-Level-2/lectures/blob/master/lecture7.md#вступление)
- [Из чего состоит VUEX](https://github.com/Geekbrains-Frontend-Level-2/lectures/blob/master/lecture7.md#из-чего-состоит-vuex)
- [Как интегрировать VUEX в наше приложение](https://github.com/Geekbrains-Frontend-Level-2/lectures/blob/master/lecture7.md#как-интегрировать-vuex-в-наше-приложение)
- [Как вызвать геттер и экшн из компонента](https://github.com/Geekbrains-Frontend-Level-2/lectures/blob/master/lecture7.md#как-вызвать-геттер-и-экшн-из-компонента)
- [Полезные ссылки](https://github.com/Geekbrains-Frontend-Level-2/lectures/blob/master/lecture7.md#полезные-ссылки)

## Вступление
В сегодняшней лекции мы продолжим знакомиться с Vue, а именно с паттерном ``Vuex``.

``Vuex`` – это паттерн управления состоянием (то есть данными) нашего приложения VUE.
В центре любого ``Vuex``-приложения находится хранилище (store). В этом хранилище как раз и будет находиться состояние нашего приложения.

Если у нас есть сайт (приложение), в котором много компонентов,
и эти компоненты обмениваются между собой данными, то зачастую такой обмен может быть очень неудобным
(у нас много всего, передавать все через props будет в конце концов проблематично).
Поэтому было бы здорово иметь какое-то общее хранилище с этими данными,
и когда какому-то компоненту они будут нужны – он к ним обратится.

Если данные необходимо изменить, то компонент будет инициализировать это изменение,
а остальные компоненты смогут тут же это изменение получить.

<img alt="VUEX" src="https://cdn-images-1.medium.com/max/1600/1*HWBipJ4YFcEGrBoJA3jhuQ.jpeg" width="500" />

## Из чего состоит VUEX
Давайте рассмотрим пример. Для начала нам надо создать простейшее VUEX хранилище.

``goods.js``
```JavaScript
const state = {
  data: 'Hello'
}

const getters = {
  getData: state => state.data,
}

const actions = {

}

const mutations = {

}

export default {
  namespaced: true,
  state,
  getters,
  actions,
  mutations,
}
```

Итак, из чего у нас состоит модуль Vuex?
1)	``State``, оно же состояние, оно же хранилище данных. Именно тут мы можем хранить то, чем будут пользоваться все наши модули.
2)	``Getters`` – нашим компонентам необходимо получать данные из хранилища. Это можно сделать при помощи ``Getters``. ``Getters`` – это функции, так что в них можно как-то обработать данные из хранилища, "вытащить" только какие-то определенные данные и так далее.
3)	``Actions`` – действия. Это функции, которые содержат бизнес-логику приложения. Эти функции НЕ могут изменять состояние нашего хранилища. То есть напрямую на данные они не влияют. Вызов действия является асинхронной операцией (то есть если мы откуда-то его вызовем, то исполнение нашего кода не прекратится). Это удобно и зачастую именно в действия выполняют какие-то запросы к серверу.
Чтобы изменить данные в хранилище, действие может запускать мутацию
4)	``Mutations`` – мутации. Именно они обновляют данные в нашем хранилище.

> По сути, это хранилище можно сравнить с глобальным объектом, куда мы помещаем данные, а потом компоненты обращаются к этому глобальному объекту.

Но есть пара моментов, которые отличают Vuex от обычного глобального объекта:
1)	Все данные – реактивны. То есть, если какой-то компонент использует данные из ``Vuex`` стора, и они там обновились, то данные обновления сразу применятся и во всех компонентах, зависящих от хранилища.
2)	Данные в хранилище нельзя изменить напрямую. Единственный способ – это вызвать так называемую мутацию. Это гарантирует, что у нас изменение хранилища будет осуществлено через единую точку входа (можно так сказать).

### Давайте разберем на примере:
<img alt="VUEX" src="https://vuex.vuejs.org/ru/vuex.png" width="500" />
Давайте представим, что у нас есть шапка (на сайте), в котрой есть форма для входа (если нет авторизации),
либо выводится никнейм пользователя (если авторизация есть).

Рассмотрим картинку сверху:
1) Шапка Vue (компонент) берет данные из ``Состояния`` (Store). Пользователя нет, поэтому рендерится форма для входа.
2) После заполнения формы авторизации, вызывается ``Действие`` (Action). В Action происходит запрос на сервер, который нас авторизует и присылает данные пользователя (анпример, его никнейм)
3) Действие вызывает ``Мутацию`` (Mutation) с пришедшими данными. ``Мутация`` помещает данные в ``Состояние`` (Store).
4) После того как Мутация изменила Состояние, компонент шапки автоматически изменится (уберет форму для входа и отобразит никнейм). Это происходит, потому что данные в состоянии реактивны.

## Как интегрировать VUEX в наше приложение

Для начала нам надо проинициализировать VUEX и подключить в него файл ``goods.js``

``store.js``
```JavaScript
import Vue from 'vue'
import Vuex from 'vuex'

import goods from './goods'

Vue.use(Vuex) // Говорим фреймворку, что собираемся использовать модуль Vuex

// Инициализируем наш стор
const store = new Vuex.Store({
  modules: {
    goods // передаем в модули импортированный стор goods.js
  },
})

export default store
```

Теперь нам надо в месте, где инициализировали ``Vue``, подключить наш модуль
``main.js``
```JavaScript
import store from './store'

new Vue({
  el: 'main',
  template: '<App />',
  components: {
    App,
  },
  store
})
```

Это все :) Теперь все наши компоненты смогут использовать наш store.

## Как вызвать геттер и экшн из компонента

### Вызываем геттер
Для примера, давайте выведем ``state.data`` (Hello) из ``goods.js``
Для этого нам необходимо обратиться к getters нашего модуля. В этом нам поможет вспомогательная функция ``mapGetters``.

> Функция ``mapGetters`` проксирует геттеры хранилища в локальные вычисляемые свойства компонента

``App.vue``
```JavaScript
import { mapGetters } from 'vuex' // необходимо имортнуть вспомогательную функцию

export default {
  // data, methods и остальное
  computed: {
    // mapGetters помещаем в computed
    ...mapGetters('goods', [ // в качестве первого аргумента указываем название модуля, в котором находятся наши данные
      'getData' // указываем название геттера
    ]
  }
}
```

### Вызываем экшн

Для начала давайте вернемся в наш модуль Vuex (``goods.js``) и напишем необходимый функционал

Первым делом напишем мутацию. Именно она будет изменять наши данные.
```JavaScript
...
const mutations = {
  changeDataMutation (state, newData) {
    state.data = newData
  }
}
...
```
Первым параметром передаем наш текущий ``state``, вторым параметром передаем новые данные.

Далее давайте напишем экшн, который и будет вызывать в себе данную мутацию:
```JavaScript
...
const actions = {
  changeData ({ commit }, newData) {
    commit('changeDataMutation', newData)
  }
}
...
```
Первым параметром в экшене передается объект с контекстом нашего Vuex-модуля.
В нем находится ``state`` (как и в мутации), а также в нем находится метод, который помогает запускать сами мутации. Называется он ``commit``

Теперь вернемся в наш ``App.vue`` и сделаем кнопку, которая будет запускать все это дело
```JavaScript
<button @click="onClick">Поменять Hello на something</button>

...

import { mapAction } from 'vuex'

...

export default {
  methods: {
    ...mapActions('goods', [
      'changeData'
    ]),
    onClick () {
      this.changeData('something')
    }
  }
}
```

Выглядит это абсолютно также как использование getters, только в данном случае мы размещаем ``mapActions`` в ``methods``

## Полезные ссылки
- [Документация](https://vuex.vuejs.org/ru/)
