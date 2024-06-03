<template>
  <div id="app">
    <v-card style="height: 80%; width: 50vw; min-width: 400px">
      <template v-slot:title>
        フラッシュ背番号暗算
      </template>
      <template v-slot:text>
        <div style="height: 50vh; display: flex; justify-content: center; align-items: center; background-color: grey;">
          <div v-if="countdown > 0" style="font-size: 5em;">{{ countdown }}</div>
          <v-img v-if="showImage" :src="staticImage" aspect-ratio="2"></v-img>
          <div v-if="showInputForm" style="font-size: 3em;">背番号の合計は？</div>
          <div v-if="showMessage" style="font-size: 3em;">{{ message }}</div>
        </div>
        <v-btn v-if="showStartButton" color="primary" @click="startGame" style="margin-top: 2rem;">スタート</v-btn>
        <div>
          <input v-if="showInputForm" style="height: 40px; border: 1.5px solid darkgray; margin-top: 2rem; border-radius: 5px;"  type="number" inputmode="numeric" v-model="inputValue"/>
          <v-btn v-if="showInputForm" :disabled="inputValue === ''" color="primary" @click="answer">回答</v-btn>
        </div>
        <v-btn v-if="showMessage" color="primary" @click="retry" style="margin-top: 2rem;">もう一回</v-btn>
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
        '00.jpg',
        '13.jpg',
        '14.jpg',
        '15.jpg',
        '16.jpg'
      ],
    };
  },
  methods: {
    answer() {
      this.showInputForm = false;
      if (parseInt(this.inputValue, 10) === this.total){
        this.message = `正解！合計は${this.total}です！`;
      } else {
        this.message = `残念！正解は${this.total}です！`;
      }
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
  height: 100vh;
  /* justify-content: center; */
  align-items: center;
}
</style>