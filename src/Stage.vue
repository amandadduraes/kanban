<template>
    <div class="list" :style="bgStyle">
        <h2>{{ title }}</h2>
        <div class="to-do-list">
            <ol>
                <li v-for="task in filteredTasks" :key="task.text">
                    <span class="text">{{ task.text }}</span>
                    <span class="actions">
                        <button class="remove" @click="remove(task)">&#10006;</button>
                        <button class="next" v-if="nextStage" @click="move(task)">
                            &#10095;
                        </button>
                    </span>
                </li>
            </ol>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        stage: String,
        title: String,
        tasks: Array,
        nextStage: String,
        bgColor: String
    },
    methods: {
        remove(task) {
            this.$emit('removeTask', task)
        },
        move(task) {
            this.$emit('moveStage', task, this.nextStage)
        }
    },
    computed: {
        filteredTasks(){
            return this.tasks?.filter(task => task.stage == this.stage)
        },
        bgStyle() {
            return `background-color: ${this.bgColor}`
        }
    }
}
</script>

<style>
.list {
  min-width: 31%;
  margin: 0 1%;
  padding: 10px;
  background-color: #f0f0f0;
  border-radius: 3px;
  overflow-y: auto;
}

.list:first-child {
  margin-left: 0;
}

.list:last-child {
  margin-right: 0;
}

.to-do-list ol {
  list-style-type: decimal;
  list-style-position: inside;
  padding: 0;
  margin: 0;
}

.to-do-list ol li {
  padding: 5px;
  margin-top: 5px;
  border: 1px solid crimson;
  box-shadow: 2px 2px 2px rgba(0, 0, 0, 0.2x);
  display: flex;
  justify-content: space-between;
}

.to-do-list ol li .text {
  max-width: calc(100% - 60px);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.to-do-list ol li .text.completed {
  text-decoration: line-through;
}

.to-do-list ol li .actions {
  width: 45px;
  display: flex;
  vertical-align: middle;
  height: 100%;
  justify-content: flex-end;
}

.to-do-list ol li .actions button {
  width: 20px;
  padding: 2px;
  height: 20px;
  line-height: 15px;
  border-radius: 10px;
  font-size: 11px;
  border: 1px solid #ffaaaa;
  background-color: transparent;
  cursor: pointer;
}

.to-do-list ol li .actions button.next {
  border: 1px solid #229966;
  margin-left: 5px;

}

.to-do-list ol li .actions button.remove {
  border: 1px solid #992222;
  cursor: pointer;
}
</style>