<template>
  <div id="app">
    <header class="text-center">
      <h1 >My Music</h1>
    </header>
    <main :class="back">
      <section class="player text-center">
        <h2 class="song-name animate__animated ">
          {{ corrent.title }}-<span class="artist">{{ corrent.artist }}</span>
        </h2>
        <div class="control">
          <button class="side" @click="prev">prev</button>
          <button class="center" v-if="!isplaying" @click="paly">paly</button>
          <button class="center" v-else @click="pause">pause</button>
          <button class="side" @click="next">next</button>
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
      back:"main1",
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
      this.player.addEventListener(
        "ended",
        function () {
          this.index++;
          if (this.index > this.songs.length - 1) {
            this.index = 0;
          }
          this.corrent = this.songs[this.index];
          this.player.src = this.corrent.src;
          this.player.play();
          this.isplaying = true;
        }.bind(this)
      );
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

* {
  --animate-duration: 6s;
}
header {
  color: white;
  padding: 20px 10px;
  position: fixed;
  width: 100%;
  background-color: rgba(0, 0, 0, 0.6);
}

.main1{
  background-image: url("assets/max-kleinen-Rr1uLDXzCYg-unsplash.jpg");
  min-height: 100vh;
  background-size: cover;
  background-position: center;
}

.player {
  padding-top: 150px;
}

.song-name {
  font-size: 60px;
  color: #ddd;
}
.song-name .artist
{
 color: #9701B5;
}
.control ul li {
  list-style: none;
}
  button
  {
    background:none;
    font-size: 40px;
    color: white;
    border: none;
    outline: none;
  }
  .player button{
    margin-left: 20px;
    font-size: 25px;
  }
  .control
  {
    margin-top: 70px;
    margin-bottom: 50px;
  }
  .control button
  {
    border: solid 2px #9701B5;
    padding: 10px 20px;
    border-radius: 20px;
    text-transform: uppercase;
    font-size: 20px;
    transition: 500ms;
  }
.control button:focus
{
  outline: none;

}
  .control .side{
    border: white solid 2px ;
  }

   .control .center
  {
    background-color: white;
    color: #9701B5;
    font-weight: 600;
    border: none;
    transform: scale(1.2,1.2);
  }
  .control button:hover{
    background-color: white;
    color:  #9701B5;
    transition: 500ms;

  }
  .play-list h3
  {
    color: white;
    font-size: 40px;
    font-weight: 900;
    margin-bottom: 40px;
  }
  .play-list button
  {
    display: block;
    margin-top: 20px;
    font-size: 20px;
    padding-left: 20px;
    padding: 10px;
    text-align: left;
    margin-left: 10%;
    width: 80%;
    background-color: white;
    color: #9701B5;
    border: solid 2px #9701B5;
    border-radius: 10px 20px 10px 20px;
    transition: 500ms;
  }
.play-list button:hover
{
  color: white;
  background-color: #9701B5;
  border-radius: 20px 10px 20px 10px;
  transition: 500ms;

}
</style>
