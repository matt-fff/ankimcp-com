<script lang="ts">
	import {
		Code,
		Shield,
		CheckCircle,
		Layers,
		Cpu,
		Bot,
		Copy,
		Check,
		ArrowRight,
		Zap,
		Settings,
		Radio
	} from 'lucide-svelte';

	let copied = $state(false);
	let featureCardsVisible = $state(false);
	let connectionVisible = $state(false);
	let configVisible = $state(false);
	let statsVisible = $state(false);
	let ctaVisible = $state(false);

	const configJson = `{
  "mcpServers": {
    "ankimcp": {
      "type": "sse",
      "url": "http://localhost:4473/sse"
    }
  }
}`;

	function copyConfig() {
		navigator.clipboard.writeText(configJson);
		copied = true;
		setTimeout(() => (copied = false), 2000);
	}

	$effect(() => {
		const observers: IntersectionObserver[] = [];

		const createObserver = (
			selector: string,
			callback: (visible: boolean) => void,
			threshold = 0.15
		) => {
			const el = document.querySelector(selector);
			if (!el) return;
			const obs = new IntersectionObserver(
				(entries) => {
					entries.forEach((entry) => {
						if (entry.isIntersecting) {
							callback(true);
							obs.unobserve(entry.target);
						}
					});
				},
				{ threshold }
			);
			obs.observe(el);
			observers.push(obs);
		};

		createObserver('.feature-cards-section', () => (featureCardsVisible = true));
		createObserver('.connection-section', () => (connectionVisible = true));
		createObserver('.config-section', () => (configVisible = true));
		createObserver('.stats-section', () => (statsVisible = true));
		createObserver('.cta-section', () => (ctaVisible = true));

		return () => observers.forEach((obs) => obs.disconnect());
	});
</script>

<svelte:head>
	<title>AnkiMCP - AI-Powered Anki Integration</title>
	<meta
		name="description"
		content="Connect your Anki collection to AI assistants through the Model Context Protocol (MCP). Glassmorphism Aurora design."
	/>
</svelte:head>

