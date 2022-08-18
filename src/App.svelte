<script lang="ts" context="module">
  export interface Todo {
    id: number;
    text: string;
    done: boolean;
  }
</script>

<script lang="ts">
  import TodoList from "./lib/TodoList.svelte";

  let todos: Array<Todo> = [];

  $: done_todos = todos.filter((todo) => todo.done);
  $: todo_todos = todos.filter((todo) => !todo.done);

  let inputValue = "";

  function handleSubmit() {
    if (!inputValue) return;

    todos.push({
      id: Date.now(),
      text: inputValue,
      done: false,
    });

    todos = todos;

    inputValue = "";
  }

  function todoChecked() {
    todos = todos;
  }
</script>

<form on:submit|preventDefault={handleSubmit}>
  <input bind:value={inputValue} />
  <input type="submit" value="Add" />
</form>

<TodoList on:todoChecked={todoChecked} {todos} list={todo_todos} />
<TodoList on:todoChecked={todoChecked} {todos} list={done_todos} isDoneList />
