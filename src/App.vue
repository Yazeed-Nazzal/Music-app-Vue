<template>
  <div id="app">
    <header class="text-center">
      <h1>My Music</h1>
    </header>
    <main>
      <section class="player text-center">
        <h2 class="song-name">
          {{ corrent.title }}-<sapn>{{ corrent.artist }}</sapn>
        </h2>
        <div class="control">
          <button @click="prev">prev</button>
          <button v-if="!isplaying" @click="paly">paly</button>
          <button v-else @click="pause">pause</button>
          <button @click="next">next</button>
        </div>
      </section>

      <section class="play-list">
        <h3 class="text-center">PlayList</h3>
        <button
          @click="paly(song)"
          v-for="song in songs"
          :key="song.src"
          :class="song.src == corrent.src ? 'song playing' : 'song'"
        >
          {{ song.title }}-{{ song.artist }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      isplaying: false,
      corrent: {},
      index: "0",
      songs: [
        {
          title: "Apparat",
          artist: "Dark",
          src: require("./assets/Apparat - Goodbye - Dark (Netflix) Theme Song.mp3"),
        },
        {
          title: "Devil",
          artist: "Dark",
          src: require("./assets/Soap&Skin - Me And The Devil.mp3"),
        },
      ],
      player: new Audio(),
    };
  },
  created() {
    this.corrent = this.songs[this.index];
    this.player.src = this.corrent["src"];
  },
  methods: {
    paly(Song) {
      if (typeof Song.src != "undefined") {
        this.corrent = Song;
        this.player.src = this.corrent.src;
      }
      this.player.play();
      this.isplaying = true;
      this.player.addEventListener("ended", function () {
        this.index++;
        if (this.index > this.songs.length - 1) {
          this.index = 0;
        }
        this.corrent = this.songs[this.index];
        this.player.src = this.corrent.src;
        this.player.play();
        this.isplaying = true;
      }.bind(this));
    },
    pause() {
      this.player.pause();
      this.isplaying = false;
    },
    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.corrent = this.songs[this.index];
      this.player.src = this.corrent.src;
      this.player.play();
      this.isplaying = true;
    },
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = 0;
      }
      this.corrent = this.songs[this.index];
      this.player.src = this.corrent.src;
      this.player.play();
      this.isplaying = true;
    },
  },
};
</script>

<style>
* {
  font-family: Verdana;
}

header {
  color: white;
  padding: 20px 10px;
  position: fixed;
  width: 100%;
  background-color: rgba(0, 0, 0, 0.6);
}

main {
  background-image: url("assets/pexels-alex-conchillos-3888585.jpg");
  min-height: 100vh;
  background-size: cover;
  background-position: center;
}

.player {
  padding-top: 150px;
}

.song-name {
  color: white;
}
.control ul li {
  list-style: none;
}
</style>
