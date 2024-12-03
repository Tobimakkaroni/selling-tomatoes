<script>
  import { onMount, onDestroy } from 'svelte';

  let currentSlide = 0;
  const slides = [
    { src: "/images/reviews/betterreview1.png", alt: "Wild Landscape" },
    { src: "/images/reviews/betterreview2.png", alt: "Camera" },
    { src: "/images/reviews/betterreview3.png", alt: "Exotic Fruits" }
  ];

  function nextSlide() {
    currentSlide = (currentSlide + 1) % slides.length;
  }

  function prevSlide() {
    currentSlide = (currentSlide - 1 + slides.length) % slides.length;
  }

  let interval;

  onMount(() => {
    interval = setInterval(nextSlide, 5000); // Change slide every 5 seconds
  });

  onDestroy(() => {
    clearInterval(interval);
  });
</script>

<div class="relative w-full overflow-hidden" style="height: 400px;"> <!-- Adjust height as needed -->
  {#each slides as slide, i}
    <div
      class="absolute w-full h-full transition-transform duration-500 ease-in-out"
      style="transform: translateX({(i - currentSlide) * 100}%)">
      <img
        src={slide.src}
        class="block w-full h-full object-cover rounded-lg"
        alt={slide.alt} />
    </div>
  {/each}

  <button
  class="absolute left-0 top-1/2 transform -translate-y-1/2 bg-black bg-opacity-50 text-white p-2 rounded-r"
  on:click={prevSlide}
  aria-label="Previous slide">
  <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
    <path stroke-linecap="round" stroke-linejoin="round" d="M15.75 19.5L8.25 12l7.5-7.5" />
  </svg>
</button>

<button
  class="absolute right-0 top-1/2 transform -translate-y-1/2 bg-black bg-opacity-50 text-white p-2 rounded-l"
  on:click={nextSlide}
  aria-label="Next slide">
  <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
    <path stroke-linecap="round" stroke-linejoin="round" d="M8.25 4.5l7.5 7.5-7.5 7.5" />
  </svg>
</button>
</div>