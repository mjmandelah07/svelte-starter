<script>
  import Crud from './lib/Crud.svelte';

  let name = 'App';
</script>

<h1>CRUD {name} for Foods</h1>
<div>
  <Crud />
</div>

<style>

  h1{
    color: rgb(30, 142, 190);
    text-align: center;
    margin-bottom: 30px;
  }
  
  div {
    display: flex;
    justify-content: center;
  }

  @media screen and (min-width: 767.98px) {
    div {
      margin: 0 auto;
      max-width: 500px;
    }
  }
</style>
