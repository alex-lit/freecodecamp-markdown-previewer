<script lang="ts">
  import marked from 'marked';
  import { Component, Vue } from 'vue-property-decorator';

  @Component<App>({})
  export default class App extends Vue {
    input = '';

    markup = '';

    setMarkup() {
      this.markup = marked(this.input, { breaks: true });
    }

    created() {
      fetch('/content.md')
        .then(response => response.text())
        .then(text => {
          this.input = text;
          this.setMarkup();
        });
    }
  }
</script>

<template>
  <main class="app">
    <textarea id="editor" v-model="input" class="app__textarea" @keyup="setMarkup"> </textarea>
    <article id="preview" class="app__markup typography" v-html="markup"></article>
  </main>
</template>

<style lang="postcss" scoped>
  .app {
    display: grid;
    min-width: 800px;
    height: 100vh;
    align-items: center;
    justify-content: center;
    grid-template-columns: 1fr 1fr;
  }

  .app__textarea {
    overflow: auto;
    height: 100vh;
    padding: 40px;
    border: none;
    background-color: var(--gray--100);
    box-shadow: inset -1px 0 0 var(--gray--200);
    font-family: var(--font-family--monospace);
    resize: none;
  }

  .app__markup {
    overflow: auto;
    height: 100vh;
    padding: 40px;
  }
</style>
