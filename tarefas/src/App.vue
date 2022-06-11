<template>
	<div id="app">
		<h1>Tarefas</h1>
		<new-task v-on:taskAdded="addTask" />
		<task-grid v-bind:tasks="tasks" />
	</div>
</template>

<script>
import TaskGrid from './components/TaskGrid.vue'
import NewTask from './components/NewTask.vue'
import barramento from './barramento'
export default {
	components:{TaskGrid, NewTask},
	data(){
		return{
			tasks:[
				{name:'lavar roupa', pending:true},
				{name:'fazer almoço', pending:false}
			]
		}
	},
	methods:{
		addTask(task){

			const sameName = t => t.name === task.name
			const reallyNew = this.tasks.filter(sameName).length == 0
			if(reallyNew){
				this.tasks.push( {
					name: task.name,
					pending: task.pending || true
				} )
			}
			
		}
	},
	created(){
		barramento.$on('deleteTask', task =>{
			const i = this.tasks.indexOf(task)
			this.tasks.splice(i, 1)
		} )
	}

}
</script>

<style>
	body {
		font-family: 'Lato', sans-serif;
		background: linear-gradient(to right, rgb(22, 34, 42), rgb(58, 96, 115));
		color: #FFF;
	}

	#app {
		display: flex;
		flex: 1;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 100%;
		
	}

	#app h1 {
		margin-bottom: 5px;
		font-weight: 300;
		font-size: 3rem;
	}
</style>
