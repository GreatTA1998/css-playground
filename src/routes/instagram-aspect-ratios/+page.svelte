<script lang="ts">
  const INSTAGRAM_LANDSCAPE_MAX = 1.91; // 1.91:1
  const INSTAGRAM_PORTRAIT_MIN = 0.75; // 3:4 = 0.75:1 (minimum portrait ratio, most portrait)

  const BASE_WIDTH = 140;
  const getHeight = (aspectRatio: number, width = BASE_WIDTH) => width / aspectRatio;

  const ratios = [
    { ratio: 2.5, classic: '2.5:1', normalized: '2.5:1', label: 'Too Wide', status: 'cropped', color: 'too-wide' },
    { ratio: 1.91, classic: '1.91:1', normalized: '1.91:1', label: 'Widest IG', status: 'supported', color: 'landscape' },
    { ratio: 4/3, classic: '4:3', normalized: '1.33:1', label: 'iPhone Landscape', status: 'supported', color: 'iphone-landscape' },
    { ratio: 3/4, classic: '3:4', normalized: '0.75:1', label: 'iPhone Portrait / Tallest IG', status: 'supported', color: 'iphone-portrait' },
    { ratio: 0.5, classic: '1:2', normalized: '0.5:1', label: 'Too Tall', status: 'cropped', color: 'too-tall' },
  ];

  // Calculate crop overlay for cropped items
  // Instagram uses object-fit: cover - scales image to cover container, crops excess from center
  // The overlay shows what portion of the original photo Instagram would display
  const getCropOverlay = (ratio: number) => {
    if (ratio > INSTAGRAM_LANDSCAPE_MAX) {
      // Too wide - Instagram displays at 1.91:1 max (landscape)
      // Photo is scaled to cover: fits to height, then crops width from sides (centered)
      // Show a 1.91:1 box centered horizontally within the original photo
      const actualHeight = getHeight(ratio);
      const targetAspectRatio = INSTAGRAM_LANDSCAPE_MAX;
      
      // Instagram would display this at targetAspectRatio
      // At the original photo's height, the visible width would be: actualHeight * targetAspectRatio
      const visibleWidth = actualHeight * targetAspectRatio;
      const visibleHeight = actualHeight;
      
      // Center horizontally
      const leftOffset = (BASE_WIDTH - visibleWidth) / 2;
      
      return {
        width: visibleWidth,
        height: visibleHeight,
        top: 0,
        left: leftOffset,
      };
    } else if (ratio < INSTAGRAM_PORTRAIT_MIN) {
      // Too tall - Instagram displays at 3:4 min (portrait)
      // Photo is scaled to cover: fits to width, then crops height from top/bottom (centered)
      // Show a 3:4 box centered vertically within the original photo
      const actualWidth = BASE_WIDTH;
      const targetAspectRatio = INSTAGRAM_PORTRAIT_MIN;
      
      // Instagram would display this at targetAspectRatio
      // At the original photo's width, the visible height would be: actualWidth / targetAspectRatio
      const visibleWidth = actualWidth;
      const visibleHeight = actualWidth / targetAspectRatio;
      
      // Center vertically
      const actualHeight = getHeight(ratio);
      const topOffset = (actualHeight - visibleHeight) / 2;
      
      return {
        width: visibleWidth,
        height: visibleHeight,
        top: topOffset,
        left: 0,
      };
    }
    return null;
  };
</script>

