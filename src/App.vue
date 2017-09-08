<template>
  <div id="app">
    <h1>{{title}}</h1>
    <ul>
      <input v-model="newItem" @keyup.enter="addNews"/>
      <!-- v-bind:class="[liclass]" -->      
      <li v-for="item in items" v-bind:class="{finished:item.isFinished}" v-on:click="toggleFinish(item)">
        {{item.label}}
      </li>
      <p>child tells me:{{childwords}}</p>
      <!-- 两种都可以 -->
      <ComponentA msgfromfather="you dispatch!" v-on:child-tell-me-something="listenToMyBoy"></ComponentA>
      <!-- <component-a></component-a> -->
    </ul>
  </div>
</template>

<script>
import Store from './store'
import ComponentA from './components/componentA'

export default {
  data:function(){
    return{
      title:'this is a todo lists',
      items:Store.fetch(),
      liclass:'this is a li class',
      newItem:'',
      childwords:''
    }    
  },
  methods:{
    toggleFinish:function(item){
        item.isFinished =! item.isFinished;
    },
    addNews:function(){
      this.items.push({
        "label":this.newItem,
        "isFinished":false
      });
      this.newItem = '';
    },
    listenToMyBoy:function(msg){
      this.childwords=msg
    }
  },
  components:{ComponentA},
  watch:{
    items:{
      handler:function(items){
        Store.save(items);
      },
      deep:true
    }
  }
}
</script>

<style>
.finished{text-decoration: underline;}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
