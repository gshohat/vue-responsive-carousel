<script setup>

const props = defineProps(['images']);
const imagesLength = props.images.length;
let index = 0;

const loadImage = () => {
  let image = new Image()
  image.src = props.images[index].src;
  image.onload = () => {
    document.querySelector('.current-image').style['background-image'] = 'url('+ image.src + ')';
  }
}
loadImage();

const directions = {
  Right: Symbol('right'),
  Left: Symbol('left'),
}

function handleClick (event, direction) {
  direction === directions.Right ? index++ : index--;
  index = (index + imagesLength) % imagesLength;

  const currentImageElement = document.querySelector('.current-image');

  currentImageElement.style['opacity'] = 0;
  currentImageElement.style['background-image'] = 'url('+ props.images[index].src + ')';

  currentImageElement.animate(
      {
        opacity: [0, 1]
      },
      {
        fill: "both",
        duration: 1500,
        rangeStart: "cover 0%",
        rangeEnd: "cover 100%",
      },
  );
}

</script>

<template>

  <div class="current-image-wrapper">
    <div class="current-image"/>

    <div class="arrows">
      <div class="arrow-wrapper" @click="handleClick(e, directions.Left)">
        <object class="arrow" data="/icons/arrow-left.svg" type="image/svg+xml" style="pointer-events:none"></object>
      </div>

      <div class="arrow-wrapper" @click="handleClick(e, directions.Right)">
        <object class="arrow" data="/icons/arrow-right.svg" type="image/svg+xml" style="pointer-events:none"></object>
      </div>
    </div>

  </div>
</template>

<style scoped>

.current-image-wrapper {
  height: 100%;
}

.current-image {
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  height: 100%;
  animation: fadeIn 2s;
}


@keyframes fadeIn {
  0% { opacity: 0; }
  100% { opacity: 100; }
}

.arrows {
  display: flex;
  justify-content: space-between;
  padding: 1rem;
  position: relative;
  top: -50%;
  transform: translate(0, calc(-50%));
}

.arrow-wrapper:hover {
  cursor: pointer;
}

.arrow {
  height: 1.5rem;
}

</style>
