<script>
  export default { 
    data () {
      return {
        newTodo:'',
        todos:[],
        selectedTab:'all',
      };

    },
    mounted(){
      //當網頁開啟後,先執行裡面的JS
      //先把資料從localStorage特定key拿出來資料,丟入todos裡面
      if(localStorage.getItem('todo')){
        this.todos = JSON.parse(localStorage.getItem('todo'));
      }
    },
    computed: {
      //篩選todolist
      filterTodos() {
        return this.todos.filter((item)=>{
          if(this.selectedTab === 'all'){
            return this.todos;
          }else if(this.selectedTab === 'isTodo'){
            return item.checkBox == true;
          }else if(this.selectedTab ==='notTodo'){
            return item.checkBox == false;
          }
        });
      }
    },
    methods:{
      //新增
      addTodo() {
        // 如果為空值就無法輸入
        let value = this.newTodo.trim();
          if (!value) {
          return;
          }
        const id = this.todos.length ?? 0;
        this.todos.push({ 
          id:id+1,
          todo:this.newTodo,
          checkBox: false,
        });
        this.newTodo = '';

      localStorage.setItem('todo', JSON.stringify(this.todos));
      },

      //刪除
      deleteTodo(index) {
        this.todos.splice(index, 1);

        localStorage.setItem('todo', JSON.stringify(this.todos));
      },

      //修改
      editTodo(index) {
      let newText = prompt('修改文字');
      if (newText === null || newText.trim() === '') {
        return;
      }
      this.todos[index].todo = newText.trim();

      localStorage.setItem('todo', JSON.stringify(this.todos));
      },

    },


  };



</script>


<template>
 <container class="w-screen h-screen flex flex-col">
  <nav class="bg-cyan-200 w-full h-[30px]">todolist</nav>
  <content-all class="bg-rose-400 w-full h-full flex justify-center items-center">
    <content class="bg-white w-[70%] h-[650px] rounded-md flex flex-col">
      <up class="w-full flex flex-col mb-2">
        <up-1 class="w-full flex justify-center items-center gap-11">
          <input v-model="newTodo" type="text" id="" placeholder="請輸入文字" class="w-[88%] h-[30px]  ml-3 px-3 py-2 bg-white border shadow-sm">
          <button @click="addTodo()" class="bg-blue-500 hover:bg-rose-400 text-white font-bold py-2 px-4 rounded m-3  flex items-center justify-center">
          <span class="material-symbols-outlined">note_add</span>
        </button>
        </up-1>
        <up-2 class="w-full flex gap-2 pl-3 border-b-2 border-gray-600">
          <button class="up-2-button" type="button" :class="{'active': selectedTab == 'all' }" @click="selectedTab = 'all'">全部</button>
          <button class="up-2-button" type="button" :class="{'active': selectedTab === 'isTodo' }" @click="selectedTab = 'isTodo'">已執行</button>
          <button class="up-2-button" type="button" :class="{'active': selectedTab === 'notTodo' }" @click="selectedTab = 'notTodo'">未執行</button>
        </up-2>
      </up>
      <list class="w-full grid grid-cols-3 gap-4 px-3 pt-1 text-lg">
        <list-1>執行</list-1>
        <list-2>事項</list-2>
        <list-3 class="text-center">功能</list-3>
      </list>
      <ud class="w-full px-3 overflow-y-auto">
        <ud-in v-for="(todo, index) in filterTodos" :key="index" class="w-full grid grid-cols-3 gap-4 items-center py-3 border-b-2 border-slate-300">
          <ud-list>
            <input class="ml-2" type="checkbox" v-model="todo.checkBox">
          </ud-list>
          <span>{{ todo.todo }}</span>
          <ud-list class="flex justify-center gap-2">
            <button @click="editTodo(index)" class="ud-list-button" type="button">
              <span class="material-symbols-outlined">edit_document</span>
            </button>
            <button @click="deleteTodo(index)" class="ud-list-button" type="button">
              <span class="material-symbols-outlined">scan_delete</span>
            </button>
          </ud-list>
        </ud-in>
      </ud>
    </content>
  </content-all>
 </container>
</template>

<style scoped>
.up-2-button.active{
  @apply bg-red-500 text-black;
}
.up-2-button{
  @apply bg-blue-500 text-white rounded-t-md p-2;
}
.ud-list-button{
  @apply bg-blue-500 text-white rounded p-2 flex items-center justify-center
bg-blue-500 text-white rounded p-2 flex items-center justify-center
}

</style>

