<template>
  <header>
    <div class="container">
      <nav class="row align-items-center my-1">
        <div class="col-auto">
          <RouterLink to="/">
            <img src="/img/logo.png" class="logo align-text-top" alt="Vue logo" />
          </RouterLink>
        </div>
        <div class="col">
          <h1 class="f-monofett">TypingCodingWord</h1>
        </div>
      </nav>
    </div>
  </header>
</template>

<script>
</script>


<style lang="scss" scoped>
.logo {
  height: 4em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #41B883);
}
</style>