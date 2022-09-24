<script lang="ts">
  import { nanoid } from "nanoid"
  import Footer from "./components/Footer.svelte"
  import Header from "./components/Header.svelte"
  import NewPollForm from "./components/NewPollForm.svelte"
  import PollList from "./components/PollList.svelte"
  import Tabs from "./shared/Tabs.svelte"
  import type { Poll } from "./types/Polls"

  // Tabs
  let items = ["Current Polls", "Add New Poll"]
  let activeItem = "Current Polls"

  const tabChange = (e: { detail: string }) => {
    activeItem = e.detail
  }

  // Polls
  let polls: Poll[] = [
    {
      id: nanoid(),
      question: "R or Python?",
      answerA: "R",
      answerB: "Python",
      votesA: 13,
      votesB: 24,
    },
  ]

  const handleAddPoll = (e: { detail: Poll }) => {
    const newPoll = e.detail
    polls = [newPoll, ...polls]

    console.log(polls)
    activeItem = "Current Polls"
  }

  const handleVote = (e: { detail: { id: string; option: string } }) => {
    const { id, option } = e.detail

    // copy polls array
    let copiedPolls = [...polls]
    let upvotedPoll = copiedPolls.find((poll) => poll.id === id)

    if (option === "a") upvotedPoll.votesA++
    if (option === "b") upvotedPoll.votesB++

    polls = copiedPolls
  }

  const handleDelete = (e: { detail: string }) => {
    const id = e.detail
    polls = polls.filter((poll) => poll.id !== id)
  }
</script>

<Header />
<main>
  <Tabs {items} {activeItem} on:tabChange={tabChange} />
  {#if activeItem === "Current Polls" && polls.length === 0}
    <p>No poll has been added yet.</p>
  {:else if activeItem === "Current Polls"}
    <PollList {polls} on:vote={handleVote} on:delete={handleDelete} />
  {:else if activeItem === "Add New Poll"}
    <NewPollForm on:addPoll={handleAddPoll} />
  {/if}
</main>
<Footer />

<style>
  main {
    max-width: 960px;
    margin: 40px auto;
  }
  main p {
    text-align: center;
    font-size: 1.2rem;
    font-weight: bold;
    color: #505050;
  }
</style>
