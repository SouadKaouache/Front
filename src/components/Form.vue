<script>
    export default{
        name:"Form",
        data() {
    return {
      content: "",
      imageUrl: "",
      userId: "",
      likes: "",
      dislikes: "",
      userLiked: [],
      userDisliked: []
    }
  },
  methods: {
    changeImage(e) {
      this.imageUrl = e.target.files[0];
    },
    sendPost() {
      const url = "http://localhost:3000/api/post/form"
      const userId = localStorage.getItem("userId")
      const formData = new FormData()
      formData.append("content", this.content)
      formData.append("imageUrl", this.imageUrl)
      formData.append("userId", userId)
      formData.append("likes", this.likes)
      formData.append("dislikes", this.dislikes)
      formData.append("userLiked", this.userLiked)
      formData.append("userDisliked", this.userDisliked)
      console.log(FormData)
      console.log(formData)
      
      const paramsFetch = {
        headers:   {Authorization: `Bearer ${localStorage.getItem("token")}`,
                  "Accept": "application/json",},
        method: "POST",
      body: formData,
      }
      console.log("paramsFetch:", paramsFetch)
      console.log(FormData)
      console.log(formData)
      console.log(this.content)
      console.log(this.imageUrl)
      fetch(url, paramsFetch)
        .then((res) => {
          if (res.status === 201) {
            return res.json()
          } else {
            throw new Error("Le fetch de Form.vue a échoué.")
          }
        })
        .then((res) => {
          this.$router.go()
        })
        .catch((err) => console.log("err:", err))
    }
  }
}
</script>
<template>
  <div class="container-md">
    <div class="form-floating was-validated">
   <div contenteditable class="form-control" id="text" name="text" placeholder="Saisie obligatoire" required>	
    <input v-model="this.content" name="content" id="content" />{{content}}
</div>
   <label class="form-label" for="form3Example1cg">Partagez du contenu avec vos collègues</label>

</div>
<div class="mb-2">
  <div class="file btn btn-primary">
							Ajouter une image
              <span v-if="this.imageUrl" >{{ this.imageUrl.name}}</span>
							<input type="file" v-on="this.imageUrl"  accept="image/*" id="file-input" @change="changeImage"/>
						</div>
</div>
<div class="col">
  <label for="floatingSelectGrid" class="form-label">Veuillez séléctionner votre service.</label>
  <select class="form-select" id="validationCustom04" required>
      <option selected disabled value="">Veuillez choisir</option>
        <option selected>ressources humaines</option>
        <option value="1">comptabilité</option>
        <option value="2">commercial</option>
        <option value="3">informatique</option>
        <option value="3">autre</option>
      </select>
      <div class="invalid-feedback">
      Merci de séléctionner votre service
    </div>  
      </div>
      <div class="d-grid gap-2 d-md-flex justify-content-md-end">
<button type="submit" class="btn btn-success btn-lg mt-2 "  @click="sendPost">Publier</button></div>
<hr class = "mt-5"/>
</div>
</template>
<style scoped>
div {
  position: relative;
  overflow: hidden;
}
input {
  position: absolute;
  font-size: 50px;
  opacity: 0;
  right: 0;
  top: 0;
}
.form-floating > .form-control{
  height: auto;
  min-height: 10rem;
  max-height: 25rem;
}

.form-control{
min-height: 15%;
}
.container-md{
    margin-top: 5%;
    max-width: 80%;
  }
    body{
      font-family: lato-black, sans-serif;
        background: linear-gradient(90deg, rgba(253,45,1,0.5) 0%, rgba(255,215,215,0.5) 35%, rgba(78,81,102,0.5) 100%);}
</style>