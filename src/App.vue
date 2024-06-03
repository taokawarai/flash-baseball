<template>
  <div id="app">
    <v-card style="height: 80%; min-width: 95vw;">
      <template v-slot:title>
        <h2>フラッシュ背番号暗算</h2>
      </template>
      <template v-slot:text>
        <div style="height: 40vh; display: flex; justify-content: center; align-items: center; background-color: #FFFFAA;">
          <div v-if="countdown > 0" style="font-size: 5em;">{{ countdown }}</div>
          <v-img v-if="showImage" :src="staticImage" aspect-ratio="3"></v-img>
          <div v-if="showInputForm" style="font-size: 3em;">背番号の合計は？</div>
          <div v-if="showMessage" style="font-size: 3em;">{{ message }}</div>
        </div>
        <div style="text-align: right;">
          <a href="https://npb.jp/bis/teams/rst_t.html" target="_blank">
            カンニング
          </a>
        </div>
        <v-btn v-if="showStartButton" color="primary" @click="startGame" style="margin-top: 1rem;">スタート</v-btn>
        <div>
          <input v-if="showInputForm" style="height: 40px; border: 1.5px solid darkgray; margin-top: 1rem; border-radius: 5px;"  type="number" inputmode="numeric" v-model="inputValue"/>
          <v-btn v-if="showInputForm" :disabled="inputValue === ''" color="primary" @click="answer">回答</v-btn>
        </div>
        <v-btn v-if="showMessage" color="primary" @click="retry" style="margin-top: 1rem;">もう一回</v-btn>
      </template>
    </v-card>
  </div>
</template>

<script>
export default {
  name: 'App',
  components: {
  },
  data() {
    return {
      showStartButton: true,
      showImage: false,
      showInputForm: false,
      showMessage: false,
      countdown: 0,
      inputValue: '',
      total: 0,
      message: '',
      staticImage: "00.jpg",
      selectedImages: [],
      images: [ 
        '0.jpg',
        '00.jpg',
        '1.jpg',
        '2.jpg',
        '3.jpg',
        '4.jpg',
        '5.jpg',
        '7.jpg',
        '8.jpg',
        '12.jpg',
        '13.jpg',
        '14.jpg',
        '15.jpg',
        '16.jpg',
        '17.jpg',
        '19.jpg',
        '20.jpg',
        '21.jpg',
        '25.jpg',
        '26.jpg',
        '27.jpg',
        '28.jpg',
        '30.jpg',
        '32.jpg',
        '33.jpg',
        '34.jpg',
        '35.jpg',
        '36.jpg',
        '37.jpg',
        '38.jpg',
        '39.jpg',
        '40.jpg',
        '41.jpg',
        '43.jpg',
        '44.jpg',
        '45.jpg',
        '46.jpg',
        '47.jpg',
        '48.jpg',
        '49.jpg',
        '50.jpg',
        '51.jpg',
        '52.jpg',
        '53.jpg',
        '54.jpg',
        '55.jpg',
        '56.jpg',
        '57.jpg',
        '58.jpg',
        '59.jpg',
        '60.jpg',
        '61.jpg',
        '62.jpg',
        '63.jpg',
        '64.jpg',
        '65.jpg',
        '66.jpg',
        '67.jpg',
        '68.jpg',
        '69.jpg',
        '93.jpg',
        '94.jpg',
        '95.jpg',
        '97.jpg',
        '99.jpg'
      ],
    };
  },
  mounted() {
    this.inputValue = '';
  },
  methods: {
    answer() {
      this.showInputForm = false;
      if (parseInt(this.inputValue, 10) === this.total){
        this.message = `正解！合計は${this.total}です！`;
      } else {
        this.message = `残念！正解は${this.total}です！`;
      }
      this.inputValue = '';
      this.showMessage = true;
    },
    retry(){
      this.showMessage = false;
      this.showStartButton = true;
    },
    async startGame() {
      this.showStartButton = false;
      await this.startCountdown();
      this.selectRandomImages();
      await this.showImages();
      this.showInputForm = true;
    },
    startCountdown() {
      return new Promise((resolve) => {
        this.countdown = 3;
        const interval = setInterval(() => {
          this.countdown--;
          if (this.countdown === 0) {
            clearInterval(interval);
            resolve();
          }
        }, 1000);
      });
    },
    selectRandomImages() {
      this.selectedImages = [];
      let imagesCopy = [...this.images];
      for (let i = 0; i < 5; i++) {
        const randomIndex = Math.floor(Math.random() * imagesCopy.length);
        // console.log(randomIndex);
        this.selectedImages.push(imagesCopy.splice(randomIndex, 1)[0]);
      }
      // console.log(this.selectedImages);
      this.total = this.selectedImages.reduce((acc, image) => {
        return acc + parseInt(image.split('.')[0]);
      }, 0);
      // console.log(this.total);
    },
    showImages() {
      this.showImage = true;
      return new Promise(async (resolve) => {
        for (let i = 0; i < this.selectedImages.length; i++) {
          const image = this.selectedImages[i];
          this.staticImage = image;
          // console.log(image);
          await new Promise(res => setTimeout(res, 1000)); // 1秒待つ
        }
        resolve();
        this.showImage = false;
      });
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  display: flex;
  height: 100svh;
  width: 100%;
  justify-content: center;
  align-items: center;
  margin: 0;
  padding: 0;
  /* 背景画像のURLを設定 */
  /* background-image: url('bg.jpg'); */
  /* 画像の表示サイズや位置、繰り返しの設定など */
  /* background-size: cover;
  background-position: center;
  background-repeat: no-repeat; */
}
</style>