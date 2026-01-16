<script lang="ts">
	let dynamicColor = '#3b82f6';
	let dynamicSize = 16;

	// Helper function to decode HTML entities for readable display
	function decodeHtmlEntities(text: string): string {
		if (typeof document === 'undefined') {
			return text
				.replace(/&lt;/g, '<')
				.replace(/&gt;/g, '>')
				.replace(/&amp;/g, '&');
		}
		const textarea = document.createElement('textarea');
		textarea.innerHTML = text;
		return textarea.value;
	}
</script>

<div class="vanilla-extract-deep-dive">
	<div class="deep-dive-header">
		<h2>Vanilla Extract: Complete Solution</h2>
		<p class="deep-dive-subtitle">See how Vanilla Extract handles all four critical use cases in SvelteKit</p>
	</div>

	<div class="use-case-grid">
		<!-- Use Case 1: Sharing CSS Classes -->
		<div class="use-case-card">
			<div class="use-case-header">
				<h3>1. Sharing CSS Classes</h3>
				<p class="use-case-description">Import and reuse styles across SvelteKit components</p>
			</div>
			<div class="use-case-content">
				<div class="use-case-code">
					<pre><code>{decodeHtmlEntities(`// src/lib/styles/typography.css.ts
import { style } from '@vanilla-extract/css';

export const heading = style({
  fontSize: '1.5rem',
  fontWeight: 600,
  color: '#0d0d0d',
  marginBottom: '1rem'
});

// src/routes/about/+page.svelte
<script>
  import * as t from '$lib/styles/typography.css';
</script>

<h1 class={t.heading}>About Page</h1>

// ✅ Type-safe, zero-runtime, reusable`)}</code></pre>
				</div>
				<div class="use-case-visual">
					<div class="shared-demo">
						<h1 class="shared-heading">Shared Heading</h1>
						<p class="shared-body">This body text uses shared styles</p>
						<h2 class="shared-heading">Another Heading</h2>
						<p class="shared-body">Same styles, different component</p>
					</div>
				</div>
			</div>
		</div>

		<!-- Use Case 2: Dynamic Values -->
		<div class="use-case-card">
			<div class="use-case-header">
				<h3>2. Dynamic Values</h3>
				<p class="use-case-description">CSS variables + Svelte reactive state for runtime values</p>
			</div>
			<div class="use-case-content">
				<div class="use-case-code">
					<pre><code>{decodeHtmlEntities(`// src/lib/styles/components.css.ts
import { style } from '@vanilla-extract/css';

export const dynamicBox = style({
  padding: '1rem',
  border: '2px solid var(--color)',
  color: 'var(--color, #3b82f6)',
  fontSize: 'var(--size, 16px)'
});

// src/lib/components/DynamicCard.svelte
<script>
  import * as s from '$lib/styles/components.css';
  
  let color = '#3b82f6';
  let size = 16;
  $: cssVars = \`--color: \${color}; --size: \${size}px;\`;
</script>

<div class={s.dynamicBox} style={cssVars}>
  Dynamic Content
</div>

// ✅ Build-time CSS + reactive variables`)}</code></pre>
				</div>
				<div class="use-case-visual">
					<div class="dynamic-controls-small">
						<div class="control-group-small">
							<label for="color-small">Color:</label>
							<input
								type="color"
								id="color-small"
								bind:value={dynamicColor}
								class="color-input-small"
							/>
						</div>
						<div class="control-group-small">
							<label for="size-small">Size: {dynamicSize}px</label>
							<input
								type="range"
								id="size-small"
								min="12"
								max="24"
								bind:value={dynamicSize}
								class="size-input-small"
							/>
						</div>
					</div>
					<div
						class="dynamic-box-demo"
						style="--color: {dynamicColor}; --size: {dynamicSize}px;"
					>
						Dynamic Content
					</div>
				</div>
			</div>
		</div>

		<!-- Use Case 3: Less Verbosity (Utility-like) -->
		<div class="use-case-card">
			<div class="use-case-header">
				<h3>3. Less Verbosity (Utility-like)</h3>
				<p class="use-case-description">Concise utilities for common patterns + Svelte class directive compatibility</p>
			</div>
			<div class="use-case-content">
				<div class="use-case-code">
					<pre><code>{decodeHtmlEntities(`// src/lib/styles/utilities.css.ts
import { style } from '@vanilla-extract/css';

// Common flex patterns - much more concise!
export const flex = style({ display: 'flex' });
export const flexCenter = style({ 
  display: 'flex', 
  alignItems: 'center' 
});
export const flexCenterGap = style({
  display: 'flex',
  alignItems: 'center',
  gap: '4px'
});
export const flexCol = style({ flexDirection: 'column' });
export const gap = (size: string) => style({ gap: size });

// Instead of: display: flex; align-items: center; row-gap: 4px;
// Just use: class={u.flexCenterGap}

// src/lib/components/NavBar.svelte
<script>
  import * as u from '$lib/styles/utilities.css';
  
  let isActive = false;
</script>

<!-- Works with Svelte's class directive arrays/objects -->
<nav class={[u.flexCenterGap, { [u.flexCol]: isMobile }]}>
  <a class={isActive ? u.active : ''}>Home</a>
  <a>About</a>
</nav>

<!-- Or combine multiple utilities -->
<div class={u.flex + ' ' + u.gap('1rem')}>
  <button>Click</button>
  <button>More</button>
</div>

// ✅ Concise, type-safe, compatible with Svelte's class syntax`)}</code></pre>
				</div>
				<div class="use-case-visual">
					<div class="utility-demo">
						<div class="utility-flex-center-gap">
							<button class="utility-button-small">Home</button>
							<button class="utility-button-small">About</button>
							<button class="utility-button-small utility-active">Contact</button>
						</div>
					</div>
				</div>
			</div>
		</div>

		<!-- Use Case 4: Composing Classes -->
		<div class="use-case-card">
			<div class="use-case-header">
				<h3>4. Composing Classes</h3>
				<p class="use-case-description">Combine styles with Svelte's class directive (arrays, objects, conditionals)</p>
			</div>
			<div class="use-case-content">
				<div class="use-case-code">
					<pre><code>{decodeHtmlEntities(`// src/lib/styles/components.css.ts
import { style, styleVariants } from '@vanilla-extract/css';
import { recipe } from '@vanilla-extract/recipes';

// Base + variants composition
const base = style({ padding: '1rem', borderRadius: '6px' });
const colors = styleVariants({
  primary: { background: '#3b82f6', color: '#fff' },
  secondary: { background: '#6b7280', color: '#fff' }
});
export const button = style([base, colors.primary]);

// Recipe for variants
export const card = recipe({
  base: { padding: '1.5rem', borderRadius: '8px' },
  variants: {
    elevation: {
      sm: { boxShadow: '0 1px 2px rgba(0,0,0,0.05)' },
      md: { boxShadow: '0 4px 6px rgba(0,0,0,0.1)' }
    }
  }
});

// src/lib/components/ButtonGroup.svelte
<script>
  import * as styles from '$lib/styles/components.css';
  import * as u from '$lib/styles/utilities.css';
  
  let isSelected = false;
  let isDisabled = false;
</script>

<!-- Array syntax - Svelte converts to string -->
<button class={[styles.button, u.flexCenterGap]}>
  <span>Icon</span>
  <span>Label</span>
</button>

<!-- Object syntax - conditional classes -->
<button 
  class={{
    [styles.button]: true,
    [styles.buttonSecondary]: isSelected,
    [styles.disabled]: isDisabled
  }}
>
  Toggle
</button>

<!-- Mix arrays, objects, and strings -->
<div class={[
  u.flexCenterGap,
  styles.card({ elevation: 'md' }),
  { 'custom-class': isSelected }
]}>
  Content
</div>

// ✅ Full compatibility with Svelte's class directive`)}</code></pre>
				</div>
				<div class="use-case-visual">
					<div class="composition-demo">
						<button class="composed-button composed-primary composed-flex-center-gap">
							<span>✓</span>
							<span>Primary</span>
						</button>
						<button class="composed-button composed-secondary">Secondary</button>
						<div class="composed-card composed-elevated">
							<h4 style="margin: 0 0 0.5rem;">Elevated Card</h4>
							<p style="margin: 0; font-size: 0.875rem;">Composed styles</p>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>

	<div class="vanilla-summary">
		<h3>Key Takeaways</h3>
		<div class="summary-grid">
			<div class="summary-item">
				<strong>✅ Sharing:</strong> Import styles from shared modules - type-safe and zero-runtime, works across routes and components
			</div>
			<div class="summary-item">
				<strong>✅ Dynamic:</strong> CSS variables + Svelte reactive state for runtime values - no CSS-in-JS runtime overhead
			</div>
			<div class="summary-item">
				<strong>✅ Less Verbose:</strong> Utility functions and recipes reduce repetition while maintaining type safety
			</div>
			<div class="summary-item">
				<strong>✅ Composition:</strong> Combine base styles, variants, and recipes - works seamlessly with Svelte's class directive
			</div>
		</div>
	</div>
