<div class="TodoContainer-comp" class:dark-theme={currTheme === "dark"} class:light-theme={currTheme === "light"}>
    <div class="container">
        <div class="todo-list">
            {#if filteredTodoList.length == 0}
                <p class="no-todos"> YOU HAVE NO TODOS </p>
            {:else}
                {#each filteredTodoList as todo}
                    <div class="draggable" draggable="true" on:drag={onDrag}>
                        <Todo on:completedTodo = { ({detail}) => {dispatch("completedTodo",detail)} } 
                            on:deleteTodo = { ({detail}) => { dispatch("deleteTodo",detail)} }
                            {todo} {windowWidth} {currTheme}/>
                    </div>
                {/each} 
            {/if}
        </div>
        <div class="footer">
            <p class="num-of-items">
                {todoLeft != 0 ? todoLeft : ''} {todoLeft > 1 ? 'items' : (todoLeft == 0 ? 'no items' : 'item')} left
            </p>
            {#if currDevice === "pc"}
                <div class="filters">
                    {#each ["All","Active","Completed"] as filter}
                        <p class="filter" class:selected={currFilter == filter} on:click={() => {selectFilter(filter)}}>
                            {filter}
                        </p>
                    {/each}
                </div>
            {/if}
            <p class="clear-compelte" on:click={()=> {dispatch("deleteCompleted")}}>
                Clear Completed
            </p>
        </div>
    </div>
    {#if currDevice === "phone"}
        <div class="filters mobile-filters">
            {#each ["All","Active","Completed"] as filter}
                <p class="filter" class:selected={currFilter == filter} on:click={() => {selectFilter(filter)}}>
                    {filter}
                </p>
            {/each}
        </div>
    {/if}
</div>
<svelte:window bind:innerWidth={windowWidth} />
<script>
import { createEventDispatcher } from "svelte";
import Todo from "./Todo.svelte"

const dispatch = createEventDispatcher()
export let currDevice = "pc"
export let filteredTodoList = []
export let todoLeft = 0 
export let currTheme = "dark"
let windowWidth = 0

let currFilter = "All"
const selectFilter = (filter) => {
    if(currFilter == filter) { return }
    currFilter = filter
    dispatch("selectFilter",currFilter)
}
const onDrag = (e) => {
    
}
</script>

<style type="text/scss">
@import "../shared";
.dark-theme {
    .container {
        background: hsl(235, 24%, 19%);
        box-shadow: #00000080 0 25px 30px -10px;
    }
    .no-todos {
        color: white;
        border-bottom: 1px solid hsl(237, 14%, 26%);
    }
    .footer {
        color:  hsl(234, 11%, 52%); 
    }
    .mobile-filters {
        background: hsl(235, 24%, 19%);
        box-shadow: #00000080 0 25px 30px -10px;
    }
}
.light-theme {
    .container {
        background: white;
        box-shadow: #0000000d 0 15px 20px -10px;
    }
    .no-todos {
        color: black;
        border-bottom: 1px solid hsl(236, 33%, 92%);
    }
    .footer {
        color: hsl(236, 9%, 61%); 
    }
    .mobile-filters {
        background: white;
        box-shadow: #00000026 0 0px 6px -1px;
    }
}
.TodoContainer-comp {
    transform: translateY(-23px);
    width: mathFromPcToTablet(682,300);
    .filters {
        display: flex;
        gap: applyMath(25,10,1823,500);
        font-weight: 700;
        .filter {
            color:  hsl(234, 11%, 52%); 
            &:hover {
                cursor: pointer;
                color:  hsl(236, 33%, 92%);
            }
        }
        .selected {
            color: hsl(220, 98%, 61%);
            &:hover {
                color: hsl(220, 98%, 61%);
            }
        }
    }
    .container {
        height: fit-content;
        border-radius: 5px;
        .no-todos {
            text-align: center;
            opacity: 0.8;
            padding: 20px;
        }
        .footer {
            display: flex;
            justify-content: space-between;
            padding: mathFromPcToTablet(20,10);
            font-size: mathFromPcToTablet(16,13);
            .num-of-items {
                cursor: context-menu;
            }
            .clear-compelte:hover {
                cursor: pointer;
                color:  hsl(236, 33%, 92%);
            }
        }
    }
    .mobile-filters {
        transform: translateY(10px);
        border-radius: 5px;

        padding: 10px;
        gap: 15px;
        justify-content: center;
    }

    

}
</style>