<template>
  <div>
    <div v-for="(item, index) in items" :key="index">
      <img v-if="item.type === 'image'" :src="item.url" alt="Slika">
      <iframe v-else :src="item.url" frameborder="0" allowfullscreen></iframe>
      <p>Datum i vrijeme postavljanja: {{ item.date }}</p>
      <span @click="toggleLike(index)">
        <i class="fas fa-heart" :class="{ 'liked': item.liked }"></i>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    items: Array,
  },
  methods: {
    toggleLike(index) {
      const post = this.items[index];
      if (post.liked) {
        post.liked = false;
      } else {
        post.liked = true;
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
/* Resetiranje osnovnih stilova */
body, html {
  margin: 0;
  padding: 0;
  font-family: Arial, sans-serif;
}

/* Postavljanje pozadinske boje za cijelu stranicu */
body {
  background-color: #f2f2f2;
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
  font-size: 11px;
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
  font-size: 16px;
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
