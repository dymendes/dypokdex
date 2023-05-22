<template>
  <header>
    <div class="logotipo">
      <a href="/">
          <img src="@/assets/logo.png" alt="Logo do site" class="logo">
          <h1>Pokedéx</h1>
      </a>
    </div>

    <form method="get" autocomplete="off">
      <input type="text" name="search" id="search" class="search" placeholder="Procure por um pokémon." v-model="search" v-on:keyup="emit($event)">
      <img src="@/assets/search.png" alt="Ícone de busca">
    </form>
  </header>
</template>


<script>
export default (await import("vue")).defineComponent({
  name: "Header",
  data() {
    return {
      search: ""
    }
  },
  methods: {
    emit($event) {
      $event.preventDefault()
      console.log("enviado")
      this.$emit("search", { search: this.search })
    }
  }
})
</script>

<style scoped>
  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 5px 15px;
    border-bottom: 2px solid #ef4036;
  }

  header .logotipo a {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  header .logo {
    width: 80px;
    margin-right: 15px;
  }

  header form {
    position: relative;
  }

  header form .search {
    width: 225px;
    padding: 10px;
    outline: none;
    border: 2px solid;
    border-radius: 5px;
    color: #ef4036;
  }

  header form img {
    position: absolute;
    top: 4px;
    right: 5px;
  }
</style>