<script lang="ts">
  import { tweened } from "svelte/motion"
  import { createEventDispatcher } from "svelte"
  import Card from "../shared/Card.svelte"
  import type { Poll } from "../types/Polls"

  const dispatch = createEventDispatcher()
  export let poll: Poll

  $: totalVotes = poll.votesA + poll.votesB
  $: percentA = Math.floor((100 / totalVotes) * poll.votesA) || 0
  $: percentB = Math.floor((100 / totalVotes) * poll.votesB) || 0
  const tweenedA = tweened(0)
  const tweenedB = tweened(0)
  $: tweenedA.set(percentA)
  $: tweenedB.set(percentB)

  const handleVotes = (option, id) => {
    dispatch("vote", { option, id })
  }
  const handleDelete = (id) => {
    dispatch("delete", id)
  }
</script>

<Card>
  <div class="poll">
    <h3>{poll.question}</h3>
    <p>Total Votes: {totalVotes}</p>
    <div class="answer" on:click={() => handleVotes("a", poll.id)}>
      <div class="percent percent-a" style="width: {$tweenedA}%" />
      <span>{poll.answerA} ({poll.votesA})</span>
    </div>
    <div class="answer" on:click={() => handleVotes("b", poll.id)}>
      <div class="percent percent-b" style="width: {$tweenedB}%" />
      <span>{poll.answerB} ({poll.votesB})</span>
    </div>
    <div class="delete">
      <button on:click={() => handleDelete(poll.id)}>Delete</button>
    </div>
  </div>
</Card>

<style>
  h3 {
    margin: 0 auto;
  }
  p {
    margin-top: 6px;
    font-size: 14px;
    color: #aaa;
    margin-bottom: 30px;
  }
  .answer {
    background: #333333;
    cursor: pointer;
    margin: 10px auto;
    position: relative;
  }
  .answer:hover {
    opacity: 0.4;
  }
  span {
    display: inline-block;
    padding: 10px 20px;
  }
  .percent {
    height: 100%;
    position: absolute;
    box-sizing: border-box;
  }
  .percent-a {
    background-color: rgba(217, 128, 27, 0.281);
    border-left: 3px solid #d9801b;
  }
  .percent-b {
    background-color: rgba(87, 217, 27, 0.281);
    border-left: 3px solid rgba(87, 217, 27);
  }
  .delete {
    padding-top: 30px;
    text-align: center;
  }
  .delete button:hover {
    background-color: #d9281b;
  }
</style>
