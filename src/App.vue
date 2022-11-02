<script setup>
import InvitesTable from './components/InvitesTable.vue';
import Numbers from './components/Numbers.vue';
import Edit from './components/Edit.vue';

</script>

<template>
  <main>
    <header>
      <h1>Elliott & Danielle's Wedding Data</h1>
    </header>
    <ul>
      <li><a href="/">Home</a></li>
      <li><a href="#numbers">Numbers</a></li>
      <li><a href="#Edit">Edit</a></li>

    </ul>

    <body>
      <div>
        <!-- <InvitesTable /> -->
       <!-- <Numbers/> -->
       <Edit />
      </div>
    </body>
  </main>
</template>

<style scoped>
body {
  margin-top: 20px;

}

header {
  line-height: 1.5;
}

ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
  overflow: hidden;
  background-color: #333;
}

li {
  float: left;
}

li a {
  display: block;
  color: white;
  text-align: center;
  padding: 14px 16px;
  text-decoration: none;
}

/* Change the link color to #111 (black) on hover */
li a:hover {
  background-color: #111;
}

table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #dddddd;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
