<script>
    import { onMount } from 'svelte';
    let colors = ['Red', 'Green', 'Yellow'];
    let selectedColor = colors[0];
    let message = '';
    let address = '';
  
    function handleSubmit() {
      const orderData = { selectedColor, message, address };
      fetch('/api/orders', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(orderData)
      }).then(response => response.json())
        .then(data => {
          window.location.href = '/order-summary';
        });
    }
  </script>
  
  <main>
    <h1>Customize Your Tomato</h1>
    <form on:submit|preventDefault={handleSubmit}>
      <label for="color">Choose a color:</label>
      <select bind:value={selectedColor} id="color">
        {#each colors as color}
          <option value={color}>{color}</option>
        {/each}
      </select>
  
      <label for="message">Custom Message:</label>
      <textarea bind:value={message} id="message"></textarea>
  
      <label for="address">Delivery Address:</label>
      <input type="text" bind:value={address} id="address" required />
  
      <button type="submit">Submit Order</button>
    </form>
  </main>
  
  <style>
  </style>