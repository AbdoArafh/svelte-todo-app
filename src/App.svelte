<script lang="ts" context="module">
  export interface Todo {
    id: number;
    text: string;
    done: boolean;
  }
</script>

<script lang="ts">
  import TodoList from "./lib/TodoList.svelte";
  import { crossfade } from "svelte/transition";
  import { quintOut } from "svelte/easing";

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

  const [send, receive] = crossfade({
    duration: (d) => Math.sqrt(d * 200),

    fallback(node, _params) {
      const style = getComputedStyle(node);
      const transform = style.transform === "none" ? "" : style.transform;

      return {
        duration: 600,
        easing: quintOut,
        css: (t) => `
        transform: ${transform} scale(${t});
        opacity: ${t}
      `,
      };
    },
  });
</script>

<form on:submit|preventDefault={handleSubmit}>
  <input bind:value={inputValue} />
  <input type="submit" value="Add" />
</form>

<TodoList
  {send}
  {receive}
  on:todoChecked={todoChecked}
  {todos}
  list={todo_todos}
/>
<TodoList
  {send}
  {receive}
  on:todoChecked={todoChecked}
  {todos}
  list={done_todos}
  isDoneList
/>
