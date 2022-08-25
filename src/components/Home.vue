<script setup>
import gsap from "gsap";
import { onMounted, ref } from "vue";

const isSmallScreen = window.innerWidth < parseFloat(getComputedStyle(document.body).fontSize) * 50 ? true : false

const sloganWrapper = ref(null);
const slogan = ref(null);

const blurDashHorizontal = ref(null);
const blurDashVertical = ref(null);

const introContainer = ref(null);
const introDescription = ref(null);
const introPortrait = ref(null);

const vitruvianContainer = ref(null);
const vitruvianCircle = ref(null);
const vitruvianLeftArm = ref(null);
const vitruvianRightArm = ref(null);
const vitruvianLeftLeg = ref(null);
const vitruvianRightLeg = ref(null);

// state variable to keep track of which animation has fired last
let lastAnimation = 0

function onWheel(e) {
  const direction = e.deltaY < 0 ? 'up' : 'down'

  switch(lastAnimation) {
    case 0: 
      if (direction === 'down') {
        animateIntroductionIn()
        lastAnimation = 1
      }
      break;

    case 1:
      if (direction === 'up' && window.pageYOffset < 70) {
        animateIntroductionOut()
        lastAnimation = 0
      }
      break;
  }
}

function animateIntroductionIn() {
  if (isSmallScreen) {
    gsap.timeline()
      .set(introContainer.value, { visibility: "visible" }, '<')
      .to(introDescription.value, {
        transform: "translateX(0)",
        duration: 0.7,
      }, '<')
      return
  }

  const introContainerHeight = Math.floor(introContainer.value.getBoundingClientRect().height)
  const sloganWrapperHeight = Math.floor(sloganWrapper.value.getBoundingClientRect().height)
  const blurDashHeight = introContainerHeight + sloganWrapperHeight / 2

  gsap
    .timeline()
    .to(blurDashHorizontal.value, { opacity: 0, transform: "scaleX(0.1)", duration: 0.3 })
    .to(blurDashVertical.value, {opacity: 1, height: blurDashHeight + 250 + 'px', transform: 'translateX(-50%) scale(0.3, 1)', duration: 0.3})
    .set(introContainer.value, { visibility: "visible" }, '<')
    .to(introDescription.value, {
      transform: "translateX(0)",
      duration: 0.7,
    }, '<')
    .to(
      introPortrait.value,
      { transform: "translateX(0)", duration: 0.7 },
      "<"
    );
}

function animateIntroductionOut() {
  if (isSmallScreen) return

  gsap.timeline()
    .to(blurDashVertical.value, {opacity: 0, transform: 'translateX(-50%) scale(0)', duration: 0.3})
    .to(blurDashHorizontal.value, {opacity: 1, transform: 'scale(1)', duration: 0.4})
}

function handleVitruvianIntersect() {
  if (lastAnimation === 1) {
    animateVitruvianManIn()
    lastAnimation++
  } else if (lastAnimation === 2) {
    animateVitruvianManOut()
    lastAnimation--
  }
}

function animateVitruvianManIn() {
  const duration = 1

  gsap.timeline()
    .to(vitruvianCircle.value, {opacity: 1, duration, ease: 'ease-in'})
    .to(vitruvianLeftArm.value, {opacity: 1, transform: 'rotate(22deg)', duration}, "<")
    .to(vitruvianRightArm.value, {opacity: 1, transform: 'rotate(-22deg)', duration}, "<")
    .to(vitruvianLeftLeg.value, {opacity: 1, transform: 'rotate(20deg)', duration}, "<")
    .to(vitruvianRightLeg.value, {opacity: 1, transform: 'rotate(-20deg)', duration}, "<")
}

