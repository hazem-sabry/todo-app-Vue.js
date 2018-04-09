<template>
    <div id="show-list">

        <div class="btn-wrapper">
            <button class="btn" @click="showCompleted = !showCompleted"> <span v-show="!showCompleted">Show</span> <span v-show="showCompleted">Hide</span> completed TODOs</button>
            <button class="btn" @click="clear()">Clear all</button>
        </div>

        <ul class="pending" v-show="this.$parent.pending.length > 0">
            <h3>You have <span>{{ this.$parent.pending.length }}</span> Pending TODOs</h3>
            <li v-show="!todo.done" @click.self="comleteTODOs(todo.id)" v-for="(todo, index) in this.$parent.todoList" :key="index" v-custom-color="colors">
                
                <i @click.self="comleteTODOs(todo.id)" class="icon-pencil" v-bind:class="{'move-pencile': liIndex == index}"></i>
                <i class="dot" v-bind:class="{'move-line': liIndex == index}"></i>

                <span @click.self="comleteTODOs(todo.id)">
                    {{ todo.title }}
                </span>

                <i class="icon-trash" @click="deleteTODOs(todo)" @mouseover="liIndex = index" @mouseout="liIndex = null"></i>
            </li>
        </ul>

        
        <ul class="completed" v-show="showCompleted">
            <h3>You have <span>{{ this.$parent.completed.length }}</span> Completed TODOs</h3>
            <li v-show="todo.done" v-for="(todo, index) in this.$parent.todoList" :key="index" @click.self="rePend(todo.id)">
                <i @click="rePend(todo.id)" class="icon-pencil" v-bind:class="{'move-pencile': liIndex == index}"></i>
                <i class="dot" v-bind:class="{'move-line': liIndex == index}"></i>

                <span @click="rePend(todo.id)">
                    {{ todo.title }}
                </span>

                <i class="icon-trash" @click="deleteTODOs(todo)" @mouseover="liIndex = index" @mouseout="liIndex = null"></i>
            </li>
        </ul>
    </div>    
</template>

<script>
export default {
    name: 'show-list',
    data() {
        return {
            colors: ["#9b59b6", "#3498db", "#2ecc71", "#f1c40f", "#e67e22", "#e74c3c"],
            liIndex: null, // this variable is to fire the animation to the selected li
            showCompleted: false
        }
    },
    methods: {
        deleteTODOs: function(item) {
            this.$parent.todoList.splice(this.$parent.todoList.indexOf(item), 1);
            localStorage.setItem('todo_list', JSON.stringify(this.$parent.todoList));
            // I delete the selected index from the array, then reset the localStorage again
            // Because it accept string values, so I can't delete particualr index

            if(this.$parent.completed.length == '0'){
                this.showCompleted = true;
            }
        },
        comleteTODOs: function(id) {
            this.$emit('complete', id);
        },
        rePend: function(id) {
            this.$emit('repend', id)
        },
        clear: function() {
            this.$parent.todoList = [];
        },
        hazem: function() {
            console.log(this.$refs.hazem);
        }
    },
    directives: {
        'custom-color': {
            bind(el, binding){
                // I pass the array of colors as a parameter in the directive
                el.style.borderTop = '2px solid ' + binding.value[Math.floor(Math.random() * 6)];
            }
        }
    }
}
</script>

<style scoped>
    h3 {
        margin: 0 0 0 15px;
    }

    h3 span {
        font-family: 'Pacifico', cursive;
        font-size: 1.5em;
    }

    .btn-wrapper {
        text-align: center;
        margin: 25px 0 -15px;
    }

    .btn-wrapper .btn {
        border: none;
        outline: none;
        padding: 4px 8px;
        display: inline-block;
        border-radius: 2px;
        background-color: #0099ff21;
        color: #0083dad5;
        cursor: pointer;
        font-family: 'Pacifico', cursive;
        margin: 0 10px 0 0;
        box-shadow: 2px 0px 3px 1px  #0099ff21;
        transition: background-color .3s ease,
                    box-shadow .3s ease;
    }
    .btn-wrapper .btn:hover {
        box-shadow: 2px 2px 5px 2px #0099ff21;
    }

    ul {  
        list-style: none;
        padding: 0;
    }

    ul li {
        position: relative;
        padding: 13px;
        margin: 0 13px 8px;
        font-weight: 600;
        border-radius: 5px;
        background-color: #0099ff08;
        box-shadow: 2px 0px 3px 1px rgba(236, 240, 241,.6);
        cursor: pointer;
        user-select: none;
        transition: box-shadow .5s ease;
    }

    ul li:hover{
        box-shadow: 2px 2px 5px 2px rgb(214, 216, 216);
    }

    ul li i.icon-pencil {
        position: absolute;
        left: 10px;
        top: 17px;
        transition: all .2s ease;
    }

    ul li i.dot{
        width: 0px;
        height: 1px;
        border-bottom: 1px solid #5555556e;
        display: block;
        position: absolute;
        top: 25px;
        left: 27px;
        transition: all .2s ease;
    }

    i.move-line {
        top: 25px !important;
        width: calc(85% - 23px) !important;
    }

    i.move-pencile {
        top: 10px !important;
        left: 85% !important;
    }

    ul li span {
        margin-left: 20px;
        display: inline-block;
        max-width: 280px;
        word-wrap: break-word;
    }

    ul li i.icon-trash {
        position: absolute;
        top: 15px;
        right: 8px;
        padding: 3px;
        transform: rotateZ(180deg) scale(.2);
        opacity: 0;
        transition: all .35s ease;
        will-change: transform;
    }

    ul li:hover i.icon-trash {
        transform: rotateZ(0deg) scale(1.3);
        opacity: 1;
    }

    i.icon-trash:hover {
        font-weight: bold;
        color: rgba(231, 76, 60,1.0);
    }

    ul.completed li{
        opacity: .2;
        transition: opacity .3s ease;
    }

    ul.completed li:hover{
         opacity: 1;
    }
</style>
