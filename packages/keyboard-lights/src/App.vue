<script setup>
import gsap from 'gsap';
import { onMounted } from 'vue';

const keyboard = [
  ['Q', 'W', 'E', 'R', 'T', 'Y', 'U', 'I', 'O', 'P'],
  ['A', 'S', 'D', 'F', 'G', 'H', 'J', 'K', 'L'],
  ['Z', 'X', 'C', 'V', 'B', 'N', 'M']
];

onMounted(() => {
  const tl = gsap.timeline({ duration: 0.8, yoyo: false });

  // tl.to('.inner-box', { duration: 1, rotate: 360 });

  const keys = document.querySelectorAll('.text-box');
  const result = document.querySelector('.result');
  window.addEventListener('keydown', (e) => {
    keys.forEach((key) => {
      if (key.textContent === e.key.toUpperCase()) {
        key.parentElement.children[0].classList.add('active');

        setTimeout(() => {
          key.parentElement.children[0].classList.remove('active');
        }, 500);

        const span = document.createElement('span');
        span.textContent = e.key.toUpperCase();
        result.appendChild(span);

        const spans = document.querySelectorAll('.result span');
        if (spans.length > 10) {
          const firstSpan = spans[0];
          tl.to(firstSpan, {
            duration: 0.1,
            opacity: 0,
            scale: 0.5,
            ease: 'power2.out',
            onComplete: () => {
              firstSpan.remove();
            }
          });
        }

        // timeline animation for the key in the keyboard
        const keyRect = key.getBoundingClientRect();
        const spanRect = span.getBoundingClientRect();
        const coordinates = {
          x:
            keyRect.left +
            keyRect.width / 2 -
            (spanRect.left + spanRect.width / 2),
          y:
            keyRect.top +
            keyRect.height / 2 -
            (spanRect.top + spanRect.height / 2)
        };

        tl.from(span, {
          duration: 0.1,
          x: coordinates.x,
          y: coordinates.y,
          opacity: 0,
          scale: 0.5,
          ease: 'power2.out'
        });
      }
    });
  });
});
</script>

<template>
  <div class="container">
    <div class="row" v-for="row in keyboard">
      <div class="out-box" v-for="key in row">
        <div class="inner-box"></div>
        <div class="text-box">{{ key }}</div>
      </div>
    </div>

    <div class="result"></div>
  </div>
</template>

<style>
@import url(./styles.css);
</style>
