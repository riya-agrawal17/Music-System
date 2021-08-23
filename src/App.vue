<template>
  <div id="app">
    <header>
      <h1>Music</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">
          {{ current.title }} - <span>{{ current.artist }}</span>
        </h2>
        <div class="controls">
          <button class="prev" @click="prev">
            <i class="fas fa-backward"></i> Prev
          </button>
          <button class="play" v-if="!isPlaying" @click="play">
            <i class="fas fa-play"></i> Play
          </button>
          <button class="pause" v-else @click="pause">
            <i class="fas fa-pause"></i> Pause
          </button>
          <button class="next" @click="next">
            Next <i class="fas fa-forward"></i>
          </button>
        </div>
      </section>
      <section class="playlist">
        <h3>My Playlist</h3>
        <button
          v-for="song in songs"
          :key="song.src"
          @click="play(song)"
          :class="song.src == current.src ? 'song playing' : 'song'"
        >
          {{ song.title }} - {{ song.artist }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: 'Realme',
          artist: 'John',
          src: require('./assets/music1.mp3'),
        },
        {
          title: 'Soniyo',
          artist: 'Sonu Nigam',
          src: require('./assets/music2.mp3'),
        },
      ],
      player: new Audio(),
    };
  },
  methods: {
    play(song) {
      if (typeof song.src != 'undefined') {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener(
        'ended',
        function() {
          this.index++;
          if (this.index > this.songs.length - 1) {
            this.index = 0;
          }
          this.current = this.songs[this.index];
          this.play(this.current);
        }.bind(this)
      );
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: sans-serif;
  background-image: url('./assets/mus.jpeg');
}
header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-image: linear-gradient(to right, #070707, #f4f4fa,#070707);
  color: rgba(168, 61, 67, 0.877);
}
main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}
.song-title {
  color: #c2eb94;
  font-size: 22px;
  font-weight: 500;
  text-align: center;
}
.song-title span {
  font-weight: 400;
  font-style: italic;
}
.controls {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 30px 15px;
}
button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}
button:hover {
  opacity: 0.8;
}
.play {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: #fff;
  background-color: rgb(36, 54, 36);
}
.pause {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: #fff;
  background-color: palevioletred;
}
.next,
.prev {
  font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color: #fff;
  background-color: #201d41;
}
.playlist {
  padding: 0px 30px;
}
.playlist h3 {
  color: #eef08c;
  text-decoration: underline; 
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 30px;
  margin-top: 200px;
  text-align: center;
}
.playlist .song {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
  color: yellowgreen;
}
.playlist .song:hover {
  color: #e6d3d3;
}
.playlist .song.playing {
  color: #fff;
  background-image: linear-gradient(to right, #293313, #1c1f47);
}
</style>
