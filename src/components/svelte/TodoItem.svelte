<script lang="ts">
  import { useCloud } from "freestyle-sh";
  import { TheTodoList, TodoItem } from "../../cloudstate/todo-list";

  export let item: { id: string; text: string; completed: boolean };
  export let removeTodo: () => void;

  const todoItem = useCloud<typeof TodoItem>(item.id);
  const todoList = useCloud<typeof TheTodoList>("todo-list");
</script>

<div>
  <br />
  <input
    type="checkbox"
    bind:checked={item.completed}
    on:change={() => {
      todoItem.toggleCompletion().then((result) => {
        item.completed = result.completed;
      });
    }}
  />
  <span>{item.text}</span> &nbsp; &nbsp;
  <button class="rounded-md bg-slate-700 p-1.5"
    on:click={() => {
    todoList.removeItem(item.id);
    removeTodo();
  }}
  >
    Remove
  </button>
</div>
