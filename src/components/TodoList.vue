<template>
  <div>
    <ul>
        <li v-for="(todoItem, idx) in todoItems" v-bind:key="todoItem.item" class="shadow">
            <span v-on:click="toggleComplete(todoItem, idx)">
              <i class="checkBtn fas fa-check"
              v-bind:class="{checkBtnCompleted: todoItem.completed}"
             ></i>
            </span>
            <span v-bind:class="{textCompleted: todoItem.completed}">{{ todoItem.item }}</span>
            <span class="removeBtn" v-on:click="removeTodo(todoItem, idx)">
                <i class="fa-regular fa-trash-can"></i>
            </span>
        </li>
    </ul>
  </div>
</template>

<script>
export default {
    data(){
        return {
            todoItems: [],
        }
    },
    methods: {
        removeTodo(todoItem, idx){
            localStorage.removeItem(todoItem.item);
            console.log("removeTodo: "+idx,todoItem);
            // javascript 문법, 특정 인덱스에서 하나를 지운다.
            this.todoItems.splice(idx, 1);
        },
        toggleComplete: function(todoItem, idx){
          todoItem.completed = !todoItem.completed;
          // 로컬스토리지 갱신
          localStorage.removeItem(todoItem.item);
          localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
        }
    },
    created(){
        if(localStorage.length > 0){
            for(var i = 0; i < localStorage.length; i++){
                if(localStorage.key(i) !== 'loglevel:webpack-dev-server'){
                  this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))))
                    // console.log(typeof(localStorage.getItem(localStorage.key(i))));
                }
            }
        }
    },
}
</script>

<style scoped>
ul {
  list-style-type: none;
  padding-left: 0px;
  margin-top: 0;
  text-align: left;
}
li {
  display: flex;
  min-height: 50px;
  height: 50px;
  line-height: 50px;
  margin: 0.5rem 0;
  padding: 0 0.9rem;
  background: white;
  border-radius: 5px;
}
.removeBtn {
  margin-left: auto;
  color: #de4343;
}
.checkBtn {
  line-height: 45px;
  /* color: black; */
  color: #62acde;
  margin-right: 5px;
  margin-top: 15px;
}
.checkBtnCompleted {
  /* color: #62acde; */
  color: #b3adab;
}
.textCompleted {
  text-decoration: line-through;
  color: #b3adab;
}
</style>