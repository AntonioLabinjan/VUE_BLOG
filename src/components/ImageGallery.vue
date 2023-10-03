<template>
  <div>
    <div v-for="(item, index) in items" :key="index">
      <img v-if="item.type === 'image'" :src="item.url" alt="Slika">
      <iframe v-else :src="item.url" frameborder="0" allowfullscreen></iframe>
      <p>Uploaded on: {{ item.date }}</p>
      <div>
        <span @click="toggleLike(index)">
          <i class="fas fa-heart" :class="{ 'liked': item.liked, 'disliked': item.disliked }"></i>
        </span>
        <span @click="toggleDislike(index)">
          <i class="fas fa-thumbs-down" :class="{ 'disliked': item.disliked, 'liked': item.liked }"></i>
        </span>
        <span @click="copyText(item.text)">
          <i class="fas fa-clipboard"></i> Copy
        </span>
      </div>
    </div>
  </div>
</template>

<script>
import '@fortawesome/fontawesome-free/css/all.css';
export default {
  props: {
    items: Array,
	searchQuery: String,
  },
  methods: {
    toggleLike(index) {
      const post = this.items[index];
      if (post.liked) {
        post.liked = false;
      } else {
        post.liked = true;
        if (post.disliked) {
          post.disliked = false;
        }
      }
    },
    toggleDislike(index) {
      const post = this.items[index];
      if (post.disliked) {
        post.disliked = false;
      } else {
        post.disliked = true;
        if (post.liked) {
          post.liked = false;
        }
      }
    },
	copyText(text) {
      const textarea = document.createElement("textarea");
      textarea.value = text;
      document.body.appendChild(textarea);
      textarea.select();
      textarea.setSelectionRange(0, 99999); 
      document.execCommand("copy");
      document.body.removeChild(textarea);
      alert("Text copied to clipboard.");
    },
  },
};
</script>

<style scoped>
.liked {
  color: red; 
  cursor: pointer; 
}

.disliked {
	color: red;
	cursor: pointer;
}


body, html {
  margin: 0;
  padding: 0;
  font-family: Arial, sans-serif;
}


body {
  background-color: light-blue;
}


h2 {
  text-align: center;
  color: #333;
  padding: 20px;
}


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


div {
  background-color: #fff;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin: 10px 0;
  padding: 20px;
  position: relative;
}


img {
  max-width: 100%;
  height: auto;
  border: 1px solid #ccc; 
  border-radius: 5px; 
  padding: 5px; 
  margin-bottom: 10px;
}



p {
  color: #777;
  font-size: 14px;
  margin-top: 10px;
}


button:hover {
  background-color: #ff6347;
  color: #fff;
}

</style>