function animateVitruvianManOut() {
  const duration = 1

  gsap.timeline()
    .to(vitruvianCircle.value, {opacity: 0, duration, ease: 'ease-in'})
    .to(vitruvianLeftArm.value, {opacity: 0, transform: 'rotate(0)', duration}, "<")
    .to(vitruvianRightArm.value, {opacity: 0, transform: 'rotate(0)', duration}, "<")
    .to(vitruvianLeftLeg.value, {opacity: 0, transform: 'rotate(0)', duration}, "<")
    .to(vitruvianRightLeg.value, {opacity: 0, transform: 'rotate(0)', duration}, "<")
}

onMounted(() => {
  // move the blurdash div to slogan container
  slogan.value.after(blurDashHorizontal.value);
  slogan.value.after(blurDashVertical.value);

  gsap
    .timeline()
    .from(slogan.value, { opacity: 0, duration: 1 }, "1")
    .from(
      blurDashHorizontal.value,
      { opacity: 0, transform: "scale(0)", duration: 0.7 },
      "1"
    );

  // create an intersection observer for the vitruvian container
  const observer = new IntersectionObserver(handleVitruvianIntersect, {root: null, rootMargin: '0px', threshold: 0.5})
  observer.observe(vitruvianContainer.value)
});
</script>

<template>
  <div class="main" @wheel.passive="onWheel">
    <img
      src="../assets/shooting_thing_horizontal.png"
      ref="blurDashHorizontal"
      class="blur-dash-horizontal"
    />
    <img src="../assets/shooting_thing_vertical.png" ref="blurDashVertical" class="blur-dash-vertical">
    <div class="slogan-wrapper" ref="sloganWrapper">
      <div class="slogan-container">
        <img src="../assets/darkness_glow.png" ref="slogan" />
      </div>
    </div>
    <div class="introduction-container" ref="introContainer">
      <div class="description" ref="introDescription">
        <h1>Short Introduction</h1>
        <p>
          Lorem ipsum dolor sit amet consectetur adipisicing elit. Aperiam
          ducimus impedit dolor et libero minima, natus id molestiae reiciendis
          veritatis exercitationem magnam perspiciatis fugit, mollitia placeat
          officiis quisquam repellendus. Sapiente. Autem perferendis possimus
          maxime fugiat facilis cumque neque natus nulla corrupti officiis
          aliquid tempore, reiciendis, ipsa optio dicta. Nemo consequuntur eum
          id sit accusantium rerum? Illo dolor et tempora deleniti.
        </p>
      </div>

      <div class="self-portrait" ref="introPortrait" v-if="!isSmallScreen">
        <img src="../assets/selfportrait.jpg" />
      </div>
    </div>

    <div class="vitruvian-wrapper">
      <div class="visual-stories">VisualStories</div>
      <div class="vitruvian-container" ref="vitruvianContainer">    
        <img src="../assets/vitruvian_ring_black.png" alt="" class="vitruvian-circle" ref="vitruvianCircle">

        <img src="../assets/vitruvian_rotate_pc-04_black.png" alt="" class="vitruvian-rotate-4" ref="vitruvianLeftArm">
        <img src="../assets/vitruvian_rotate_pc-06_black.png" alt="" class="vitruvian-rotate-6" ref="vitruvianRightArm">
        <img src="../assets/vitruvian_rotate_pc-07_black.png" alt="" class="vitruvian-rotate-7" ref="vitruvianLeftLeg">
        <img src="../assets/vitruvian_rotate_pc-08_black.png" alt="" class="vitruvian-rotate-8" ref="vitruvianRightLeg">

        <img src="../assets/vitruvian_main_pc-01_black.png" class="vitruvian-main-1">
        <img src="../assets/vitruvian_main_pc-02_black.png" class="vitruvian-main-2">
        <img src="../assets/vitruvian_main_pc-03_black.png" class="vitruvian-main-3">
        <img src="../assets/vitruvian_main_pc-04_black.png" class="vitruvian-main-4">
        <img src="../assets/vitruvian_main_pc-05_black.png" class="vitruvian-main-5">
        <img src="../assets/vitruvian_main_pc-06_black.png" class="vitruvian-main-6">
        <img src="../assets/vitruvian_main_pc-07_black.png" class="vitruvian-main-7">
        <img src="../assets/vitruvian_main_pc-08_black.png" class="vitruvian-main-8">        
      </div>
      <div class="inspiring-sessions">Inspiring Sessions</div>
    </div>
  </div>
