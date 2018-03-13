<template>
  <div class="form">
    <label>Author</label>
    <input v-model="author" type="text">
    <label>Text</label>
    <input v-model="text" type="text">
    <label>Title</label>
    <input v-model="Title" type="text">
    <label>Upload image</label>
    <input class="file" type="file" id="files" name="files" multiple>
    <button @click="sendPost()" type="submit">Send</button>
  </div>
</template>

<script>
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
          console.log(body);
          this.list = body;
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
  background: #42b983;
  width: 400px;          
  display: flex;
  flex-direction: column;
  margin: 0 auto;
  padding: 15px;
}

.file {
  margin: 0 auto;
} 


</style>
