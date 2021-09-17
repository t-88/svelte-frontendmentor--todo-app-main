<div id="app" > 
	{#if currTheme === "dark"}
		{#if currDevice === "pc"} 
			<img class="bg-desktop" src="./assets/bg-desktop-dark.jpg" alt="bg-desktop-dark">
		{:else if currDevice === "phone"} 
			<img class="bg-mobile" src="./assets/bg-mobile-dark.jpg" alt="bg-mobile-dark">
		{/if}
	{/if}
	{#if currTheme === "light"}
		{#if currDevice === "pc"} 
			<img class="bg-desktop" src="./assets/bg-desktop-light.jpg" alt="bg-desktop-light">
		{:else if currDevice === "phone"} 
			<img class="bg-mobile" src="./assets/bg-mobile-light.jpg" alt="bg-mobile-light">
		{/if}
	{/if}	

	
	<div class="header">
		<h1 class="title">TODO</h1>
		<div class="theme-changer" on:click={() => { currTheme =  currTheme === "dark" ? "light" : "dark"}}>
			{#if currTheme === "dark"} 
				<svg class="icon-sun" xmlns="http://www.w3.org/2000/svg" width="26" height="26"><path fill="#FFF" fill-rule="evenodd" d="M13 21a1 1 0 011 1v3a1 1 0 11-2 0v-3a1 1 0 011-1zm-5.657-2.343a1 1 0 010 1.414l-2.121 2.121a1 1 0 01-1.414-1.414l2.12-2.121a1 1 0 011.415 0zm12.728 0l2.121 2.121a1 1 0 01-1.414 1.414l-2.121-2.12a1 1 0 011.414-1.415zM13 8a5 5 0 110 10 5 5 0 010-10zm12 4a1 1 0 110 2h-3a1 1 0 110-2h3zM4 12a1 1 0 110 2H1a1 1 0 110-2h3zm18.192-8.192a1 1 0 010 1.414l-2.12 2.121a1 1 0 01-1.415-1.414l2.121-2.121a1 1 0 011.414 0zm-16.97 0l2.121 2.12A1 1 0 015.93 7.344L3.808 5.222a1 1 0 011.414-1.414zM13 0a1 1 0 011 1v3a1 1 0 11-2 0V1a1 1 0 011-1z"/></svg>
			{:else if currTheme === "light"} 
				<svg xmlns="http://www.w3.org/2000/svg" width="26" height="26"><path fill="#FFF" fill-rule="evenodd" d="M13 0c.81 0 1.603.074 2.373.216C10.593 1.199 7 5.43 7 10.5 7 16.299 11.701 21 17.5 21c2.996 0 5.7-1.255 7.613-3.268C23.22 22.572 18.51 26 13 26 5.82 26 0 20.18 0 13S5.82 0 13 0z"/></svg>
			{/if}
		</div>
	</div>
	
	<CreateTodo  {currTheme}  on:submitTodo={({detail}) => {todoList = [...todoList,detail]}}/>
	<TodoContainer on:completedTodo={completeTodo} 
				   on:selectFilter={({detail}) => {applyedFilter = detail}} 
				   on:deleteTodo={deleteTodo}
				   on:deleteCompleted={() => {todoList = [...todoList.filter( todo => !todo.completed )]}}
				   {todoLeft} {filteredTodoList} {currDevice} {currTheme}/>
	<p class="drag-drop-text">Drag and drop to reorder list</p>
	
</div>
<svelte:window bind:innerWidth={windowWidth} />

<script>
import CreateTodo from "./components/CreateTodo.svelte";
import TodoContainer from "./components/TodoContainer.svelte";

let windowWidth = 0
let currDevice = null
let deviceList = { phone: 500 }

$: {
	windowWidth
	if(windowWidth > deviceList.phone) {
		currDevice = "pc"
	} else {
		currDevice = "phone"
	}
}

let todoList = []
let todoLeft = 0
let applyedFilter = "All"

$: filteredTodoList = filterTodos(applyedFilter,todoList)
const filterTodos = (filter,todoList) => {
	if(filter === "All") { return todoList}
	else if(filter === "Completed") { return todoList.filter( todo => todo.completed )}
	else if(filter === "Active") { return todoList.filter( todo => !todo.completed )} 
}

const completeTodo = ({detail}) => {
	let index = todoList.findIndex(todo  => todo.id === detail)
	todoList[index].completed = !todoList[index].completed
}
const deleteTodo = ({detail}) => {
	let index = todoList.findIndex(todo  => todo.id === detail)
	todoList.splice(index,1)
	todoList = [...todoList]
}

let todoListLoaded = false
$: {
	todoList // call this block of code whenever todoList changes
	const updatedOther = () => {
		todoLeft = todoList.filter( todo => !todo.completed ).length
	}
	if(!todoListLoaded) { // its like on mount function is called to get saved todos
		let loadedTodos =  JSON.parse(localStorage.getItem("todoList"))
		if(!loadedTodos) {
			todoList = []
		} else {
			todoList = loadedTodos
		} 
		todoListLoaded = true	
		updatedOther()
	} else { // its like an update function 
		localStorage.setItem("todoList",JSON.stringify(todoList))	
		updatedOther()
	}
}

let currTheme = "dark" 
let initTheme = false
$: {
	currTheme
	if(!initTheme) {
		initTheme = true
		let prevTheme = localStorage.getItem("theme")
		if(prevTheme) {
			currTheme = prevTheme
		}
	}

	if(currTheme === "dark") {
		document.querySelector("html").style.background = "hsl(235, 21%, 11%)"
	} else if (currTheme === "light") {
		document.querySelector("html").style.background = "hsl(0, 0%, 98%)"
	}
	localStorage.setItem("theme",currTheme)
}
</script>

<style type="text/scss">
@import "../shared";
#app {
	position: relative;
	display: flex;
	flex-direction: column;
	align-items: center;

	padding-top: mathFromPcToTablet(100,50);
	gap: mathFromPcToTablet(60,30);
	img {
		position: absolute;
		width: 100%;
		top: 0;
		left: 0;
	}
	.bg-desktop {
		height: applyMath(360,200,1823,500);
	}
	.bg-mobile{
		height: applyMath(200,180,500,320);
	}
	.header {
		position: relative;
		display: flex;
		align-items: center;
		justify-content: center;
		gap: mathFromPcToTablet(450,130);
		.title {
    		letter-spacing: 10px;
			height: mathFromPcToTablet(40,25);
			font-size: mathFromPcToTablet(55,35);
			color: white;
		}
	}
	:global(.CreateTodo-comp) {
		position: relative;
	}
	:global(.TodoContainer-comp) {
		position: relative;
	}
	.drag-drop-text {
		color: hsl(234, 11%, 52%); 
		transform: translateY(-30px);
		font-size: mathFromPcToTablet(16,12);
	}
	.theme-changer {
		cursor: pointer;
	}
}
</style>
