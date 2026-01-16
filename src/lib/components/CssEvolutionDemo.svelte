<script lang="ts">
	type Era = 'html' | 'css' | 'css-modules' | 'css-in-js' | 'tailwind' | 'vanilla-extract';

	let selectedEra: Era = 'html';
	let selectedExample: 'layout' | 'reuse' | 'dynamic' = 'layout';
	let dynamicColor = '#3b82f6';
	let dynamicSize = 16;

	// Helper function to decode HTML entities for readable display
	function decodeHtmlEntities(text: string): string {
		if (typeof document === 'undefined') {
			// SSR fallback - simple entity replacement
			return text
				.replace(/&lt;/g, '<')
				.replace(/&gt;/g, '>')
				.replace(/&amp;/g, '&');
		}
		const textarea = document.createElement('textarea');
		textarea.innerHTML = text;
		return textarea.value;
	}

	const eras: { id: Era; name: string; year: string }[] = [
		{ id: 'html', name: 'Presentational HTML', year: '1991-1996' },
		{ id: 'css', name: 'CSS', year: '1996+' },
		{ id: 'css-modules', name: 'CSS Modules', year: '2015+' },
		{ id: 'css-in-js', name: 'CSS-in-JS', year: '2015+' },
		{ id: 'tailwind', name: 'Tailwind CSS', year: '2017+' },
		{ id: 'vanilla-extract', name: 'Vanilla Extract', year: '2019+' }
	];

	const layoutExamples = {
		html: {
			title: 'Layout with Presentational HTML',
			description: 'Using HTML table elements and attributes for layout',
			code: `<table width="100%" cellpadding="20" cellspacing="0">
  <tr>
    <td width="200" valign="top" bgcolor="#f0f0f0">
      <font size="4" face="Arial">Sidebar</font>
    </td>
    <td valign="top">
      <font size="4" face="Arial">Main Content</font>
      <p align="justify">Content goes here...</p>
    </td>
  </tr>
</table>`,
			visual: 'html-layout'
		},
		css: {
			title: 'Layout with CSS',
			description: 'Using CSS for layout with classes',
			code: `<!-- HTML -->
<div class="container">
  <aside class="sidebar">Sidebar</aside>
  <main class="content">Main Content</main>
</div>

/* CSS */
.container {
  display: flex;
  gap: 1rem;
}
.sidebar {
  width: 200px;
  background: #f0f0f0;
}
.content {
  flex: 1;
}`,
			visual: 'css-layout'
		},
		'css-modules': {
			title: 'Layout with CSS Modules',
			description: 'Scoped CSS classes that don\'t conflict globally',
			code: `<!-- Component.svelte -->
<div class={styles.container}>
  <aside class={styles.sidebar}>Sidebar</aside>
  <main class={styles.content}>Main Content</main>
</div>

/* Component.module.css */
.container {
  display: flex;
  gap: 1rem;
}
.sidebar {
  width: 200px;
  background: #f0f0f0;
}
.content {
  flex: 1;
}`,
			visual: 'css-modules-layout'
		},
		'css-in-js': {
			title: 'Layout with CSS-in-JS',
			description: 'Styles defined in JavaScript/TypeScript',
			code: `import styled from 'styled-components';

const Container = styled.div\`
  display: flex;
  gap: 1rem;
\`;

const Sidebar = styled.aside\`
  width: 200px;
  background: #f0f0f0;
\`;

const Content = styled.main\`
  flex: 1;
\`;

// Usage
<Container>
  <Sidebar>Sidebar</Sidebar>
  <Content>Main Content</Content>
</Container>`,
			visual: 'css-in-js-layout'
		},
		tailwind: {
			title: 'Layout with Tailwind CSS',
			description: 'Utility-first CSS classes',
			code: `<!-- HTML with utility classes -->
<div class="flex gap-4">
  <aside class="w-[200px] bg-gray-100">
    Sidebar
  </aside>
  <main class="flex-1">
    Main Content
  </main>
</div>

<!-- No separate CSS file needed! -->`,
			visual: 'tailwind-layout'
		},
		'vanilla-extract': {
			title: 'Layout with Vanilla Extract',
			description: 'Type-safe, zero-runtime CSS-in-TypeScript',
			code: `// styles.css.ts
import { style } from '@vanilla-extract/css';

export const container = style({
  display: 'flex',
  gap: '1rem'
});

export const sidebar = style({
  width: '200px',
  background: '#f0f0f0'
});

export const content = style({
  flex: 1
});

// Component.tsx
import * as styles from './styles.css';

<div className={styles.container}>
  <aside className={styles.sidebar}>Sidebar</aside>
  <main className={styles.content}>Main Content</main>
</div>`,
			visual: 'vanilla-extract-layout'
		}
	};

	const reuseExamples = {
		html: {
			title: 'Reusing Styles with HTML',
			description: 'Copying attributes across elements',
			code: `<!-- Repeated attributes everywhere -->
<h1>
  <font size="5" face="Arial" color="#333">Title 1</font>
</h1>
<h1>
  <font size="5" face="Arial" color="#333">Title 2</font>
</h1>
<h1>
  <font size="5" face="Arial" color="#333">Title 3</font>
</h1>

<!-- No way to reuse without copying! -->`,
			visual: 'html-reuse'
		},
		css: {
			title: 'Reusing Styles with CSS Classes',
			description: 'Define once, use everywhere',
			code: `<!-- HTML -->
<h1 class="heading">Title 1</h1>
<h1 class="heading">Title 2</h1>
<h1 class="heading">Title 3</h1>

/* CSS - Define once */
.heading {
  font-size: 1.5rem;
  font-family: Arial, sans-serif;
  color: #333;
  margin-bottom: 1rem;
}

/* Reuse across components */
/* header.css */
.header .heading { /* ... */ }

/* article.css */
.article .heading { /* ... */ }`,
			visual: 'css-reuse'
		},
		'css-modules': {
			title: 'Reusing Styles with CSS Modules',
			description: 'Import styles from shared modules',
			code: `// shared/typography.module.css
.heading {
  font-size: 1.5rem;
  font-family: Arial, sans-serif;
  color: #333;
  margin-bottom: 1rem;
}

// Header.svelte
&lt;script&gt;
  import headingStyles from '../shared/typography.module.css';
&lt;/script&gt;
<h1 class={headingStyles.heading}>Title</h1>

// Article.svelte
&lt;script&gt;
  import headingStyles from '../shared/typography.module.css';
&lt;/script&gt;
<h1 class={headingStyles.heading}>Title</h1>

// Styles are scoped but reusable!`,
			visual: 'css-modules-reuse'
		},
		'css-in-js': {
			title: 'Reusing Styles with CSS-in-JS',
			description: 'Export styled components or style objects',
			code: `// shared/typography.ts
import styled from 'styled-components';

export const Heading = styled.h1\`
  font-size: 1.5rem;
  font-family: Arial, sans-serif;
  color: #333;
  margin-bottom: 1rem;
\`;

// Header.tsx
import { Heading } from '../shared/typography';
<Heading>Title</Heading>

// Article.tsx
import { Heading } from '../shared/typography';
<Heading>Title</Heading>

// Or with style objects:
export const headingStyles = {
  fontSize: '1.5rem',
  fontFamily: 'Arial, sans-serif',
  color: '#333',
  marginBottom: '1rem'
};`,
			visual: 'css-in-js-reuse'
		},
		tailwind: {
			title: 'Reusing Styles with Tailwind',
			description: 'Extract utilities into components or @apply',
			code: `<!-- Option 1: Component extraction -->
<!-- Button.svelte -->
<button class="px-4 py-2 bg-blue-500 text-white rounded">
  <slot />
</button>

<!-- Option 2: Custom classes with @apply -->
/* styles.css */
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer components {
  .heading {
    @apply text-2xl font-bold text-gray-800 mb-4;
  }
}

<!-- Usage -->
<h1 class="heading">Title</h1>

<!-- Option 3: Tailwind config for design tokens -->
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: {
        brand: { /* ... */ }
      }
    }
  }
}`,
			visual: 'tailwind-reuse'
		},
		'vanilla-extract': {
			title: 'Reusing Styles with Vanilla Extract',
			description: 'Export style classes and use across components',
			code: `// shared/typography.css.ts
import { style } from '@vanilla-extract/css';

export const heading = style({
  fontSize: '1.5rem',
  fontFamily: 'Arial, sans-serif',
  color: '#333',
  marginBottom: '1rem'
});

// Header.tsx
import * as typography from '../shared/typography.css';
<h1 className={typography.heading}>Title</h1>

// Article.tsx
import * as typography from '../shared/typography.css';
<h1 className={typography.heading}>Title</h1>

// Type-safe and zero-runtime!
// Also supports design tokens:
export const tokens = {
  color: {
    primary: '#333',
    // ...
  }
};`,
			visual: 'vanilla-extract-reuse'
		}
	};

	const dynamicExamples = {
		html: {
			title: 'Dynamic Styling with HTML',
			description: 'Not possible - HTML attributes are static. Must use inline styles or JavaScript.',
			code: `<!-- Static only - no dynamic styling -->
<font size="5" color="#333">Title</font>

<!-- Workaround: Inline styles with JavaScript -->
<div style="color: red;">Title</div>

<!-- Requires JavaScript to change -->
&lt;script&gt;
  document.getElementById('el').style.color = 'blue';
&lt;/script&gt;

<!-- No way to pass dynamic values from components! -->`,
			visual: 'html-dynamic',
			possible: false
		},
		css: {
			title: 'Dynamic Styling with CSS',
			description: 'Limited - Use CSS custom properties (variables) set via inline styles or JavaScript',
			code: `<!-- HTML -->
<div class="dynamic-box" style="--color: #3b82f6; --size: 16px;">
  Dynamic Content
</div>

/* CSS */
.dynamic-box {
  color: var(--color, #000);
  font-size: var(--size, 14px);
  padding: 1rem;
  background: rgba(59, 130, 246, 0.1);
  border: 2px solid var(--color);
}

/* Still requires inline styles or JS to set variables */
/* Cannot directly use component props/state */`,
			visual: 'css-dynamic',
			possible: true
		},
		'css-modules': {
			title: 'Dynamic Styling with CSS Modules',
			description: 'Same as CSS - Use CSS custom properties set via inline styles',
			code: `// Component.svelte
&lt;script&gt;
  let color = '#3b82f6';
  let size = 16;
&lt;/script&gt;

<div 
  class={styles.dynamicBox} 
  style="--color: {color}; --size: {size}px;"
>
  Dynamic Content
</div>

/* Component.module.css */
.dynamicBox {
  color: var(--color, #000);
  font-size: var(--size, 14px);
  padding: 1rem;
  background: rgba(59, 130, 246, 0.1);
  border: 2px solid var(--color);
}

/* Works but requires inline styles */`,
			visual: 'css-modules-dynamic',
			possible: true
		},
		'css-in-js': {
			title: 'Dynamic Styling with CSS-in-JS',
			description: 'Excellent - Direct access to props, state, and JavaScript values',
			code: `import styled from 'styled-components';

// Props-based styling
const DynamicBox = styled.div\`
  color: \${props => props.color || '#000'};
  font-size: \${props => props.size || 14}px;
  padding: 1rem;
  background: rgba(59, 130, 246, 0.1);
  border: 2px solid \${props => props.color};
\`;

// Usage
<DynamicBox color="#3b82f6" size={16}>
  Dynamic Content
</DynamicBox>

// Or with style objects
const styles = (color, size) => ({
  color,
  fontSize: \`\${size}px\`,
  padding: '1rem',
  background: 'rgba(59, 130, 246, 0.1)',
  border: \`2px solid \${color}\`
});`,
			visual: 'css-in-js-dynamic',
			possible: true
		},
		tailwind: {
			title: 'Dynamic Styling with Tailwind',
			description: 'Limited - Use arbitrary values or inline styles. Not truly dynamic from props.',
			code: `<!-- Option 1: Arbitrary values (static) -->
<div class="text-[#3b82f6] text-[16px] p-4 bg-blue-100 border-2 border-[#3b82f6]">
  Dynamic Content
</div>

<!-- Option 2: Inline styles (defeats purpose) -->
<div 
  class="p-4 bg-blue-100 border-2"
  style="color: \${color}; font-size: \${size}px; border-color: \${color};"
>
  Dynamic Content
</div>

<!-- Option 3: CSS variables with Tailwind -->
<div 
  class="p-4 bg-blue-100 border-2"
  style="--color: {color}; --size: {size}px;"
  class="text-[color:var(--color)] text-[length:var(--size)] border-[color:var(--color)]"
>
  Dynamic Content
</div>

<!-- Not ideal - Tailwind is designed for static utilities -->`,
			visual: 'tailwind-dynamic',
			possible: true
		},
		'vanilla-extract': {
			title: 'Dynamic Styling with Vanilla Extract',
			description: 'Good - Use CSS variables or runtime styles. Type-safe approach.',
			code: `// styles.css.ts
import { style } from '@vanilla-extract/css';

export const dynamicBox = style({
  padding: '1rem',
  background: 'rgba(59, 130, 246, 0.1)',
  border: '2px solid var(--color)',
  color: 'var(--color, #000)',
  fontSize: 'var(--size, 14px)'
});

// Component.tsx
import * as styles from './styles.css';

function Component({ color = '#3b82f6', size = 16 }) {
  return (
    <div 
      className={styles.dynamicBox}
      style={{
        '--color': color,
        '--size': \`\${size}px\`
      } as React.CSSProperties}
    >
      Dynamic Content
    </div>
  );
}

// Or use runtime styles (less type-safe)
import { style } from '@vanilla-extract/css';
const dynamicStyle = (color: string, size: number) => 
  style({
    color,
    fontSize: \`\${size}px\`,
    // ...
  });`,
			visual: 'vanilla-extract-dynamic',
			possible: true
		}
	};
