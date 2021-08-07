<script>
  let todos = [];
  let task = "";
  let error = "";

  const addTodo = () => {
    let todo = {
      task: task,
      isComplete: false,
      createdAt: new Date(),
    };
    if (task !== "") {
      todos = [todo, ...todos];
      error = "";
    } else {
      error = "Task is empty";
    }
    task = "";
  };

  const markTodoAsComplete = (index) => {
    todos[index].isComplete = !todos[index].isComplete;
  };

  const deleteTodo = (index) => {
    let deleteItem = todos[index];
    todos = todos.filter((item) => item != deleteItem);
  };

  const keyIsPressed = (event) => {
    if (event.key === "Enter") {
      addTodo();
    }
  };

  $: console.table(todos);
</script>

<input type="text" placeholder="Add a task" bind:value={task} />
<button on:click={addTodo}>Add</button>

<ol>
  {#each todos as item, index}
    <li class:complete={item.isComplete}>
      <span>
        {item.task}
      </span>
      <span>
        <button on:click={() => markTodoAsComplete(index)}>✔</button>
        <button on:click={() => deleteTodo(index)}>✘</button>
      </span>
    </li>
  {:else}
    <p>No todos found</p>
  {/each}
  <p class="error">{error}</p>
</ol>

<svelte:window on:keydown={keyIsPressed} />

<style>
  .complete {
    text-decoration: line-through;
  }
  .error {
    color: red;
  }
</style>
