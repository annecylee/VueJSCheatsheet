<template>
  <div id="app" class="container-fluid">
    <nav class="navbar navbar-default row" id="header">
      <div class="navbar-header">
        <h1>Vue.js Cheatsheet</h1>
      </div>
      <!-- <span class="glyphicon glyphicon-menu-hamburger menu" aria-hidden="true"></span> -->
      <ul class="nav navbar-nav navbar-right" id="navigation">
        <li class="active"><a href="index.html">Home</a></li>
        <li><a href="www.tingweili.com">Contact</a></li>
      </ul>
    </nav>
    <div class="row main">
      <aside class="col-md-2 col-lg-3 history">
        <h4>Articles</h4>
        <ul>
          <li><a href="index.html#links">Useful links related to Vue.js</a></li>
          <li><a href="index.html#event-bus">Non Parent-Child Communication - Global Event Bus</a></li>
        </ul>
      </aside>
      <article class="col-md-10 col-lg-9 post">

        <h3 id="links">Useful links related to Vue.js</h3>
        <small class="date">May 16, 2017</small>
        <p>Here are some useful links that will make your development easier, from components to dev tools.</p>
        <ul>
          <li> <a href="http://element.eleme.io/#/en-US/component/layout">Element -- a Vue 2.0 based component library for developers</a>
            <p>It provides demo and sample code for varied compnents, such as form, data, notice, navigation.</p>
          </li>
          <li> <a href="http://yuche.github.io/vue-strap/">VueStrap -- Bootstrap components built with Vue.js, no 3rd party plugins required.</a>
            <p>Another component library that will speed up your effeciency.</p>
          </li>
          <li> <a href="http://monterail.github.io/vue-multiselect/#sub-option-groups">Vue-multiselect -- the most complete selecting solution for Vue.js</a>
            <p>It contains single/multiple select, Server-side Rendering support, Ajax support, and fully configurable.</p>
          </li>
          <small>To be continune...</small>
        </ul>
        <hr>
        <h3 id="event-bus">Non Parent-Child Communication - Global Event Bus</h3>
        <small class="date">May 6, 2017</small>
        <p>According to Vue's <a href="http://vuex.vuejs.org/en/intro.html">document</a>,
          there are two ways for non parent-child communication (e.g. sibling communication).
          One is using Vuex, <cite>"a state management pattern + library for Vue.js applications."</cite>
          Another way is using global event bus. Vuex is more for medium-to-large-scale SPA. If your app is simple,
          Vuex will be verbose and daunting. A gobal event bus will be good enough. Here is an example to use event bus to translate page.
        </p>
        <h4 class="subtitle">Initializing</h4>
        <p>First, let's create an event bus and export it to make it sharable with outher compnents. In this scenario, the empty Vue instance is used as a central event bus.</p>
          <p class="file-name">eventBus.js</p>
          <pre><code>import Vue from 'vue'
export const EventBus = new Vue()</code></pre>
        <h4 class="subtitle">Sending Events</h4>
        <p>To send event, using <code>EventBus.$emit(channel: string, payload1: any, …)</code></p>
        <p class="file-name">CompnentA.vue</p>
        <pre><code>export default {

  data: function () {
    return {
      lanCode: 0,
      isEn: true,
    }
  },

  methods: {
    setLan: function () {
      this.isEn = !this.isEn
      if (this.isEn) {
        this.lanCode = 0
      } else {
        this.lanCode = 1
      }
      EventBus.$emit('setLan', this.lanCode)
    }
  },

}</code></pre>

        <h4 class="subtitle">Receiving Events</h4>
        <p>To receive event, first you need to import EventBus. Then use <code>EventBus.$on(channel: string,  callback(payload1,…))</code> to listen to changes and run callback functions.</p>
        <p class="file-name">CompnentB.vue</p>
        <pre><code>import { EventBus } from './event-bus.js'
export default {
  created: function () {
    EventBus.$on('setLan', (lanCode) => {
      this.lanCode = lanCode
    })
  },
}</code></pre>
        <p>Here is the <a href="http://www.tingweili.com/PointMattersDemo/">live demo</a> for internationalization your page using event bus.</p>
        <small class="reference">Reference:
          <ul>
            <li><a href="https://alligator.io/vuejs/global-event-bus/">Creating a Global Event Bus with Vue.js</a></li>
            <li><a href="https://devblog.digimondo.io/building-a-simple-eventbus-in-vue-js-64b70fb90834">Building a Simple Event Bus in Vue.js</a></li>
            <li><a href="https://vuejs.org/v2/guide/components.html#Non-Parent-Child-Communication">Vue.js Document</a></li>
          </ul>
        </small>

        <hr>
      </article>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>

<style>

.* {
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
}

#app {
  font-family: 'Source Sans Pro', sans-serif, 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding: 0;
  font-size: 18px;
}

article, aside {
  text-align: left;
}

h1, h3, h4 {
  font-weight: normal;
  text-align: center;
}

h1 {
  font-size:20px;
  line-height: 1.1;
  font-family: 'Cabin Sketch', cursive;
  color: #fff;
}

hr {
  margin: 80px 0;
}

.nav ul {
  list-style-type:inherit;
  padding: 0;
  color: #0099ff;
}

li {
  margin: 0 10px;
}

a {
  color: #0099ff;
}

pre {
  padding: 10px 20px;
}


.navbar {
  margin-bottom: 20px;
  padding: 0 40px;
  background-color: #000;
}

#navigation {
  font-size: 14px;
}

#navigation .active > a {
  background: none;
  color: #0099ff;
  font-weight: bolder;
}


.main {
  max-width: 1200px;
  margin: 0 auto;
  padding: 30px;
}

.file-name {
  text-align:right;
  color:#999; font-size:14px;
  line-height:5px;
}

.history {
  border: 1px solid #ccc;
  font-size: 14px;
  padding: 30px;
}

.post {
  padding: 10px 50px;
}

.date {
  display: block;
  text-align: center;
  margin: 20px;
  color: #999;
}

.subtitle {
  text-align: left;
  color: #666;
  margin-top: 30px;
  font-size: 18px;
}

.reference {
  line-height: 60px;
  opacity: 0.7;
}

.reference ul{
  list-style: none;
  display: inline-flex;
}
</style>
