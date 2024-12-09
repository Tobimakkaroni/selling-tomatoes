<script>
  import { onMount, tick } from 'svelte';
  import { fade, fly } from 'svelte/transition';
  import Carousel from '$lib/Carousel.svelte';
  import Navbar from '$lib/Navbar.svelte';
  import Footer from '$lib/Footer.svelte';

  export const data = {};

    const images = [
      "/images/tomato-icon.png",
      "/images/tomato-icon.png",
      "/images/tomato-icon.png",
      "/images/tomato-icon.png",  
      "/images/tomato-icon.png"
    ];
  
    let showHero = false;
    let showFeatures = false;
  
    onMount(async () => {
        showHero = true;
        await tick();
        createFallingTomatoes();
        setTimeout(() => showFeatures = true, 500);
    });
  
    function scrollToCustomize() {
        const element = document.getElementById('customize-section');
        if (element) {
          element.scrollIntoView({ behavior: 'smooth' });
        }
      }
    
      function createFallingTomatoes() {
        const container = document.querySelector('.falling-tomatoes');
        if (!container) {
          console.error('Container not found');
          return;
        }

      const tomatoCount = 50;
      const maxtomatoesize = 200;
      const mintomatoesize = 100;

      function createTomato() {
        if (!container) return;
        const tomato = document.createElement('img');
        tomato.src = '/images/tomato-icon.png';
        tomato.alt = 'Falling tomato';
        tomato.classList.add('tomato');
        tomato.style.left = `${Math.random() * 100}%`;
        tomato.style.width = `${Math.random() * (maxtomatoesize - mintomatoesize) + mintomatoesize}px`;
        tomato.style.animationDuration = `${Math.random() * 3 + 2}s`;
        container.appendChild(tomato);

        tomato.addEventListener('animationend', () => {
          if (container.contains(tomato)) {
            container.removeChild(tomato);
            createTomato();
          }
        });
      }

      for (let i = 0; i < tomatoCount; i++) {
        createTomato();
      }
}
</script>
  
