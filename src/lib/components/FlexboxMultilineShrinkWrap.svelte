<script>
  let redWidth = $state(480);
  let blueWidth = $state(720);
</script>

<div class="controls">
  <label>
    Red div width: <input type="range" min="100" max="1000" step="10" bind:value={redWidth} />
    <span>{redWidth}px</span>
  </label>
  <label>
    Blue div width: <input type="range" min="100" max="1000" step="10" bind:value={blueWidth} />
    <span>{blueWidth}px</span>
  </label>
</div>

<div style="display: inline-flex; border: solid purple; flex-wrap: wrap;">
  <div class="c1" style="flex: 0 0 {redWidth}px;"></div>
  <div class="c2" style="flex: 0 0 {blueWidth}px;"></div>
</div>

<style>
  .controls {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    margin-bottom: 1rem;
    padding: 1rem;
    background: #f5f5f5;
    border-radius: 4px;
  }

  .controls label {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    font-size: 0.9rem;
  }

  .controls input[type="range"] {
    flex: 1;
    max-width: 400px;
  }

  .controls span {
    min-width: 60px;
    font-weight: 600;
    color: #333;
  }

  .c1 {
    border: solid red;
    height: 300px;
  }

  .c2 {
    border: solid blue;
    height: 100px;
  }
</style>



