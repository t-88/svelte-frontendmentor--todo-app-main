<div class="Todo-comp" class:dark-theme={currTheme === "dark"} class:light-theme={currTheme === "light"}>
    <div class="container">
        <Toggele {currTheme} completed={todo.completed} callBack={onComplete}/>
        <p class:completed={todo.completed}>{todo.todo}</p>
    </div>

    <img on:click={onDelete} class="delete" class:phone-delete={windowWidth <= 768} src="../assets/icon-cross.svg" alt="cross">
</div>

<script>
import { createEventDispatcher } from "svelte"
import Toggele from "./Toggle.svelte"

export let todo = {}
export let windowWidth = 0
export let currTheme = "dark"
const dispatch = createEventDispatcher()
const onComplete = () => {
    todo = {...todo , completed: !todo.completed}
    dispatch("completedTodo",todo.id)
} 
const onDelete = () => {
    dispatch("deleteTodo",todo.id)
}
</script>

<style type="text/scss">
@import "../shared";
.dark-theme {
    border-bottom: 1px solid hsl(237, 14%, 26%);
    p {
        color: hsl(234, 39%, 85%);
    }
    .completed {
        color: hsl(233, 14%, 35%);
    }
}
.light-theme {
    border-bottom: 1px solid hsl(236, 33%, 92%);
    p {
        color: hsl(235, 19%, 35%);
    }
    .completed {
        color: hsl(233, 11%, 84%);
        text-decoration-color: #9394a580;
    }
}
.Todo-comp {
    padding: mathFromPcToTablet(20,10) 
             mathFromPcToTablet(30,10);
    display: flex;
    align-items: center;
    cursor: context-menu;
    justify-content: space-between;
    .delete {
        display: none;
        cursor: pointer;
        width: mathFromPcToTablet(18,13);
    }
    .phone-delete {
        display: block;
    }
    &:hover {
        .delete {
            display: inline-block;
        }
    }
    p {
        font-size: mathFromPcToTablet(20,12);
        width: mathFromPcToTablet(540,220);
        overflow-wrap: break-word;
    }
    .container {
        display: flex;
        display: flex;
        align-items: center;
        gap: mathFromPcToTablet(20,10);
    }
    .completed {
        text-decoration: line-through;
    }
}
</style>