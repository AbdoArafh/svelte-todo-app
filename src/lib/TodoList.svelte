<script lang="ts">
  // animations
  import { fly, slide } from "svelte/transition";
  import { flip } from "svelte/animate";
  import type { CrossfadeParams, TransitionConfig } from "svelte/transition";

  // functions
  import { createEventDispatcher } from "svelte";

  // components
  import type { Todo } from "../App.svelte";

  export let list: Array<Todo> = [];
  export let todos: Array<Todo> = [];
  export let isDoneList = false;
  $: list = todos.filter((todo) => todo.done === isDoneList);

  const dispatch = createEventDispatcher();

  type fallback = (
    node: Element,
    params: CrossfadeParams & { key: any }
  ) => () => TransitionConfig;

  export let send: fallback;
  export let receive: fallback;

  function handleChecked(todo: Todo) {
    todo.done = !isDoneList;
    dispatch("todoChecked");
  }
</script>

{#if list.length}
  <div out:slide>
    <h2 transition:fly={{ x: 100 }}>{isDoneList ? "Done" : "Todo"}</h2>
    <ul class="todos" class:done={isDoneList}>
      {#each list as todo (todo.id)}
        <li
          class="item"
          in:receive={{ key: todo.id }}
          out:send={{ key: todo.id }}
          animate:flip
        >
          <label>
            <input
              type="checkbox"
              on:input={() => handleChecked(todo)}
              checked={isDoneList}
            />
            <span class="text">{todo.text}</span>
          </label>
        </li>
      {/each}
    </ul>
  </div>
{/if}
