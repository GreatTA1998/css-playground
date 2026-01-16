<script lang="ts">
	import CssEvolutionDemo from './CssEvolutionDemo.svelte';
	import VanillaExtractDeepDive from './VanillaExtractDeepDive.svelte';

	type TimelineEvent = {
		year: number;
		title: string;
		description: string;
	};

	const events: TimelineEvent[] = [
		// HTML Era (pre-CSS)
		{
			year: 1991,
			title: 'HTML 1.0',
			description: 'Tim Berners-Lee creates HTML. Tags like <h1>, <h2>, <p> exist before CSS. Styling done via presentational HTML attributes.'
		},
		{
			year: 1993,
			title: 'HTML 2.0',
			description: 'Standardized HTML with tags like <table>, <form>. Still no CSS - styling via attributes like bgcolor, align, font.'
		},
		{
			year: 1994,
			title: 'JavaScript Created',
			description: 'Brendan Eich creates JavaScript (originally Mocha, then LiveScript) in 10 days at Netscape.'
		},
		// CSS Introduction
		{
			year: 1994,
			title: 'CSS Proposed',
			description: 'Håkon Wium Lie proposes Cascading Style Sheets. Separation of content (HTML) and presentation (CSS).'
		},
		{
			year: 1996,
			title: 'CSS 1.0',
			description: 'First CSS specification released. Basic styling: fonts, colors, spacing, alignment. Finally separates style from structure.'
		},
		{
			year: 1997,
			title: 'CSS Positioning',
			description: 'CSS positioning (relative, absolute, fixed) enables layout control beyond tables. Revolutionizes web design.'
		},
		{
			year: 1998,
			title: 'CSS 2.0',
			description: 'Media types, positioning, z-index, generated content. More powerful layout capabilities.'
		},
		// Preprocessor Era Begins
		{
			year: 2006,
			title: 'SASS Released',
			description: 'Hampton Catlin creates SASS. Variables, nesting, mixins solve CSS limitations.'
		},
		{
			year: 2009,
			title: 'LESS Released',
			description: 'Alexis Sellier creates LESS. Similar to SASS but uses JavaScript. Popularized CSS preprocessors.'
		},
		{
			year: 2010,
			title: 'Stylus Released',
			description: 'TJ Holowaychuk creates Stylus. More flexible syntax, optional braces/semicolons. Less popular but influential.'
		},
		{
			year: 2012,
			title: 'Flexbox (CSS)',
			description: 'Flexbox layout module becomes widely supported. One-dimensional layouts finally solved natively.'
		},
		{
			year: 2013,
			title: 'CSS Custom Properties (Variables)',
			description: 'CSS Variables (custom properties) proposed. Native CSS variables reduce need for preprocessors.'
		},
		{
			year: 2015,
			title: 'CSS Variables Browser Support',
			description: 'CSS Custom Properties gain widespread browser support. Native variables become viable alternative to SASS/LESS.'
		},
		{
			year: 2016,
			title: 'PostCSS Popularity',
			description: 'PostCSS ecosystem grows. Transforms CSS with JavaScript plugins. Enables autoprefixer, CSS modules, etc.'
		},
		{
			year: 2017,
			title: 'CSS Grid',
			description: 'CSS Grid Layout becomes widely supported. Two-dimensional layouts solved natively. Game changer for layouts.'
		},
		// Modern Tooling Era
		{
			year: 2017,
			title: 'Tailwind CSS v0.1',
			description: 'Adam Wathan releases Tailwind CSS. Utility-first CSS framework. Challenges component-based CSS approaches.'
		},
		{
			year: 2018,
			title: 'CSS Modules Mature',
			description: 'CSS Modules become standard in build tools. Scoped CSS solves global namespace pollution. Used with React, Vue, etc.'
		},
		{
			year: 2019,
			title: 'Vanilla Extract',
			description: 'Mark Dalgleish creates Vanilla Extract. Zero-runtime CSS-in-TypeScript. Type-safe, build-time CSS generation.'
		},
		{
			year: 2020,
			title: 'CSS Container Queries',
			description: 'Container queries proposed. Style based on container size, not viewport. More flexible responsive design.'
		},
		{
			year: 2022,
			title: 'CSS Container Queries Support',
			description: 'Container queries gain browser support. Revolutionary for component-based design. Less reliance on media queries.'
		},
		{
			year: 2023,
			title: 'CSS Nesting Browser Support',
			description: 'Native CSS nesting becomes widely supported. SASS nesting now available in vanilla CSS. Preprocessors less necessary.'
		},
		{
			year: 2024,
			title: 'CSS Layers',
			description: 'CSS Cascade Layers mature. Better control over specificity and cascade. Solves CSS architecture problems.'
		}
	];

	// Group events by year for better visualization
	const eventsByYear = events.reduce((acc, event) => {
		const key = event.year;
		if (!acc[key]) acc[key] = [];
		acc[key].push(event);
		return acc;
	}, {} as Record<number, TimelineEvent[]>);

	const years = Object.keys(eventsByYear).map(Number).sort((a, b) => a - b);
</script>

