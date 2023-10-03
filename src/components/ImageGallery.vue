<template>
  <div>
    <center><h1>VueBlog</h1></center>
    <input type="file" @change="uploadImages" multiple>
    <textarea v-model="postText" placeholder="Type in post text"></textarea>
    <input v-model="videoLink" placeholder="Type in video URL">
    <button @click="addPost">Add post</button>
    <button @click="addVideo">Add video</button>
    <div v-for="(item, index) in items" :key="index">
      <div v-if="item.type === 'image'">
        <img :src="item.url" alt="Uploaded image">
        <p>Uploaded on: {{ item.date }}</p>
        <div>
          <p v-if="item.description">{{ item.description }}</p>
          <input
            type="text"
            v-model="item.editDescription"
            placeholder="Type in or edit picture description"
          >
          <button @click="saveDescription(index)">Save description</button>
          <button @click="deleteDescription(index)" class="delete">Delete description</button>
        </div>
        <button @click="deleteImage(index)" class="delete">Delete image</button>
        <span @click="toggleLike(index)">
          <i class="fas fa-heart" :class="{ 'liked': item.liked }"></i>
        </span>
        <span @click="toggleDislike(index)">
          <i class="fas fa-thumbs-down" :class="{ 'disliked': item.disliked }"></i>
        </span>
      </div>
      <div v-else-if="item.type === 'text'">
        <p>Posted on: {{ item.date }}</p>
        <p>{{ item.text }}</p>
        <button @click="editText(index)">Edit post</button>
        <button @click="deleteText(index)" class="delete">Delete post</button>
        <span @click="toggleLike(index)">
          <i class="fas fa-heart" :class="{ 'liked': item.liked }"></i>
        </span>
        <span @click="toggleDislike(index)">
          <i class="fas fa-thumbs-down" :class="{ 'disliked': item.disliked }"></i>
        </span>
        <span @click="copyText(item.text)">
          <i class="fas fa-copy"></i> Copy
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
        <p>Posted on: {{ item.date }}</p>
         <button @click="deleteVideo(index)" class="delete">Delete video</button>
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
      if (confirm("Are you sure you want to delete?")) {
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
        post.disliked = false;
      } else {
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
      if(confirm('Are you sure you want to delete this video?')){
        this.items.splice(index,1);
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
  background-color: lightblue;
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

button.delete {
 background-color: #ff6347;
 color: #fff;
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

h1 {
  color: #007bff;
}
</style>