<div class="aurora-page">
	<!-- Aurora Background -->
	<div class="aurora-bg" aria-hidden="true">
		<div class="aurora-blob blob-1"></div>
		<div class="aurora-blob blob-2"></div>
		<div class="aurora-blob blob-3"></div>
		<div class="aurora-blob blob-4"></div>
	</div>

	<!-- Hero Section -->
	<section class="hero-section">
		<div class="glass-hero">
			<h1 class="hero-title">AnkiMCP</h1>
			<p class="hero-tagline">
				Bridge your Anki flashcard collection with AI assistants through the Model Context
				Protocol. Study smarter with seamless AI integration.
			</p>
			<div class="hero-buttons">
				<a href="#quickstart" class="glass-btn glass-btn-primary">
					<Zap class="btn-icon" size={18} />
					Get Started
				</a>
				<a href="/docs" class="glass-btn glass-btn-secondary">
					Documentation
					<ArrowRight class="btn-icon" size={18} />
				</a>
			</div>
		</div>
	</section>

	<!-- Feature Cards Section -->
	<section class="feature-cards-section">
		<div class="section-container">
			<div class="feature-cards-grid" class:visible={featureCardsVisible}>
				<div class="glass-feature-card card-purple">
					<div class="card-tint tint-purple" aria-hidden="true"></div>
					<div class="card-content">
						<Code size={32} class="feature-icon" />
						<h3 class="feature-title">MCP Protocol</h3>
						<p class="feature-desc">
							Built on the Model Context Protocol standard, providing seamless integration with
							Claude Desktop and any MCP-compatible AI host.
						</p>
					</div>
				</div>
				<div class="glass-feature-card card-green">
					<div class="card-tint tint-green" aria-hidden="true"></div>
					<div class="card-content">
						<Shield size={32} class="feature-icon" />
						<h3 class="feature-title">Permission Control</h3>
						<p class="feature-desc">
							Fine-grained permissions let you control exactly what AI assistants can read, write,
							or modify in your collection.
						</p>
					</div>
				</div>
				<div class="glass-feature-card card-blue">
					<div class="card-tint tint-blue" aria-hidden="true"></div>
					<div class="card-content">
						<CheckCircle size={32} class="feature-icon" />
						<h3 class="feature-title">Full API</h3>
						<p class="feature-desc">
							Access decks, notes, cards, and review statistics. Create, update, and manage your
							Anki content programmatically.
						</p>
					</div>
				</div>
			</div>
		</div>
	</section>

	<!-- Connection Visualization -->
	<section class="connection-section">
		<div class="section-container">
			<div class="connection-vis" class:visible={connectionVisible}>
				<div class="connection-node node-left">
					<div class="node-circle">
						<Layers size={24} />
					</div>
					<span class="node-label">Anki</span>
				</div>
				<div class="connection-line line-left">
					<div class="traveling-particle"></div>
				</div>
				<div class="connection-node node-center">
					<div class="node-circle node-circle-large">
						<Cpu size={28} />
					</div>
					<span class="node-label">AnkiMCP</span>
				</div>
				<div class="connection-line line-right">
					<div class="traveling-particle particle-delayed"></div>
				</div>
				<div class="connection-node node-right">
					<div class="node-circle">
						<Bot size={24} />
					</div>
					<span class="node-label">AI Assistant</span>
				</div>
			</div>
		</div>
	</section>

	<!-- Config Panel -->
	<section class="config-section" id="quickstart">
		<div class="section-container">
			<h2 class="section-title" class:visible={configVisible}>MCP Configuration</h2>
			<div class="glass-config" class:visible={configVisible}>
				<div class="config-titlebar">
					<div class="titlebar-dots">
						<span class="dot dot-red"></span>
						<span class="dot dot-yellow"></span>
						<span class="dot dot-green"></span>
					</div>
					<span class="titlebar-label">mcp.json</span>
					<button class="glass-copy-btn" onclick={copyConfig}>
						{#if copied}
							<Check size={14} />
							<span>Copied</span>
						{:else}
							<Copy size={14} />
							<span>Copy</span>
						{/if}
					</button>
				</div>
				<pre class="config-code"><code>{@html syntaxHighlight(configJson)}</code></pre>
			</div>
		</div>
	</section>

	<!-- Stats Row -->
	<section class="stats-section">
		<div class="section-container">
			<div class="stats-row" class:visible={statsVisible}>
				<div class="glass-stat-pill">
					<Settings size={18} class="stat-icon" />
					<span class="stat-text">9 API Methods</span>
					<div class="stat-accent"></div>
				</div>
				<div class="glass-stat-pill">
					<Radio size={18} class="stat-icon" />
					<span class="stat-text">SSE Real-time</span>
					<div class="stat-accent"></div>
				</div>
				<div class="glass-stat-pill">
					<Zap size={18} class="stat-icon" />
					<span class="stat-text">3-Step Setup</span>
					<div class="stat-accent"></div>
				</div>
			</div>
		</div>
	</section>

	<!-- CTA Section -->
	<section class="cta-section">
		<div class="section-container">
			<div class="cta-glow" aria-hidden="true"></div>
			<div class="cta-content" class:visible={ctaVisible}>
				<h2 class="cta-title">Ready to connect your flashcards to AI?</h2>
				<p class="cta-subtitle">
					Start using AnkiMCP today and let AI assistants help you study, create cards, and
					analyze your learning progress.
				</p>
				<div class="cta-buttons">
					<a href="https://github.com/shivros/ankimcp" class="glass-btn glass-btn-cta-primary">
						View on GitHub
					</a>
					<a href="/docs" class="glass-btn glass-btn-cta-secondary"> Read the Docs </a>
				</div>
			</div>
		</div>
	</section>
</div>

<script lang="ts" module>
	function syntaxHighlight(json: string): string {
		return json
			.replace(/&/g, '&amp;')
			.replace(/</g, '&lt;')
			.replace(/>/g, '&gt;')
			.replace(
				/"([^"]+)"(?=\s*:)/g,
				'<span style="color: #67e8f9;">"$1"</span>'
			)
			.replace(
				/:\s*"([^"]+)"/g,
				': <span style="color: #86efac;">"$1"</span>'
			)
			.replace(
				/[{}]/g,
				(match) => `<span style="color: rgba(255,255,255,0.7);">${match}</span>`
			);
	}
</script>

