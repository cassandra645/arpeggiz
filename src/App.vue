<template>
  <div id="app">
    <header>
      <img src="./assets/logos/arpeggiz-logo-base.png" alt="arpeggiz logo" class="app-logo">
    </header>
    <main>
      <div class="playerfull" :style="{backgroundImage: `url(${current.backgd})`}">
        <section class="player">
          <div class="song-description">
            <div class="song-informations">
              <h2 class="title"> {{ current.title }}</h2>
              <!-- <div>~</div> -->
              <div class="artist">{{ current.artist }}</div>
              <!-- <div>~</div> -->
              <div class="album">{{ current.album }}</div>
              <div class="style">{{ current.style }}</div>
            </div>
            <img v-bind:src="current.image" class="cover">
          </div>
        </section>
        <div class="progress-bar-container">
          <div :style="{backgroundColor: progressBarColor, width: progressBarWidth}" class="progress-bar">{{ updateProgressBar() }}
          <div class="progress-bar-slider"></div>
          </div>
        </div>
        <section class="controls-panel">
          <div :style="{color: timerColor}" class="timer-current">{{ turnSecondsToMinutes (player.currentTime) }}</div>
          <div class="controls">
            <img @click="shuffle" class="shuffle" src="./assets/logos/shuffle.png" alt="shuffle controller icon">
            <img @click="previous" class="prev control" src="./assets/logos/previous.png" alt="previous controller icon">
            <img v-if="!isPlaying" @click="play" class="play control" src="./assets/logos/play.png" alt="play controller icon">
            <img v-else @click="pause" class="pause control" src="./assets/logos/pause.png" alt="pause controller icon">
            <img  @click="next" class="next control" src="./assets/logos/next.png" alt="next controller icon">
            <img  @click="repeat" class="repeat" src="./assets/logos/repeat.png" alt="repeat controller icon">
          </div>
          <div class="timer-duration">{{ turnSecondsToMinutes (player.duration) }}</div>
        </section>
      </div>
      <section class="playlist scrollbar">
        <h3 class="playlist-title">Playlist</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
          {{ song.title }} - - <span class="artist-playlist">{{ song.artist }}</span>
        </button>
      </section>
    </main>
  </div>
</template>

