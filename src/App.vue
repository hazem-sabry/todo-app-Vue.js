<template>
  <div id="app">

    <section class="app-wrapper">
      <h1>We keep your TODO<small>s</small> alive </h1>
      <h2 class="day">{{ today.day }}</h2> <h4> <sub> {{ today.date }}</sub></h4>
      <add-form @todoList="recieveTODOs($event)"></add-form>
      <show-todo-list @complete="completedTodo($event)" @repend="rePendTodo($event)"></show-todo-list>
    </section>
  </div>
</template>

<script>
import addForm from './components/add-form.vue';
import showTODOList from './components/show-todo-list.vue';

export default {
  name: 'app',
  components: {
    'add-form': addForm,
    'show-todo-list': showTODOList
  },
  data () {
    return {
      todoList: [
                {"id": 0, "title": "Upload TODO app to GiHub", "done": false},
                {"id": 1, "title": "Review Vue.js animations", "done": false},
                {"id": 2, "title": "Create new app with Vue.js :D", "done": false}
            ]
    }
  },
  computed: {
    today: function() {

      var weekDays = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saterday'];
      var today = new Date();

      var dd = today.getDate();
      var mm = today.getMonth()+1; //january = 0 ..!
      var yyyy = today.getFullYear();

      var day = weekDays[today.getDay()];

      if(dd<10){
        dd = '0'+dd;
      }

      if(mm<10){
        mm = '0'+mm;
      }

      today = {
        day: day,
        date: dd+'-'+mm+'-'+yyyy
      }

      return today;
    },
    pending: function() {
      return this.todoList.filter( function(item) {
        return !item.done; // return TODOs that have done: false
      })
    },
    completed: function () {
        return this.todoList.filter(function(item){
            return item.done;
        })
    }
  },
  methods: {
    recieveTODOs: function (recievedTODOs){
        this.todoList = JSON.parse(recievedTODOs);
    },
    completedTodo: function(sentID) {
      this.todoList.forEach(function (todo) {
        if (todo.id == sentID) {
          todo.done = true;
        }
      });
      localStorage.setItem('todo_list', JSON.stringify(this.todoList));
    },
    rePendTodo: function(sentID) {
      this.todoList.forEach(function (todo) {
        if (todo.id == sentID) {
          todo.done = false;
        }
      });
      localStorage.setItem('todo_list', JSON.stringify(this.todoList));
    }
  },
  watch: {
      todoList: {
          handler: function(updatedList) {
              localStorage.setItem('todo_list', JSON.stringify(updatedList));
              // set the localStorage, By the way it accept only String
          }
      }
  }
}
</script>

<style lang="scss">
.app-wrapper {
  width: 60%;
  max-width: 450px;
  min-width: 400px;
  min-height: 250px;
  margin: 30px auto;
  padding: 10px;
  background-color: #fff;
  border-radius: 3px;
  font-family: 'Source Sans Pro', sans-serif;
  font-weight: 100;
}

h1 {
  text-align: center;
  margin: 0;
  font-weight: 600;
  font-family: 'Pacifico', cursive;
  color: #3E4451; // #C62D42;
}

h2 {
  margin: 15px 0 10px 20px;
  display: inline-block;
  color: #61afef;
}

h4{
  display: inline-block;
  margin: 0;
  font-weight: bold
}

h4 sub {
  color: #61afef;
  font-weight: 400;
}
</style>
