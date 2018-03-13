<template>
  <div class="form">
    <label>Author</label>
    <input v-model="author" type="text">
    <label>Text</label>
    <input v-model="text" type="text">
    <label>Title</label>
    <input v-model="title" type="text">
    <label>Upload image</label>
    <input class="file" type="file" id="files" name="files" multiple>
    <button @click="sendPost()" type="submit">Send</button> 
    <ul class="post__list">
      <li class="post__item" v-for="item in list">
        <img class="img" :src = "'https://tzf.herokuapp.com/' + item.image" alt="">
          <div class="inform">
            <h1 class="title">{{item.title}}</h1>
            <p class="text">{{item.text}}</p>                
            <p class="created">{{item.created}}</p>
          </div>
      </li>
    </ul>
  </div>
</template>

<script>
let moment = require('moment');
export default {
  name: 'PostSend',
  data () {
    return {
      author: '',
      text: '',
      title: '',
      list: []
    }
  },
  mounted() {
    this.getPostsList()
  },
  
  methods: {
    parseBody(body) {
      const formData = new FormData();
      for (var key in body) {
      if (body.hasOwnProperty(key)) {
        var element = body[key];
        if(element) {
          formData.append(key, element);
          }
        }
      }        
      return formData;
    },
    getPostsList() {
      this.$http.get('https://tzf.herokuapp.com/news')
      
    .then(response => {
      console.log(response);
          return response.body;
    })

    .then(body => {
      body.news.forEach(element => {
        element.created = moment(element.created).format('MMMM Do YYYY, h🇲🇲ss a')
      });
        this.list = body.news;
      })
    },

    sendPost() {
      let newData = {
        author:this.author,
        text:this.text,
        title: this.title,
        image: document.querySelector('#files').files[0]
      };
      console.log(newData);
      this.$http.post('https://tzf.herokuapp.com/create-news', this.parseBody(newData))
      .then(response => {
        response.json();
        console.log(response);
      })      
    },
  }
}
</script>

<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

label {
  font-size: 1.5rem;
}

input {
  margin: 15px;
  padding: 10px;
}

button {
  margin-top: 10px;
  padding: 10px;
}

.form {
  background: #adf;
  width: 800px;          
  display: flex;
  flex-direction: column;
  margin: 0 auto;
  padding: 15px;
}

.file {
  margin: 0 auto;
} 

.post__list {
  list-style: none;
  margin: 0 auto;
}

.post__item{
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 500px;
  margin: 10px 0;
  padding-top: 50px;
  background: #fff;
  width: 800px;
}

.img {
  border-radius: 50%;
  width: 150px;
  height: 150px;
  top: -25px;
  left: 20px;
  position: relative;
}

.inform {                    
  width: 330px;
  margin-left: 20px;
}

.title{
  text-align: left;
}

.text {
  text-align: left;
}

.created {
  text-align: right;
  position: relative;
  right: 20px;

}


</style>
