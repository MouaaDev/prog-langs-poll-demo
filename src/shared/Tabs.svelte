<script lang="ts">
  import { createEventDispatcher } from "svelte"

  const dispatch = createEventDispatcher()

  export let items: string[]
  export let activeItem: string
</script>

<div class="tabs">
  <ul>
    {#each items as item (item)}
      <li on:click={() => dispatch("tabChange", item)}>
        <div class:active={item === activeItem}>{item}</div>
      </li>
    {/each}
  </ul>
</div>

<style>
  .tabs {
    margin-bottom: 40px;
  }
  ul {
    display: flex;
    justify-content: center;
    padding: 0;
    list-style: none;
  }
  li {
    margin: 0 16px;
    font-size: 1.4rem;
    /* color: wheat; */
    cursor: pointer;
  }
  .active {
    color: #d9801b;
    border-bottom: 2px solid #d9801b;
    padding-bottom: 8px;
    font-weight: bold;
  }
</style>
