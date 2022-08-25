<script setup>
import { onMounted, ref } from "vue";
import gsap from 'gsap'

const emit = defineEmits(['changePageIdx'])

const spotlight = ref(null)
const spotlightBg = ref(null)
const primaryNav = ref(null)
const navUl = ref(null)
const mobileNavToggle = ref(null)

let lockMenu = false

function toggleMobileNav() {
    if (lockMenu) return 

    if (!primaryNav.value.hasAttribute('data-visible')) {
        primaryNav.value.toggleAttribute('data-visible')
        gsap.timeline()
            .from(primaryNav.value, {opacity: 0, transform: 'translateX(-300px)', duration: 0.2})
            .from(navUl.value.childNodes[0], {opacity: 0, transform: 'translateY(-50px) scale(0)', duration: 0.2})
            .from(navUl.value.childNodes[1], {opacity: 0, transform: 'translateX(-300px) scale(0)', duration: 0.2}, '<')
            .from(navUl.value.childNodes[2], {opacity: 0, transform: 'translateX(300px) scale(0)', duration: 0.2}, '<')
    } else {
        gsap.timeline({onStart: () => (lockMenu = true), onComplete: () => (lockMenu = false)})
            .to(primaryNav.value, {opacity: 0, transform: 'translateX(-300px)', duration: 0.2})
            .set(primaryNav.value, {opacity: 1, transform: 'translateX(0)'}, '0.3')
        setTimeout(() => {
            primaryNav.value.toggleAttribute('data-visible')
        }, 200);
    }
}

onMounted(() => {
    gsap.timeline()
        .from(spotlightBg.value, {opacity: 0, transform: 'scale(0)', duration: 0.35})
        .from(spotlight.value, {opacity: 0, transform: "translateX(-300px)", duration: 0.65}, "<+=0.22")

    // check inner width for 50em
    if (window.innerWidth >= parseFloat(getComputedStyle(document.body).fontSize) * 50) {
        gsap.timeline()        
            .from(navUl.value.childNodes[0], {opacity: 0, transform:'translateY(-50px)', duration: 0.3}, '0.5')
            .from(navUl.value.childNodes[1], {opacity: 0, transform:'translateY(-50px)', duration: 0.3}, "<+=0.15")
            .from(navUl.value.childNodes[2], {opacity: 0, transform:'translateY(-50px)', duration: 0.3}, "<+=0.15")
    } else {
        gsap.timeline()
            .from(mobileNavToggle.value, {opacity: 0, transform: 'translateY(-50px)', duration: 0.3}, '0.7')
    }
})
</script>

<template>
    <header>
        <div class="header-logo-container">
            <img src="../assets/spotlight_BG.png" ref="spotlightBg" class="spotlight-bg">
            <img src="../assets/logo_spotlight_empty.png" ref="spotlight" class="spotlight">
        </div>
        <button class="mobile-nav-toggle" ref="mobileNavToggle" @click="toggleMobileNav">
            <img src="../assets/outline_menu_white_24dp.png" alt="" class="icon-open">
            <img src="../assets/outline_close_white_24dp.png" alt="" class="icon-close">
        </button>
        <nav class="primary-navigation" ref="primaryNav">
            <ul ref="navUl">
                <li @click="emit('changePageIdx', 0)">Home</li>
                <li @click="emit('changePageIdx', 1)">Visual Stories</li>
                <li @click="emit('changePageIdx', 1)">About me</li>
            </ul>
        </nav>
    </header>
    <slot />
    <footer>footer</footer>
</template>

<style scoped>
header {
    height: 115px;
    /* background: red; */
    display: grid;
    grid-template-columns: 300px 1fr;
    position: fixed;
    background-color: black;
    z-index:5;
    width: 100%;
}

.header-logo-container {
    position: relative;
}

.header-logo-container img {
    position: absolute;
}

.spotlight-bg {
    transform-origin: top left;
}

.primary-navigation {
    display: flex;
    justify-content: flex-end;
    align-items: center;
    margin-right: 50px;
}

ul {
    position: relative;
    list-style: none;
    display: flex;
    gap: 1rem;
}

li {
    font-size: 1.2rem;
    font-weight: bold;
    cursor: pointer;
    transition: color .2s ease-out;
}

li:hover {
    color: gray;
}

.mobile-nav-toggle {
    display: none;
}

@media(max-width: 50em) {
    .primary-navigation {
        display: none;
        position: fixed;
        inset: 10rem 2rem auto;
        z-index: 5;
        padding: 1rem 0;
        background-color: #000;
        border: 2px solid white;
        border-radius: 5px;
    }

    .primary-navigation[data-visible] {
        display: block;
    }

    .primary-navigation ul {
        flex-direction: column;
        align-items: center;
    }

    .mobile-nav-toggle {
        display: block;
        cursor: pointer;
        background: transparent;
        border: 0;
        padding: 0.5em;
        margin-inline: auto 2rem;
    }

    .mobile-nav-toggle .icon-close {
        display: none;
    }
}



footer {
    height: 115px;
}
</style>