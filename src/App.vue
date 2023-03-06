<template>
	<div class="main">
		<!-- title -->
		<section id="title">
			<span>What's up,</span>
			<input type="text" placeholder="Name here" v-model="name" />
		</section>
		<!-- to-do input -->
		<section id="input">
			<h4>CREATE A TODO</h4>
			<p>What's on your todo list ?</p>
			<input
				type="text"
				placeholder="e.g. make a video"
				v-model="enteredTask"
			/>
		</section>
		<!-- category -->
		<section id="category">
			<p>Pick a category</p>
			<div class="categories">
				<div class="c c1">
					<input
						type="radio"
						name="type"
						id="business"
						value="business"
						v-model="category"
					/>
					<label for="business">Business</label>
				</div>
				<div class="c c2">
					<input
						type="radio"
						name="type"
						id="personal"
						value="personal"
						v-model="category"
					/>
					<label for="personal">Personal</label>
				</div>
			</div>
		</section>
		<button class="add" @click="addTask">Add todo</button>
		<section id="list">
			<p>Todo List</p>
			<ul>
				<li v-for="(task, index) in tasks" :key="index">
					<div class="todo">
						<span
							:class="{
								c1: task.category == 'business',
								c2: task.category == 'personal',
							}"
							><input type="radio" v-model="task.done"
						/></span>
						<span :class="{ content: task.done == 'on' }">
							{{ task.content }}
						</span>
					</div>
					<span><button @click="deleteTask(index)">Delete</button></span>
				</li>
			</ul>
		</section>
	</div>
</template>

<script>
export default {
	data() {
		return {
			tasks: [],
			enteredTask: '',
			category: null,
			name: '',
		}
	},
	methods: {
		addTask() {
			this.tasks.push({
				content: this.enteredTask,
				category: this.category,
				done: false,
				id: new Date().getTime(),
			})

			this.enteredTask = ''
		},
		deleteTask(index) {
			this.tasks.splice(index, 1)
		},
	},
	mounted() {
		this.name = localStorage.getItem('name')
		this.tasks = JSON.parse(localStorage.getItem('tasks') || [])
	},
	watch: {
		name(newValue) {
			localStorage.setItem('name', newValue)
		},
		tasks: {
			handler: function (newValue) {
				localStorage.setItem('tasks', JSON.stringify(newValue))
			},
			deep: true,
		},
	},
}
</script>

<style scoped>
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
}
body {
	font-family: sans-serif;
	background-color: whitesmoke;
}
.main {
	margin-left: 100px;
}
#title {
	margin: 2rem 0;
}
#title span {
	color: black;
	font-weight: bold;
	font-size: 1.5rem;
	margin-right: 1rem;
}
#title input {
	font-size: 1.3rem;
	border: none;
	outline: none;
	background: transparent;
	font-weight: bold;
}
#input h4 {
	font-weight: 400;
	font-size: 1rem;
}
#input p,
#category p {
	font-size: 0.8rem;
	padding-top: 0.6rem;
}
#input input {
	width: 360px;
	padding: 0.5rem 1rem;
	margin: 1rem 0 1.5rem 0;
	background: white;
	border-radius: 5px;
	outline: none;
	border: 1px solid white;
	box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.5);
}
#category .categories {
	display: flex;
	gap: 1rem;
	margin-top: 0.5rem;
}
#category .c {
	width: 170px;
	height: 70px;
	background: white;
	display: flex;
	align-items: center;
	justify-content: center;
	flex-direction: column;
	row-gap: 0.5rem;
	box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.5);
	cursor: pointer;
}
#category .c1 input,
#category .c2 input {
	position: relative;
	appearance: none;
	width: 20px;
	height: 20px;
	border-radius: 50%;
	border: 1px solid blue;
	cursor: pointer;
	display: inline-block;
}
#category .c1 input:focus {
	background: blue;
	text-decoration: line-through;
}
#category .c2 input {
	border: 1px solid pink;
}
#category .c2 input:focus {
	background: pink;
	text-decoration: line-through;
}

.add {
	width: 360px;
	padding: 0.5rem 1rem;
	margin: 1rem 0 1.5rem 0;
	background: #f724b0;
	cursor: pointer;
	border-radius: 5px;
	outline: none;
	border: 1px solid #f724b0;
	color: white;
	box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.5);
}
#list p {
	text-transform: uppercase;
	margin-bottom: 1rem;
}
#list ul li {
	padding: 1rem;
	background: white;
	box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.5);
	width: 360px;
	display: flex;
	justify-content: space-between;
	align-items: center;
	margin-bottom: 0.7rem;
}
#list ul li button {
	padding: 0.5rem;
	background: #b61b1b;
	color: white;
	border: none;
	border-radius: 3px;
	cursor: pointer;
}
.todo {
	display: flex;
	align-items: center;
	gap: 0.5rem;
}
.todo input {
	position: relative;
	appearance: none;
	width: 20px;
	height: 20px;
	border-radius: 50%;
	border: 1px solid blue;
	cursor: pointer;
	display: inline-block;
}
.todo input:focus {
	background: blue;
}
.c1 input,
.c2 input {
	position: relative;
	appearance: none;
	width: 20px;
	height: 20px;
	border-radius: 50%;
	border: 1px solid blue;
	cursor: pointer;
	display: inline-block;
}
.c1 input:focus {
	background: blue;
}
.c2 input {
	border: 1px solid pink;
}
.c2 input:focus {
	background: pink;
}
.content {
	text-decoration: line-through;
}
</style>
