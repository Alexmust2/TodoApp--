<template>
    <div class="list__container"> 
        <div class="list__header">
                <span>Готово</span>
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
    name: 'ReadyTasksList',

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
                title: 'Задача создана в «Готово»',
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
                title: 'Задача перенесана в «Готово»',
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

<style lang="scss">

@import url('https://fonts.googleapis.com/css2?family=Inter:wght@500;600&display=swap');

.list__header {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    padding: 7px 38px;
    gap: 8px;

    background: #53C666;
    border-radius: 8px 8px 0px 0px;

    height: 32px;

    flex: none;
    order: 0;
    align-self: stretch;
    flex-grow: 0;
    span {
        font-size: 14px;
        font-weight: 600;
        font-family: 'Inter', sans-serif;
    }
}
.list__container {

    box-sizing: border-box;
    margin:0 8px;


    display: flex;
    flex-direction: column;
    align-items: flex-start;
    padding: 0px;

    max-height: 688px;
    width: 178px;


    border-radius: 8px;


    flex: none;
    order: 4;
    align-self: stretch;
    flex-grow: 1;
  
}

.list {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    padding: 8px 16px 8px 8px;
    gap: 8px;
    max-height: 564px;

    overflow-y: auto;

    background: #F7F7F7;
    border: 1px solid #E3E5E8;

    flex: none;
    order: 1;
    align-self: stretch;
    flex-grow: 1;
}

.task {
    display: flex;
    flex-direction: row;
    padding: 8px 24px 8px 8px;
    gap: 13px;

    width: 153.6px;

    background: #FFFFFF;

    border: 1px solid #C4CAD4;
    border-radius: 4px;


    span {
    width: 121.6px;

    margin: auto;

    font-style: normal;
    font-weight: 400;
    font-size: 14px;
    line-height: 18px;

    font-feature-settings: 'tnum' on, 'lnum' on;

    color: #1C2530;

    flex: none;
    order: 0;
    align-self: stretch;
    flex-grow: 0;
    z-index: 55;
    }

    .task__title {
        overflow-wrap: break-word;
    }

    .task__more {
        width: 10px;
        height: 10px;
        font-size: 18px;
        cursor: pointer;
        position: relative;
        margin: 13px 0 19px 0;
    }
    textarea {
        overflow-wrap: break-word;
    }
}


.edit__container {
    width: 20px;
    height: 20px;
    button{
        border: none;
        background: none;
        cursor: pointer;
    }
}

.active {
    border: 1px solid #3D86F4;
    .task__more {
        visibility: hidden;
    }
}

.deletemodal__content {
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    margin-left: 40px;

    .deletemodal__title {
        width: 420px;
        height: 30px;
        font-size: 24px;
        margin-top: 24px;
        margin-bottom: 25px;
    }
    .deletemodal__task-title {
        width: 420px;
        height: 18px;
        word-wrap: break-word;
        font-weight: 400;
        font-size: 14px;
        margin-top: 25px;
    }
    .deletemodal__buttons {
        display: flex;
        flex-direction: row;
        align-items: flex-start;
        padding: 0px;
        width: 420px;
        height: 36px;
        button {
            display: flex;
            flex-direction: row;
            justify-content: center;
            align-items: center;
            padding: 8px 16px;
            gap: 8px;
            width: 202px;
            height: 36px;
            border: 1px solid #C4CAD4;
            border-radius: 4px;
            margin-top: 33px;
            font-size: 14px;
            background: none;
            cursor: pointer;
        }
    }
}

.addtask {
    width: 62px;
    height: 18px;

    font-style: normal;
    font-weight: 400;
    font-size: 14px;
    line-height: 18px;

    text-align: center;

    background: none;
    border: none;

    color: #3D86F4;
    display: flex;
    align-items: center;
    margin-top: 18px;
}

.addmode {
    display: flex;
    flex-direction: row;
    padding: 8px 24px 8px 8px;
    gap: 13px;

    width: 153.6px;
    min-height: 10px;

    background: #FFFFFF;

    border: 1px solid #C4CAD4;
    border-radius: 4px;

    textarea {
        width: 80%;
        border: none;
        overflow: hidden;
        outline:0px none transparent;
    }
    textarea:focus, input:focus{
        outline: 0;
    }
}
</style>