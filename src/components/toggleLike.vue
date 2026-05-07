<script setup lang="ts">
import { ref } from 'vue'

withDefaults(
  defineProps<{
    color?: 'white' | 'black'
    modelValue?: boolean
    inWishlist?: boolean
    disabled?: boolean
  }>(),
  {
    color: 'white',
    modelValue: false,
    inWishlist: false,
    disabled: false,
  },
)

const emit = defineEmits<{
  'update:modelValue': [value: boolean]
}>()

const isHovered = ref(false)
</script>

<template>
  <button
    type="button"
    class="toggle-like"
    :class="[
      `toggle-like--${color}`,
      modelValue && !inWishlist && 'toggle-like--active',
    ]"
    :disabled="disabled"
    :aria-label="inWishlist ? 'Retirer de la wishlist' : modelValue ? 'Retirer des favoris' : 'Ajouter aux favoris'"
    :aria-pressed="modelValue"
    @click="emit('update:modelValue', !modelValue)"
    @mouseenter="isHovered = true"
    @mouseleave="isHovered = false"
  >
    <span
      class="toggle-like__icon"
      :class="modelValue && !inWishlist && 'toggle-like__icon--bounce'"
    >

      <!-- Trash — color=white -->
      <svg
        v-if="inWishlist && color === 'white'"
        width="28" height="30"
        viewBox="0 0 28 30"
        fill="none"
        overflow="visible"
        aria-hidden="true"
        xmlns="http://www.w3.org/2000/svg"
      >
        <defs>
          <filter id="tl-shadow-trash-w" x="0" y="0" width="28" height="30" filterUnits="userSpaceOnUse" color-interpolation-filters="sRGB">
            <feFlood flood-opacity="0" result="BackgroundImageFix"/>
            <feColorMatrix in="SourceAlpha" type="matrix" values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 127 0" result="hardAlpha"/>
            <feOffset dy="2"/>
            <feGaussianBlur stdDeviation="3"/>
            <feComposite in2="hardAlpha" operator="out"/>
            <feColorMatrix type="matrix" values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0.2 0"/>
            <feBlend mode="normal" in2="BackgroundImageFix" result="effect1_dropShadow"/>
            <feBlend mode="normal" in="SourceGraphic" in2="effect1_dropShadow" result="shape"/>
          </filter>
        </defs>
        <g filter="url(#tl-shadow-trash-w)">
          <path d="M7 8.2H21Z" fill="#564345" fill-opacity="0.5" shape-rendering="crispEdges"/>
          <path d="M19.4444 8.2V19.4C19.4444 20.2 18.6667 21 17.8889 21H10.1111C9.33333 21 8.55556 20.2 8.55556 19.4V8.2" fill="#564345" fill-opacity="0.5" shape-rendering="crispEdges"/>
          <path d="M10.8889 8.2V6.6C10.8889 5.8 11.6667 5 12.4444 5H15.5556C16.3333 5 17.1111 5.8 17.1111 6.6V8.2" fill="#564345" fill-opacity="0.5" shape-rendering="crispEdges"/>
          <path
            d="M7 8.2H21M19.4444 8.2V19.4C19.4444 20.2 18.6667 21 17.8889 21H10.1111C9.33333 21 8.55556 20.2 8.55556 19.4V8.2M10.8889 8.2V6.6C10.8889 5.8 11.6667 5 12.4444 5H15.5556C16.3333 5 17.1111 5.8 17.1111 6.6V8.2M12.4444 12.2V17M15.5556 12.2V17"
            stroke="white"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
            shape-rendering="crispEdges"
          />
        </g>
      </svg>

      <!-- Trash — color=black -->
      <svg
        v-else-if="inWishlist && color === 'black'"
        width="28" height="30"
        viewBox="0 0 28 30"
        fill="none"
        overflow="visible"
        aria-hidden="true"
        xmlns="http://www.w3.org/2000/svg"
      >
        <g>
          <path d="M7 8.2H21Z" fill="white" shape-rendering="crispEdges"/>
          <path d="M19.4444 8.2V19.4C19.4444 20.2 18.6667 21 17.8889 21H10.1111C9.33333 21 8.55556 20.2 8.55556 19.4V8.2" fill="white" shape-rendering="crispEdges"/>
          <path d="M10.8889 8.2V6.6C10.8889 5.8 11.6667 5 12.4444 5H15.5556C16.3333 5 17.1111 5.8 17.1111 6.6V8.2" fill="white" shape-rendering="crispEdges"/>
          <path
            d="M7 8.2H21M19.4444 8.2V19.4C19.4444 20.2 18.6667 21 17.8889 21H10.1111C9.33333 21 8.55556 20.2 8.55556 19.4V8.2M10.8889 8.2V6.6C10.8889 5.8 11.6667 5 12.4444 5H15.5556C16.3333 5 17.1111 5.8 17.1111 6.6V8.2M12.4444 12.2V17M15.5556 12.2V17"
            stroke="#130C0C"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
            shape-rendering="crispEdges"
          />
        </g>
      </svg>

      <!-- Heart — color=white, active+hover → grand cœur rouge 32×30 (même chemin que default) -->
      <svg
        v-else-if="color === 'white' && modelValue && isHovered"
        width="32" height="30"
        viewBox="0 0 32 30"
        fill="none"
        overflow="visible"
        aria-hidden="true"
        xmlns="http://www.w3.org/2000/svg"
      >
        <defs>
          <filter id="tl-shadow-heart-w-active-hover" x="0" y="0" width="32" height="30" filterUnits="userSpaceOnUse" color-interpolation-filters="sRGB">
            <feFlood flood-opacity="0" result="BackgroundImageFix"/>
            <feColorMatrix in="SourceAlpha" type="matrix" values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 127 0" result="hardAlpha"/>
            <feOffset dy="2"/>
            <feGaussianBlur stdDeviation="3"/>
            <feComposite in2="hardAlpha" operator="out"/>
            <feColorMatrix type="matrix" values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0.2 0"/>
            <feBlend mode="normal" in2="BackgroundImageFix" result="effect1_dropShadow"/>
            <feBlend mode="normal" in="SourceGraphic" in2="effect1_dropShadow" result="shape"/>
          </filter>
        </defs>
        <g filter="url(#tl-shadow-heart-w-active-hover)">
          <path
            d="M22.3 14.7778C23.641 13.48 25 11.9244 25 9.88889C25 8.59228 24.4785 7.34877 23.5502 6.43192C22.6219 5.51508 21.3628 5 20.05 5C18.466 5 17.35 5.44444 16 6.77778C14.65 5.44444 13.534 5 11.95 5C10.6372 5 9.37813 5.51508 8.44982 6.43192C7.52152 7.34877 7 8.59228 7 9.88889C7 11.9333 8.35 13.4889 9.7 14.7778L16 21L22.3 14.7778Z"
            fill="var(--surface-notification)"
            shape-rendering="crispEdges"
          />
          <path
            d="M22.3 14.7778C23.641 13.48 25 11.9244 25 9.88889C25 8.59228 24.4785 7.34877 23.5502 6.43192C22.6219 5.51508 21.3628 5 20.05 5C18.466 5 17.35 5.44444 16 6.77778C14.65 5.44444 13.534 5 11.95 5C10.6372 5 9.37813 5.51508 8.44982 6.43192C7.52152 7.34877 7 8.59228 7 9.88889C7 11.9333 8.35 13.4889 9.7 14.7778L16 21L22.3 14.7778Z"
            stroke="white"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
            shape-rendering="crispEdges"
          />
        </g>
      </svg>

      <!-- Heart — color=white, active static → petit cœur rouge 28×26 -->
      <svg
        v-else-if="color === 'white' && modelValue"
        width="28" height="26"
        viewBox="0 0 28 26"
        fill="none"
        overflow="visible"
        aria-hidden="true"
        xmlns="http://www.w3.org/2000/svg"
      >
        <defs>
          <filter id="tl-shadow-heart-w-active" x="0" y="0" width="28" height="26" filterUnits="userSpaceOnUse" color-interpolation-filters="sRGB">
            <feFlood flood-opacity="0" result="BackgroundImageFix"/>
            <feColorMatrix in="SourceAlpha" type="matrix" values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 127 0" result="hardAlpha"/>
            <feOffset dy="2"/>
            <feGaussianBlur stdDeviation="3"/>
            <feComposite in2="hardAlpha" operator="out"/>
            <feColorMatrix type="matrix" values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0.2 0"/>
            <feBlend mode="normal" in2="BackgroundImageFix" result="effect1_dropShadow"/>
            <feBlend mode="normal" in="SourceGraphic" in2="effect1_dropShadow" result="shape"/>
          </filter>
        </defs>
        <g filter="url(#tl-shadow-heart-w-active)">
          <path
            d="M18.9 12.3333C19.943 11.36 21 10.1933 21 8.66667C21 7.69421 20.5944 6.76158 19.8724 6.07394C19.1503 5.38631 18.1711 5 17.15 5C15.918 5 15.05 5.33333 14 6.33333C12.95 5.33333 12.082 5 10.85 5C9.82892 5 8.84965 5.38631 8.12764 6.07394C7.40562 6.76158 7 7.69421 7 8.66667C7 10.2 8.05 11.3667 9.1 12.3333L14 17L18.9 12.3333Z"
            fill="var(--surface-notification)"
            shape-rendering="crispEdges"
          />
          <path
            d="M18.9 12.3333C19.943 11.36 21 10.1933 21 8.66667C21 7.69421 20.5944 6.76158 19.8724 6.07394C19.1503 5.38631 18.1711 5 17.15 5C15.918 5 15.05 5.33333 14 6.33333C12.95 5.33333 12.082 5 10.85 5C9.82892 5 8.84965 5.38631 8.12764 6.07394C7.40562 6.76158 7 7.69421 7 8.66667C7 10.2 8.05 11.3667 9.1 12.3333L14 17L18.9 12.3333Z"
            stroke="white"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
            shape-rendering="crispEdges"
          />
        </g>
      </svg>

      <!-- Heart — color=white, default (outline) -->
      <svg
        v-else-if="color === 'white'"
        width="32" height="30"
        viewBox="0 0 32 30"
        fill="none"
        overflow="visible"
        aria-hidden="true"
        xmlns="http://www.w3.org/2000/svg"
      >
        <defs>
          <filter id="tl-shadow-heart-w-default" x="0" y="0" width="32" height="30" filterUnits="userSpaceOnUse" color-interpolation-filters="sRGB">
            <feFlood flood-opacity="0" result="BackgroundImageFix"/>
            <feColorMatrix in="SourceAlpha" type="matrix" values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 127 0" result="hardAlpha"/>
            <feOffset dy="2"/>
            <feGaussianBlur stdDeviation="3"/>
            <feComposite in2="hardAlpha" operator="out"/>
            <feColorMatrix type="matrix" values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0.2 0"/>
            <feBlend mode="normal" in2="BackgroundImageFix" result="effect1_dropShadow"/>
            <feBlend mode="normal" in="SourceGraphic" in2="effect1_dropShadow" result="shape"/>
          </filter>
        </defs>
        <g filter="url(#tl-shadow-heart-w-default)">
          <path
            d="M22.3 14.7778C23.641 13.48 25 11.9244 25 9.88889C25 8.59228 24.4785 7.34877 23.5502 6.43192C22.6219 5.51508 21.3628 5 20.05 5C18.466 5 17.35 5.44444 16 6.77778C14.65 5.44444 13.534 5 11.95 5C10.6372 5 9.37813 5.51508 8.44982 6.43192C7.52152 7.34877 7 8.59228 7 9.88889C7 11.9333 8.35 13.4889 9.7 14.7778L16 21L22.3 14.7778Z"
            fill="#564345"
            fill-opacity="0.5"
            shape-rendering="crispEdges"
          />
          <path
            d="M22.3 14.7778C23.641 13.48 25 11.9244 25 9.88889C25 8.59228 24.4785 7.34877 23.5502 6.43192C22.6219 5.51508 21.3628 5 20.05 5C18.466 5 17.35 5.44444 16 6.77778C14.65 5.44444 13.534 5 11.95 5C10.6372 5 9.37813 5.51508 8.44982 6.43192C7.52152 7.34877 7 8.59228 7 9.88889C7 11.9333 8.35 13.4889 9.7 14.7778L16 21L22.3 14.7778Z"
            stroke="white"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
            shape-rendering="crispEdges"
          />
        </g>
      </svg>

      <!-- Heart — color=black, default (outline) -->
      <svg
        v-else-if="color === 'black' && !modelValue"
        width="22" height="20"
        viewBox="0 0 22 20"
        fill="none"
        overflow="visible"
        aria-hidden="true"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          d="M18 12C19.49 10.54 21 8.79 21 6.5C21 5.04131 20.4205 3.64236 19.3891 2.61091C18.3576 1.57946 16.9587 1 15.5 1C13.74 1 12.5 1.5 11 3C9.5 1.5 8.26 1 6.5 1C5.04131 1 3.64236 1.57946 2.61091 2.61091C1.57946 3.64236 1 5.04131 1 6.5C1 8.8 2.5 10.55 4 12L11 19L18 12Z"
          fill="white"
          stroke="#130C0C"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
      </svg>

      <!-- Heart — color=black, active (filled red) -->
      <svg
        v-else
        width="14" height="12"
        viewBox="0 0 14 12"
        fill="none"
        overflow="visible"
        aria-hidden="true"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          d="M11.9 7.33333C12.943 6.36 14 5.19333 14 3.66667C14 2.69421 13.5944 1.76158 12.8724 1.07394C12.1503 0.386309 11.1711 0 10.15 0C8.918 0 8.05 0.333333 7 1.33333C5.95 0.333333 5.082 0 3.85 0C2.82892 0 1.84965 0.386309 1.12764 1.07394C0.405624 1.76158 0 2.69421 0 3.66667C0 5.2 1.05 6.36667 2.1 7.33333L7 12L11.9 7.33333Z"
          fill="var(--surface-notification)"
        />
      </svg>

    </span>
  </button>
</template>

<style scoped>
.toggle-like {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 32px;
  height: 32px;
  padding: 0;
  border: none;
  border-radius: var(--radius-focus-mode);
  background: transparent;
  cursor: pointer;
  transition: box-shadow 150ms ease;
}

.toggle-like__icon {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  transition: transform 150ms ease;
}

.toggle-like--white .toggle-like__icon {
  position: relative;
  top: 2px;
}

/* Scale sur hover uniquement pour les états non-actifs */
.toggle-like:not(:disabled):not(.toggle-like--active):hover .toggle-like__icon {
  transform: scale(1.15);
}

/* Active+hover : le SVG switche directement, pas de scale CSS */
.toggle-like--active:not(:disabled):hover .toggle-like__icon {
  transform: none;
}

.toggle-like:focus-visible {
  outline: none;
  box-shadow:
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-default);
}

.toggle-like:disabled {
  opacity: 0.5;
  cursor: not-allowed;
  pointer-events: none;
}

@keyframes like-bounce {
  0%   { transform: scale(1); }
  30%  { transform: scale(1.35); }
  65%  { transform: scale(0.9); }
  100% { transform: scale(1); }
}

.toggle-like__icon--bounce {
  animation: like-bounce 350ms ease;
}
</style>
