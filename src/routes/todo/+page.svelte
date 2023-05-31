<h1>Todo</h1>
<script>
	let name = '';
    let tasks = [];


    function addTask() {
        tasks = [{todo:name, done:false},...tasks];
    }

</script>
<body>
    <input bind:value={name} placeholder="What is there Todo...?">
    <button on:click={addTask}>
        Add task
    </button>
    
    <fieldset>
        {#each tasks as task}

            <label><input bind:checked={task.done} style="{task.done ? "color: white" : "color:brown"}" type="checkbox" required />{task.todo }</label>
        {/each}
    </fieldset>
</body>

<style>
button {
    background-color: brown;
    border: 4px solid rgb(166, 97, 97);
}
body {
    width: 100%;
    height: 100vh;
    margin: 0;
    padding: 0;
    background-color: rgb(203, 145, 145);
}
input {
    background-color: rgb(166, 97, 97);
    color: brown;
    border: 4px solid; 
}


</style>