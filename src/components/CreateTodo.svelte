<div class="CreateTodo-comp" class:dark-theme={currTheme === "dark"} class:light-theme={currTheme === "light"}>
    <Toggle {currTheme} {completed} callBack={() => { completed = !completed }}/>
    <input on:keyup={onSubmit} bind:value={todo}  placeholder="Create a new todo...">
</div>
<script>
import {_} from "keygenerator"
import { createEventDispatcher } from "svelte";
import Toggle from "./Toggle.svelte"

let completed = false
let todo = ""
const dispatch = createEventDispatcher()
const onSubmit = e => {
    if(e.key != "Enter" || !todo) { return }
    dispatch("submitTodo",{todo,completed,id:_()})
    todo = ""
    completed = false
}

export let currTheme = "dark" 

</script>

<style type="text/scss">
@import "../shared";
.dark-theme {
    background-color: hsl(235, 24%, 19%);
    ::placeholder {
        color: hsl(234, 39%, 85%);
    }
    input {
        color: hsl(236, 33%, 92%);
    }
}
.light-theme {
    background-color: white; 
    ::placeholder {
        color: hsl(236, 9%, 61%);
    }
    input {
        color: hsl(235, 19%, 35%);
    }
}
.CreateTodo-comp {
    display: flex;
    border-radius: 5px;

    width: mathFromPcToTablet(682,300);
    height: mathFromPcToTablet(70,45);
    padding: 0 mathFromPcToTablet(30,10);
    gap: mathFromPcToTablet(20,10);
    ::placeholder {
        opacity: 0.5;
    }
    input {
        outline: none;
        border: none;
        background-color: inherit;
        width: 100%;
        height: 100%;
        caret-color: hsl(220, 98%, 61%);

        font-size: mathFromPcToTablet(19,12);
    }
}



</style>