</script>

<div class="demo-container">
	<div class="header">
		<h2>CSS Evolution: Solving the Same Problem</h2>
		<p class="subtitle">See how layout, style reuse, and dynamic styling evolved across different eras</p>
	</div>

	<div class="controls">
		<div class="example-selector">
			<button
				class="example-btn"
				class:active={selectedExample === 'layout'}
				onclick={() => (selectedExample = 'layout')}
			>
				Layout
			</button>
			<button
				class="example-btn"
				class:active={selectedExample === 'reuse'}
				onclick={() => (selectedExample = 'reuse')}
			>
				Style Reuse
			</button>
			<button
				class="example-btn"
				class:active={selectedExample === 'dynamic'}
				onclick={() => (selectedExample = 'dynamic')}
			>
				Dynamic Styling
			</button>
		</div>

		<div class="era-selector">
			{#each eras as era}
				<button
					class="era-btn"
					class:active={selectedEra === era.id}
					onclick={() => (selectedEra = era.id)}
				>
					<span class="era-name">{era.name}</span>
					<span class="era-year">{era.year}</span>
				</button>
			{/each}
		</div>
	</div>

	<div class="example-section">
		{#if selectedExample === 'layout'}
			{@const example = layoutExamples[selectedEra]}
			{@const decodedCode = decodeHtmlEntities(example.code)}
			<div class="example-header">
				<h3>{example.title}</h3>
				<p class="example-description">{example.description}</p>
			</div>

			<div class="example-content">
				<div class="code-block">
					<pre><code>{decodedCode}</code></pre>
				</div>

				<div class="visual-demo layout-demo">
					<div class="visual-container">
						<aside class="visual-sidebar">Sidebar</aside>
						<main class="visual-content">Main Content</main>
					</div>
				</div>
			</div>
		{:else if selectedExample === 'reuse'}
			{@const example = reuseExamples[selectedEra]}
			{@const decodedCode = decodeHtmlEntities(example.code)}
			<div class="example-header">
				<h3>{example.title}</h3>
				<p class="example-description">{example.description}</p>
			</div>

			<div class="example-content">
				<div class="code-block">
					<pre><code>{decodedCode}</code></pre>
				</div>

				<div class="visual-demo reuse-demo">
					<div class="visual-reuse">
						<h1 class="visual-heading">Title 1</h1>
						<h1 class="visual-heading">Title 2</h1>
						<h1 class="visual-heading">Title 3</h1>
					</div>
				</div>
			</div>
		{:else}
			{@const example = dynamicExamples[selectedEra]}
			{@const decodedCode = decodeHtmlEntities(example.code)}
			<div class="example-header">
				<h3>{example.title}</h3>
				<p class="example-description">
					{example.description}
					{#if !example.possible}
						<span class="not-possible-badge">Not Possible</span>
					{/if}
				</p>
			</div>

			<div class="example-content">
				<div class="code-block">
					<pre><code>{decodedCode}</code></pre>
				</div>

				<div class="visual-demo dynamic-demo">
					{#if !example.possible}
						<div class="not-possible-message">
							<p>❌ Dynamic styling not possible with this approach</p>
							<p class="note">Must use inline styles or JavaScript workarounds</p>
						</div>
					{:else}
						<div class="dynamic-controls">
							<div class="control-group">
								<label for="color">Color:</label>
								<input
									type="color"
									id="color"
									bind:value={dynamicColor}
									class="color-input"
								/>
								<span class="color-value">{dynamicColor}</span>
							</div>
							<div class="control-group">
								<label for="size">Font Size: {dynamicSize}px</label>
								<input
									type="range"
									id="size"
									min="12"
									max="24"
									bind:value={dynamicSize}
									class="size-input"
								/>
							</div>
						</div>
						<div
							class="dynamic-box"
							style="--color: {dynamicColor}; --size: {dynamicSize}px;"
						>
							Dynamic Content
						</div>
					{/if}
				</div>
			</div>
		{/if}
	</div>
</div>

<style>
	.demo-container {
		max-width: 1200px;
		margin: 0 auto;
		padding: 2rem;
		font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
	}

	.header {
		margin-bottom: 2rem;
		text-align: center;
	}

	.header h2 {
		font-size: 1.75rem;
		font-weight: 600;
		margin: 0 0 0.5rem;
		color: #0d0d0d;
		letter-spacing: -0.02em;
	}

	.subtitle {
		font-size: 0.9375rem;
		color: #6b7280;
		margin: 0;
	}

	.controls {
		display: flex;
		flex-direction: column;
		gap: 1.5rem;
		margin-bottom: 2rem;
	}

	.example-selector {
		display: flex;
		gap: 0.5rem;
		justify-content: center;
	}

	.example-btn {
		padding: 0.5rem 1.25rem;
		font-size: 0.875rem;
		font-weight: 500;
		color: #6b7280;
		background: #ffffff;
		border: 1px solid #e5e7eb;
		border-radius: 6px;
		cursor: pointer;
		transition: all 0.2s;
		font-family: inherit;
	}

	.example-btn:hover {
		background: #f9fafb;
		border-color: #d1d5db;
	}

	.example-btn.active {
		color: #0d0d0d;
		background: #0d0d0d;
		color: #ffffff;
		border-color: #0d0d0d;
	}

	.era-selector {
		display: flex;
		flex-wrap: wrap;
		gap: 0.5rem;
		justify-content: center;
	}

	.era-btn {
		display: flex;
		flex-direction: column;
		align-items: center;
		padding: 0.75rem 1rem;
		font-size: 0.8125rem;
		font-weight: 500;
		color: #6b7280;
		background: #ffffff;
		border: 1px solid #e5e7eb;
		border-radius: 6px;
		cursor: pointer;
		transition: all 0.2s;
		font-family: inherit;
		min-width: 120px;
	}

	.era-btn:hover {
		background: #f9fafb;
		border-color: #d1d5db;
	}

	.era-btn.active {
		color: #0d0d0d;
		background: #0d0d0d;
		color: #ffffff;
		border-color: #0d0d0d;
	}

	.era-name {
		font-weight: 500;
		margin-bottom: 0.25rem;
	}

	.era-year {
		font-size: 0.75rem;
		opacity: 0.8;
	}

	.era-btn.active .era-year {
		opacity: 0.9;
	}

	.example-section {
		background: #ffffff;
		border: 1px solid #e5e7eb;
		border-radius: 8px;
		overflow: hidden;
		margin-bottom: 1.5rem;
	}

	.example-header {
		padding: 1.5rem;
		border-bottom: 1px solid #e5e7eb;
		background: #f9fafb;
	}

	.example-header h3 {
		font-size: 1.25rem;
		font-weight: 600;
		margin: 0 0 0.5rem;
		color: #0d0d0d;
	}

	.example-description {
		font-size: 0.875rem;
		color: #6b7280;
		margin: 0;
		line-height: 1.5;
	}

	.example-content {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 0;
	}

	.code-block {
		padding: 1.5rem;
		background: #ffffff;
		border-right: 1px solid #e5e7eb;
		overflow-x: auto;
	}

	.code-block pre {
		margin: 0;
		font-size: 0.8125rem;
		line-height: 1.6;
		color: #0d0d0d;
	}

	.code-block code {
		font-family: 'SF Mono', Monaco, 'Cascadia Code', 'Roboto Mono', Consolas, 'Courier New', monospace;
		white-space: pre;
	}

	.visual-demo {
		padding: 1.5rem;
		background: #fafafa;
		display: flex;
		align-items: center;
		justify-content: center;
		min-height: 200px;
	}

	.visual-container {
		display: flex;
		gap: 1rem;
		width: 100%;
		max-width: 500px;
	}

	.visual-sidebar {
		width: 200px;
		background: #f0f0f0;
		padding: 1rem;
		border-radius: 4px;
		font-size: 0.875rem;
		color: #0d0d0d;
	}

	.visual-content {
		flex: 1;
		background: #ffffff;
		padding: 1rem;
		border-radius: 4px;
		border: 1px solid #e5e7eb;
		font-size: 0.875rem;
		color: #0d0d0d;
	}

	.visual-reuse {
		width: 100%;
		max-width: 500px;
	}

	.visual-heading {
		font-size: 1.5rem;
		font-family: Arial, sans-serif;
		color: #333;
		margin: 0 0 1rem;
		font-weight: 600;
	}

	.not-possible-badge {
		display: inline-block;
		margin-left: 0.5rem;
		padding: 0.25rem 0.5rem;
		font-size: 0.75rem;
		font-weight: 600;
		color: #dc2626;
		background: #fee2e2;
		border-radius: 4px;
	}

	.not-possible-message {
		text-align: center;
		padding: 2rem;
		color: #6b7280;
	}

	.not-possible-message p {
		margin: 0.5rem 0;
		font-size: 0.9375rem;
	}

	.not-possible-message .note {
		font-size: 0.8125rem;
		color: #9ca3af;
	}

	.dynamic-controls {
		display: flex;
		flex-direction: column;
		gap: 1.25rem;
		margin-bottom: 1.5rem;
		padding: 1rem;
		background: #ffffff;
		border-radius: 6px;
		border: 1px solid #e5e7eb;
	}

	.control-group {
		display: flex;
		flex-direction: column;
		gap: 0.5rem;
	}

	.control-group label {
		font-size: 0.875rem;
		font-weight: 500;
		color: #0d0d0d;
	}

	.color-input {
		width: 60px;
		height: 36px;
		border: 1px solid #e5e7eb;
		border-radius: 4px;
		cursor: pointer;
	}

	.size-input {
		width: 100%;
		height: 6px;
		border-radius: 3px;
		background: #e5e7eb;
		outline: none;
		cursor: pointer;
	}

	.size-input::-webkit-slider-thumb {
		appearance: none;
		width: 18px;
		height: 18px;
		border-radius: 50%;
		background: #0d0d0d;
		cursor: pointer;
	}

	.size-input::-moz-range-thumb {
		width: 18px;
		height: 18px;
		border-radius: 50%;
		background: #0d0d0d;
		border: none;
		cursor: pointer;
	}

	.color-value {
		font-size: 0.8125rem;
		font-family: 'SF Mono', Monaco, 'Cascadia Code', 'Roboto Mono', Consolas, 'Courier New', monospace;
		color: #6b7280;
		margin-top: 0.25rem;
	}

	.dynamic-box {
		padding: 1.5rem;
		background: rgba(59, 130, 246, 0.1);
		border: 2px solid var(--color, #3b82f6);
		border-radius: 6px;
		color: var(--color, #3b82f6);
		font-size: var(--size, 16px);
		font-weight: 500;
		text-align: center;
		transition: all 0.2s ease;
	}

	@media (max-width: 968px) {
		.example-content {
			grid-template-columns: 1fr;
		}

		.code-block {
			border-right: none;
			border-bottom: 1px solid #e5e7eb;
		}

		.era-selector {
			justify-content: flex-start;
		}

		.era-btn {
			min-width: 100px;
			padding: 0.625rem 0.875rem;
		}
	}

	@media (max-width: 640px) {
		.demo-container {
			padding: 1.5rem 1rem;
		}

		.header h2 {
			font-size: 1.5rem;
		}

		.era-selector {
			flex-direction: column;
		}

		.era-btn {
			width: 100%;
		}
	}
</style>