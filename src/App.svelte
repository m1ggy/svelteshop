<script>
  import Items from './components/Items.svelte';
  import Navbar from './components/navbar.svelte';
</script>

<svelte:head>
  <link rel="stylesheet" href="./global.css" />
</svelte:head>
<Navbar />
<main>
  <Items />
</main>

<style>
  main {
    display: flex;
    justify-content: space-evenly;
    align-items: center;
  }
  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
