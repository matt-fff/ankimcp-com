<script lang="ts">
	import { Copy, Check, Terminal, Wifi, Zap, Shield, Database, Radio } from 'lucide-svelte';

	// Typing animation state
	let typedText = $state('');
	let showCursor = $state(true);
	let typingDone = $state(false);
	const fullText =
		'Expose your Anki collection to AI assistants via MCP — enabling them to study, create cards, and analyze your learning.';

	// Copy button state
	let copied = $state(false);

	// Scroll reveal state
	let commandsVisible = $state(false);
	let architectureVisible = $state(false);
	let installVisible = $state(false);

	// Feature commands for the command palette
	const features = [
		{ cmd: 'MCP Protocol', desc: 'Standardized AI integration with Claude Desktop' },
		{ cmd: 'Permissions', desc: 'Fine-grained read/write/delete control' },
		{ cmd: 'Full API', desc: 'Decks, notes, cards, review statistics' },
		{ cmd: 'Real-time', desc: 'SSE-based live server connection' }
	];

	let visibleFeatures = $state(0);

	// Typing effect
	$effect(() => {
		let i = 0;
		const interval = setInterval(() => {
			if (i < fullText.length) {
				typedText = fullText.slice(0, i + 1);
				i++;
			} else {
				typingDone = true;
				clearInterval(interval);
			}
		}, 28);

		return () => clearInterval(interval);
	});

	// Blinking cursor
	$effect(() => {
		const interval = setInterval(() => {
			showCursor = !showCursor;
		}, 530);
		return () => clearInterval(interval);
	});

	// Intersection observer for scroll reveals
	$effect(() => {
		if (typeof IntersectionObserver === 'undefined') return;

		const observer = new IntersectionObserver(
			(entries) => {
				for (const entry of entries) {
					if (entry.isIntersecting) {
						const id = (entry.target as HTMLElement).dataset.section;
						if (id === 'commands') {
							commandsVisible = true;
							// Stagger feature reveals
							let count = 0;
							const featureInterval = setInterval(() => {
								count++;
								visibleFeatures = count;
								if (count >= features.length) clearInterval(featureInterval);
							}, 300);
						}
						if (id === 'architecture') architectureVisible = true;
						if (id === 'install') installVisible = true;
						observer.unobserve(entry.target);
					}
				}
			},
			{ threshold: 0.2 }
		);

		// Defer to next tick so DOM elements exist
		setTimeout(() => {
			const sections = document.querySelectorAll('[data-section]');
			sections.forEach((el) => observer.observe(el));
		}, 0);

		return () => observer.disconnect();
	});

	function copyInstall() {
		navigator.clipboard.writeText('pip install ankimcp');
		copied = true;
		setTimeout(() => (copied = false), 2000);
	}
</script>

<svelte:head>
	<title>AnkiMCP - Terminal Noir</title>
	<meta
		name="description"
		content="AnkiMCP bridges your Anki flashcard collections with AI assistants through the Model Context Protocol."
	/>
</svelte:head>

