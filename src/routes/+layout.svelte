<script lang="ts">
	import Header from './Header.svelte';
	import '../app.css';

	let { children } = $props();
</script>

<div class="app-shell">
	<Header />
	<main>
		{@render children()}
	</main>

	<footer>
		<div class="footer-content">
			<div class="footer-section footer-brand">
				<h3>AnkiMCP</h3>
				<p>AI-powered Anki integration via MCP</p>
			</div>
			<div class="footer-section">
				<h4>Links</h4>
				<ul>
					<li><a href="/">Home</a></li>
					<li><a href="/docs">Documentation</a></li>
					<li><a href="https://github.com/shivros/ankimcp">GitHub</a></li>
				</ul>
			</div>
			<div class="footer-section">
				<h4>Related</h4>
				<ul>
					<li><a href="https://apps.ankiweb.net/">Anki</a></li>
					<li><a href="https://claude.ai/">Claude</a></li>
					<li><a href="https://github.com/modelcontextprotocol">MCP</a></li>
				</ul>
			</div>
		</div>
		<div class="footer-bottom">
			<p>&copy; 2024 AnkiMCP. Open source project.</p>
		</div>
	</footer>
</div>

<style>
	.app-shell {
		display: flex;
		flex-direction: column;
		min-height: 100vh;
		background: #050510;
	}

	main {
		flex: 1;
		padding: 0;
	}

	footer {
		position: relative;
		z-index: 1;
		background: rgba(5, 5, 16, 0.8);
		backdrop-filter: blur(16px);
		-webkit-backdrop-filter: blur(16px);
		border-top: 1px solid rgba(255, 255, 255, 0.06);
		padding: 3rem 0 1.5rem;
	}

	.footer-content {
		max-width: 1100px;
		margin: 0 auto;
		padding: 0 2rem;
		display: grid;
		grid-template-columns: 2fr 1fr 1fr;
		gap: 2rem;
	}

	.footer-section h3 {
		font-size: 1.25rem;
		font-weight: 800;
		letter-spacing: -0.02em;
		margin: 0 0 0.5rem 0;
		background: linear-gradient(135deg, #ffffff 0%, #c4b5fd 60%, #818cf8 100%);
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		background-clip: text;
	}

	.footer-section h4 {
		color: rgba(255, 255, 255, 0.5);
		font-size: 0.85rem;
		font-weight: 600;
		text-transform: uppercase;
		letter-spacing: 0.06em;
		margin: 0 0 0.75rem 0;
	}

	.footer-section p {
		color: rgba(255, 255, 255, 0.4);
		margin: 0;
		font-size: 0.9rem;
		line-height: 1.6;
	}

	.footer-section ul {
		list-style: none;
		padding: 0;
		margin: 0;
	}

	.footer-section li {
		margin-bottom: 0.5rem;
	}

	.footer-section a {
		color: rgba(255, 255, 255, 0.45);
		text-decoration: none;
		transition: color 0.2s;
		font-size: 0.9rem;
	}

	.footer-section a:hover {
		color: #c4b5fd;
	}

	.footer-bottom {
		max-width: 1100px;
		margin: 2.5rem auto 0;
		padding: 1.25rem 2rem 0;
		border-top: 1px solid rgba(255, 255, 255, 0.06);
		text-align: center;
	}

	.footer-bottom p {
		color: rgba(255, 255, 255, 0.3);
		font-size: 0.8rem;
		margin: 0;
	}

	@media (max-width: 768px) {
		.footer-content {
			grid-template-columns: 1fr;
			gap: 1.5rem;
			padding: 0 1.5rem;
		}

		.footer-bottom {
			padding: 1rem 1.5rem 0;
		}
	}
</style>
