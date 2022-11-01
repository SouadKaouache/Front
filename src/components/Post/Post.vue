<script>
import Comments from './Comments.vue'
    export default {
        name:"Post",
        components:{Comments},     
        props: ["content", "imageUrl", "userId", "email", "likes", "dislikes", "userLiked", "userDisliked"],
        data() {
    return {
    }
  },
  mounted() {},
  methods: {
    deletePost(e) {
      console.log("id of the post to delete:", this.$props.id)
      const url = "http://localhost:3000/api/post/" 
      const paramsFetch = {
        Authorization: `Bearer ${localStorage.getItem("token")}`,
      "Accept": "application/json"
      }
      fetch(url + this.$props.id, {
        paramsFetch: { ...paramsFetch, "Content-Type": "application/json" },
        method: "DELETE"
      })
        .then((res) => {
          if (res.status === 200) {
            return res.json()
          } else {
            throw new Error("Suppression impossible.")
          }
        })
        .then((res) => {
          console.log("res:", res)
          this.$router.go()
        })
        .catch((err) => console.log("err:", err))
    }
  }
}


</script>
<template>
<div class="container-md">
<div class="card mb-3 m-auto mb-5 mt-5">
<div class="card-header">Publié par {{}}<div class="d-inline-flex position-relative  ms-3">
  <span class="position-absolute bottom-55 end-0 translate-middle p-1 bg-danger border border-light rounded-circle">
  </span>
</div></div>
<img :src="url" class="card-img-top" alt="..." />{{imageUrl}}
  <div class="card-body">
    <p class="card-text">{{content}}</p>
    <p class="card-text"><small class="text-muted">Dernière mise à jour il y a 3 minutes</small></p>
    <div v-for="comment in comments">
    <Comments :email="comment.user.email" :content="comment.content"></Comments></div>
    <img class="rounded-circle shadow-4 me-3 border border-danger" src="https://www.fillmurray.com/50/50" alt="Avatar" style="width: 50px; height: 50px;" >

 
<div class="form-floating was-validated mt-2">
   <div contenteditable class="form-control" id="text" name="text" placeholder="Saisie obligatoire" required>		
</div>
   <label class="form-label" for="form3Example1cg">Publiez un commentaire</label>
   <div class="d-grid gap-2 d-md-flex justify-content-md-end">
<button type="submit" class="btn btn-success mt-2 ">Envoyer le commentaire</button>
<button type="submit" v-if="currentUser === email" class="btn btn-danger mt-2 " @click="deletePost">Supprimer la publication</button></div>

</div>
  </div>
</div>
</div>
  </template>
<style scoped>
 .form-floating > .form-control{
  height: auto;
 }
  body{
    font-family: lato-black, sans-serif;
  }
@media(min-width: 769px){
  .card{
    font-family: lato-black, sans-serif;
    max-width: 60%;
  }} 

@media(min-width: 380px) and (max-width: 768px){
  .card{
    max-width: 90%
   }}
</style>