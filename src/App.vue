<template>
  <div id="app">
    <div class="jumbotron jumbotron-fluid">
      <div class="container">
        <h1 class="display-4">Vue + contentful</h1>
        <p class="lead">contentfulとvueの組み合わせ</p>
      </div>
    </div>
    <div class="container">
      <div class="row text-center my-5">
        <div class="h2 col-12">
          <span class="category">果物</span>
        </div>
      </div>

      <div v-for="b in entries" :key="b.id" class="card mb-3">
        <div class="row no-gutters">
          <div class="col-md-4 img-fluid">
            <img :src="'http:'+b.fields.picture.fields.file.url" class="card-img">
          </div>
          <div class="col-md-8">
            <div class="card-body">
              <h5 class="card-title">{{b.fields.title}}</h5>
              <p class="card-text">{{b.fields.description.content[0].content[0].value}}</p>
              <p class="card-text">
                <small class="text-muted">{{b.fields.published}}</small>
              </p>
            </div>
          </div>
        </div>
      </div>

      <div class="row text-center my-5">
        <div class="h2 col-12">
          <span class="category">ブログ</span>
        </div>
      </div>

      <div v-for="a in articles" :key="a.id" class="card mb-3">
        <div v-html="a"></div>
      </div>
    </div>
  </div>
</template>



<script>
var md = require('markdown-it')();
const contentful = require("contentful");
const SPACE_ID = "ki9s3zz7clpp";
const ACCESS_TOKEN = "3x6fM_OM7wSULlGHg_7cUVYUSESiCsWSVNqB-2GzqpU";
const client = contentful.createClient({
  space: SPACE_ID,
  accessToken: ACCESS_TOKEN
});


export default {
  data(){
    return {entries: []};
  },
  created(){
    client.getEntries({content_type: "book"}).then(response => {
        this.entries = response.items;
      })
      client.getEntries({content_type: "article"}).then(response => {
        this.articles = [];
        response.items.forEach(e => {
          this.articles.push(md.render(e.fields.content));
        });
      });
  }
}
</script>

<style>
span.category{
  border-top: 3px solid rgb(255, 182, 6);
}
</style>