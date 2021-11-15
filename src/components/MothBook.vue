<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.app{
    margin-top:60px;
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
}
img{
    height:auto;
    width:auto;
    max-height:200px;
    max-width:300px;
}
</style>

<template>
    <div class="app">
        <w-card v-for="o in book_data" :key="o.id">
            <img :src="require('@/assets/book_data/medium/'+o.img)" alt="">
        </w-card>
    </div>
</template>

<script>
import book_data from '../assets/book_data/book_data.json'
import taxonomy_data from '../assets/book_data/taxonomy_data.json'
import user_data from '../assets/book_data/user_data.json'

export default {
  name: 'MothBook',
  data() {
      return {
          book_data: book_data,
          user_data: user_data,
          taxonomy_data: taxonomy_data,
      }
  },
  created() {
  },
  methods: {
      img_url(img, size){
          let op = "@/assets/book_data/"
          if(size == 'm'){
              op += "medium/"
          } else if(size == 'l'){
              op += "large/"
          }
          op += img
          return op
      }
  }
}
</script>