<script>
import {Song} from "./classes.js"
export default {
  name: 'App',
  data () {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      progressBarWidth: "",
      progressBarColor: "",
      timerColor: "",
      fraction: "",
      percent: "",
      player: new Audio(),
      songs: [
        new Song(
          'Shitataru Morou',
          'Dir en Grey',
          'Dum Spiro Spero',
          'Progressive Metal',
          require('./assets/sounds/shitataru-morou.mp3'),
          require('./assets/img/dum-spiro-spero.jpg'),
          require('./assets/img/dir-en-grey.jpg')
        ),
        new Song(
          'You \'ve Seen the Butcher',
          'Deftones',
          'Diamond Eyes',
          'Metal',
          require('./assets/sounds/youve-seen-the-butcher.mp3'),
          require('./assets/img/diamond-eyes.jpg'),
          require('./assets/img/deftones.jpg')
        ),
        new Song(
          'I Speak Astronomy',
          'Jinjer',
          'King of Everything',
          'Progressive Metal',
          require('./assets/sounds/i-speak-astronomy.mp3'),
          require('./assets/img/king-of-everything.jpg'),
          require('./assets/img/jinjer.jpg')
        ),
        new Song(
          'Ghost Of Perdition',
          'Opeth',
          'Ghost Reveries',
          'Progressive Metal',
          require('./assets/sounds/ghost-of-perdition.mp3'),
          require('./assets/img/ghost-reveries.jpg'),
          require('./assets/img/opeth.jpg')
        ),
        new Song(
          'Theft Wandering And Lost',
          'Cocteau Twins',
          'Four-Calendar Café',
          'Dream Pop',
          require('./assets/sounds/theft-and-wandering-around-lost.mp3'),
          require('./assets/img/four-calendar-cafe.jpg'),
          require('./assets/img/cocteau-twins.jpg')
        ),
        new Song(
          'The Headmaster Ritual',
          'The Smiths',
          'Meat Is Murder',
          'Post Punk',
          require('./assets/sounds/the-headmaster-ritual.mp3'),
          require('./assets/img/meat-is-murder.jpg'),
          require('./assets/img/the-smiths.jpg')
        ),
        new Song(
          'For Jasmine',
          'Bôa',
          'Twilight',
          'Alternative Rock',
          require('./assets/sounds/for-jasmine.mp3'),
          require('./assets/img/twilight-album.jpg'),
          require('./assets/img/boa.jpg')
        ),
        new Song(
          'Jigsaw Falling Into Place',
          'Radiohead',
          'In Rainbows',
          'Alternative Rock',
          require('./assets/sounds/jigsaw-falling-into-place.mp3'),
          require('./assets/img/in-rainbows.jpg'),
          require('./assets/img/radiohead.jpg')
        ),
        new Song(
          'Only',
          'Nine Inch Nails',
          'With Teeth',
          'Electro',
          require('./assets/sounds/only.mp3'),
          require('./assets/img/with-teeth.jpg'),
          require('./assets/img/nine-inch-nails.jpg')
        ),
        new Song(
          'Delete',
          'Killing Joke',
          'Pylon',
          'Metal',
          require('./assets/sounds/delete.mp3'),
          require('./assets/img/pylon.jpg'),
          require('./assets/img/killing-joke.jpg')
        ),
        new Song(
          'Latour',
          'Sukekiyo',
          'Immortalis',
          'Progressive Rock',
          require('./assets/sounds/latour.mp3'),
          require('./assets/img/immortalis.jpg'),
          require('./assets/img/sukekiyo.jpg')
        ),
        new Song(
          'New Gold Dream',
          'Simple Minds',
          'New Gold Dream (81/82/83/84)',
          'New Wave',
          require('./assets/sounds/new-gold-dream.mp3'),
          require('./assets/img/new-gold-dream81-82-83-84.jpg'),
          require('./assets/img/simple-minds.jpg')
        ),
        new Song(
          'Blackstar',
          'David Bowie',
          'Blackstar',
          'Legendary',
          require('./assets/sounds/blackstar.mp3'),
          require('./assets/img/blackstar.jpg'),
          require('./assets/img/david-bowie.jpg')
        ),
        new Song(
          'Oktrovenie',
          'Irfan',
          'Irfan',
          'Ethereal',
          require('./assets/sounds/oktrovenie.mp3'),
          require('./assets/img/irfan-album.jpg'),
          require('./assets/img/irfan.jpg')
        ),
        new Song(
          'Tada Touku E',
          'The Novembers',
          'Hallelujah',
          'Indie Rock',
          require('./assets/sounds/tada-touku-e.mp3'),
          require('./assets/img/hallelujah.jpg'),
          require('./assets/img/the-novembers.jpg')
        ),
        new Song(
          'Only Shallow',
          'My Bloody Valentine',
          'Loveless',
          'Shoegaze',
          require('./assets/sounds/only-shallow.mp3'),
          require('./assets/img/loveless.jpg'),
          require('./assets/img/my-bloody-valentine.jpg')
        ),
        new Song(
          'Sweetest Chill',
          'Siouxsie And The Banshees',
          'Tinderbox',
          'Post Punk',
          require('./assets/sounds/sweetest-chill.mp3'),
          require('./assets/img/tinderbox.jpg'),
          require('./assets/img/siouxsie-and-the-banshees.jpg')
        ),
        new Song(
          'It\'s You',
          'PJ Harvey',
          'Uh Huh Her',
          'Rock',
          require('./assets/sounds/its-you.mp3'),
          require('./assets/img/uhhuhher.jpg'),
          require('./assets/img/pj-harvey.jpg')
        ),
        new Song(
          'The Pot',
          'Tool',
          '10,000 Days',
          'Progressive  Metal',
          require('./assets/sounds/the-pot.mp3'),
          require('./assets/img/10000-days.jpg'),
          require('./assets/img/tool.jpg')
        ),
        new Song(
          'Two People in a Room',
          'Wire',
          '154',
          'Post Punk',
          require('./assets/sounds/two-people-in-a-room.mp3'),
          require('./assets/img/154.jpg'),
          require('./assets/img/wire.jpg')
        ),
        new Song(
          'Maps in Her Wrists and Arms',
          'And Also The Trees',
          'Virus Meadow',
          'Cold Wave',
          require('./assets/sounds/maps-in-her-wrists-and-arms.mp3'),
          require('./assets/img/virus-meadow.jpg'),
          require('./assets/img/and-also-the-trees.jpg')
        ),
      ],
    }
  },
  methods: {

    play (song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }

      this.player.play();
      this.player.addEventListener('ended', function () {
        this.index++;
        if (this.index > this.songs.length - 1) {
          this.index = 0;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
      }.bind(this));
      this.isPlaying = true;
      console.log(Song)
    },

    progressBar () {
      if(this.player.currentTime > 0)
      {
        this.fraction = this.player.currentTime / this.player.duration;
        this.percent  = Math.abs(this.fraction * 100);
        this.progressBarWidth = this.percent + '%';
        if(this.isPlaying === true) {
          this.progressBarColor = "#0aebc5";
          this.timerColor = "#0aebc5"
        }
        else {
          this.progressBarColor = "#ff7600";
          this.timerColor = "#ff7600"
        }
      }
    },

    updateProgressBar () {
      this.player.addEventListener('timeupdate', this.progressBar)
    },

    turnSecondsToMinutes (time) {
      time = Number(time);
      let minutes = Math.floor(time % 3600 / 60);
      let seconds = Math.floor(time % 3600 % 60);

      let minutesDisplay = this.timerDisplay(minutes);
      let secondsDisplay = this.timerDisplay(seconds);
      return minutesDisplay + ":" + secondsDisplay;
    },

    timerDisplay(timer) {
      timer = timer > 0 ? (timer <= 9 ? "0" + timer : timer) : "00";
      return timer;
    },

    pause () {
      this.player.pause();
      this.isPlaying = false;
    },

    shuffle () {
      this.index = Math.floor(Math.random() * this.songs.length);
      this.current = this.songs[this.index];
      this.play(this.current);
    },

    repeat() {
      this.current = this.songs[this.index];
      this.play(this.current);
    },

    next () {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    },
    previous () {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created () {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  },

}
</script>

<style>
/**Layout */
.progress-bar-container {
  height: 8px;
  background-color: #1b182e;
}

.progress-bar {
  height: inherit;
  position: relative;
}

.progress-bar-slider {
  position: absolute;
  top: 50%;
  left: 100%;
  transform: translateY(-50%);
  width: 5px;
  height: 10px;
  background-color: #fff;
  transition: 0.5 ease-in-out;
}

.progress-bar:hover {
  transform: scaleY(1.3);
}

*
{
  margin : 0;
  padding: 0;
  box-sizing: border-box;
}

body
{
  font-family: sans-serif;
  color: #ffffff;
  background-color: #0f0e18;
}

main {
  width: 100%;
  margin: 0 auto;
}

/**Header's appearance */
header
{
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
  background-color: #09080f;
  color: #ffffff;
  max-height: 12vh;
}

.app-logo
{
  max-width: 150px;
	height: auto;
}

/**Player's appearance */
.backgd {
  position: absolute;
  z-index: -1000;
  width: 100%;
  min-height: 450px;
  top: 6%;
  left: 0%;
  filter: blur(2px) brightness(70%);
}

.cover {
  display: block;
  margin: auto;
  margin-bottom: 20px;
  height: 150px;
  width: 150px;
}

.cover:hover {
  filter: brightness(75%);
}

.player {
  display: flex;
  justify-content: center;
  align-items: flex-end;
  padding: 5% ;
  padding-bottom: 0% ;
  min-height: 73vh;
  padding-block: 10%;
}

.playerfull {
  max-height: 90vh;
  background-repeat: no-repeat;
  background-size: cover;
  background-position:center;
}

.song-description {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.song-informations {
  display: flex;
  flex-direction: column;
  align-items: center;
  color: #ffffff;
  font-size: 18px;
  font-weight: 700;
  font-variant: small-caps;
  margin-block: 25px;
}

.song-informations .title {
  font-size: 30px;
  font-weight: 600;
}

.song-informations .artist {
  /* color: #03eac5; */
  background: linear-gradient(to right, #06739e, #0aebc5, #06739e);
  background-clip: text;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -moz-text-fill-color: transparent;  -webkit-text-fill-color: transparent;
  font-size: 25px;
  font-weight: 600;
}

.song-informations .album {
  font-size: 15px;
  color:  #fff;
}

.song-informations .style {
  font-size: 14px;
  font-weight: lighter;
  font-variant: normal;
  color:  #ffffff;
  margin-top: 15px;
}

/**Controls' appearance */
.controls-panel {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 15px;
  background-color: #05050b;
  min-height: 15vh;
}

.controls {
  display: flex;
  align-items: center;
}

button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}

.control {
  margin-left: 15px;
  margin-right: 15px;
}

.control:hover {
  cursor: pointer;
}

.play, .pause{
  width: 80px;
  height: 80px;
  display: block;
}

.next, .prev {
  width: 60px;
  height: 60px;
  display: block;
}

.next:hover, .prev:hover {
  filter: brightness(150%);
}

.shuffle, .repeat {
  width: 30px;
  height: 30px;
}

.shuffle:hover, .repeat:hover {
  filter: brightness(150%);
  cursor: pointer;
}

.timer-current, .timer-duration {
  align-self: flex-start;
  font-weight: bolder;
  margin-top: -10px;
  font-size: 12px;
}

.timer-current {
  margin-left: -5px;
}

.timer-duration {
  margin-right: -5px;
}

/**Playlist's appearance */
.playlist {
  padding-top: 15px;
  background-color: #09080f50;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='100%25' height='100%25' viewBox='0 0 1600 800'%3E%3Cg fill-opacity='0.47'%3E%3Cpath fill='%230d0c13' d='M486 705.8c-109.3-21.8-223.4-32.2-335.3-19.4C99.5 692.1 49 703 0 719.8V800h843.8c-115.9-33.2-230.8-68.1-347.6-92.2C492.8 707.1 489.4 706.5 486 705.8z'/%3E%3Cpath fill='%23100f17' d='M1600 0H0v719.8c49-16.8 99.5-27.8 150.7-33.5c111.9-12.7 226-2.4 335.3 19.4c3.4 0.7 6.8 1.4 10.2 2c116.8 24 231.7 59 347.6 92.2H1600V0z'/%3E%3Cpath fill='%2312111a' d='M478.4 581c3.2 0.8 6.4 1.7 9.5 2.5c196.2 52.5 388.7 133.5 593.5 176.6c174.2 36.6 349.5 29.2 518.6-10.2V0H0v574.9c52.3-17.6 106.5-27.7 161.1-30.9C268.4 537.4 375.7 554.2 478.4 581z'/%3E%3Cpath fill='%2314141d' d='M0 0v429.4c55.6-18.4 113.5-27.3 171.4-27.7c102.8-0.8 203.2 22.7 299.3 54.5c3 1 5.9 2 8.9 3c183.6 62 365.7 146.1 562.4 192.1c186.7 43.7 376.3 34.4 557.9-12.6V0H0z'/%3E%3Cpath fill='%23161620' d='M181.8 259.4c98.2 6 191.9 35.2 281.3 72.1c2.8 1.1 5.5 2.3 8.3 3.4c171 71.6 342.7 158.5 531.3 207.7c198.8 51.8 403.4 40.8 597.3-14.8V0H0v283.2C59 263.6 120.6 255.7 181.8 259.4z'/%3E%3Cpath fill='%23181822' d='M1600 0H0v136.3c62.3-20.9 127.7-27.5 192.2-19.2c93.6 12.1 180.5 47.7 263.3 89.6c2.6 1.3 5.1 2.6 7.7 3.9c158.4 81.1 319.7 170.9 500.3 223.2c210.5 61 430.8 49 636.6-16.6V0z'/%3E%3Cpath fill='%231b1b24' d='M454.9 86.3C600.7 177 751.6 269.3 924.1 325c208.6 67.4 431.3 60.8 637.9-5.3c12.8-4.1 25.4-8.4 38.1-12.9V0H288.1c56 21.3 108.7 50.6 159.7 82C450.2 83.4 452.5 84.9 454.9 86.3z'/%3E%3Cpath fill='%231d1d27' d='M1600 0H498c118.1 85.8 243.5 164.5 386.8 216.2c191.8 69.2 400 74.7 595 21.1c40.8-11.2 81.1-25.2 120.3-41.7V0z'/%3E%3Cpath fill='%23202029' d='M1397.5 154.8c47.2-10.6 93.6-25.3 138.6-43.8c21.7-8.9 43-18.8 63.9-29.5V0H643.4c62.9 41.7 129.7 78.2 202.1 107.4C1020.4 178.1 1214.2 196.1 1397.5 154.8z'/%3E%3Cpath fill='%2322222b' d='M1315.3 72.4c75.3-12.6 148.9-37.1 216.8-72.4h-723C966.8 71 1144.7 101 1315.3 72.4z'/%3E%3C/g%3E%3C/svg%3E");
  background-attachment: fixed;
  background-size: cover;
  min-height: 89vh;
}

.playlist-title {
  background: linear-gradient(to right, #06739e, #0aebc5, #06739e);
  background-clip: text;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -moz-text-fill-color: transparent;  -webkit-text-fill-color: transparent;
  font-size: 28px;
  font-weight: 500;
  font-variant: small-caps;
  padding-bottom: 20px;
  text-align: center;
}

.playlist .song {
  display: block;
  color: white;
  width: 100%;
  font-size: 14px;
  font-weight: 700;
  cursor: pointer;
  margin: auto;
  padding: 15px;
  background: #24243b50;
  transition: 0.3s ease-in-out;
}

.playlist .song:hover {
  color:#0aebc5;
  font-size: 14px;
  background-color: #09080f50;
}

.playlist .song.playing {
  color: #ffffff;
  font-size: 14px;
  font-weight: 600;
  margin: auto;
  background-image: linear-gradient(to right, #06739e, #0aebc5, #06739e);
  background-size: 200% 200%;
  padding: 15px 30px;
  width: 100%;
  animation: activateSong 5s ease-in infinite;
}

@keyframes activateSong {
  0%{background-position:10% 0%}
  50%{background-position:91% 100%}
  100%{background-position:10% 0%}
}

.artist-playlist {
  font-weight: 200;
}


/**Responsive design */
@media screen and (min-width: 1000px) {
  main {
    display: flex;
    flex-direction: row-reverse;
  }


  .playerfull {
    width: 80%;
  }

  .playlist {
    width: 40%;
  }

  .playlist .song {
    font-size: 12px;
    font-weight: 700;
    padding: 12.5px;
  }

  .player
  {
    height: 70vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .scrollbar {
    width: 300px;
    height: 100px;
    overflow-y: scroll;
    scrollbar-color: #302e3a #09080f50;
    scrollbar-width: thin;
  }

  .play, .pause
  {
    width: 60px;
    height: 60px;
  }

  .next, .prev
  {
    width: 50px;
    height: 50px;
  }

  .option
  {
    width: 25px;
    height: 25px;
  }
}
</style>
