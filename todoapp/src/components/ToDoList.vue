<template>
  <div>
    <Window v-if="isAddToDoVisible">
      <input 
        v-model="titleToDo" 
        type="text" 
        placeholder="Заголовок"
        class="todo-input"
      >
      <input 
        v-model="descToDo" 
        type="text" 
        placeholder="Описание"
        class="todo-input"
      >
      <select
        v-model="grupToDo"
        class="todo-select"
      >
        <option
          v-for="grup in grups"
          :key="grup.id"
        >
          {{ grup.title }}
        </option>
      </select>
      <div class="todo-win_btns">
        <button
          @click="AddToDo"
          class="todo-item_btn"
        >
          Создать
        </button>
        <button
          @click="isAddToDoVisible = !isAddToDoVisible"
          class="todo-item_btn"
        >
          Закрыть
        </button>
      </div>
    </Window>

    <Window v-if="isAddGrupVisible">
      <input 
        v-model="titleGrup" 
        type="text" 
        placeholder="Заголовок"
        class="todo-input"
      >
      <div class="todo-win_btns">
        <button
          @click="AddGrup"
          class="todo-item_btn"
        >
          Создать
        </button>
        <button
          @click="isAddGrupVisible = !isAddGrupVisible"
          class="todo-item_btn"
        >
          Закрыть
        </button>
      </div>
    </Window>

    <input 
      v-model="searchToDo" 
      type="search" 
      class="todo-search" 
      placeholder="Поиск"
    >
    <div class="todo-buttons">
      <button
        @click="sortCompleted = !sortCompleted"
        class="todo-button"
      >
        <p v-if="!sortCompleted">
          Не выполненно
        </p>
        <p v-else>
          Все задачи
        </p>
        <input 
          v-model="sortCompleted" 
          id="sortCompleted" 
          type="checkbox" 
          class="todo-checkbox"
        >
      </button>
      <button
        @click="isAddToDoVisible = !isAddToDoVisible"
        class="todo-button"
      >
        Создать задание
      </button>
      <button
        @click="isAddGrupVisible = !isAddGrupVisible"
        class="todo-button"
      >
        Создать группу
      </button>
    </div>
    <div class="todo-grups">
      <div 
        v-for="(grup, index) in grups" 
        :key="grup.id" 
        class="todo-grup"
      >
        <h2	class="todo-grup_name">
          {{ grup.title }}
          <button 
            v-if="grup.title != 'No Grup'" 
            @click="RemoveGrup(index, grup.title)" 
            class="todo-item_btn"
          >
            Удалить
          </button>
        </h2>
			
        <div 
          v-for="(todo, index) in filters" 
          :key="todo.id" 
          class="todo-items"
        >
          <div 
            v-if="todo.grup == grup.title" 
            :class="{ 'todo-completed': todo.completed }"
          >
            <div 
              v-if="sortCompleted == true && todo.completed == false || sortCompleted == false" 
              class="todo-item"
            >
              <div class="todo-item_text">
                <h3>{{ todo.title }}</h3>
                <p class="todo-item_description">
                  {{ todo.description }}
                </p>
              </div>
              <div class="todo-item_btns">
                <button
                  v-if="!todo.completed"
                  @click="CompletedToDo(todo)"
                  class="todo-item_btn"
                >
                  Завершить
                </button>
                <button
                  v-else
                  @click="CompletedToDo(todo)"
                  class="todo-item_btn"
                >
                  Отменить
                </button>
                <button
                  @click="RemoveToDo(index)"
                  class="todo-item_btn"
                >
                  Удалить
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
	import Window from './Window'
	export default {
		name: 'ToDoList',
		components: {
			Window
		},
		data () {
			return {
				isAddToDoVisible: false,
				isAddGrupVisible: false,
				sortCompleted: false,
				searchToDo: '',
				titleToDo: '',
				descToDo: '',
				grupToDo: 'No Grup',
				titleGrup: '',
				todos: [
					{
						'id': 1,
						'title': 'Заголовок',
						'description': 'Описание',
						'completed': true,
						'grup': 'No Grup',
					},
					{
						'id': 2,
						'title': 'Заголовок 2',
						'description': 'Описание 2',
						'completed': false,
						'grup': 'Ok Grup',
					},
					{
						'id': 3,
						'title': 'заголовок 3',
						'description': 'Описание 3',
						'completed': false,
						'grup': 'No Grup',
					}
				],
				grups: [
					{
						'id': 1,
						'title': 'No Grup',
					},
					{
						'id': 2,
						'title': 'Ok Grup',
					}
				]
			}
		},
		computed: {
			filters(){
				return this.todos.filter(todos => {
					return todos.title.toLowerCase().includes(this.searchToDo.toLowerCase())
				});
			}
		},
		methods:{
			AddToDo() {
				this.todos.push({
					id: this.todos.length+1,
					title: this.titleToDo,
					description: this.descToDo,
					completed: false,
					grup: this.grupToDo,
				})
				this.titleToDo = ''
				this.descToDo = ''
				this.grupToDo = 'No Grup'
			},
			CompletedToDo(todo){
				if(todo.completed){
					todo.completed = false;
				} else {
					todo.completed = true;
				}
			},
			RemoveToDo(index){
				this.todos.splice(index, 1)
			},
			AddGrup(){
				this.grups.push({
					id: this.grups.length+1,
					title: this.titleGrup,
				})
				this.titleGrup = ''
			},
			RemoveGrup(index, title){
				this.grups.splice(index, 1)
				for(let i = 0; i < this.todos.length; i++){
					if (this.todos[i].grup == title) {
						this.RemoveToDo(i);
					}
				}
			}
		}
	}
