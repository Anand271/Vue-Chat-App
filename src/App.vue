<template>
  <ChatApp /> 
    <!-- <TodoApp /> -->
</template>

<script>
import ChatApp from './components/ChatApp.vue'
// import TodoApp from './components/TodoApp.vue'

export default {
  name: 'App',
  components: {
    ChatApp,
    // TodoApp
  }
}
</script>

<style>

:root {
  --body-bg: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
  --msger-bg: #fff;
  --border: 2px solid #ddd;
  --left-msg-bg: #ececec;
  --right-msg-bg: #579ffb;
}

html {
  box-sizing: border-box;
}

*,
*:before,
*:after {
  margin: 0;
  padding: 0;
  box-sizing: inherit;
}

body {
  display: flex;
  justify-content: center;
  align-items: center;
  height: auto;
  background-image: var(--body-bg);
  font-family: Helvetica, sans-serif;
  /* overflow: hidden; */
}

</style>
