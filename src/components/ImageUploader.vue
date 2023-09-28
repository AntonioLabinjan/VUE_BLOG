<template>
  <div>
    <center><h1>VueBlog</h1></center>
    <input type="file" @change="uploadImages" multiple>
    <textarea v-model="postText" placeholder="Unesite tekst objave"></textarea>
    <input v-model="videoLink" placeholder="Unesite URL videa">
    <button @click="addPost">Dodaj objavu</button>
    <button @click="addVideo">Dodaj video</button>
    <div v-for="(item, index) in items" :key="index">
      <div v-if="item.type === 'image'">
        <img :src="item.url" alt="Prenesena slika">
        <p>Datum i vrijeme postavljanja: {{ item.date }}</p>
        <div>
          <p v-if="item.description">{{ item.description }}</p>
          <input
            type="text"
            v-model="item.editDescription"
            placeholder="Unesite ili uredite opis slike"
          >
          <button @click="saveDescription(index)">Spremi opis</button>
          <button @click="deleteDescription(index)">Obriši opis</button>
        </div>
        <button @click="deleteImage(index)">Obriši sliku</button>
        <span @click="toggleLike(index)">
          <i class="fas fa-heart" :class="{ 'liked': item.liked }"></i>
        </span>
        <span @click="toggleDislike(index)">
          <i class="fas fa-thumbs-down" :class="{ 'disliked': item.disliked }"></i>
        </span>
      </div>
      <div v-else-if="item.type === 'text'">
        <p>Datum i vrijeme postavljanja: {{ item.date }}</p>
        <p>{{ item.text }}</p>
        <button @click="editText(index)">Uredi objavu</button>
        <button @click="deleteText(index)">Obriši objavu</button>
        <span @click="toggleLike(index)">
          <i class="fas fa-heart" :class="{ 'liked': item.liked }"></i>
        </span>
        <span @click="toggleDislike(index)">
          <i class="fas fa-thumbs-down" :class="{ 'disliked': item.disliked }"></i>
        </span>
      </div>
      <div v-else-if="item.type === 'video'">
        <iframe
          width="560"
          height="315"
          :src="getEmbedUrl(item.url)"
          frameborder="0"
          allowfullscreen
        ></iframe>
        <p>Datum i vrijeme postavljanja: {{ item.date }}</p>
         <button @click="deleteVideo(index)">Obriši video</button>
        <span @click="toggleLike(index)">
          <i class="fas fa-heart" :class="{ 'liked': item.liked }"></i>
        </span>
        <span @click="toggleDislike(index)">
          <i class="fas fa-thumbs-down" :class="{ 'disliked': item.disliked }"></i>
        </span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [], 
      postText: "", 
      videoLink: "",
    };
  },
  methods: {
    uploadImages(event) {
      const files = event.target.files;
      for (let i = 0; i < files.length; i++) {
        const file = files[i];
        if (file) {
          const reader = new FileReader();
          reader.onload = () => {
            const date = new Date(); 
            this.items.push({
              type: 'image',
              url: reader.result,
              date: date.toLocaleString(),
              description: "", 
              editDescription: "", 
              liked: false,
              disliked: false
            });
          };
          reader.readAsDataURL(file);
        }
      }
    },
    addPost() {
      if (this.postText.trim() !== "") {
        const date = new Date(); 
        this.items.push({
          type: 'text',
          date: date.toLocaleString(),
          text: this.postText,
          liked: false,
          disliked: false
        });
        this.postText = ""; 
      }
    },
    deleteImage(index) {
      this.items.splice(index, 1);
    },
    saveDescription(index) {
      this.items[index].description = this.items[index].editDescription.trim();
      this.items[index].editDescription = ""; 
    },
    deleteDescription(index) {
      this.items[index].description = "";
      this.items[index].editDescription = "";
    },
    editText(index) {
      const editedText = prompt("Uredi tekst posta", this.items[index].text);
      if (editedText !== null) {
        this.items[index].text = editedText.trim();
      }
    },
    deleteText(index) {
      if (confirm("Jeste li sigurni da želite obrisati ovaj tekst?")) {
        this.items.splice(index, 1);
      }
    },
    addVideo() {
      if (this.videoLink.trim() !== "") {
        const date = new Date(); 
        this.items.push({
          type: 'video',
          url: this.videoLink,
          date: date.toLocaleString(),
          liked: false,
          disliked: false
        });
        this.videoLink = "";
      }
    },
    toggleLike(index) {
      const post = this.items[index];
      if (post.liked) {
        post.liked = false;
      } else {
        post.liked = true;
        post.disliked = false;
      }
    },
    toggleDislike(index) {
      const post = this.items[index];
      if (post.disliked) {
        // Undislajkaj post
        post.disliked = false;
      } else {
        // Dislajkaj post i poništi lajk
        post.disliked = true;
        post.liked = false;
      }
    },
    getEmbedUrl(url) {
      if (url.includes('youtube.com')) {
        const videoId = url.split('v=')[1];
        return `https://www.youtube.com/embed/${videoId}`;
      }
      return url;
    },
    deleteVideo(index) {
      if(confirm('Jeste li sigurni da želite obrisati video?')){
        this.items.splice(index,1);
      }
    },
  },
};
</script>

<style scoped>
/* Stil za lajk ikonu */
.liked {
  color: red; /* Promijenite boju ikone kada je lajkano */
  cursor: pointer; /* Dodajte pokazivač ruke za lajk ikonu */
}
.disliked {
  color: red;
  cursor: pointer;
}
/* Resetiranje osnovnih stilova */
body, html {
  margin: 0;
  padding: 0;
  font-family: Arial, sans-serif;
}

/* Postavljanje pozadinske boje za cijelu stranicu */
body {
  background-color: #f5f5f5;
}

/* Glavni naslov */
h2 {
  text-align: center;
  color: #333;
  padding: 20px;
}

/* Unos teksta i gumb za dodavanje posta */
input[type="file"], textarea {
  display: block;
  width: 100%;
  padding: 10px;
  margin: 10px 0;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 16px;
}

button {
  display: block;
  background-color: #007bff;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 10px 0;
  border-radius: 5px;
  cursor: pointer;
  font-size: 11px;
}

button:hover {
  background-color: #0056b3;
}

/* Stil za pojedinačni post */
div {
  background-color: #fff;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin: 10px 0;
  padding: 20px;
  position: relative;
}

/* Stil za sliku u postu */
img {
  max-width: 100%;
  height: auto;
}

/* Stil za datum i vrijeme */
p {
  color: #777;
  font-size: 14px;
  margin-top: 10px;
}

/* Stil za uređivanje i brisanje teksta/posta */
button:hover {
  background-color: #ff6347;
  color: #fff;
}
</style>