</script>

<style lang="scss">
.todo-search {
	width: 100%;
	height: 48px;
	background: #FAFAFA;
	box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.24), 0px 0px 2px rgba(0, 0, 0, 0.12);
	border-radius: 2px;
	padding: 16px 12px;
	margin-bottom: 20px;
	font-size: 18px;
	line-height: 16px;
	&:focus {
		outline: 0;
	}
	&::placeholder{
		color: rgba(0, 0, 0, 0.5438);
	}
}

.todo-buttons{
	display: flex;
	justify-content: space-between;	
	flex-wrap: wrap;

	@media screen and (max-width: 800px) {
		justify-content: center;
	}
}

.todo-button{
	background: #009688;
	box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.24), 0px 0px 2px rgba(0, 0, 0, 0.12);
	border-radius: 10px;
	color: #FFFFFF;
	width: 200px;
	height: 55px;
	margin-bottom: 20px;

	&:hover {
		background: linear-gradient(0deg, rgba(189, 189, 189, 0.3), rgba(189, 189, 189, 0.3)), #009688;
		box-shadow: 0px 8px 8px rgba(0, 0, 0, 0.24), 0px 0px 8px rgba(0, 0, 0, 0.12);
	}
}

.todo-checkbox{
	display: none;
}

.todo-input{
	width: 100%;
	background: #FAFAFA;
	box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.24), 0px 0px 2px rgba(0, 0, 0, 0.12);
	border-radius: 2px;
	margin-bottom: 10px;
	&:focus {
		outline: 0;
	}
	&::placeholder{
		color: rgba(0, 0, 0, 0.5438);
	}
}

.todo-select{
	display: block;
	margin-bottom: 10px;
}

.todo-grups{
	display: flex;
	justify-content: space-between;
	flex-wrap: wrap;
	@media screen and (max-width: 800px) {
		justify-content: center;
	}
}

.todo-grup{
	flex: 1 1 auto;
	margin: 0 20px 20px;
	max-width: 380px;
	min-width: 320px;
	background: #FAFAFA;
	box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.24), 0px 0px 2px rgba(0, 0, 0, 0.12);
}

.todo-grup_name{
	display: flex;
	justify-content: space-between;
	padding: 10px;
	background: #3E50B4;
	color: #EEEEEE;
}

.todo-items{
	color: red;
}

.todo-item{
	display: flex;
	justify-content: space-between;
	padding: 18px 10px;
	position: relative;
	overflow: hidden;

	&:before{
		content: "";
		height: 5px;
		background: #3E50B4;
		width: 100%;
		top: 97%;
		left: 0;
		position: absolute;
	}
}

.todo-item_false{
	display: none;
}

.todo-item_text{
	margin: 10px;
	display: flex;
	flex-direction: column;
}

.todo-item_description{
	margin-left: 20px;
	height: 30px;
}

.todo-item_btns{
	display: flex;
	flex-direction: column;
	justify-content: space-between;

}

.todo-win_btns{
	display: flex;
	justify-content: space-between;
}

.todo-item_btn{
	width: 100px;
}

.todo-completed{
	color: green;
}
	
</style>