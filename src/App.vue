<template>
  <div id="app">
    <div v-for="(item, index) of images" :key="index" style="border:1px #000 solid;width:30%; margin:auto;">
        {{item.name}}
        <img :src="item.link" alt="" style="width:300px; height:300px;">
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  components: {
  },
  data() {
    return {
      images: ""
    }
  },
  mounted() {
    let parser = new DOMParser();
    let images = [];
    let today = new Date();
    today = today.toUTCString();
    axios.get('https://mystorennbzh.blob.core.windows.net/images?sp=rl&st=2021-03-05T08:36:01Z&se=2021-03-19T16:36:01Z&spr=https&sv=2020-02-10&sr=c&sig=5S4tCseXl9KU%2Fm1Peirl%2FrURw%2FeqyD%2BOOrcr0RTxACk%3D&comp=list&restype=container',{
      headers: {
        'x-ms-date':today,
        'x-ms-version': '2020-06-12'
      }
    }).then(function (response) {
      let arr = parser.parseFromString(response.data, 'text/xml').getElementsByTagName('Blob');
      arr.forEach(element => {
          images.push({
            name: element.firstChild.firstChild.data,
            link: `https://mystorennbzh.blob.core.windows.net/images/${element.firstChild.firstChild.data}?sp=rl&st=2021-03-05T08:36:01Z&se=2021-03-19T16:36:01Z&spr=https&sv=2020-02-10&sr=c&sig=5S4tCseXl9KU%2Fm1Peirl%2FrURw%2FeqyD%2BOOrcr0RTxACk%3D`
          });
      });
    });
      this.images = images;
      console.log(images);
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
