<template>
    <div class="container">
        <div class="row justify-content-center mt-5">
            <div class="col-md-6">
                <div class="card">
                    <div class="card-header">Todo Component</div>

                    <div class="card-body">
                        <div class="input-group">

                        <input type="text" v-model="todo_input" class="form-control"
                           placeholder="todo-item"
                           id="box" style="width: 30vw" />
                        <button type="button" class="btn btn-info" @click="add()">Add</button>
                        </div>
                        <table class="table table-striped">
                        <thead>
                            <tr>
                            <th scope="col">#</th>
                            <th scope="col">Todo Item</th>
                            <th scope="col">Action</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="(todo,index) in todos" :key="index">
                            <th scope="row">{{++index}}</th>
                            <td>{{todo.todo_item}}</td>
                            <td><button type="button" class="btn btn-sm btn-danger" @click="deleteTodo(--index)">Delete</button></td>
                            </tr>
                        </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data(){
            return{
                todos:[],
                api: 'todos',
                todo_input:''
            }

        },

        mounted() {
            // console.log('Component mounted.')
            this.axios.get(this.api).then(res =>{
                this.todos=res.data;
            })
        },

        methods:{
           add(){
               if(this.todo_input.length>0)
               {
                    let data={'todo_item':this.todo_input};
                    this.axios.post(this.api,data).then(res=>{
                    this.todos.push(res.data);
                    this.todo_input='';
                 })
               }
           },
           deleteTodo(index){

               if (!window.confirm(`Are you sure you want to delete ${this.todos[index].todo_item}`)) {
                    return;
                }

                if(this.todos[index].id){
                    this.axios.delete(this.api+'/'+this.todos[index].id).then(res=>{
                    this.todos.splice(index, 1);
               })
                }

           }
        }
    }
</script>
