<template>
  <div class="emoji-picker">
    <input v-model="searchTerm" placeholder="Search emojis..." @input="filterEmojis" />
    <div class="emoji-results">
      <div
        v-for="(emoji, index) in filteredEmojis"
        :key="index"
        @click="copyEmoji(emoji)"
      >
        {{ emoji.emoji }}
      </div>
    </div>
    <div v-if="copiedEmoji" class="copied-message">{{ copiedEmoji }} copied!</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchTerm: "",
      emojis: [
        { emoji: "😀", keywords: ["happy", "smile", "face"] },
        { emoji: "❤️", keywords: ["heart", "love"] },
        { emoji: "🌸", keywords: ["flower", "blossom"] },
		{ emoji: "🐱", keywords: ["cat", "kitty"] },
		{ emoji: "🐶", keywords: ["dog", "puppy"] },
		{ emoji: "🍕", keywords: ["pizza", "food"] },
		{ emoji: "🌞", keywords: ["sun", "sunny"] },
		{ emoji: "🌈", keywords: ["rainbow", "colorful"] },
		{ emoji: "🎉", keywords: ["celebration", "party"] },
		{ emoji: "🚀", keywords: ["rocket", "space"] },
		{ emoji: "🦄", keywords: ["unicorn", "magical"] },
		{ emoji: "🍦", keywords: ["ice cream", "dessert"] },
		{ emoji: "🎈", keywords: ["balloon", "celebrate"] },
		{ emoji: "🐬", keywords: ["dolphin", "ocean"] },
		{ emoji: "🌊", keywords: ["wave", "water"] },
		{ emoji: "🎵", keywords: ["music", "note"] },
		{ emoji: "🎮", keywords: ["game", "controller"] },
		{ emoji: "🍔", keywords: ["burger", "food"] },
		{ emoji: "🚲", keywords: ["bike", "cycling"] },
		{ emoji: "🌍", keywords: ["earth", "globe"] },
		{ emoji: "🌟", keywords: ["star", "glow"] },
		{ emoji: "🌻", keywords: ["sunflower", "plant"] },
		{ emoji: "🐠", keywords: ["fish", "aquatic"] },
		{ emoji: "📚", keywords: ["book", "reading"] },
		{ emoji: "🍂", keywords: ["fall", "autumn"] },
		{ emoji: "🍁", keywords: ["maple leaf", "fall"] },
		{ emoji: "⏰", keywords: ["clock", "time"] },
		{ emoji: "📷", keywords: ["camera", "photo"] },
		{ emoji: "🍩", keywords: ["doughnut", "food"] },
		{ emoji: "🚁", keywords: ["helicopter", "fly"] },
		{ emoji: "🌴", keywords: ["palm tree", "tropical"] },
		{ emoji: "🎶", keywords: ["musical notes", "song"] },
		{ emoji: "🏖️", keywords: ["beach", "vacation"] },
		{ emoji: "🎨", keywords: ["palette", "painting"] },
		{ emoji: "🚤", keywords: ["speedboat", "water"] },
		{ emoji: "🌮", keywords: ["taco", "food"] },
		{ emoji: "🌠", keywords: ["shooting star", "night"] },
		{ emoji: "📱", keywords: ["mobile phone", "cell"] },
		{ emoji: "🎃", keywords: ["jack-o-lantern", "halloween"] },
		{ emoji: "🍰", keywords: ["cake", "dessert"] },
		{ emoji: "🌧️", keywords: ["cloud with rain", "weather"] },
		{ emoji: "🏞️", keywords: ["national park", "nature"] },
		{ emoji: "🎤", keywords: ["microphone", "music"] },
		{ emoji: "🎸", keywords: ["guitar", "music"] },
		{ emoji: "🏄‍♂️", keywords: ["surfing", "water"] },
		{ emoji: "🎓", keywords: ["graduation cap", "education"] },
		{ emoji: "🍎", keywords: ["apple", "fruit"] },
		{ emoji: "🎥", keywords: ["movie camera", "film"] },
		{ emoji: "🌃", keywords: ["night with stars", "city"] },
		{ emoji: "📮", keywords: ["postbox", "mail"] },
		{ emoji: "🎄", keywords: ["Christmas tree", "holiday"] },
		{ emoji: "🎁", keywords: ["gift", "present"] },
		{ emoji: "🌓", keywords: ["first quarter moon", "night"] },
		{ emoji: "🐦", keywords: ["bird", "nature"] }
        // add more emojis here
      ],
      copiedEmoji: null,
    };
  },
  computed: {
    filteredEmojis() {
      return this.emojis.filter((emoji) => {
        return emoji.keywords.some((keyword) =>
          keyword.includes(this.searchTerm.toLowerCase())
        );
      });
    },
  },
  methods: {
    filterEmojis() {
    },
    copyEmoji(emoji) {
      navigator.clipboard.writeText(emoji.emoji).then(() => {
        this.copiedEmoji = emoji.emoji;
        setTimeout(() => {
          this.copiedEmoji = null; 
        }, 2000);
      });
    },
  },
};
</script>

<style scoped>
.emoji-picker {
  position: relative;
}

input {
  width: 100%;
  padding: 10px;
  font-size: 16px;
}

.emoji-results {
  position: absolute;
  top: 100%;
  left: 0;
  width: 100%;
  max-height: 200px;
  overflow-y: auto;
  background-color: #fff;
  border: 1px solid #ccc;
  border-top: none;
}

.emoji-results > div {
  padding: 5px;
  cursor: pointer;
}

.emoji-results > div:hover {
  background-color: #f0f0f0;
}

.copied-message {
  margin-top: 10px;
  color: green;
}
</style>
