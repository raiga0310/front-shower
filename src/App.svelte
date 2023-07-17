<script>
    import Header from './lib/Header.svelte';
    import Footer from './lib/Footer.svelte';
    import ShowerList from './lib/ShowerList.svelte';
</script>

<main>
  <Header />
  <ShowerList />
  <Footer />
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 90vw;
    max-height: 80vh;
    margin: 0 auto;
  }
</style>
