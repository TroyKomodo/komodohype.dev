<template>
  <div class="error">
    <div>
      <h2>404 The page you were looking for was not found...</h2>
      <h3>Go back <nuxt-link to="/">home</nuxt-link></h3>
    </div>
  </div>
</template>

<style scoped lang="scss">
.error {
  display: flex;
  flex-direction: column;
  width: 100%;
  flex-grow: 1;
  text-align: center;
  font-size: 1.5rem;
  > div {
    margin: auto;
  }
}

a {
  color: #55e43c;
  text-decoration: none;
  transition: color 200ms ease;
  font-weight: 400;
  &:hover {
    color: #22a30b;
  }
}
</style>