<style>
	/* ========== Page Wrapper ========== */
	.aurora-page {
		position: relative;
		min-height: 100vh;
		overflow-x: hidden;
		background: #050510;
		color: #ffffff;
	}

	.aurora-page :global(a) {
		color: inherit;
	}

	/* ========== Aurora Background ========== */
	.aurora-bg {
		position: fixed;
		inset: 0;
		z-index: 0;
		overflow: hidden;
		pointer-events: none;
	}

	.aurora-blob {
		position: absolute;
		border-radius: 50%;
		filter: blur(100px);
		opacity: 0.45;
		will-change: transform;
	}

	.blob-1 {
		width: 600px;
		height: 600px;
		background: #4c1d95;
		top: -10%;
		left: -5%;
		animation: aurora-drift-1 18s ease-in-out infinite;
	}

	.blob-2 {
		width: 500px;
		height: 500px;
		background: #1d4ed8;
		top: 20%;
		right: -8%;
		animation: aurora-drift-2 20s ease-in-out infinite;
	}

	.blob-3 {
		width: 450px;
		height: 450px;
		background: #0d9488;
		bottom: 10%;
		left: 15%;
		animation: aurora-drift-3 16s ease-in-out infinite;
	}

	.blob-4 {
		width: 400px;
		height: 400px;
		background: #be185d;
		bottom: -5%;
		right: 20%;
		animation: aurora-drift-4 22s ease-in-out infinite;
	}

	@keyframes aurora-drift-1 {
		0%,
		100% {
			transform: translate(0, 0) scale(1);
		}
		33% {
			transform: translate(80px, 60px) scale(1.15);
		}
		66% {
			transform: translate(-40px, 100px) scale(0.9);
		}
	}

	@keyframes aurora-drift-2 {
		0%,
		100% {
			transform: translate(0, 0) scale(1);
		}
		33% {
			transform: translate(-70px, 80px) scale(1.1);
		}
		66% {
			transform: translate(50px, -60px) scale(0.95);
		}
	}

	@keyframes aurora-drift-3 {
		0%,
		100% {
			transform: translate(0, 0) scale(1);
		}
		33% {
			transform: translate(60px, -70px) scale(1.2);
		}
		66% {
			transform: translate(-80px, 40px) scale(0.85);
		}
	}

	@keyframes aurora-drift-4 {
		0%,
		100% {
			transform: translate(0, 0) scale(1);
		}
		33% {
			transform: translate(-50px, -80px) scale(1.1);
		}
		66% {
			transform: translate(70px, 50px) scale(0.9);
		}
	}

	/* ========== Section Container ========== */
	.section-container {
		position: relative;
		z-index: 1;
		max-width: 1100px;
		margin: 0 auto;
		padding: 0 1.5rem;
	}

	.section-title {
		text-align: center;
		font-size: clamp(1.5rem, 2vw + 1rem, 2.25rem);
		font-weight: 600;
		margin-bottom: 2.5rem;
		color: #ffffff;
		opacity: 0;
		transform: translateY(20px);
		transition:
			opacity 0.8s ease,
			transform 0.8s ease;
	}

	.section-title.visible {
		opacity: 1;
		transform: translateY(0);
	}

	/* ========== Glass Utility ========== */
	/* Shared glass surface mixin applied per-element */

	/* ========== Hero Section ========== */
	.hero-section {
		position: relative;
		z-index: 1;
		display: flex;
		align-items: center;
		justify-content: center;
		min-height: 100vh;
		padding: 6rem 1.5rem 4rem;
	}

	.glass-hero {
		text-align: center;
		max-width: 720px;
		width: 100%;
		padding: clamp(2.5rem, 5vw, 4rem);
		background: rgba(255, 255, 255, 0.07);
		backdrop-filter: blur(24px);
		-webkit-backdrop-filter: blur(24px);
		border: 1px solid rgba(255, 255, 255, 0.12);
		border-radius: 1.5rem;
		box-shadow:
			0 0 80px rgba(129, 140, 248, 0.08),
			inset 0 1px 0 rgba(255, 255, 255, 0.08);
		animation: hero-float-in 1s ease-out both;
	}

	@keyframes hero-float-in {
		from {
			opacity: 0;
			transform: translateY(40px) scale(0.97);
		}
		to {
			opacity: 1;
			transform: translateY(0) scale(1);
		}
	}

	.hero-title {
		font-size: clamp(3rem, 5vw + 1rem, 6rem);
		font-weight: 800;
		letter-spacing: -0.03em;
		line-height: 1.05;
		margin: 0 0 1rem;
		background: linear-gradient(135deg, #ffffff 0%, #c4b5fd 50%, #818cf8 100%);
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		background-clip: text;
	}

	.hero-tagline {
		font-size: clamp(1rem, 1.2vw + 0.6rem, 1.25rem);
		line-height: 1.7;
		color: rgba(255, 255, 255, 0.7);
		margin: 0 0 2.5rem;
		max-width: 540px;
		margin-left: auto;
		margin-right: auto;
	}

	.hero-buttons {
		display: flex;
		gap: 1rem;
		justify-content: center;
		flex-wrap: wrap;
	}

	/* ========== Glass Buttons ========== */
	.glass-btn {
		display: inline-flex;
		align-items: center;
		gap: 0.5rem;
		padding: 0.8rem 1.75rem;
		border-radius: 0.75rem;
		font-weight: 600;
		font-size: 0.95rem;
		text-decoration: none;
		transition: all 0.3s ease;
		cursor: pointer;
		border: 1px solid rgba(255, 255, 255, 0.15);
	}

	.glass-btn:hover {
		text-decoration: none;
	}

	:global(.btn-icon) {
		flex-shrink: 0;
	}

	.glass-btn-primary {
		background: rgba(37, 99, 235, 0.3);
		backdrop-filter: blur(12px);
		-webkit-backdrop-filter: blur(12px);
		color: #ffffff;
		border-color: rgba(96, 165, 250, 0.3);
		box-shadow: 0 0 20px rgba(37, 99, 235, 0.15);
	}

	.glass-btn-primary:hover {
		background: rgba(37, 99, 235, 0.45);
		border-color: rgba(96, 165, 250, 0.5);
		box-shadow: 0 0 30px rgba(37, 99, 235, 0.25);
		transform: translateY(-2px);
	}

	.glass-btn-secondary {
		background: rgba(255, 255, 255, 0.08);
		backdrop-filter: blur(12px);
		-webkit-backdrop-filter: blur(12px);
		color: rgba(255, 255, 255, 0.9);
	}

	.glass-btn-secondary:hover {
		background: rgba(255, 255, 255, 0.14);
		border-color: rgba(255, 255, 255, 0.25);
		transform: translateY(-2px);
	}

	/* ========== Feature Cards ========== */
	.feature-cards-section {
		position: relative;
		z-index: 1;
		padding: 6rem 0;
	}

	.feature-cards-grid {
		display: grid;
		grid-template-columns: repeat(3, 1fr);
		gap: 1.5rem;
	}

	.feature-cards-grid > * {
		opacity: 0;
		transform: translateY(40px);
		transition:
			opacity 0.7s ease,
			transform 0.7s ease;
	}

	.feature-cards-grid.visible > :nth-child(1) {
		opacity: 1;
		transform: translateY(0);
		transition-delay: 0s;
	}

	.feature-cards-grid.visible > :nth-child(2) {
		opacity: 1;
		transform: translateY(0);
		transition-delay: 0.15s;
	}

	.feature-cards-grid.visible > :nth-child(3) {
		opacity: 1;
		transform: translateY(0);
		transition-delay: 0.3s;
	}

	.glass-feature-card {
		position: relative;
		overflow: hidden;
		padding: 2rem;
		border-radius: 1.25rem;
		background: rgba(255, 255, 255, 0.06);
		backdrop-filter: blur(20px);
		-webkit-backdrop-filter: blur(20px);
		border: 1px solid rgba(255, 255, 255, 0.1);
		box-shadow:
			0 0 40px rgba(0, 0, 0, 0.1),
			inset 0 1px 0 rgba(255, 255, 255, 0.06);
		transition: all 0.4s ease;
	}

	.glass-feature-card:hover {
		transform: translateY(-8px);
		border-color: rgba(255, 255, 255, 0.22);
		box-shadow:
			0 20px 60px rgba(0, 0, 0, 0.2),
			0 0 40px rgba(129, 140, 248, 0.1),
			inset 0 1px 0 rgba(255, 255, 255, 0.1);
	}

	.card-tint {
		position: absolute;
		top: -30%;
		left: -20%;
		width: 200%;
		height: 200%;
		border-radius: 50%;
		filter: blur(80px);
		opacity: 0.12;
		pointer-events: none;
		transition: opacity 0.4s ease;
	}

	.glass-feature-card:hover .card-tint {
		opacity: 0.2;
	}

	.tint-purple {
		background: #7c3aed;
	}
	.tint-green {
		background: #10b981;
	}
	.tint-blue {
		background: #2563eb;
	}

	.card-content {
		position: relative;
		z-index: 1;
	}

	:global(.feature-icon) {
		color: rgba(255, 255, 255, 0.85);
		margin-bottom: 1rem;
	}

	.feature-title {
		font-size: 1.2rem;
		font-weight: 600;
		margin: 0 0 0.75rem;
		color: #ffffff;
	}

	.feature-desc {
		font-size: 0.92rem;
		line-height: 1.7;
		color: rgba(255, 255, 255, 0.6);
		margin: 0;
	}

	/* ========== Connection Visualization ========== */
	.connection-section {
		position: relative;
		z-index: 1;
		padding: 4rem 0 6rem;
	}

	.connection-vis {
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 0;
		opacity: 0;
		transform: translateY(30px);
		transition:
			opacity 0.8s ease,
			transform 0.8s ease;
	}

	.connection-vis.visible {
		opacity: 1;
		transform: translateY(0);
	}

	.connection-node {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 0.75rem;
		flex-shrink: 0;
	}

	.node-circle {
		width: 64px;
		height: 64px;
		border-radius: 50%;
		background: rgba(255, 255, 255, 0.08);
		backdrop-filter: blur(16px);
		-webkit-backdrop-filter: blur(16px);
		border: 1px solid rgba(255, 255, 255, 0.15);
		display: flex;
		align-items: center;
		justify-content: center;
		color: rgba(255, 255, 255, 0.85);
		box-shadow: 0 0 30px rgba(129, 140, 248, 0.08);
	}

	.node-circle-large {
		width: 80px;
		height: 80px;
		background: rgba(255, 255, 255, 0.1);
		border-color: rgba(129, 140, 248, 0.3);
		box-shadow: 0 0 40px rgba(129, 140, 248, 0.15);
	}

	.node-label {
		font-size: 0.85rem;
		font-weight: 600;
		color: rgba(255, 255, 255, 0.7);
		letter-spacing: 0.02em;
	}

	.connection-line {
		position: relative;
		width: clamp(60px, 12vw, 160px);
		height: 2px;
		background: linear-gradient(
			90deg,
			rgba(129, 140, 248, 0.15),
			rgba(129, 140, 248, 0.4),
			rgba(129, 140, 248, 0.15)
		);
		margin: 0 0.5rem;
		margin-bottom: 2rem;
		overflow: visible;
	}

	.traveling-particle {
		position: absolute;
		top: 50%;
		left: -6px;
		width: 12px;
		height: 12px;
		border-radius: 50%;
		background: radial-gradient(circle, #818cf8 0%, rgba(129, 140, 248, 0) 70%);
		box-shadow: 0 0 12px 4px rgba(129, 140, 248, 0.5);
		transform: translateY(-50%);
		animation: travel-along 2.5s ease-in-out infinite;
	}

	.particle-delayed {
		animation-delay: 1.25s;
	}

	@keyframes travel-along {
		0% {
			left: -6px;
			opacity: 0;
		}
		10% {
			opacity: 1;
		}
		90% {
			opacity: 1;
		}
		100% {
			left: calc(100% - 6px);
			opacity: 0;
		}
	}

	/* ========== Config Panel ========== */
	.config-section {
		position: relative;
		z-index: 1;
		padding: 4rem 0 6rem;
	}

	.glass-config {
		max-width: 600px;
		margin: 0 auto;
		border-radius: 1rem;
		overflow: hidden;
		background: rgba(255, 255, 255, 0.05);
		backdrop-filter: blur(20px);
		-webkit-backdrop-filter: blur(20px);
		border: 1px solid rgba(255, 255, 255, 0.1);
		box-shadow: 0 0 60px rgba(0, 0, 0, 0.15);
		opacity: 0;
		transform: translateY(30px) perspective(800px) rotateX(0deg);
		transition:
			opacity 0.8s ease,
			transform 0.8s ease,
			box-shadow 0.4s ease;
		transition-delay: 0.15s;
	}

	.glass-config.visible {
		opacity: 1;
		transform: translateY(0) perspective(800px) rotateX(0deg);
	}

	.glass-config:hover {
		box-shadow:
			0 0 60px rgba(0, 0, 0, 0.15),
			0 0 30px rgba(129, 140, 248, 0.06);
		transform: translateY(0) perspective(800px) rotateX(1.5deg);
	}

	.config-titlebar {
		display: flex;
		align-items: center;
		padding: 0.75rem 1rem;
		background: rgba(255, 255, 255, 0.04);
		border-bottom: 1px solid rgba(255, 255, 255, 0.08);
	}

	.titlebar-dots {
		display: flex;
		gap: 6px;
	}

	.dot {
		width: 12px;
		height: 12px;
		border-radius: 50%;
	}

	.dot-red {
		background: #ef4444;
	}
	.dot-yellow {
		background: #eab308;
	}
	.dot-green {
		background: #22c55e;
	}

	.titlebar-label {
		flex: 1;
		text-align: center;
		font-size: 0.8rem;
		font-family: var(--font-mono);
		color: rgba(255, 255, 255, 0.4);
	}

	.glass-copy-btn {
		display: inline-flex;
		align-items: center;
		gap: 0.35rem;
		padding: 0.3rem 0.7rem;
		border-radius: 0.4rem;
		background: rgba(255, 255, 255, 0.08);
		border: 1px solid rgba(255, 255, 255, 0.1);
		color: rgba(255, 255, 255, 0.6);
		font-size: 0.75rem;
		cursor: pointer;
		transition: all 0.2s ease;
	}

	.glass-copy-btn:hover {
		background: rgba(255, 255, 255, 0.14);
		color: rgba(255, 255, 255, 0.9);
	}

	.config-code {
		padding: 1.25rem 1.5rem;
		margin: 0;
		font-family: var(--font-mono);
		font-size: 0.9rem;
		line-height: 1.6;
		color: rgba(255, 255, 255, 0.85);
		background: transparent;
		border: none;
		border-radius: 0;
		box-shadow: none;
		overflow-x: auto;
	}

	.config-code code {
		font-family: inherit;
	}

	/* ========== Stats Row ========== */
	.stats-section {
		position: relative;
		z-index: 1;
		padding: 2rem 0 6rem;
	}

	.stats-row {
		display: flex;
		justify-content: center;
		gap: 1.5rem;
		flex-wrap: wrap;
	}

	.stats-row > * {
		opacity: 0;
		transform: translateY(24px);
		transition:
			opacity 0.6s ease,
			transform 0.6s ease;
	}

	.stats-row.visible > :nth-child(1) {
		opacity: 1;
		transform: translateY(0);
		transition-delay: 0s;
	}

	.stats-row.visible > :nth-child(2) {
		opacity: 1;
		transform: translateY(0);
		transition-delay: 0.12s;
	}

	.stats-row.visible > :nth-child(3) {
		opacity: 1;
		transform: translateY(0);
		transition-delay: 0.24s;
	}

	.glass-stat-pill {
		position: relative;
		display: flex;
		align-items: center;
		gap: 0.6rem;
		padding: 0.85rem 1.75rem;
		border-radius: 999px;
		background: rgba(255, 255, 255, 0.06);
		backdrop-filter: blur(16px);
		-webkit-backdrop-filter: blur(16px);
		border: 1px solid rgba(255, 255, 255, 0.1);
		overflow: hidden;
	}

	:global(.stat-icon) {
		color: rgba(129, 140, 248, 0.8);
		flex-shrink: 0;
	}

	.stat-text {
		font-size: 0.9rem;
		font-weight: 600;
		color: rgba(255, 255, 255, 0.85);
		white-space: nowrap;
	}

	.stat-accent {
		position: absolute;
		bottom: 0;
		left: 20%;
		right: 20%;
		height: 2px;
		background: linear-gradient(90deg, transparent, #818cf8, transparent);
		animation: accent-pulse 3s ease-in-out infinite;
	}

	@keyframes accent-pulse {
		0%,
		100% {
			opacity: 0.4;
			transform: scaleX(0.7);
		}
		50% {
			opacity: 0.9;
			transform: scaleX(1);
		}
	}

	/* ========== CTA Section ========== */
	.cta-section {
		position: relative;
		z-index: 1;
		padding: 6rem 0 8rem;
	}

	.cta-glow {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		width: 500px;
		height: 300px;
		border-radius: 50%;
		background: radial-gradient(ellipse, rgba(129, 140, 248, 0.12) 0%, transparent 70%);
		pointer-events: none;
		filter: blur(40px);
	}

	.cta-content {
		position: relative;
		text-align: center;
		opacity: 0;
		transform: translateY(30px);
		transition:
			opacity 0.8s ease,
			transform 0.8s ease;
	}

	.cta-content.visible {
		opacity: 1;
		transform: translateY(0);
	}

	.cta-title {
		font-size: clamp(1.75rem, 3vw + 0.5rem, 2.75rem);
		font-weight: 700;
		margin: 0 0 1.25rem;
		color: #ffffff;
		letter-spacing: -0.02em;
	}

	.cta-subtitle {
		font-size: 1.1rem;
		line-height: 1.7;
		color: rgba(255, 255, 255, 0.6);
		max-width: 540px;
		margin: 0 auto 2.5rem;
	}

	.cta-buttons {
		display: flex;
		gap: 1rem;
		justify-content: center;
		flex-wrap: wrap;
	}

	.glass-btn-cta-primary {
		background: rgba(124, 58, 237, 0.25);
		backdrop-filter: blur(12px);
		-webkit-backdrop-filter: blur(12px);
		color: #ffffff;
		border-color: rgba(167, 139, 250, 0.3);
		box-shadow: 0 0 25px rgba(124, 58, 237, 0.15);
		padding: 0.9rem 2rem;
		font-size: 1rem;
	}

	.glass-btn-cta-primary:hover {
		background: rgba(124, 58, 237, 0.4);
		border-color: rgba(167, 139, 250, 0.5);
		box-shadow: 0 0 40px rgba(124, 58, 237, 0.25);
		transform: translateY(-2px);
	}

	.glass-btn-cta-secondary {
		background: rgba(255, 255, 255, 0.06);
		backdrop-filter: blur(12px);
		-webkit-backdrop-filter: blur(12px);
		color: rgba(255, 255, 255, 0.85);
		padding: 0.9rem 2rem;
		font-size: 1rem;
	}

	.glass-btn-cta-secondary:hover {
		background: rgba(255, 255, 255, 0.12);
		border-color: rgba(255, 255, 255, 0.25);
		transform: translateY(-2px);
	}

	/* ========== Responsive ========== */
	@media (max-width: 768px) {
		.feature-cards-grid {
			grid-template-columns: 1fr;
			gap: 1.25rem;
		}

		.connection-vis {
			flex-direction: column;
			gap: 0.5rem;
		}

		.connection-line {
			width: 2px;
			height: 50px;
			margin: 0;
			margin-bottom: 0;
			background: linear-gradient(
				180deg,
				rgba(129, 140, 248, 0.15),
				rgba(129, 140, 248, 0.4),
				rgba(129, 140, 248, 0.15)
			);
		}

		.traveling-particle {
			animation: travel-along-vertical 2.5s ease-in-out infinite;
			left: 50%;
			top: -6px;
			transform: translateX(-50%);
		}

		.particle-delayed {
			animation: travel-along-vertical 2.5s ease-in-out infinite;
			animation-delay: 1.25s;
		}

		@keyframes travel-along-vertical {
			0% {
				top: -6px;
				opacity: 0;
			}
			10% {
				opacity: 1;
			}
			90% {
				opacity: 1;
			}
			100% {
				top: calc(100% - 6px);
				opacity: 0;
			}
		}

		.hero-section {
			min-height: auto;
			padding: 8rem 1rem 4rem;
		}

		.glass-hero {
			padding: 2rem 1.5rem;
		}

		.stats-row {
			flex-direction: column;
			align-items: center;
			gap: 1rem;
		}

		.aurora-blob {
			filter: blur(80px);
			opacity: 0.3;
		}

		.blob-1 {
			width: 350px;
			height: 350px;
		}

		.blob-2 {
			width: 300px;
			height: 300px;
		}

		.blob-3 {
			width: 280px;
			height: 280px;
		}

		.blob-4 {
			width: 250px;
			height: 250px;
		}
	}

	@media (max-width: 480px) {
		.hero-buttons,
		.cta-buttons {
			flex-direction: column;
			align-items: center;
		}

		.glass-btn {
			width: 100%;
			max-width: 260px;
			justify-content: center;
		}
	}
</style>
