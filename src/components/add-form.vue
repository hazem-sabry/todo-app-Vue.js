<template>
  <div id="add-form">
      <form v-on:submit.prevent>

        <div class="input-container">
            <input type="text" id="add-text" v-model="newTodo" @blur="splitDown()"><label @click="splitUp()" id="text-label" for="add-text">Add your next TODO..!</label>
        </div>
        <button @click.prevent @click="addTODOs()" type="button"> + </button>

      </form>

  </div>
</template>
<script>
export default {
    name: 'add-form',
    data(){
        return {
            recieved: [],
            newTodo: ''
        }
    },
    methods: {
        splitUp: function() { // For the label animation
        
            const textLabel = document.querySelector('#text-label');
            textLabel.classList.add('active');
        },
        splitDown: function () { // For the label animation

            if(!this.newTodo){ // if newTodo is empity
                const textLabel = document.querySelector('#text-label');
                textLabel.classList.remove('active');
            }
        },
        getTODOs: function() {

            if (localStorage.getItem('todo_list')) {
                this.recieved = JSON.parse(localStorage.getItem('todo_list'));
                // get the stored data, then parse them from String into JSON
            } else {
                this.recieved = this.$parent.todoList;
                
            }

        },
        addTODOs: function () {
            if(this.newTodo){ //if not empity
                this.recieved.unshift({
                    id: this.recieved.length,
                    title: this.newTodo,
                    done: false
                });
            }

            // clear the text box
            this.newTodo = '';

            this.$emit('todoList', JSON.stringify(this.recieved));

            // animation: close the label
            this.splitDown();

            return true;
        }
    },
    created() {
        this.getTODOs();
        this.$emit('todoList', JSON.stringify(this.recieved));
    }

}

</script>
<style scoped>
    form {
        width: 86%;
        margin: 0 auto;
        height: 55px;
        margin-top: 15px;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .input-container {
        width: 80%;
        position: relative;
    }

    form input[type="text"] {
        width: 100%;
        border-radius: 3px;
        border: 1px solid #0083da;
        color: #0083da;
        padding: 10px;
        font-size: 1.2em;
        outline: none;
    }

    form label {
        position: absolute;
        top: 0px;
        left: 0px;
        font-size: 1.4em;
        width: 100%;
        height: 45px;
        background-color: #0099ff12;
        color: #0099ffc4;
        border-radius: 3px;
        padding: 0px 0 0 7px;
        line-height: 45px;
        user-select: none;
        will-change: top;
        will-change: width;
        will-change: height;
        transition: all .2s cubic-bezier(0.42, 0, 0.35, 1.18);
    }

    form label.active {
        top: -23px;
        left: 0px;
        font-size: 0.8em;
        font-weight: bold;
        width: 150px;
        height: 22px;
        line-height: 23px;
    }

    form button {
        margin-left: 15px;
        border: 0;
        background-color: #0099ff;
        color: #ffffff;
        padding: 0 20px;
        border-radius: 3px;
        font-size: 2.4em;
        cursor: pointer;
        outline: none;
        transition: background-color .2s ease;
    }

    form button:hover {
        background-color: #0099ffed
    }
</style>

