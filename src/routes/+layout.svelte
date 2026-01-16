<script lang="ts">
	import favicon from '$lib/assets/favicon.svg';
	import { page } from '$app/stores';

	let { children } = $props();

	const navItems = [
		{ path: '/dialog-modal-popover', label: 'Dialog/Modal/Popover' },
		{ path: '/flexbox-multiline-shrink-wrap', label: 'Flexbox Multiline Shrink Wrap' },
		{ path: '/instagram-aspect-ratios', label: 'Instagram Aspect Ratios' },
		{ path: '/modern-css', label: 'Modern CSS' }
	];

	const currentPath = $derived($page.url.pathname);
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<nav class="nav">
	<div class="nav-container">
		{#each navItems as item}
			<a
				href={item.path}
				class="nav-link"
				class:active={currentPath === item.path || (currentPath === '/' && item.path === '/instagram-aspect-ratios')}
			>
				{item.label}
			</a>
		{/each}
	</div>
</nav>

<main class="main">
	{@render children()}
</main>

<style>
	.nav {
		background: #ffffff;
		border-bottom: 1px solid #e5e7eb;
		position: sticky;
		top: 0;
		z-index: 100;
		backdrop-filter: blur(10px);
		background: rgba(255, 255, 255, 0.95);
	}

	.nav-container {
		max-width: 1400px;
		margin: 0 auto;
		display: flex;
		gap: 0.5rem;
		padding: 0 2rem;
		overflow-x: auto;
	}

	.nav-link {
		padding: 1rem 1.5rem;
		text-decoration: none;
		color: #6b7280;
		font-size: 0.875rem;
		font-weight: 500;
		white-space: nowrap;
		border-bottom: 2px solid transparent;
		transition: all 0.2s ease;
		position: relative;
	}

	.nav-link:hover {
		color: #1a1a1a;
		background: #f9fafb;
	}

	.nav-link.active {
		color: #3b82f6;
		border-bottom-color: #3b82f6;
	}

	.main {
		min-height: calc(100vh - 60px);
	}

	@media (max-width: 768px) {
		.nav-container {
			padding: 0 1rem;
		}

		.nav-link {
			padding: 0.875rem 1rem;
			font-size: 0.8125rem;
		}
	}
</style>
