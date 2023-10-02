<template>
  <div class="comments-section">
    <center><h3>Komentari:</h3></center>
    
    <div class="comment-input">
      <textarea v-model="newComment" placeholder="Unesite komentar"></textarea>
      <button @click="addComment">Dodaj komentar</button>
    </div>
    
    <div v-for="(comment, index) in comments" :key="index" class="comment" style="text-align: right;">
      <p v-if="editingIndex !== index">{{ comment.text }}</p>
      <textarea v-else v-model="newComment"></textarea>
      <p>{{ comment.date }}</p>
      <button v-if="editingIndex === index" @click="saveEditedComment(index)">Spremi</button>
      <button v-else @click="editComment(index)">Uredi</button>
      <button @click="deleteComment(index)">Obriši</button>
      <button v-if="editingIndex === index" @click="cancelEdit">Odustani</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newComment: "",
      comments: [],
      editingIndex: null, 
    };
  },
  methods: {
    addComment() {
      if (this.newComment.trim() !== "") {
        const date = new Date().toLocaleString();
        this.comments.push({
          text: this.newComment,
          date: date,
        });
        this.newComment = "";
      }
    },

    editComment(index) {
      this.editingIndex = index;
      this.newComment = this.comments[index].text;
    },

    saveEditedComment(index) {
      if (this.newComment.trim() !== "") {
        this.comments[index].text = this.newComment;
        this.editingIndex = null;
        this.newComment = "";
      }
    },

    cancelEdit() {
      this.editingIndex = null;
      this.newComment = "";
    },

    deleteComment(index) {
      if (confirm("Jeste li sigurni da želite obrisati ovaj komentar?")) {
        this.comments.splice(index, 1);
      }
    },
  },
};
</script>

<style scoped>
.comments-section {
  margin-top: 20px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.comment-input {
  margin-bottom: 10px;
}

textarea {
  width: 100%;
  padding: 5px;
  margin-bottom: 5px;
}

button {
  background-color: #007bff;
  color: #fff;
  border: none;
  padding: 5px 10px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 14px;
}

/* Stil za tipku za brisanje */
.comment button:nth-child(4) {
  background-color: #ff6347; /* Crvena boja */
}

.comment {
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 10px;
  margin-bottom: 10px;
}

.comment p {
  margin: 5px 0;
  font-size: 14px;
  text-align: right;
}

.comment textarea {
  width: 100%;
  padding: 5px;
  margin-bottom: 5px;
}
</style>
