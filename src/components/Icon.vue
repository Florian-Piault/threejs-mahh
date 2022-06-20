<template>
  <svg
    v-if="icon === 'previous'"
    xmlns="http://www.w3.org/2000/svg"
    xmlns:xlink="http://www.w3.org/1999/xlink"
    aria-hidden="true"
    role="img"
    class="iconify iconify--ooui"
    width="32"
    height="32"
    preserveAspectRatio="xMidYMid meet"
    viewBox="0 0 20 20"
  >
    <path fill="currentColor" d="m4 10l9 9l1.4-1.5L7 10l7.4-7.5L13 1z"></path>
  </svg>

  <svg
    v-if="icon === 'next'"
    xmlns="http://www.w3.org/2000/svg"
    xmlns:xlink="http://www.w3.org/1999/xlink"
    aria-hidden="true"
    role="img"
    class="iconify iconify--ooui"
    width="32"
    height="32"
    preserveAspectRatio="xMidYMid meet"
    viewBox="0 0 20 20"
  >
    <path fill="currentColor" d="M7 1L5.6 2.5L13 10l-7.4 7.5L7 19l9-9z"></path>
  </svg>
  <svg
    v-if="icon === 'reset'"
    xmlns="http://www.w3.org/2000/svg"
    xmlns:xlink="http://www.w3.org/1999/xlink"
    aria-hidden="true"
    role="img"
    class="iconify iconify--system-uicons"
    width="32"
    height="32"
    preserveAspectRatio="xMidYMid meet"
    viewBox="0 0 21 21"
  >
    <g
      fill="none"
      fill-rule="evenodd"
      stroke="currentColor"
      stroke-linecap="round"
      stroke-linejoin="round"
    >
      <path d="M3.578 6.487A8 8 0 1 1 2.5 10.5"></path>
      <path d="M7.5 6.5h-4v-4"></path>
    </g>
  </svg>
  <svg
    v-if="icon === 'zoomIn'"
    xmlns="http://www.w3.org/2000/svg"
    xmlns:xlink="http://www.w3.org/1999/xlink"
    aria-hidden="true"
    role="img"
    class="iconify iconify--zondicons"
    width="32"
    height="32"
    preserveAspectRatio="xMidYMid meet"
    viewBox="0 0 20 20"
  >
    <path
      fill="currentColor"
      fill-rule="evenodd"
      d="M12.9 14.32a8 8 0 1 1 1.41-1.41l5.35 5.33l-1.42 1.42l-5.33-5.34zM8 14A6 6 0 1 0 8 2a6 6 0 0 0 0 12zM7 7V5h2v2h2v2H9v2H7V9H5V7h2z"
    ></path>
  </svg>
  <svg
    v-if="icon === 'zoomOut'"
    xmlns="http://www.w3.org/2000/svg"
    xmlns:xlink="http://www.w3.org/1999/xlink"
    aria-hidden="true"
    role="img"
    class="iconify iconify--zondicons"
    width="32"
    height="32"
    preserveAspectRatio="xMidYMid meet"
    viewBox="0 0 20 20"
  >
    <path
      fill="currentColor"
      fill-rule="evenodd"
      d="M12.9 14.32a8 8 0 1 1 1.41-1.41l5.35 5.33l-1.42 1.42l-5.33-5.34zM8 14A6 6 0 1 0 8 2a6 6 0 0 0 0 12zM5 7h6v2H5V7z"
    ></path>
  </svg>
</template>

<script setup>
const { icon } = defineProps(["icon"]);
</script>