{#snippet terminalDot(color: string)}
	<span class="terminal-dot" style="background: {color};"></span>
{/snippet}

{#snippet sectionLabel(text: string)}
	<div class="section-label">
		<span class="section-label-bracket">[</span>
		<span class="section-label-text">{text}</span>
		<span class="section-label-bracket">]</span>
	</div>
{/snippet}

<!-- HERO -->
<div class="terminal-page">
	<section class="hero">
		<div class="scanlines"></div>
		<div class="grid-bg"></div>
		<div class="hero-content">
			<div class="hero-badge">
				<Terminal class="hero-badge-icon" />
				<span>MODEL CONTEXT PROTOCOL</span>
			</div>
			<h1 class="hero-title">AnkiMCP</h1>
			<div class="hero-subtitle">
				<span class="prompt-symbol">$</span>
				<span class="typed-text">{typedText}</span><span
					class="cursor"
					class:cursor-visible={showCursor}
					class:cursor-hidden={!showCursor}>_</span
				>
			</div>
			<div class="hero-actions">
				<a href="#install" class="btn-terminal btn-primary-t">
					<Zap class="btn-icon" />
					<span>Get Started</span>
				</a>
				<a href="/docs" class="btn-terminal btn-secondary-t">
					<Database class="btn-icon" />
					<span>Documentation</span>
				</a>
			</div>
		</div>
		<div class="hero-scroll-hint">
			<span class="scroll-arrow">v</span>
			<span class="scroll-arrow scroll-arrow-delayed">v</span>
			<span class="scroll-arrow scroll-arrow-more-delayed">v</span>
		</div>
	</section>

	<!-- COMMAND PALETTE -->
	<section class="section" data-section="commands">
		{@render sectionLabel('FEATURES')}
		<div class="terminal-window" class:visible={commandsVisible}>
			<div class="terminal-header">
				{@render terminalDot('#ff5f57')}
				{@render terminalDot('#febc2e')}
				{@render terminalDot('#28c840')}
				<span class="terminal-title">ankimcp --list-features</span>
			</div>
			<div class="terminal-body">
				<div class="command-line">
					<span class="prompt">$</span>
					<span class="command-text">ankimcp --list-features</span>
				</div>
				{#each features as feature, i}
					<div
						class="feature-line"
						class:feature-visible={i < visibleFeatures}
						style="transition-delay: {i * 80}ms;"
					>
						<span class="feature-arrow">&gt;</span>
						<span class="feature-name">{feature.cmd}</span>
						<span class="feature-separator">--</span>
						<span class="feature-desc">{feature.desc}</span>
					</div>
				{/each}
				<div class="command-line" class:feature-visible={visibleFeatures >= features.length}>
					<span class="prompt">$</span>
					<span class="cursor-block">_</span>
				</div>
			</div>
		</div>
	</section>

	<!-- ARCHITECTURE -->
	<section class="section" data-section="architecture">
		{@render sectionLabel('ARCHITECTURE')}
		<div class="architecture-box" class:visible={architectureVisible}>
			<div class="terminal-header">
				{@render terminalDot('#ff5f57')}
				{@render terminalDot('#febc2e')}
				{@render terminalDot('#28c840')}
				<span class="terminal-title">system architecture</span>
			</div>
			<div class="architecture-body">
				<div class="arch-row">
					<div class="arch-node arch-anki">
						<div class="arch-node-icon">
							<Database class="arch-icon" />
						</div>
						<div class="arch-node-label">Anki</div>
						<div class="arch-node-sub">Flashcard Engine</div>
					</div>

					<div class="arch-connection">
						<div class="arch-line"></div>
						<div class="arch-line-label">Add-on Bridge</div>
					</div>

					<div class="arch-node arch-server">
						<div class="arch-node-icon">
							<Radio class="arch-icon" />
						</div>
						<div class="arch-node-label">AnkiMCP</div>
						<div class="arch-node-sub">MCP Server</div>
					</div>

					<div class="arch-connection">
						<div class="arch-line"></div>
						<div class="arch-line-label">SSE / HTTP</div>
					</div>

					<div class="arch-node arch-ai">
						<div class="arch-node-icon">
							<Zap class="arch-icon" />
						</div>
						<div class="arch-node-label">Claude</div>
						<div class="arch-node-sub">AI Assistant</div>
					</div>
				</div>

				<div class="arch-ascii">
					<pre>┌──────────┐     ┌─────────────┐     ┌──────────┐
│   Anki   │◄───►│   AnkiMCP   │◄───►│  Claude  │
│  Desktop │     │   Server    │     │    AI    │
└──────────┘     └─────────────┘     └──────────┘
   :4473              :SSE              :MCP</pre>
				</div>
			</div>
		</div>
	</section>

	<!-- INSTALL -->
	<section class="section" id="install" data-section="install">
		{@render sectionLabel('QUICK START')}
		<div class="install-box" class:visible={installVisible}>
			<div class="terminal-header">
				{@render terminalDot('#ff5f57')}
				{@render terminalDot('#febc2e')}
				{@render terminalDot('#28c840')}
				<span class="terminal-title">installation</span>
			</div>
			<div class="install-body">
				<div class="install-step">
					<span class="install-step-num">01</span>
					<span class="install-step-label">Install the Anki Add-on</span>
				</div>
				<div class="install-command-group">
					<div class="command-line">
						<span class="prompt">$</span>
						<span class="command-text"
							>anki <span class="flag">→</span> Tools <span class="flag">→</span> Add-ons <span
								class="flag">→</span
							> Get Add-ons...</span
						>
					</div>
					<div class="command-line">
						<span class="prompt">&gt;</span>
						<span class="command-text"
							>Enter code: <span class="highlight-code">1513864660</span></span
						>
					</div>
				</div>

				<div class="install-step">
					<span class="install-step-num">02</span>
					<span class="install-step-label">Configure your MCP Host</span>
				</div>
				<div class="install-code-block">
					<div class="install-code-header">
						<span class="install-code-filename">mcp.json</span>
						<button class="copy-btn" onclick={copyInstall}>
							{#if copied}
								<Check class="copy-icon" />
								<span>Copied</span>
							{:else}
								<Copy class="copy-icon" />
								<span>Copy</span>
							{/if}
						</button>
					</div>
					<pre class="install-pre"><code>{`{
  "mcpServers": {
    "ankimcp": {
      "type": "sse",
      "url": "http://localhost:4473/sse"
    }
  }
}`}</code></pre>
				</div>

				<div class="install-step">
					<span class="install-step-num">03</span>
					<span class="install-step-label">Start using with Claude</span>
				</div>
				<div class="command-line">
					<span class="prompt">$</span>
					<span class="command-text"
						>claude <span class="string-literal">"What Anki decks do I have?"</span></span
					>
				</div>
				<div class="install-output">
					<span class="output-success">Connected to AnkiMCP server on port 4473</span>
					<span class="output-info">Found 12 decks with 3,847 cards total</span>
					<span class="output-info">Ready for AI-assisted study sessions</span>
				</div>
			</div>
		</div>
	</section>

	<!-- STATUS BAR -->
	<footer class="status-bar">
		<div class="status-bar-inner">
			<div class="status-group">
				<span class="status-dot status-dot-green"></span>
				<span class="status-label">CONNECTED</span>
			</div>
			<div class="status-divider"></div>
			<div class="status-group">
				<Wifi class="status-icon" />
				<span class="status-label">PORT 4473</span>
			</div>
			<div class="status-divider"></div>
			<div class="status-group">
				<span class="status-dot status-dot-green"></span>
				<span class="status-label">SSE ACTIVE</span>
			</div>
			<div class="status-divider"></div>
			<div class="status-group">
				<Shield class="status-icon" />
				<span class="status-label">PERMISSIONS OK</span>
			</div>
			<div class="status-spacer"></div>
			<div class="status-group">
				<span class="status-version">v1.0.0</span>
			</div>
		</div>
	</footer>
</div>

<style>
	/* ============================
	   BASE / PAGE CONTAINER
	   ============================ */
	.terminal-page {
		background: #0a0a0f;
		color: #e0e0e0;
		font-family: var(--font-mono), 'Fira Mono', 'Fira Code', 'Cascadia Code', monospace;
		min-height: 100vh;
		overflow-x: hidden;
		/* Override the layout's container padding */
		margin: -2rem;
		padding: 0;
	}

	/* ============================
	   HERO SECTION
	   ============================ */
	.hero {
		position: relative;
		min-height: 100vh;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding: 2rem;
		overflow: hidden;
	}

	/* Scanline overlay */
	.scanlines {
		position: absolute;
		inset: 0;
		background: repeating-linear-gradient(
			0deg,
			transparent,
			transparent 2px,
			rgba(0, 255, 136, 0.015) 2px,
			rgba(0, 255, 136, 0.015) 4px
		);
		pointer-events: none;
		z-index: 2;
		animation: scanline-scroll 8s linear infinite;
	}

	@keyframes scanline-scroll {
		from {
			background-position: 0 0;
		}
		to {
			background-position: 0 100vh;
		}
	}

	/* Grid background */
	.grid-bg {
		position: absolute;
		inset: 0;
		background-image:
			linear-gradient(rgba(0, 255, 136, 0.03) 1px, transparent 1px),
			linear-gradient(90deg, rgba(0, 255, 136, 0.03) 1px, transparent 1px);
		background-size: 60px 60px;
		pointer-events: none;
		z-index: 1;
	}

	.hero-content {
		position: relative;
		z-index: 3;
		text-align: center;
		max-width: 800px;
	}

	.hero-badge {
		display: inline-flex;
		align-items: center;
		gap: 0.5rem;
		padding: 0.4rem 1rem;
		border: 1px solid rgba(0, 255, 136, 0.2);
		border-radius: 2px;
		font-size: 0.7rem;
		letter-spacing: 0.2em;
		color: #00ff88;
		margin-bottom: 2rem;
		background: rgba(0, 255, 136, 0.04);
	}

	:global(.hero-badge-icon) {
		width: 14px;
		height: 14px;
	}

	.hero-title {
		font-size: clamp(3.5rem, 10vw, 8rem);
		font-weight: 900;
		letter-spacing: -0.02em;
		color: #00ff88;
		text-shadow:
			0 0 20px rgba(0, 255, 136, 0.5),
			0 0 40px rgba(0, 255, 136, 0.3),
			0 0 80px rgba(0, 255, 136, 0.15),
			0 0 120px rgba(0, 255, 136, 0.05);
		margin: 0 0 2rem 0;
		line-height: 1;
		animation: glow-pulse 4s ease-in-out infinite;
	}

	@keyframes glow-pulse {
		0%,
		100% {
			text-shadow:
				0 0 20px rgba(0, 255, 136, 0.5),
				0 0 40px rgba(0, 255, 136, 0.3),
				0 0 80px rgba(0, 255, 136, 0.15);
		}
		50% {
			text-shadow:
				0 0 30px rgba(0, 255, 136, 0.6),
				0 0 60px rgba(0, 255, 136, 0.4),
				0 0 100px rgba(0, 255, 136, 0.2),
				0 0 150px rgba(0, 255, 136, 0.08);
		}
	}

	.hero-subtitle {
		font-size: clamp(0.9rem, 1.5vw, 1.15rem);
		color: rgba(255, 255, 255, 0.6);
		line-height: 1.6;
		max-width: 640px;
		margin: 0 auto 3rem;
		min-height: 3.2em;
		text-align: left;
	}

	.prompt-symbol {
		color: #00ff88;
		margin-right: 0.75rem;
		font-weight: 700;
	}

	.typed-text {
		color: rgba(255, 255, 255, 0.7);
	}

	.cursor {
		color: #00ff88;
		font-weight: 700;
		transition: opacity 0.1s;
	}

	.cursor-visible {
		opacity: 1;
	}

	.cursor-hidden {
		opacity: 0;
	}

	.hero-actions {
		display: flex;
		gap: 1rem;
		justify-content: center;
		flex-wrap: wrap;
	}

	.btn-terminal {
		display: inline-flex;
		align-items: center;
		gap: 0.6rem;
		padding: 0.75rem 1.75rem;
		font-family: inherit;
		font-size: 0.85rem;
		font-weight: 600;
		letter-spacing: 0.05em;
		text-decoration: none;
		border-radius: 2px;
		transition: all 0.2s ease;
		text-transform: uppercase;
	}

	:global(.btn-icon) {
		width: 16px;
		height: 16px;
	}

	.btn-primary-t {
		background: #00ff88;
		color: #0a0a0f;
		border: 1px solid #00ff88;
		box-shadow:
			0 0 15px rgba(0, 255, 136, 0.25),
			inset 0 1px 0 rgba(255, 255, 255, 0.1);
	}

	.btn-primary-t:hover {
		background: #33ffaa;
		box-shadow:
			0 0 25px rgba(0, 255, 136, 0.4),
			inset 0 1px 0 rgba(255, 255, 255, 0.15);
		text-decoration: none;
		transform: translateY(-1px);
	}

	.btn-secondary-t {
		background: transparent;
		color: #7dd3fc;
		border: 1px solid rgba(125, 211, 252, 0.3);
	}

	.btn-secondary-t:hover {
		background: rgba(125, 211, 252, 0.08);
		border-color: rgba(125, 211, 252, 0.5);
		text-decoration: none;
		transform: translateY(-1px);
	}

	.hero-scroll-hint {
		position: absolute;
		bottom: 2rem;
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 0;
		z-index: 3;
		opacity: 0.4;
	}

	.scroll-arrow {
		color: #00ff88;
		font-size: 1rem;
		line-height: 1;
		animation: scroll-bounce 2s ease-in-out infinite;
	}

	.scroll-arrow-delayed {
		animation-delay: 0.15s;
	}

	.scroll-arrow-more-delayed {
		animation-delay: 0.3s;
	}

	@keyframes scroll-bounce {
		0%,
		100% {
			opacity: 0.2;
			transform: translateY(0);
		}
		50% {
			opacity: 0.8;
			transform: translateY(4px);
		}
	}

	/* ============================
	   SHARED SECTION STYLES
	   ============================ */
	.section {
		padding: 5rem 2rem;
		max-width: 900px;
		margin: 0 auto;
	}

	.section-label {
		font-size: 0.7rem;
		letter-spacing: 0.25em;
		color: rgba(255, 255, 255, 0.25);
		margin-bottom: 2rem;
		text-align: center;
	}

	.section-label-bracket {
		color: rgba(0, 255, 136, 0.3);
	}

	.section-label-text {
		margin: 0 0.3rem;
	}

	/* ============================
	   TERMINAL WINDOW
	   ============================ */
	.terminal-window,
	.architecture-box,
	.install-box {
		background: #0d0d14;
		border: 1px solid rgba(0, 255, 136, 0.1);
		border-radius: 6px;
		overflow: hidden;
		opacity: 0;
		transform: translateY(20px);
		transition:
			opacity 0.6s ease,
			transform 0.6s ease;
	}

	.terminal-window.visible,
	.architecture-box.visible,
	.install-box.visible {
		opacity: 1;
		transform: translateY(0);
	}

	.terminal-header {
		display: flex;
		align-items: center;
		gap: 6px;
		padding: 0.75rem 1rem;
		background: rgba(255, 255, 255, 0.03);
		border-bottom: 1px solid rgba(255, 255, 255, 0.06);
	}

	.terminal-dot {
		width: 10px;
		height: 10px;
		border-radius: 50%;
		display: inline-block;
	}

	.terminal-title {
		margin-left: 0.75rem;
		font-size: 0.75rem;
		color: rgba(255, 255, 255, 0.3);
		letter-spacing: 0.05em;
	}

	.terminal-body {
		padding: 1.5rem;
	}

	/* ============================
	   COMMAND PALETTE
	   ============================ */
	.command-line {
		display: flex;
		align-items: baseline;
		gap: 0.75rem;
		margin-bottom: 0.5rem;
		opacity: 0;
		transition:
			opacity 0.4s ease,
			transform 0.4s ease;
		transform: translateX(-10px);
	}

	.visible .command-line {
		opacity: 1;
		transform: translateX(0);
	}

	.prompt {
		color: #00ff88;
		font-weight: 700;
		flex-shrink: 0;
	}

	.command-text {
		color: #7dd3fc;
	}

	.feature-line {
		display: flex;
		align-items: baseline;
		gap: 0.75rem;
		padding: 0.4rem 0;
		padding-left: 1.5rem;
		opacity: 0;
		transform: translateX(-10px);
		transition:
			opacity 0.4s ease,
			transform 0.4s ease;
	}

	.feature-visible {
		opacity: 1;
		transform: translateX(0);
	}

	.feature-arrow {
		color: #ff6b35;
		font-weight: 700;
		flex-shrink: 0;
	}

	.feature-name {
		color: #00ff88;
		font-weight: 600;
		white-space: nowrap;
		min-width: 120px;
	}

	.feature-separator {
		color: rgba(255, 255, 255, 0.15);
	}

	.feature-desc {
		color: rgba(255, 255, 255, 0.5);
	}

	.cursor-block {
		color: #00ff88;
		animation: blink 1s step-end infinite;
	}

	@keyframes blink {
		0%,
		100% {
			opacity: 1;
		}
		50% {
			opacity: 0;
		}
	}

	/* ============================
	   ARCHITECTURE
	   ============================ */
	.architecture-body {
		padding: 2rem;
	}

	.arch-row {
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 0;
		flex-wrap: wrap;
		margin-bottom: 2rem;
	}

	.arch-node {
		display: flex;
		flex-direction: column;
		align-items: center;
		padding: 1.25rem 1.5rem;
		border: 1px solid rgba(0, 255, 136, 0.15);
		border-radius: 4px;
		background: rgba(0, 255, 136, 0.02);
		min-width: 120px;
		transition: all 0.3s ease;
	}

	.arch-node:hover {
		border-color: rgba(0, 255, 136, 0.35);
		background: rgba(0, 255, 136, 0.05);
		box-shadow: 0 0 20px rgba(0, 255, 136, 0.08);
	}

	.arch-node-icon {
		margin-bottom: 0.5rem;
	}

	:global(.arch-icon) {
		width: 24px;
		height: 24px;
		color: #00ff88;
	}

	.arch-anki :global(.arch-icon) {
		color: #7dd3fc;
	}

	.arch-ai :global(.arch-icon) {
		color: #ff6b35;
	}

	.arch-node-label {
		font-size: 0.9rem;
		font-weight: 700;
		color: #e0e0e0;
		margin-bottom: 0.2rem;
	}

	.arch-node-sub {
		font-size: 0.65rem;
		color: rgba(255, 255, 255, 0.3);
		letter-spacing: 0.1em;
		text-transform: uppercase;
	}

	.arch-connection {
		display: flex;
		flex-direction: column;
		align-items: center;
		padding: 0 0.5rem;
		min-width: 80px;
	}

	.arch-line {
		width: 100%;
		height: 2px;
		background: repeating-linear-gradient(
			90deg,
			#00ff88 0px,
			#00ff88 6px,
			transparent 6px,
			transparent 12px
		);
		animation: dash-flow 1s linear infinite;
		position: relative;
	}

	@keyframes dash-flow {
		from {
			background-position: 0 0;
		}
		to {
			background-position: 24px 0;
		}
	}

	.arch-line-label {
		font-size: 0.6rem;
		color: rgba(255, 255, 255, 0.25);
		margin-top: 0.4rem;
		letter-spacing: 0.05em;
		white-space: nowrap;
	}

	.arch-ascii {
		margin-top: 1rem;
		text-align: center;
	}

	.arch-ascii pre {
		display: inline-block;
		text-align: left;
		font-size: 0.75rem;
		color: rgba(0, 255, 136, 0.35);
		background: transparent;
		border: none;
		padding: 1rem;
		margin: 0;
		line-height: 1.4;
		box-shadow: none;
	}

	/* ============================
	   INSTALL SECTION
	   ============================ */
	.install-body {
		padding: 1.5rem;
	}

	.install-step {
		display: flex;
		align-items: center;
		gap: 0.75rem;
		margin-bottom: 0.75rem;
		margin-top: 1.5rem;
	}

	.install-step:first-child {
		margin-top: 0;
	}

	.install-step-num {
		font-size: 0.65rem;
		font-weight: 700;
		color: #0a0a0f;
		background: #00ff88;
		padding: 0.15rem 0.5rem;
		border-radius: 2px;
		letter-spacing: 0.05em;
	}

	.install-step-label {
		font-size: 0.85rem;
		color: rgba(255, 255, 255, 0.7);
		font-weight: 600;
	}

	.install-command-group {
		padding-left: 1rem;
		border-left: 2px solid rgba(0, 255, 136, 0.1);
		margin-left: 0.75rem;
	}

	.install-command-group .command-line {
		opacity: 1;
		transform: none;
	}

	.flag {
		color: rgba(255, 255, 255, 0.25);
	}

	.highlight-code {
		color: #ff6b35;
		font-weight: 700;
		padding: 0.1rem 0.4rem;
		background: rgba(255, 107, 53, 0.08);
		border: 1px solid rgba(255, 107, 53, 0.15);
		border-radius: 2px;
	}

	.install-code-block {
		margin: 0.75rem 0;
		margin-left: 0.75rem;
		border: 1px solid rgba(0, 255, 136, 0.1);
		border-radius: 4px;
		overflow: hidden;
	}

	.install-code-header {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 0.5rem 1rem;
		background: rgba(255, 255, 255, 0.03);
		border-bottom: 1px solid rgba(255, 255, 255, 0.06);
	}

	.install-code-filename {
		font-size: 0.7rem;
		color: rgba(255, 255, 255, 0.35);
		letter-spacing: 0.05em;
	}

	.copy-btn {
		display: inline-flex;
		align-items: center;
		gap: 0.4rem;
		padding: 0.3rem 0.75rem;
		font-family: inherit;
		font-size: 0.7rem;
		color: #00ff88;
		background: rgba(0, 255, 136, 0.06);
		border: 1px solid rgba(0, 255, 136, 0.15);
		border-radius: 2px;
		cursor: pointer;
		transition: all 0.2s ease;
		letter-spacing: 0.05em;
	}

	.copy-btn:hover {
		background: rgba(0, 255, 136, 0.12);
		border-color: rgba(0, 255, 136, 0.3);
	}

	:global(.copy-icon) {
		width: 12px;
		height: 12px;
	}

	.install-pre {
		margin: 0;
		padding: 1rem;
		font-size: 0.8rem;
		color: #7dd3fc;
		background: transparent !important;
		border: none !important;
		border-radius: 0 !important;
		box-shadow: none !important;
	}

	.install-pre code {
		font-family: inherit;
		font-size: inherit;
		background: none;
		padding: 0;
	}

	.install-output {
		margin-top: 0.5rem;
		padding-left: 1.5rem;
		display: flex;
		flex-direction: column;
		gap: 0.25rem;
	}

	.output-success {
		color: #00ff88;
		font-size: 0.8rem;
	}

	.output-success::before {
		content: '> ';
		color: rgba(0, 255, 136, 0.4);
	}

	.output-info {
		color: rgba(255, 255, 255, 0.45);
		font-size: 0.8rem;
	}

	.output-info::before {
		content: '  ';
	}

	.string-literal {
		color: #fbbf24;
	}

	/* ============================
	   STATUS BAR
	   ============================ */
	.status-bar {
		background: #080810;
		border-top: 1px solid rgba(0, 255, 136, 0.1);
		padding: 0.6rem 2rem;
		font-size: 0.7rem;
		letter-spacing: 0.08em;
	}

	.status-bar-inner {
		display: flex;
		align-items: center;
		gap: 1rem;
		max-width: 900px;
		margin: 0 auto;
	}

	.status-group {
		display: flex;
		align-items: center;
		gap: 0.4rem;
	}

	.status-dot {
		width: 6px;
		height: 6px;
		border-radius: 50%;
		display: inline-block;
	}

	.status-dot-green {
		background: #00ff88;
		box-shadow: 0 0 6px rgba(0, 255, 136, 0.5);
		animation: status-pulse 2s ease-in-out infinite;
	}

	@keyframes status-pulse {
		0%,
		100% {
			box-shadow: 0 0 4px rgba(0, 255, 136, 0.4);
		}
		50% {
			box-shadow: 0 0 10px rgba(0, 255, 136, 0.7);
		}
	}

	.status-label {
		color: rgba(255, 255, 255, 0.45);
		text-transform: uppercase;
	}

	:global(.status-icon) {
		width: 12px;
		height: 12px;
		color: rgba(0, 255, 136, 0.5);
	}

	.status-divider {
		width: 1px;
		height: 12px;
		background: rgba(255, 255, 255, 0.08);
	}

	.status-spacer {
		flex: 1;
	}

	.status-version {
		color: rgba(0, 255, 136, 0.4);
		font-weight: 600;
	}

	/* ============================
	   RESPONSIVE
	   ============================ */
	@media (max-width: 768px) {
		.terminal-page {
			margin: -1rem;
		}

		.hero {
			padding: 1.5rem;
			min-height: 90vh;
		}

		.hero-title {
			font-size: clamp(2.5rem, 12vw, 5rem);
		}

		.section {
			padding: 3rem 1rem;
		}

		.arch-row {
			flex-direction: column;
			gap: 0;
		}

		.arch-connection {
			transform: rotate(90deg);
			padding: 0.75rem 0;
			min-width: 60px;
		}

		.arch-ascii {
			display: none;
		}

		.feature-line {
			flex-wrap: wrap;
			padding-left: 0.75rem;
		}

		.feature-name {
			min-width: auto;
		}

		.feature-separator {
			display: none;
		}

		.feature-desc {
			flex-basis: 100%;
			padding-left: 1.5rem;
			font-size: 0.85rem;
		}

		.status-bar-inner {
			flex-wrap: wrap;
			gap: 0.5rem;
		}

		.status-divider:nth-child(6),
		.status-group:nth-child(7) {
			display: none;
		}

		.install-code-block {
			margin-left: 0;
		}

		.install-command-group {
			margin-left: 0;
		}
	}

	@media (max-width: 480px) {
		.hero-actions {
			flex-direction: column;
			align-items: stretch;
		}

		.btn-terminal {
			justify-content: center;
		}

		.hero-badge {
			font-size: 0.6rem;
		}

		.arch-node {
			min-width: 100px;
			padding: 1rem;
		}
	}
</style>
