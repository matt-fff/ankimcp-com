<script lang="ts">
	import { page } from '$app/state';
	import { Github, Sun, Moon } from 'lucide-svelte';
	import { onMount } from 'svelte';

	let theme = $state<'light' | 'dark'>('light');

	onMount(() => {
		const savedTheme = localStorage.getItem('theme') || 'light';
		theme = savedTheme as 'light' | 'dark';
		document.documentElement.setAttribute('data-theme', theme);
	});

	function toggleTheme() {
		theme = theme === 'light' ? 'dark' : 'light';
		localStorage.setItem('theme', theme);
		document.documentElement.setAttribute('data-theme', theme);
	}
</script>

<header>
	<div class="logo">
		<a href="/" class="logo-link">
			<h1>AnkiMCP</h1>
		</a>
	</div>

	<nav>
		<ul>
			<li class:active={page.url.pathname === '/'}>
				<a href="/">Home</a>
			</li>
			<li class:active={page.url.pathname.startsWith('/docs')}>
				<a href="/docs">Documentation</a>
			</li>
		</ul>
	</nav>

	<div class="header-actions">
		<button onclick={toggleTheme} class="theme-toggle" aria-label="Toggle theme">
			{#if theme === 'light'}
				<Moon class="h-5 w-5" />
			{:else}
				<Sun class="h-5 w-5" />
			{/if}
		</button>
		<a href="https://github.com/shivros/ankimcp" class="github-btn">
			<Github class="h-5 w-5" />
			<span>GitHub</span>
		</a>
	</div>
</header>

<style>
	header {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 1rem 2rem;
		background: rgba(5, 5, 16, 0.6);
		border-bottom: 1px solid rgba(255, 255, 255, 0.08);
		position: sticky;
		top: 0;
		z-index: 50;
		transition: all 0.2s;
		backdrop-filter: blur(20px);
		-webkit-backdrop-filter: blur(20px);
	}

	.logo h1 {
		margin: 0;
		font-size: 1.5rem;
		font-weight: 800;
		letter-spacing: -0.02em;
		background: linear-gradient(135deg, #ffffff 0%, #c4b5fd 60%, #818cf8 100%);
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		background-clip: text;
	}

	.logo-link {
		text-decoration: none;
	}

	.logo-link:hover h1 {
		background: linear-gradient(135deg, #ffffff 0%, #a78bfa 50%, #818cf8 100%);
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		background-clip: text;
	}

	nav ul {
		display: flex;
		list-style: none;
		margin: 0;
		padding: 0;
		gap: 0.5rem;
	}

	nav a {
		text-decoration: none;
		color: rgba(255, 255, 255, 0.55);
		font-weight: 500;
		padding: 0.5rem 1rem;
		border-radius: 0.5rem;
		transition: all 0.2s;
	}

	nav a:hover {
		color: rgba(255, 255, 255, 0.9);
		background: rgba(255, 255, 255, 0.08);
	}

	nav .active a {
		color: #ffffff;
		background: rgba(129, 140, 248, 0.2);
		border: 1px solid rgba(129, 140, 248, 0.15);
	}

	.header-actions {
		display: flex;
		align-items: center;
		gap: 0.75rem;
	}

	.theme-toggle {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 2.25rem;
		height: 2.25rem;
		background: rgba(255, 255, 255, 0.06);
		border: 1px solid rgba(255, 255, 255, 0.1);
		border-radius: 0.5rem;
		color: rgba(255, 255, 255, 0.6);
		cursor: pointer;
		transition: all 0.2s;
	}

	.theme-toggle:hover {
		background: rgba(129, 140, 248, 0.15);
		color: #c4b5fd;
		border-color: rgba(129, 140, 248, 0.25);
	}

	.github-btn {
		display: flex;
		align-items: center;
		gap: 0.5rem;
		padding: 0.5rem 1rem;
		background: rgba(124, 58, 237, 0.25);
		color: white;
		text-decoration: none;
		border-radius: 0.5rem;
		border: 1px solid rgba(167, 139, 250, 0.25);
		font-weight: 500;
		font-size: 0.875rem;
		transition: all 0.25s;
		box-shadow: 0 0 20px rgba(124, 58, 237, 0.1);
	}

	.github-btn:hover {
		background: rgba(124, 58, 237, 0.4);
		border-color: rgba(167, 139, 250, 0.4);
		box-shadow: 0 0 30px rgba(124, 58, 237, 0.2);
		transform: translateY(-1px);
	}

	@media (max-width: 768px) {
		header {
			padding: 0.75rem 1rem;
		}

		nav ul {
			gap: 0.25rem;
		}

		nav a {
			padding: 0.35rem 0.6rem;
			font-size: 0.875rem;
		}

		.header-actions {
			gap: 0.5rem;
		}

		.github-btn span {
			display: none;
		}

		.github-btn {
			padding: 0.5rem;
		}
	}
</style>
