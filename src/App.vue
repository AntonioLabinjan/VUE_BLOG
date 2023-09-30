<template>
  <div id="app">
    <ImageUploader @filter="handleFilter" />
    <ImageGallery :items="filteredItems" />
	<center><h5>Also visit:</h5></center>
    <div class="social-icons">
      <a href="https://www.facebook.com" target="_blank">
        <i class="fab fa-facebook"></i> <!-- Font Awesome Facebook ikonica -->
      </a>
      <a href="https://www.youtube.com" target="_blank">
        <i class="fab fa-youtube"></i> <!-- Font Awesome YouTube ikonica -->
      </a>
      <a href="https://www.twitter.com" target="_blank">
        <i class="fab fa-twitter"></i> <!-- Font Awesome Twitter ikonica -->
      </a>
      <a href="https://www.instagram.com" target="_blank">
        <i class="fab fa-instagram"></i> <!-- Font Awesome Instagram ikonica -->
      </a>
      <a href="https://www.discord.com" target="_blank">
        <i class="fab fa-discord"></i> <!-- Font Awesome Discord ikonica -->
      </a>
    </div>
	<TimeCounter />
	<right>
	<div><strong>Made by: Antonio Labinjan</strong></div>
	</right>
  </div>
</template>


<script>
import ImageUploader from "./components/ImageUploader.vue";
import ImageGallery from "./components/ImageGallery.vue";
import TimeCounter from "./components/TimeCounter.vue";

export default {
  name: 'App',
  components: {
    ImageUploader,
    ImageGallery,
	TimeCounter,
  },
  data() {
    return {
      items: [],
      searchQuery: "",
    };
  },
  computed: {
    filteredItems() {
      const query = this.searchQuery.toLowerCase();
      return this.items.filter(item => {
        if (item.type === 'text') {
          return item.text.toLowerCase().includes(query);
        } else if (item.type === 'image') {
          return (
            item.description.toLowerCase().includes(query) ||
            item.date.toLowerCase().includes(query)
          );
        }
        return false;
      });
    },
  },
  methods: {
    handleFilter(query) {
      this.searchQuery = query;
    },
  },
}
</script>

<style scoped>
.social-icons {
  margin-top: 20px;
  text-align: center; 
}

.social-icons a {
  margin-right: 10px; 
  display: inline-block;
}

.social-icons i {
  font-size: 36px; 
  color: #007bff; 
}
</style>