</div>

<style>
	.vanilla-extract-deep-dive {
		margin-top: 3rem;
		padding-top: 3rem;
		border-top: 2px solid #e5e7eb;
	}

	.deep-dive-header {
		text-align: center;
		margin-bottom: 2.5rem;
	}

	.deep-dive-header h2 {
		font-size: 1.75rem;
		font-weight: 600;
		margin: 0 0 0.5rem;
		color: #0d0d0d;
		letter-spacing: -0.02em;
	}

	.deep-dive-subtitle {
		font-size: 0.9375rem;
		color: #6b7280;
		margin: 0;
	}

	.use-case-grid {
		display: grid;
		gap: 2rem;
		margin-bottom: 2rem;
	}

	.use-case-card {
		background: #ffffff;
		border: 1px solid #e5e7eb;
		border-radius: 8px;
		overflow: hidden;
	}

	.use-case-header {
		padding: 1.5rem;
		background: #f9fafb;
		border-bottom: 1px solid #e5e7eb;
	}

	.use-case-header h3 {
		font-size: 1.125rem;
		font-weight: 600;
		margin: 0 0 0.375rem;
		color: #0d0d0d;
	}

	.use-case-description {
		font-size: 0.875rem;
		color: #6b7280;
		margin: 0;
		line-height: 1.5;
	}

	.use-case-content {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 0;
	}

	.use-case-code {
		padding: 1.5rem;
		background: #ffffff;
		border-right: 1px solid #e5e7eb;
		overflow-x: auto;
	}

	.use-case-code pre {
		margin: 0;
		font-size: 0.75rem;
		line-height: 1.5;
		color: #0d0d0d;
	}

	.use-case-code code {
		font-family: 'SF Mono', Monaco, 'Cascadia Code', 'Roboto Mono', Consolas, 'Courier New', monospace;
		white-space: pre;
	}

	.use-case-visual {
		padding: 1.5rem;
		background: #fafafa;
		display: flex;
		align-items: center;
		justify-content: center;
		min-height: 200px;
	}

	/* Shared styles demo */
	.shared-demo {
		width: 100%;
		max-width: 400px;
	}

	.shared-heading {
		font-size: 1.5rem;
		font-weight: 600;
		color: #0d0d0d;
		margin: 0 0 1rem;
	}

	.shared-body {
		font-size: 1rem;
		line-height: 1.6;
		color: #6b7280;
		margin: 0 0 1.5rem;
	}

	/* Dynamic controls small */
	.dynamic-controls-small {
		display: flex;
		flex-direction: column;
		gap: 1rem;
		margin-bottom: 1rem;
		width: 100%;
		max-width: 300px;
	}

	.control-group-small {
		display: flex;
		flex-direction: column;
		gap: 0.375rem;
	}

	.control-group-small label {
		font-size: 0.8125rem;
		font-weight: 500;
		color: #0d0d0d;
	}

	.color-input-small {
		width: 50px;
		height: 32px;
		border: 1px solid #e5e7eb;
		border-radius: 4px;
		cursor: pointer;
	}

	.size-input-small {
		width: 100%;
		height: 6px;
		border-radius: 3px;
		background: #e5e7eb;
		outline: none;
		cursor: pointer;
	}

	.size-input-small::-webkit-slider-thumb {
		appearance: none;
		width: 16px;
		height: 16px;
		border-radius: 50%;
		background: #0d0d0d;
		cursor: pointer;
	}

	.size-input-small::-moz-range-thumb {
		width: 16px;
		height: 16px;
		border-radius: 50%;
		background: #0d0d0d;
		border: none;
		cursor: pointer;
	}

	.dynamic-box-demo {
		padding: 1rem;
		background: rgba(59, 130, 246, 0.1);
		border: 2px solid var(--color, #3b82f6);
		border-radius: 6px;
		color: var(--color, #3b82f6);
		font-size: var(--size, 16px);
		font-weight: 500;
		text-align: center;
		transition: all 0.2s ease;
		width: 100%;
		max-width: 300px;
	}

	/* Utility demo */
	.utility-demo {
		width: 100%;
		max-width: 400px;
	}

	.utility-flex-center-gap {
		display: flex;
		align-items: center;
		gap: 4px;
	}

	.utility-button-small {
		padding: 0.375rem 0.75rem;
		border-radius: 4px;
		font-size: 0.875rem;
		font-weight: 500;
		border: none;
		cursor: pointer;
		font-family: inherit;
		background: #f3f4f6;
		color: #374151;
	}

	.utility-active {
		background: #3b82f6;
		color: #ffffff;
	}

	/* Composition demo */
	.composition-demo {
		display: flex;
		flex-direction: column;
		gap: 1rem;
		width: 100%;
		max-width: 400px;
	}

	.composed-button {
		padding: 0.75rem 1rem;
		border-radius: 6px;
		font-weight: 500;
		border: none;
		cursor: pointer;
		font-family: inherit;
		text-align: center;
	}

	.composed-primary {
		background: #3b82f6;
		color: #ffffff;
	}

	.composed-secondary {
		background: #6b7280;
		color: #ffffff;
	}

	.composed-outline {
		background: transparent;
		border: 2px solid #3b82f6;
		color: #3b82f6;
	}

	.composed-flex-center-gap {
		display: flex;
		align-items: center;
		gap: 4px;
	}

	.composed-card {
		padding: 1.5rem;
		border-radius: 8px;
		border: 1px solid #e5e7eb;
		background: #ffffff;
	}

	.composed-elevated {
		box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
	}

	/* Summary */
	.vanilla-summary {
		background: #f0f9ff;
		border: 1px solid #bae6fd;
		border-radius: 8px;
		padding: 1.5rem;
		margin-top: 2rem;
	}

	.vanilla-summary h3 {
		font-size: 1.125rem;
		font-weight: 600;
		margin: 0 0 1rem;
		color: #0c4a6e;
	}

	.summary-grid {
		display: grid;
		grid-template-columns: repeat(2, 1fr);
		gap: 0.75rem;
	}

	.summary-item {
		font-size: 0.875rem;
		color: #0c4a6e;
		line-height: 1.5;
	}

	.summary-item strong {
		font-weight: 600;
	}

	@media (max-width: 968px) {
		.use-case-content {
			grid-template-columns: 1fr;
		}

		.use-case-code {
			border-right: none;
			border-bottom: 1px solid #e5e7eb;
		}

		.summary-grid {
			grid-template-columns: 1fr;
		}
	}
</style>