</template>

<style scoped>
.main {
  min-height: 100%;
  width: 60%;
  margin-inline: auto;
}

.slogan-wrapper {
  height: calc(100vh - 115px);
  display: flex;
  align-items: center
}

.slogan-container {
  position: relative;
}

.blur-dash-horizontal {
  position: absolute;
  top: 100%;
  margin-inline: auto;
}

.blur-dash-vertical {
  position: absolute;
  transform: translateX(-50%) scale(0);
  transform-origin: top;
  left: 50%;
  opacity: 0;
}

.introduction-container {
  font-family: 'Comfortaa', cursive;
  display: flex;
  gap: 10rem;
  margin-top: 100px;
  padding-top: 100px;
  visibility: hidden;
  overflow: hidden;
}

.description {
  transform: translateX(-300px);
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.description h1 {
  font-size: 2.5rem;
  font-weight: bold;
  margin-bottom: 1rem;
  text-transform: uppercase;
  text-align: right;
  line-height: 1.1;
  opacity: 0.6;
}

.description p {
  font-size: 1.1rem;
  text-align: right;
  opacity: 0.9
}

.self-portrait {
  transform: translateX(300px);
  flex: 1;
}

.vitruvian-wrapper {
  display: grid;
  grid-template-columns: 100px 1fr 100px;
  margin-inline: auto;
  margin-top: 100px;
  padding-top: 100px;
  width: 100%;
  position: relative;
}

.inspiring-sessions, .visual-stories {
  font-family: 'Comfortaa', cursive;
  font-weight: bold;
  margin-top: 100px;
}

.vitruvian-container {
  display: grid;
  grid-template-columns: 22% 22% 5.95% 5.95% 22% 22%;
  grid-template-areas: 
    "leftblank leftblank head head rightblank rightblank" 
    "leftarm leftarm torso torso rightarm rightarm" 
    "leftleg leftleg leftleg rightleg rightleg rightleg";
  /* margin-inline: auto;
  margin-top: 100px;
  padding-top: 100px;
  width: 75%;
  position: relative; */
}

/* .vitruvian-container::before {
  content: '';
  position: absolute;
  top: 0;
  bottom: 0;
  left: -100vmax;
  right: -100vmax;
  background-color: white;
} */

.vitruvian-circle {
  grid-area: 1 / 1 / 4 / 7;
  z-index: 2;
  opacity: 0;
}

.vitruvian-main-1 {
  grid-area: leftblank;
  z-index: 2;
}
.vitruvian-main-2 {
  grid-area: head;
  z-index: 2;
}
.vitruvian-main-3 {
  grid-area: rightblank;
  z-index: 2;
}
.vitruvian-main-4 {
  grid-area: leftarm;
  z-index: 2;
}
.vitruvian-main-5 {
  grid-area: torso;
  z-index: 2;
}
.vitruvian-main-6 {
  grid-area: rightarm;
  z-index: 2;
}
.vitruvian-main-7 {
  grid-area: leftleg;
  z-index: 2;
}
.vitruvian-main-8 {
  grid-area: rightleg;
  z-index: 2;
}

.vitruvian-rotate-4 {
  grid-area: leftarm;
  opacity: 0;
  transform-origin: top right;
  z-index: 1;
}
.vitruvian-rotate-6 {
  grid-area: rightarm;
  opacity: 0;
  transform-origin: top left;
  z-index: 1;
}
.vitruvian-rotate-7 {
  grid-area: leftleg;
  opacity: 0;
  transform-origin: top right;
  z-index: 1;
}
.vitruvian-rotate-8 {
  grid-area: rightleg;
  opacity: 0;
  transform-origin: top left;
  z-index: 1;
}
</style>
