<template>
  <div id="app">
    <header>
      <h1 class="h1">My Music</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">{{current.title}}</h2>
        <h3 class="song-artist">Artist: {{current.artist}}</h3>
        <div class="controls">
          <button class="prev" @click="prev">Prev</button>
          <button class="play" v-if="!isPlaying" @click="play()">Play</button>
          <button class="pause" v-else @click="pause">Pause</button>
          <button class="next" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>The playlist</h3>
        <button v-for="(song, index) in songs" :key="song.src" @click="play(index)" :class="(song.src == current.src) ? 'song playing': 'song'">
          {{ song.title}} - {{ song.artist }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  created (){
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
    // this.player.play();
  },
  data: () => ({
    isPlaying: false,
    current: {},
    index: 0,
    player: new Audio(),
    songs: [
      {
        title: 'Algun dia',
        artist: 'Soda Stereo',
        src: require('./assets/mp3/Soda Stereo - Algun Dia [Some Day One Day].mp3')
      },
      {
        title: 'Cuando pase el temblor',
        artist: 'Soda Stereo',
        src: require('./assets/mp3/Soda Stereo - Cuando pase el temblor.mp3')
      },
      {
        title: 'De Música Ligera',
        artist: 'Soda Stereo',
        src: require('@/assets/mp3/Soda Stereo - De Música Ligera.mp3')
      },
      {
        title: 'Un Millon De Años Luz',
        artist: 'Soda Stereo',
        src: require('@/assets/mp3/Soda Stereo - Un Millon De Años Luz.mp3')
      },
      {
        title: 'Persiana Americana',
        artist: 'Gustavo Serati',
        src: require('@/assets/mp3/Soda Stereo Persiana Americana.mp3')
      }
    ]
  }),
  methods: {
    play(indexx) {
      if (typeof indexx != "undefined") {
        this.index = indexx;
      } else {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.player.src = this.current.src;

      this.player.play();
      this.player.addEventListener('ended', function () {
        this.next();
      }.bind(this));
      this.isPlaying=true;
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
      this.play(this.index);
    },
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;        
      }
      this.play(this.index);
    }
  }
}
</script>

<style>
* {
  @apply m-0 p-0 box-border
}
body {
  @apply font-sans
}
header {
  @apply flex justify-center items-center p-2 bg-black text-white uppercase font-bold
}
main {
  @apply w-auto max-w-md m-0 m-auto
}

.song-title {
  @apply text-gray-700 text-3xl font-bold uppercase text-center;
}

.song-artist {
  @apply text-gray-600 text-xl font-bold uppercase text-center;
}

.song-title span {
  @apply font-medium italic
}

.controls {
  @apply flex justify-between py-4 items-center
}

button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}

button:hover {
  @apply bg-opacity-75
}

.play, .pause {
  @apply text-xl font-bold py-2 px-6 mx-4 bg-red-600 text-white rounded-lg shadow-lg
}

.next, .prev {
  @apply text-sm font-bold py-2 px-6 mx-2 bg-orange-600 text-white rounded-lg shadow-lg
}

.playlist {
  @apply py-4;
}

.playlist h3 {
  @apply text-gray-700 font-extrabold py-2 text-xl mb-4 text-center bg-orange-300 uppercase;
}

.playlist .song {
  @apply block w-full py-2 font-bold text-base cursor-pointer uppercase
}

.playlist .song:hover {
  color: #FF5858;
}

.playlist .song.playing {
  @apply rounded-lg;
  color: white;
  background-image: linear-gradient(to right, #cc2e5d, #FF5858);
}
</style>