<div class="container">
	<CssEvolutionDemo />
	
	<VanillaExtractDeepDive />

	<div class="timeline">
		<div class="timeline-line"></div>
		
		{#each years as year}
			<div class="year-group">
				<div class="year-marker">{year}</div>
				<div class="year-events">
					{#each eventsByYear[year] as event}
						<div class="event">
							<div class="event-content">
								<span class="event-title">{event.title}</span> — <span class="event-description">{event.description}</span>
							</div>
						</div>
					{/each}
				</div>
			</div>
		{/each}
	</div>

	<div class="insights">
		<h2>Key Insights</h2>
		<div class="insights-grid">
			<div class="insight-card">
				<h3>HTML Predates CSS</h3>
				<p>
					HTML tags like <code>&lt;h1&gt;</code>, <code>&lt;h2&gt;</code>, <code>&lt;p&gt;</code> existed for years before CSS. Early web styling relied on presentational HTML attributes like <code>bgcolor</code>, <code>align</code>, and <code>font</code>.
				</p>
			</div>
			<div class="insight-card">
				<h3>Preprocessors Fill Gaps</h3>
				<p>
					SASS (2006) and LESS (2009) emerged to solve CSS limitations: no variables, nesting, or mixins. They became essential tools for large-scale CSS development.
				</p>
			</div>
			<div class="insight-card">
				<h3>Native CSS Catches Up</h3>
				<p>
					CSS Variables (2015), Nesting (2023), and Container Queries (2022) bring preprocessor features to native CSS, reducing dependency on build tools.
				</p>
			</div>
		</div>
	</div>
</div>

<style>
	/* Standard font sizes */
	:global(.text-sm) { font-size: 0.875rem; }
	:global(.text-base) { font-size: 1rem; }
	:global(.text-lg) { font-size: 1.125rem; }
	:global(.text-xl) { font-size: 1.25rem; }
	:global(.text-2xl) { font-size: 1.5rem; }
	:global(.text-3xl) { font-size: 2rem; }

	.container {
		max-width: 1200px;
		margin: 0 auto;
		padding: 3rem 2rem;
		font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
		color: #0d0d0d;
	}

	.header {
		margin-bottom: 3rem;
		text-align: center;
	}

	h1 {
		font-size: 2rem;
		font-weight: 600;
		margin: 0 0 0.5rem;
		line-height: 1.2;
		letter-spacing: -0.02em;
	}

	.subtitle {
		font-size: 1rem;
		color: #6b7280;
		line-height: 1.5;
		margin: 0;
		max-width: 700px;
		margin-left: auto;
		margin-right: auto;
	}

	.timeline {
		position: relative;
		margin-bottom: 4rem;
	}

	.timeline-line {
		position: absolute;
		left: 2rem;
		top: 0;
		bottom: 0;
		width: 1px;
		background: #e5e7eb;
	}

	.year-group {
		position: relative;
		margin-bottom: 1.5rem;
	}

	.year-marker {
		position: absolute;
		left: 0;
		top: 0;
		width: 4rem;
		height: 1.5rem;
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 0.875rem;
		font-weight: 500;
		color: #0d0d0d;
		background: #ffffff;
		border: 1px solid #e5e7eb;
		border-radius: 4px;
		z-index: 2;
	}

	.year-events {
		margin-left: 6rem;
		display: flex;
		flex-direction: column;
		gap: 0.5rem;
	}

	.event {
		position: relative;
	}

	.event-content {
		font-size: 0.875rem;
		line-height: 1.5;
		color: #6b7280;
	}

	.event-title {
		font-weight: 600;
		color: #0d0d0d;
	}

	.event-description {
		color: #6b7280;
	}

	.insights {
		margin-top: 4rem;
		padding-top: 3rem;
		border-top: 1px solid #e5e7eb;
	}

	h2 {
		font-size: 1.5rem;
		font-weight: 600;
		margin: 0 0 1.5rem;
		line-height: 1.4;
	}

	.insights-grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
		gap: 1rem;
	}

	.insight-card {
		background: #f9fafb;
		border: 1px solid #e5e7eb;
		border-radius: 6px;
		padding: 1.25rem;
	}

	h3 {
		font-size: 1rem;
		font-weight: 600;
		color: #0d0d0d;
		margin: 0 0 0.5rem;
		line-height: 1.4;
	}

	.insight-card p {
		font-size: 0.875rem;
		color: #6b7280;
		line-height: 1.5;
		margin: 0;
	}

	.insight-card code {
		font-size: 0.8125rem;
		font-family: 'SF Mono', Monaco, 'Cascadia Code', 'Roboto Mono', Consolas, 'Courier New', monospace;
		background: #ffffff;
		padding: 0.125rem 0.375rem;
		border-radius: 3px;
		border: 1px solid #e5e7eb;
		color: #0d0d0d;
	}

	@media (max-width: 768px) {
		.container {
			padding: 2rem 1.5rem;
		}

		h1 {
			font-size: 1.5rem;
		}

		.subtitle {
			font-size: 0.875rem;
		}

		.timeline-line {
			left: 1.5rem;
		}

		.year-marker {
			width: 3rem;
			font-size: 0.8125rem;
		}

		.year-events {
			margin-left: 4.5rem;
		}

		.insights-grid {
			grid-template-columns: 1fr;
		}
	}
</style>