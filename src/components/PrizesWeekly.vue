<template>
  <div class="weekly-rewards" v-if="rewards">
    <div class="rewards-container">
      <div v-for="(reward, key) in rewards" :key="reward" class="reward" :class="{ 'big': key == 1 }">
        <a class="reward-img radient-border-passthrough">
          <img :src="jeec_brain_url + reward.image">
          <p class="ranking first" v-if="key == 1">1<sub>st</sub></p>
          <p class="ranking second" v-if="key == 0">2<sub>nd</sub></p>
          <p class="ranking third" v-if="key == 2">3<sub>rd</sub></p>
        </a>
        <p>{{ reward.name }}</p>
      </div>
    </div>
  </div>
</template>



<script>

//import { Carousel, Slide } from 'vue3-carousel'
//import 'vue3-carousel/dist/carousel.css'

export default {
  name: "WeeklyPrizes",
  props: {
    rewards: Object,
  },
  data() {
    return {
      jeec_brain_url: process.env.VUE_APP_JEEC_BRAIN_URL,
    };
  },
  computed: {},
};
</script>

<style scoped>
.weekly-rewards {
  font-weight: 600;
}

.rewards-container {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  max-width: 600px;
  margin: 0 auto;
  gap: 20px;
  padding-bottom: 1rem;
}

.reward {
  display: flex;
  flex-direction: column;
  align-items: center;
  flex-basis: 30%;
  flex-shrink: 1;
  position: relative;
  min-width: auto;
}

.reward > p {
  padding-top: 0.6rem;
  position: absolute;
  text-align: center;
  top: 100%;
}

.reward.big {
  flex-shrink: 0.8;
  flex-basis: 40%;
}


.reward-img {
  position: relative;
  max-width: 120px;
  width: 100%;
  aspect-ratio: 1;
  box-shadow: 0px 0px 15px #F72585;
  display: flex;
  justify-content: center;
  align-items: center;
  --border-radius: 50%;
  --border-background: linear-gradient(135deg, #A414A4, #F72585 40%, #7209B7)
}

.reward-img::before {
  content: "";
}

.reward.big .reward-img {
  max-width: 140px;
}

.reward-img img{
  width: 97%;
  height: 97%;
  border-radius: 50%;
  object-fit: contain;
  background-color: white;
}

.ranking {
  position: absolute;
  bottom: 0;
  left: 0;
  border-radius: 50%;
  height: 35px;
  width: 35px;
  box-shadow: 0px 0px 15px #F72585;
  display: flex;
  justify-content: center;
  align-items: center;
}

.first {
  background-color: #C1A875;
  border: #ddc695 2px solid;
}

.second {
  background-color: #a8a8a8;
  border: #c4c2c2 2px solid;
}

.third {
  background-color: #C9705C;
  border: #e18a77 2px solid;
}
</style>