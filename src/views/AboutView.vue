<template>
  <div class="about">
    <h1>About This App</h1>
    <p>This is a simple to-do application built with <a href="https://vuejs.org/">Vue.js</a></p>

    <section>
      <h2>Libraries Used</h2>
      <ul>
        <li><strong>Iconify</strong>: for inserting various icons</li>
        <li><strong>Sass</strong>: a preprocessor scripting language compiled into CSS</li>
        <li><strong>UID</strong>: library for generating unique IDs</li>
      </ul>
    </section>

    <section>
      <h2>Features</h2>
      <ul>
        <li>Add as many new tasks as you need</li>
        <li>Update or delete tasks</li>
        <li>Mark them off as you complete</li>
        <li>Save them for next time using local storage</li>
      </ul>
    </section>
  </div>
</template>

<script>
export default {
  name: 'About'
}
</script>

<style scoped>
.about {
  padding: 20px;
  max-width: 800px;
  margin: 0 auto;
  font-family: Arial, sans-serif;
  line-height: 1.6;
  color: #333;
}

.about h1 {
  font-size: 2.5em;
  margin-bottom: 0.5em;
  color: #2c3e50;
}

.about h2 {
  font-size: 1.8em;
  margin-top: 1.5em;
  margin-bottom: 0.5em;
  color: #34495e;
}

.about p {
  font-size: 1.2em;
  margin-bottom: 1em;
}

.about ul {
  list-style-type: disc;
  margin-left: 20px;
}

.about a {
  color: #3498db;
  text-decoration: none;
}

.about a:hover {
  text-decoration: underline;
}
</style>
