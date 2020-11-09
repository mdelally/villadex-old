<template>
  <div class="container mx-auto px-8">
    <slot />
  </div>
</template>

<static-query>
query {
  metadata {
    siteName
  }
}
</static-query>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Coustard:wght@400&family=Nunito:wght@200;400;700;900&display=swap");

body {
  font-family: "Nunito", -apple-system, system-ui, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  margin: 0;
  padding: 0;
  line-height: 1.5;
  background-color: #fffff0;
}

h1,
h2,
h3,
h4 {
  font-family: "Coustard", serif;
}

a {
  text-decoration: underline;
  font-weight: 700;
  transition: all 0.2s ease-in-out;

  &:hover {
    opacity: 0.8;
  }
}
</style>
