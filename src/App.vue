<template>
  <div id="app">
    <h2>ToDoリスト</h2>
    <label v-for="option in options" :key="option">
      <input type="radio" v-model="current" :value="option.value">{{ option.label }}
    </label>
    <br>
    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>コメント</th>
          <th>状態</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(todo,index) in filterTodos" :key="index">
          <td>{{ index }}</td>
          <td>{{ todo.item }}</td>
          <td>
            <button @click="changeState(todo)">{{ labels[todo.state] }}</button>
          </td>
          <td>
            <button @click="deleteItem(index)">削除</button>
          </td>
        </tr>
      </tbody>
    </table>
    <h3>新規タスクの追加</h3>
    <input type="text" v-model="newItem">
    <button @click.prevent="addItem">追加</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newItem:'',
      todos:[],
      options:[
        {value:-1,label:'すべて'},
        {value:0,label:'作業中'},
        {value:1,label:'完了'}
      ],
      current:-1
    }
  },
  methods:{
    addItem(){
      if(!this.newItem) return
      const todo={
        id:this.todos.length,
        item:this.newItem,
        state:0
      }
      this.todos.push(todo)
      this.newItem=''
    },
    changeState(item){
      item.state=item.state?0:1
    },
    deleteItem(index){
      this.todos.splice(index,1)
    }
  },
  computed:{
    filterTodos(){
      return this.todos.filter(todo=>{
        return this.current < 0 ? true : this.current === todo.state
      },this)
    },
    labels() {
    return this.options.reduce(function(accu, curr) {
      console.log(accu)
      console.log(curr)
      return Object.assign(accu, { [curr.value]: curr.label })
    }, {})
  }
  }
}
</script>

<style>

</style>