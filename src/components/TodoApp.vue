<template>
  <div class="container">
    <h2 class="text-center mt-5">My Vue Todo App</h2>

    <div class="d-flex">
        <input v-model="task" type="text"  @keyup.enter="submitTask" ref="input" placeholder="Enter task" class="form-control">
        <button @click="submitTask" class="btn btn-warning rounded-0"><i class="fa fa-plus"></i></button>
    </div>

    <button class="btn btn-danger mt-3  mb-3 float-end small" @click="clearAll">
          <i class="fa fa-trash"></i> Clear All
    </button>

    <table class="table table-bordered mt-5">
        <thead>
            <tr>
                <th scope="col">Task</th>
                <th scope="col">Status</th>
                <th scope="col" class="text-center">#</th>
                <th scope="col" class="text-center">#</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="(task, index) in tasks" :key="index">
                <td>
                    <span :class="{'finished': task.status === 'finished'}">{{ task.name }}</span></td>
            <td style="width: 120px">
                <span class="pointer" @click="changeStatus(index)"
                    :class="{'text-danger' : task.status === 'to-do',
                             'text-warning' : task.status === 'in-progress',
                             'text-success' : task.status === 'finished'
                            }"
                >
                    {{ firstCharUpper(task.status) }}
                </span>
            </td>
                <td>
                    <div class="text-center" @click="editTask(index)">
                        <i class="fa fa-pen" style="color: blue"></i>
                    </div>
                </td>
                <td>
                    <div class="text-center" @click="deleteTask(index)">
                        <i class="fa fa-trash" style="color: red"></i>
                    </div>
                </td>
            </tr>
        </tbody>
    </table>
  </div>
</template>

<script>

export default {
    name: 'TodoApp',
    props: {
        
    },
    data() {
        return {
            task: '',
            editedTask: null,
            availableStatuses: ['to-do', 'in-progress', 'finished'],
            tasks: [
                { 
                    name: 'Steal bananas from the store',
                    status: 'to-do'
                },
                {
                    name: 'Eat 1kg chocolate in 1 hour',
                    status: 'in-progress'
                }
            ]
        }
    },
    mounted() {
        this.$refs.input.focus()
    },
    computed: {
        
    },
    methods: {
        submitTask() {
            if(this.task.length === 0) return alert('Please Enter Task !');

            if(this.editedTask === null){
                this.tasks.push({
                    name: this.task,
                    status: 'to-do'
                });
                this.$refs.input.focus()
            }else {
                this.tasks[this.editedTask].name = this.task;
                this.editedTask = null;
                this.$refs.input.focus()
            }
        
            this.task = '';

        },
        deleteTask(index){
            this.tasks.splice(index,1);
        },
        editTask(index){
            this.task = this.tasks[index].name;
            this.editedTask = index;
            this.$refs.input.focus()
        },
        changeStatus(index){
            let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
            if(++newIndex > 2) newIndex=0;
            this.tasks[index].status = this.availableStatuses[newIndex];
        },
        firstCharUpper(str){
            return str.charAt(0).toUpperCase() + str.slice(1);
        },
        clearAll() {
            this.tasks = [];
        }
    }
}
</script>

<style>
.pointer {
    cursor: pointer;
}
.finished {
    text-decoration: line-through;
}
</style>