<template>
  <div id="app">
    <header>
      <img src="./assets/logos/arpeggiz-logo-base.png" alt="arpeggiz logo" class="app-logo">
    </header>
    <main>
      <div class="playerfull" :style="{backgroundImage: `url(${current.backgd})`}">
        <section class="player">
          <div class="songdescription">
            <img v-bind:src="current.image" class="cover">
              <div class="song-informations">
                <h2 class="song-title"> "{{ current.title }}"</h2>
                <div>~</div>
                <div class="artist">{{ current.artist }}</div>
                <div class="album"> - {{ current.album }} - </div>
                <div class="style">- {{ current.style }} -</div>
                <div class="timer">
                  <div class="timer-current">{{ turnSecondsToMinutes (player.currentTime) }}</div> / <div class="timer-duration">{{ turnSecondsToMinutes (player.duration) }}</div>
                </div>
              </div>
          </div>
        </section>
        <div class="progress-bar-container">
          <div :style="{backgroundColor: progressBarColor, width: progressBarWidth}" class="progress-bar">{{ updateProgressBar() }}
          <div class="progress-bar-slider"></div>
          </div>
        </div>
        <section class="controls">
          <img @click="shuffle" class="option shuffle" src="./assets/logos/shuffle.png" alt="shuffle controller icon">
          <img @click="previous" class="prev control" src="./assets/logos/previous.png" alt="previous controller icon">
          <img v-if="!isPlaying" @click="play" class="play control" src="./assets/logos/play.png" alt="play controller icon">
          <img v-else @click="pause" class="pause control" src="./assets/logos/pause.png" alt="pause controller icon">
          <img  @click="next" class="next control" src="./assets/logos/next.png" alt="next controller icon">
          <img  @click="repeat" class="option repeat" src="./assets/logos/repeat.png" alt="repeat controller icon">
        </section>
      </div>
      <section class="playlist scrollbar">
        <h3>Playlist</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
          {{ song.title }} - - <span class="artist-playlist">{{ song.artist }}</span>
        </button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      progressBarWidth: "",
      progressBarColor: "",
      fraction: "",
      percent: "",
      player: new Audio(),
      songs: [
        {
          title: 'Shitataru Morou',
          artist: 'Dir en Grey',
          album: 'Dum Spiro Spero',
          style: 'Metal',
          src: require('./assets/sounds/shitataru-morou.mp3'),
          image: require('./assets/img/dum-spiro-spero.jpg'),
          backgd: require('./assets/img/direngrey.jpg')
        },
        {
          title: 'For Jasmine',
          artist: 'Bôa',
          album: 'Twilight',
          style: 'Alternative Rock',
          src: require('./assets/sounds/for-jasmine.mp3'),
          image: require('./assets/img/twilight-album.jpg'),
          backgd: require('./assets/img/boa.jpg')
        },
        {
          title: 'Sextape ',
          artist: 'Deftones',
          album: 'Diamond Eyes',
          style: 'Metal',
          src: require('./assets/sounds/sextape.mp3'),
          image: require('./assets/img/diamond-eyes.jpg'),
          backgd: require('./assets/img/deftones.jpg')
        },
        {
          title: 'Ghost Of Perdition',
          artist: 'Opeth',
          album: 'Ghost Reveries',
          style: 'Metal',
          src: require('./assets/sounds/ghost-of-perdition.mp3'),
          image: require('./assets/img/ghost-reveries.jpg'),
          backgd: require('./assets/img/opeth.jpg')
        },
         {
          title: 'Theft Wandering And Lost',
          artist: 'Cocteau Twins',
          album: 'Four-Calendar Café',
          style: 'Dream-Pop',
          src: require('./assets/sounds/theft-and-wandering-around-lost.mp3'),
          image: require('./assets/img/four-calendar-cafe.jpg'),
          backgd: require('./assets/img/cocteau-twins.jpg')
        },
        {
          title: 'The Headmaster Ritual',
          artist: 'The Smiths',
          album: 'Meat Is Murder',
          style: 'Post-Punk',
          src: require('./assets/sounds/the-headmaster-ritual.mp3'),
          image: require('./assets/img/meat-is-murder.jpg'),
          backgd: require('./assets/img/the-smiths.jpg')
        },
        {
          title: 'Jigsaw Falling Into Place',
          artist: 'Radiohead',
          album: 'In Rainbows',
          style: 'Alternative Rock',
          src: require('./assets/sounds/jigsaw-falling-into-place.mp3'),
          image: require('./assets/img/in-rainbows.jpg'),
          backgd: require('./assets/img/radiohead.jpg')
        },
        {
          title: 'Only',
          artist: 'Nine Inch Nails',
          album: 'With Teeth',
          style: 'Electro',
          src: require('./assets/sounds/only.mp3'),
          image: require('./assets/img/with-teeth.jpg'),
          backgd: require('./assets/img/nine-inch-nails.jpg')
        },
        {
          title: 'Delete',
          artist: 'Killing Joke',
          album: 'Pylon',
          style: 'Metal',
          src: require('./assets/sounds/delete.mp3'),
          image: require('./assets/img/pylon.jpg'),
          backgd: require('./assets/img/killing-joke.jpg')
        },
        {
          title: 'Latour',
          artist: 'Sukekiyo',
          album: 'Immortalis',
          style: 'Metal',
          src: require('./assets/sounds/latour.mp3'),
          image: require('./assets/img/immortalis.jpg'),
          backgd: require('./assets/img/sukekiyo.jpg')
        },
        {
          title: 'Pale Shelter',
          artist: 'Tears For Fears',
          album: 'The Hurting',
          style: 'New Wave',
          src: require('./assets/sounds/pale-shelter.mp3'),
          image: require('./assets/img/the-hurting.jpg'),
          backgd: require('./assets/img/tears-for-fears.jpg')
        },
        {
          title: 'Blackstar',
          artist: 'David Bowie',
          album: 'Blackstar',
          style: 'Legendary',
          src: require('./assets/sounds/blackstar.mp3'),
          image: require('./assets/img/blackstar.jpg'),
          backgd: require('./assets/img/david-bowie.jpg')
        },
        {
          title: 'Oktrovenie',
          artist: 'Irfan',
          album: 'Irfan',
          style: 'Ethereal',
          src: require('./assets/sounds/oktrovenie.mp3'),
          image: require('./assets/img/irfan-album.jpg'),
          backgd: require('./assets/img/irfan.jpg')
        },
        {
          title: 'Kuroi Niji',
          artist: 'The Novembers',
          album: 'Hallelujah',
          style: 'Indie Rock',
          src: require('./assets/sounds/kuroi-niji.mp3'),
          image: require('./assets/img/hallelujah.jpg'),
          backgd: require('./assets/img/the-novembers.jpg')
        },
        {
          title: 'Only Shallow',
          artist: 'My Bloody Valentine',
          album: 'Loveless',
          style: 'Shoegaze',
          src: require('./assets/sounds/only-shallow.mp3'),
          image: require('./assets/img/loveless.jpg'),
          backgd: require('./assets/img/my-bloody-valentine.jpg')
        },
        {
          title: 'Spellbound',
          artist: 'Siouxsie And The Banshees',
          album: 'Juju',
          style: 'Post Punk',
          src: require('./assets/sounds/spellbound.mp3'),
          image: require('./assets/img/juju.jpg'),
          backgd: require('./assets/img/siouxsie-and-the-banshees.jpg')
        },
        {
          title: 'It\'s You',
          artist: 'PJ Harvey',
          album: 'Uh Huh Her',
          style: 'Rock',
          src: require('./assets/sounds/its-you.mp3'),
          image: require('./assets/img/uhhuhher.jpg'),
          backgd: require('./assets/img/pjharvey.jpg')
        },
        {
          title: 'The Pot',
          artist: 'Tool',
          album: '10,000 Days',
          style: 'Metal',
          src: require('./assets/sounds/the-pot.mp3'),
          image: require('./assets/img/10000-days.jpg'),
          backgd: require('./assets/img/tool.jpg')
        },
        {
          title: 'Buggin Out',
          artist: 'A Tribe Called Quest',
          album: 'The Low End Theory',
          style: 'Rap',
          src: require('./assets/sounds/buggin-out.mp3'),
          image: require('./assets/img/a-low-end-theory.jpg'),
          backgd: require('./assets/img/a-tribe-called-quest.jpg')
        }
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
    },

    progressBar () {
      if(this.player.currentTime > 0)
      {
        this.fraction = this.player.currentTime / this.player.duration;
        this.percent  = Math.abs(this.fraction * 100);
        this.progressBarWidth = this.percent + '%';
        if(this.isPlaying === true) {
          this.progressBarColor = "#0aebc5";
        }
        else {
          this.progressBarColor = " #ff7600";
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
  align-items: center;
  padding: 5% ;
  padding-bottom: 0% ;
  min-height: 73vh;
}

.playerfull {
  max-height: 90vh;
  background-repeat: no-repeat;
  background-size: cover;
  background-position:center;
}

.song-informations {
  color: #ffffff;
  font-size: 28px;
  font-weight: 700;
  font-variant: small-caps;
  text-align: center;
}

.song-informations .artist {
  color: #06ae97;
  font-weight: 400;
}

.song-informations .album {
  font-size: 18px;
  color:  #ff7600;
}

.song-informations .style {
  font-size: 14px;
  font-variant: normal;
  color:  #ffffff;
  margin-top: 15px;
}

.song-informations .timer {
  display: flex;
  justify-content: space-around;
  margin: 0 auto;
  margin-top: 20px;
  padding: 10px;
  color: #b0adc4;
  background-color:#ffffff3a;
  border-radius: 50px;
  font-size: 0.7em;
  font-weight: normal;
  width: 150px;
  color: #fff;
}

.song-informations .timer-duration {
  font-weight: bold;
}

/**Options'appearance */
.options {
  display: flex;
  justify-content: space-around;
  align-items: center;
  padding: 20px;
}

.shuffle, .repeat {
  width: 30px;
  height: 30px;
}

.shuffle:hover, .repeat:hover {
  filter: brightness(150%);
  cursor: pointer;
}

/**Controls' appearance */
.controls {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px 15px;
  background-color: #05050b;
  min-height: 15vh;
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
}

.next, .prev {
  width: 60px;
  height: 60px;
}

.next:hover, .prev:hover {
  filter: brightness(150%);
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

.playlist h3 {
  color: #ff7600;
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
  background-image: linear-gradient(to right, #069e92, #0aebc5, #06ae92);
  background-size: 200% 200%;
  padding: 15px 30px;
  width: 100%;
  transform: scale(1.1);
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
