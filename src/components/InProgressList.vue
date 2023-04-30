<template>
    <div class="list__container"> 
        <div class="list__header">
                <span>В процессе</span>
        </div>
        <div class="list"
        @dragover="dragOver"
        @drop="drop"
        >
            <div 
            class="task" 
            v-for="(task, index) in tasks" 
            :class="{active: task.isActive, task: !task.isActive}"
            :key="task.id"
            :draggable="true"
            @dragstart="dragStart($event, task)"
            @dragend="dragEnd($event, index, title)"
            >
                <span 
                class="task__title"
                :contenteditable="activeIndex === index" 
                @input="updateText($event.target.innerText, index)"
                :key="index"
                >
                {{ task.title }}
                </span>
                <div v-if="task.editMode" class="edit__container">
                    <button @click="cancelEdit(index)"><img src="@/assets/x.png"></button>
                    <button @click="save(index)" style="margin-left: 3px;"><img src="@/assets/yes.png"></button>
                </div>
                <span class="task__more" @click="openModal(index)" :key="index">...</span>
                <MoreModal v-if="task.isOpen">
                    <button @click="edit(index), task.isOpen = false"><img src="@/assets/edit.png"/> Редактировать</button>
                    <button  style="align-self: flex-start;" @click="DeleteModal(index), task.isOpen = false"><img src="@/assets/delete.png"/> Удалить</button>
                </MoreModal>
                <DeleteModal v-if="task.Delete">
                    <div class="deletemodal__content">
                        <span class="deletemodal__title">Удалить задачу?</span>
                        <span class="deletemodal__task-title">{{ DeleteTaskTitle }}</span>
                        <div class="deletemodal__buttons">
                            <button @click="removeTask(index, task)">Удалить</button>
                            <button style="margin-left: 16px;" @click="task.Delete = false">Отменить</button>
                        </div>
                    </div>
                </DeleteModal>
            </div>
            <button @click="addMode = true, focused = true" v-if="!addMode" class="addtask"><img style="margin-right: 8px;" src="@/assets/add.png">Добавить</button>
            <div class="addmode" v-if="addMode">
                <textarea @focus="focused = true" placeholder="Введите текст..." v-model="addedText"></textarea>
                <div class="edit__container">
                    <button @click="cancelAdd"><img src="@/assets/x.png"></button>
                    <button @click="saveText" style="margin-left: 3px;"><img src="@/assets/yes.png"></button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import DeleteModal from './UI/DeleteModal.vue';
import MoreModal from './UI/MoreModal.vue';

export default {
    name: 'InProgressList',
components:{
    MoreModal,
    DeleteModal
},
    data() {
        return {
            tasks:[
            ],
            addedText: "",
            addMode: false,
            activeIndex: null,
            selectedItem: null,
            Delete: false,
            DeleteTaskTitle: '',
            focused: false,
            isActive: false
        }
    },
    methods: {
        DeleteModal(index) {
            this.tasks.forEach((task, i) => {
                if (i === index) {
                    task.Delete = true;
                    this.DeleteTaskTitle = task.title
                } else{
                    task.Delete = false;
                }
            });
        },

        removeTask(index, task) {
            this.tasks.splice(index, 1);
            this.$notify({
                title: 'Задача удалена',
                text: task.title,
            });
        },
        saveText() {
            this.addMode = false;
            this.tasks.push({
                title: this.addedText,
                isOpen: false,
                editMode: false,
                Delete: false,
            })
            this.addedText = ''
            this.$notify({
                title: 'Задача создана в «В процессе»',
                text: 'Задача ' + this.tasks.length,
            });
        },
        cancelAdd() {
            this.addMode = false;
            this.addedText = "";
        },

        save(index) {
            this.activeIndex = null
            this.tasks[index].editMode = false,
            this.tasks[index].isActive = false
        },

        cancelEdit(index) {
          this.activeIndex = null
          this.tasks[index].title = this.tasks[index].title
        }, 

        edit(index){
            this.activeIndex = index;
            this.tasks.forEach((task, i) => {
                if(i === index) {
                    task.editMode = true
                    task.isActive = true
                } else {
                    task.editMode = false
                    task.isActive = false
                }
            })
            
        },
        dragOver(event) {
            event.preventDefault()
            event.dataTransfer.dropEffect = 'move'
        },
        drop(event) {
            event.preventDefault()
            const task = JSON.parse(event.dataTransfer.getData('text/plain'))
            this.tasks.push(task)
            this.$notify({
                title: 'Задача перенесана в «В процессе»',
                text: task.title,
            });
        },
        updateText(title){
            this.NewTitle = title

        },  
        openModal(index) {
            this.tasks.forEach((task, i) => {
                if (i === index) {
                    task.isOpen = true;
                } else{
                    task.isOpen = false;
                }
            });
        },
        dragStart(event, task) {
            event.dataTransfer.effectAllowed = 'move'
            event.dataTransfer.setData('text/plain', JSON.stringify(task))

    },
        dragEnd(event, index) {
            event.dataTransfer.clearData()
            this.tasks.splice(index, 1)
    }
  },
        watch: {
            tasks() {
                this.$emit('update-tasks', this.tasks)
            }
        }
}
</script>

<style scoped lang="scss">
.list__header {
    background-color: #FFD466;
}

.list__container {
    flex: none;
    order: 3;
    align-self: stretch;
    flex-grow: 1;
}
</style>