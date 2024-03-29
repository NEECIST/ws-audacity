<template>
  <div class="company__section">
    <div class="tier__title">
      <div v-if="tier != ''" class="tier">
        <svg>
          <defs>
            <mask :id="'companies__mask__' + tier" x="0" y="0" width="100%" height="100%">
              <rect fill="white" x="0" y="0" width="100%" height="100%" />
              <text class="mask__text" x="50%" y="50%">{{ tier }}</text>
            </mask>
          </defs>

          <rect class="mask__base" :mask="'url(#companies__mask__' + tier + ')'" x="0" y="0" width="100%" height="105%" />

        </svg>
        <div :id="'tier__text__background-' + tier" class="tier__text__background"></div>
        <h2>{{ tier }}</h2>
      </div>
      <h2>{{ type }}</h2>
    </div>
    <div class="carousel__border" :class="{ expanded: expanded }">
      <div class="carousel__wrapper">
        <Transition  name="fade-grow" mode="out-in">
          <div v-if="expanded" class="expanded-wrapper">
            <slide v-for="company in companies">
              <a target="_blank" :href="company.link ? company.link : 'jeec.ist'" class="carousel__item">
                <img class="company__logo" :src="jeec_api_url + company.logo" :alt="company.name">
              </a>
            </slide>
          </div>
          <div v-else>
            <carousel v-if="companies.length > 1" :breakpoints="carousel_breakpoints" :autoplay="2000" :wrap-around="true"
              :transition="500">
              <slide v-for="company in companies" :key="company">
                <a target="_blank" :href="company.link ? company.link : 'jeec.ist'" class="carousel__item">
                  <img class="company__logo" :src="jeec_api_url + company.logo" :alt="company.name">
                </a>
              </slide>
            </carousel>
            <slide v-else v-for="company in companies" :key="company">
              <a target="_blank" :href="company.link ? company.link : 'jeec.ist'" class="carousel__item">
                <img class="company__logo_solo" :src="jeec_api_url + company.logo" :alt="company.name">
              </a>
            </slide>
          </div>
        </Transition>
      </div>
      <button class="expand-button" v-if="canExpand" @click="expanded = !expanded">
        <p v-if="expanded">Hide</p>
        <p v-else>Show All</p>
      </button>
    </div>
  </div>
</template>

<script>
import 'vue3-carousel/dist/carousel.css'
import { Carousel, Slide } from 'vue3-carousel'

export default {
  props: { type: String, tier: String, companies: Object },
  components: {
    Carousel,
    Slide,
  },
  data() {
    return {
      jeec_api_url: process.env.VUE_APP_JEEC_BRAIN_URL,
      imagesLoaded: false,
      expanded: false,
      canExpand: true,
      carousel_breakpoints: {
        0: { itemsToShow: 2.3 },
        640: { itemsToShow: 2.5 },
        1007: { itemsToShow: 2.7 }
      }
    }
  },
  created() {
    if (this.companies.length < 4) {
      this.canExpand = false;
    }
  }
} 
</script>

<style scoped>
.company__section {
  width: 100%;
}

/* #region title */
.tier__title {
  position: relative;
  z-index: 1;
}

.tier__title h2 {
  font-size: 2.8rem;
}

.tier__title>h2 {
  position: relative;
  z-index: 2;
  text-transform: uppercase;
}

.tier {
  position: relative;
  z-index: 1;
  width: fit-content;
  margin: 0 auto;
  padding: 0 1rem;
}

.tier>div {
  position: absolute;
  left: 50%;
  top: 50%;
  translate: -50% -50%;
  width: 90%;
  height: 90%;
  pointer-events: none;
  z-index: 1;
}

.tier>h2 {
  text-align: center;
  text-transform: uppercase;
  z-index: -1;
}

.tier svg {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 2;
}

.mask__base {
  fill: var(--clr-dark-background);
}

.mask__text {
  text-anchor: middle;
  dominant-baseline: middle;
  alignment-baseline: central;
  font-size: 2.8rem;
  font-family: "Russo One";
  text-transform: uppercase;
}

.tier__text__background {
  width: 80%;
  height: 80%;
  position: relative;
  z-index: -1;
  margin: auto auto;
}

.tier__text__background::before {
  content: "";
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(77deg, #576265 11.6%, #9EA1A1 25.31%, #848B8A 48.06%, #576265 55.72%, #576265 77.23%, #757A7B 85.34%, #576265 91.31%);
  mix-blend-mode: luminosity;
}

.tier__text__background::after {
  content: "";
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(339deg, rgba(255, 255, 255, 0.00) 52.79%, #FFF 95.95%);
  mix-blend-mode: overlay;
}

#tier__text__background-platinum {
  background-color: #A0B2C6;
}

#tier__text__background-gold {
  background-color: #C1A875;
}

#tier__text__background-silver {
  background-color: #CDC9C2;
}

#tier__text__background-bronze {
  background-color: #C9705C;
}

/* #endregion */

.carousel__border {
  border-radius: 10px;
  max-width: 900px;
  width: 60%;
  height: 200px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin: 0 auto;
  margin-top: 1rem;
  position: relative;
  background-color: rgb(37, 37, 37);
  margin-bottom: 2rem;
}

.carousel__border.expanded {
  height: auto;
}

.carousel__wrapper {
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: calc(100% - 4px);
  height: calc(100% - 4px);
  border-radius: 9px;
  overflow: hidden;
  background: rgb(61, 61, 61);

  max-height: calc(100% - 4px);
  transition: all 0.5s ease-in-out;
  transition-delay: 0;
}

.carousel__border.expanded .carousel__wrapper {
  max-height: 5000px;
  transition: all 0.5s ease-in-out;
  transition-delay: 0.5s;
}

.fade-grow-enter-active,
.fade-grow-leave-active {
  transition: all 0.5s ease-in-out;
}

.fade-grow-leave-active {
  transition-duration: 0s;
}

.fade-grow-enter-from,
.fade-grow-leave-to {
  opacity: 0;
}

.expanded-wrapper {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.expanded-wrapper .carousel__slide {
  width: auto;
  flex-basis: 33%;
}

.carousel__slide {
  padding: 10px;
  scale: 1;
  width: 200px;
}

.carousel__slide--sliding {
  transition: 0.5s;
}

.carousel__slide--active {
  scale: 1.1;
}

.carousel__item {
  background-color: rgb(255, 255, 255);
  width: 100%;
  height: 170px;
  border-radius: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.company__logo {
  width: 90%;
  height: 90%;
  object-fit: contain;
}

.company__logo_solo {
  width: 70%;
  height: 70%;
  object-fit: contain;
}

.expand-button {
  position: absolute;
  top: calc(100% + 0.2rem);
  right: 0.3rem;
  background: none;
  border: none;
  cursor: pointer;
  border: 2px solid var(--clr-dark-font);
  padding: 0.3rem 1ch;
  border-radius: 20px;
  height: 1.8rem;
}

@media screen and (max-width: 1007px) {
  .mask__text {
    font-size: 2.6rem;
  }

  .tier__title h2 {
    margin-top: -0.1rem;
    font-size: 2.6rem;
  }

  .carousel__border {
    width: 80%;
  }

  .carousel__slide {
    padding: 10px 8px;
  }
}

@media screen and (max-width: 640px) {
  .mask__text {
    font-size: 2.4rem;
  }

  .tier__title h2 {
    margin-top: -0.3rem;
    font-size: 2.4rem;
  }

  .carousel__border {
    width: 100%;
    height: 170px;
  }

  .carousel__slide {
    padding: 10px 5px;
  }

  .carousel__item {
    height: 140px;
  }
}
</style>