<div class="container">
  <div class="header">
    <h1>Instagram Aspect Ratios</h1>
  </div>

  <div class="ratios-container">
    {#each ratios as ratio, index}
      <div class="ratio-card" class:separator-before={index === 1 || index === 4}>
        <div class="ratio-visual {ratio.color}" style="width: {BASE_WIDTH}px; height: {getHeight(ratio.ratio)}px;">
          {#if ratio.status === 'cropped'}
            {@const cropOverlay = getCropOverlay(ratio.ratio)}
            {#if cropOverlay}
              <div 
                class="crop-overlay"
                style="width: {cropOverlay.width}px; height: {cropOverlay.height}px; top: {cropOverlay.top}px; left: {cropOverlay.left}px;"
              >
                <div class="crop-box"></div>
              </div>
            {/if}
          {/if}
        </div>
        <div class="ratio-meta">
          <div class="ratio-label">{ratio.label}</div>
          <div class="ratio-normalized">{ratio.normalized}</div>
        </div>
      </div>
    {/each}
  </div>

  <div class="footer">
    <h2>History</h2>
    <p>
      <strong>2010</strong> — Instagram launched with only square (1:1) photos. This was its iconic look, which matched the instant camera vibe (Polaroid / retro filters).
    </p>
    <p>
      <strong>2015</strong> — Added portrait (4:5) and landscape (1.91:1) 
      support, because real cameras and video ads found that cropping everything to a square was annoying and often ruined compositions. Square (1:1) remained an option.
    </p>
    <p>
      <strong>2025</strong> — Extended portrait support to 3:4 so iPhone photos no longer need cropping.
    </p>
    
    <p>
      All iPhone models since 2007 use 4:3 as the default camera ratio (3:4 in portrait). 
      The Camera app's 16:9 and 1:1 modes crop the 4:3 image rather than using the full sensor.
    </p>
  </div>
</div>

<style>
  .container {
    max-width: 1400px;
    margin: 0 auto;
    padding: 4rem 2rem;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
    color: #1a1a1a;
    background: #ffffff;
  }

  .header {
    margin-bottom: 3rem;
    text-align: center;
  }

  h1 {
    font-size: 1.5rem;
    font-weight: 600;
    margin: 0;
    color: #0d0d0d;
    letter-spacing: -0.02em;
  }

  .ratios-container {
    display: flex;
    gap: 1rem;
    align-items: flex-end;
    justify-content: center;
    margin-bottom: 3rem;
    flex-wrap: nowrap;
    width: 100%;
    min-width: 0;
  }

  .ratios-container::-webkit-scrollbar {
    height: 6px;
  }

  .ratios-container::-webkit-scrollbar-track {
    background: transparent;
  }

  .ratios-container::-webkit-scrollbar-thumb {
    background: rgba(0, 0, 0, 0.2);
    border-radius: 3px;
  }

  .ratio-card {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.75rem;
    flex-shrink: 0;
    position: relative;
  }

  .ratio-card.separator-before::before {
    content: '';
    position: absolute;
    left: -0.5rem;
    top: 0;
    bottom: 0;
    width: 1px;
    background: #d1d5db;
    z-index: 1;
  }

  .ratio-visual {
    border: 1px solid #e5e7eb;
    border-radius: 6px;
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    background: #f9fafb;
    margin: 0 auto;
    transition: border-color 0.2s ease;
  }

  .ratio-visual:hover {
    border-color: #d1d5db;
  }

  .ratio-visual.landscape,
  .ratio-visual.iphone-landscape,
  .ratio-visual.iphone-portrait {
    border-color: #3b82f6;
    background: #eff6ff;
  }

  .ratio-visual.too-wide,
  .ratio-visual.too-tall {
    border-color: #d1d5db;
    background: #f9fafb;
    background-image: 
      repeating-linear-gradient(
        45deg,
        #f3f4f6,
        #f3f4f6 10px,
        #e5e7eb 10px,
        #e5e7eb 20px
      );
    position: relative;
  }

  .ratio-visual.too-wide::before,
  .ratio-visual.too-tall::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(249, 250, 251, 0.7);
    z-index: 0;
  }

  .ratio-visual.too-wide .crop-overlay,
  .ratio-visual.too-tall .crop-overlay {
    z-index: 1;
  }

  .crop-overlay {
    position: absolute;
    pointer-events: none;
  }

  .crop-box {
    width: 100%;
    height: 100%;
    border: 2px solid #3b82f6;
    border-radius: 4px;
    background: rgba(59, 130, 246, 0.08);
    box-shadow: 0 0 0 1px rgba(59, 130, 246, 0.2);
  }

  .ratio-meta {
    text-align: center;
    min-width: 0;
  }

  .ratio-label {
    font-size: 0.8125rem;
    font-weight: 500;
    color: #0d0d0d;
    margin-bottom: 0.25rem;
  }

  .ratio-normalized {
    font-size: 0.75rem;
    color: #6b7280;
    font-weight: 400;
  }

  .footer {
    max-width: 800px;
    margin: 4rem auto 0;
    padding-top: 3rem;
    border-top: 1px solid #e5e7eb;
  }

  .footer h2 {
    font-size: 1rem;
    font-weight: 600;
    color: #0d0d0d;
    margin: 0 0 1rem 0;
    letter-spacing: -0.01em;
  }

  .footer p {
    font-size: 0.875rem;
    color: #6b7280;
    line-height: 1.6;
    margin: 0 0 1rem 0;
  }

  .footer p:last-child {
    margin-bottom: 0;
  }

  .footer strong {
    color: #0d0d0d;
    font-weight: 500;
  }

  @media (max-width: 768px) {
    .container {
      padding: 2rem 1.5rem;
    }

    .ratios-container {
      gap: 0.5rem;
      overflow-x: auto;
      padding-bottom: 0.5rem;
      -webkit-overflow-scrolling: touch;
      scrollbar-width: thin;
      scrollbar-color: rgba(0, 0, 0, 0.2) transparent;
    }

    .ratios-container::-webkit-scrollbar {
      height: 4px;
    }

    .ratios-container::-webkit-scrollbar-thumb {
      background: #d1d5db;
      border-radius: 2px;
    }

    .ratio-card {
      flex: 0 0 auto;
      min-width: 100px;
    }

    .ratio-visual {
      max-width: 100px;
    }

    .footer {
      margin-top: 3rem;
      padding-top: 2rem;
    }

    .footer h2 {
      font-size: 0.9375rem;
    }

    .footer p {
      font-size: 0.8125rem;
    }
  }
</style>