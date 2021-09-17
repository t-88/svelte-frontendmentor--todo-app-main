<div class="Toggle-comp" class:dark-theme={currTheme === "dark"} class:light-theme={currTheme === "light"}>
    <div class="circle" class:completed={completed} on:click={onClick}>
        <div class="inside-circle"></div>
        {#if completed}
            <img src="../assets/icon-check.svg" alt="">
        {/if}
    </div>
</div>

<script>
export let currTheme = "dark" 
export let completed = false
export let callBack = () => {}
const onClick = () => {
    completed = !completed
    callBack()
}
</script>

<style type="text/scss">
@import "../shared";
.dark-theme {
    .circle {
        border: 2px solid #cacde81a;
        &:hover {
            .inside-circle {
                background: hsl(235, 24%, 19%);
            }
        }
    }
}
.light-theme {
    .circle {
        border: 2px solid #cacde880;
        &:hover {
            .inside-circle {
                background: white;
            }
        }
    }

}
.Toggle-comp {

    display: flex;
    align-items: center;
    justify-content: center;

    width: 30px;
    .circle {
        background-color: transparent;
        border-radius: 100%;
        $size: mathFromPcToTablet(25,20);
        width: $size;
        height: $size;
        cursor: pointer;
        display: flex;
        align-items: center;
        justify-content: center;
        &:hover {
            border: none;
            background: linear-gradient(130deg,hsl(192, 100%, 67%) , hsl(280, 87%, 65%));
            .inside-circle {
                width: applyMath(21,16,1823,501);
                height: applyMath(21,17,1823,501);
                border-radius: 100%;
            }
        }
        &.completed {
            border: none;
            background: linear-gradient(130deg,hsl(192, 100%, 67%) , hsl(280, 87%, 65%));
            .inside-circle { display: none; }
            img {
                width: 16px;
            }
        }
    }
        
}
</style>