<main>
  <Navbar />

    {#if showHero}
      <section class="hero" in:fade={{ duration: 1000 }}>
        <div class="falling-tomatoes"></div>
        <div class="hero-content">
          <h1>Custom tomatoes: A Juicy Surprise!</h1>
          <p>Send personalized, colorful tomatoes with your message to anyone, anywhere!</p>
          <button on:click={scrollToCustomize} class="cta-button">Create Your Tomato</button>
        </div>
      </section>
    {/if}
  
    {#if showFeatures}
      <section class="features" in:fly={{ y: 50, duration: 1000 }}>
        <h2 class="features-header">Why Choose Our Custom tomatoes?</h2>
        <div class="feature-grid">
          <div class="feature-item">
            <img src="/icons/palette.svg" alt="Color palette" />
            <h3>Vibrant Colors</h3>
            <p>Choose from a rainbow of tomato colors to match any occasion.</p>
          </div>
          <div class="feature-item">
            <img src="/icons/message.svg" alt="Message icon" />
            <h3>Personal Messages</h3>
            <p>Add your heartfelt words directly on the tomato skin.</p>
          </div>
          <div class="feature-item">
            <img src="/icons/shipping.svg" alt="Shipping icon" />
            <h3>Nationwide Delivery</h3>
            <p>We'll carefully pack and ship your tomatoes anywhere in the country.</p>
          </div>
        </div>
      </section>
    {/if}
  
    <section class="testimonials">
      <h2 class="testimonial-header">What Our Customers Say</h2>
      <div class="testimonial-carousel">
        <!-- TODO: fix carousel and fill it with example texts -->
        <Carousel />
      </div>
    </section>

    <section class="how-it-works">
      <h2 class="how-it-works-header">How It Works</h2>
      <div class="steps">
        <div class="step">
          <div class="step-number">1</div>
          <p>Choose your tomato color</p>
        </div>
        <div class="step">
          <div class="step-number">2</div>
          <p>Add your custom message</p>
        </div>
        <div class="step">
          <div class="step-number">3</div>
          <p>Enter delivery address</p>
        </div>
        <div class="step">
          <div class="step-number">4</div>
          <p>We deliver the surprise!</p>
        </div>
      </div>
    </section>
    
      <section id="customize-section" class="cta-section">
        <div class="cta-card">
          <h2 class="cta-header">Ready to Surprise Someone?</h2>
          <p class="cta-message">Create your custom tomato message today!</p>
          <a href="/customize" class="cta-button">Start Customizing</a>
        </div>
      </section>

      <Footer />
</main>
  
<style lang="postcss">

    :global(html), :global(body) {
        margin: 0;
        padding: 0;
        width: 100%;
        height: 100%;
    }

    main {
      font-family: 'Arial', sans-serif;
      color: #333;
    }
  
    .hero {
        position: relative;
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 16rem 2rem;
        background: linear-gradient(135deg, #ff6b6b, #feca57);
        color: white;
        overflow: hidden;
        height: 100vh;
    }
  
    .falling-tomatoes {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      pointer-events: none;
    }

    :global(.tomato) {
        position: absolute;
        animation: fall linear infinite;
    }

    @keyframes fall {
        from {
            transform: translateY(-100%);
        }
        to {
            transform: translateY(160vh); /* without this vh > 160, the animations is cut of */
        }
    }
  
    .hero-content {
      position: relative;
      z-index: 1;
    }
  
    .hero-content {
      flex: 1;
    }
  
    .hero h1 {
      font-size: 3.5rem;
      margin-bottom: 1rem;
    }
  
    .hero p {
      font-size: 1.2rem;
      margin-bottom: 2rem;
    }

    .features, .how-it-works, .testimonials, .cta-section {
      padding: 4rem 2rem;
      text-align: center;
    }

    .cta-section {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background-color: #e59a19;
      background-image: radial-gradient(circle at center center, #ffffff, #e59a19), repeating-radial-gradient(circle at center center, #ffffff, #ffffff, 40px, transparent 80px, transparent 40px);
      background-blend-mode: multiply;
    }

    .cta-card {
      background-color: white;
      padding: 10%;
      border-radius: 5px;
      -webkit-box-shadow: 0px 0px 47px -7px rgba(0,0,0,0.75);
      -moz-box-shadow: 0px 0px 47px -7px rgba(0,0,0,0.75);
      box-shadow: 0px 0px 47px -7px rgba(0,0,0,0.75);
    }

    .testimonials {
      background-color: #477280;
    }

    .testimonial-header {
      color: white;
      font-size: 32px;
      font-weight: bolder;
      margin-bottom: 40px;
    }

    .features-header {
      color: rgb(0, 0, 0);
      font-size: 32px;
      font-weight: bolder;
      margin-bottom: 40px;
    }

    .how-it-works-header {
      color: rgb(0, 0, 0);
      font-size: 32px;
      font-weight: bolder;
      margin-bottom: 40px;
    }

    .cta-header {
      font-weight: 800;
      font-size: 42px;
    }
  
    .cta-button {
      padding: 1rem 2rem;
      font-size: 1.2rem;
      background-color: #ff0000;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      transition: background-color 0.3s ease;
      box-shadow: 0px 0px 20px 0px rgba(0,0,0,0.5);
    }
  
    .cta-button:hover {
      background-color: rgb(255, 166, 0);
    }
  
    .features, .how-it-works {
      padding: 4rem 2rem;
      text-align: center;
    }
    
    .feature-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 2rem;
      margin-top: 2rem;
    }
  
    .feature-item {
      background-color: #e9e9e9;
      padding: 2rem;
      border-radius: 10px;
      box-shadow: 0 5px 15px rgba(0,0,0,0.3);
      transition: transform 0.3s ease;
    }
    
    .feature-item:hover {
      transform: translateY(-5px);
    }
    
    .feature-item img {
      width: 100%;
      height: 64px;
      margin-bottom: 1rem;
      display: flex;
      flex-direction: row;
      justify-content: center;
    }
  
    .steps {
      display: flex;
      justify-content: space-around;
      margin-top: 2rem;
    }
    
    .step {
      display: flex;
      flex-direction: column;
      align-items: center;
    }
  
    .step-number {
      width: 40px;
      height: 40px;
      background-color: #4ecdc4;
      color: white;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 1.2rem;
      margin-bottom: 1rem;
    }
    
    .cta-message {
        margin-bottom: 60px;
        font-size: 22px;
    }

  </style>