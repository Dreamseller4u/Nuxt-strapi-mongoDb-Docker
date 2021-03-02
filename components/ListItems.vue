<template>
    <div class="list">
        <h1>List items</h1>
        <div v-if="error">
        {{ error }}
        </div>
        <ul v-else>
            <li v-for="item in items" :key="item.id">
                {{ item.name }}
            </li>
        </ul>
    </div>
</template>
<script>
export default {
     name: 'App',
  data () {
    return {
      items: [],
      error: null,
      headers: {'Content-Type': 'application/json'}
    }
  },
  methods: {
    parseJSON: function (resp) {
      return (resp.json ? resp.json() : resp);
    },
    checkStatus: function (resp) {
      if (resp.status >= 200 && resp.status < 300) {
        return resp;
      }
      return this.parseJSON(resp).then((resp) => {
        throw resp;
      });
    }
  },
  async mounted () {
    try {
      const response = await fetch("http://localhost:1337/items", {
        method: 'GET',
        headers: this.headers,
      }).then(this.checkStatus)
        .then(this.parseJSON);
        this.items = response
    } catch (error) {
      this.error = error
    }
  }
}
</script>
<style >
.list ul {
  padding: 0px;
}
.list ul li {
    list-style:none;
    margin: 10px;
    padding: 0px;
}
</style>