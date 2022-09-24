<script lang="ts">
  import { flip } from "svelte/animate"
  import { fade, scale, slide } from "svelte/transition"
  import type { Poll } from "src/types/Polls"
  import PollDetails from "./PollDetails.svelte"

  export let polls: Poll[] = []
</script>

<div class="poll-list">
  {#each polls as poll (poll.id)}
    <div
      in:fade
      out:scale|local={{ duration: 500 }}
      animate:flip={{ duration: 500 }}
    >
      <PollDetails {poll} on:vote on:delete />
    </div>
  {/each}
</div>

<style>
  .poll-list {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 20px;
  }
</style>
