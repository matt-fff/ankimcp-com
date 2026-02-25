<script lang="ts">
	import { onMount } from 'svelte';
	import {
		Layers,
		Shield,
		Zap,
		Download,
		ArrowRight,
		Settings,
		Play,
		BookOpen,
		Radio
	} from 'lucide-svelte';

	let mounted = $state(false);
	let cells = $state<HTMLElement[]>([]);

	const codeHtml = `<span class="json-brace">{</span>
  <span class="json-key">"mcpServers"</span>: <span class="json-brace">{</span>
    <span class="json-key">"ankimcp"</span>: <span class="json-brace">{</span>
      <span class="json-key">"type"</span>: <span class="json-str">"sse"</span>,
      <span class="json-key">"url"</span>: <span class="json-str">"http://localhost:4473/sse"</span>
    <span class="json-brace">}</span>
  <span class="json-brace">}</span>
<span class="json-brace">}</span>`;

	onMount(() => {
		mounted = true;

		const observer = new IntersectionObserver(
			(entries) => {
				for (const entry of entries) {
					if (entry.isIntersecting) {
						(entry.target as HTMLElement).classList.add('cell-visible');
						observer.unobserve(entry.target);
					}
				}
			},
			{ threshold: 0.1, rootMargin: '0px 0px -40px 0px' }
		);

		const allCells = document.querySelectorAll('.bento-cell');
		allCells.forEach((cell, i) => {
			(cell as HTMLElement).style.transitionDelay = `${i * 70}ms`;
			observer.observe(cell);
		});

		return () => observer.disconnect();
	});
</script>

<svelte:head>
	<title>AnkiMCP - Bridge Anki with AI Assistants</title>
	<meta
		name="description"
		content="Connect your Anki flashcard collection to Claude and other AI assistants through the Model Context Protocol."
	/>
</svelte:head>

<div class="bento-page">
	<div class="bento-grid">
		<!-- Hero Cell -->
		<div class="bento-cell cell-hero">
			<div class="hero-inner">
				<h1 class="hero-title">AnkiMCP</h1>
				<p class="hero-tagline">Your flashcards, understood by AI.</p>
			</div>
		</div>

		<!-- Feature Cell 1: MCP Protocol -->
		<div class="bento-cell cell-feature cell-feature-1">
			<div class="feature-icon icon-lavender">
				<Layers size={28} strokeWidth={1.5} />
			</div>
			<h3 class="feature-name">MCP Protocol</h3>
			<p class="feature-desc">
				Standardized integration with Claude Desktop, Claude Code, and any MCP-compatible host.
			</p>
		</div>

		<!-- Feature Cell 2: Permission Control -->
		<div class="bento-cell cell-feature cell-feature-2">
			<div class="feature-icon icon-mint">
				<Shield size={28} strokeWidth={1.5} />
			</div>
			<h3 class="feature-name">Permission Control</h3>
			<p class="feature-desc">
				Fine-grained access control over what AI can read, write, or modify in your collection.
			</p>
		</div>

		<!-- Feature Cell 3: Real-time Sync -->
		<div class="bento-cell cell-feature cell-feature-3">
			<div class="feature-icon icon-peach">
				<Zap size={28} strokeWidth={1.5} />
			</div>
			<h3 class="feature-name">Instant Access</h3>
			<p class="feature-desc">
				SSE-powered real-time connection. Decks, notes, cards, and stats available the moment Anki
				opens.
			</p>
		</div>

		<!-- Code Preview Cell -->
		<div class="bento-cell cell-code">
			<div class="code-header">
				<span class="code-dot red"></span>
				<span class="code-dot yellow"></span>
				<span class="code-dot green"></span>
				<span class="code-filename">mcp.json</span>
			</div>
			<pre class="code-block"><code>{@html codeHtml}</code></pre>
		</div>

		<!-- Stats Cell -->
		<div class="bento-cell cell-stats">
			<div class="stat">
				<span class="stat-number">9</span>
				<span class="stat-label">API Methods</span>
			</div>
			<div class="stat-divider"></div>
			<div class="stat">
				<span class="stat-number">3</span>
				<span class="stat-label">Step Setup</span>
			</div>
			<div class="stat-divider"></div>
			<div class="stat">
				<span class="stat-icon"><Radio size={22} strokeWidth={1.5} /></span>
				<span class="stat-label-sse">SSE Real-time</span>
			</div>
		</div>

		<!-- Architecture Cell -->
		<div class="bento-cell cell-arch">
			<h3 class="arch-title">How It Works</h3>
			<div class="arch-flow">
				<div class="arch-node">
					<div class="arch-node-icon">
						<BookOpen size={20} strokeWidth={1.5} />
					</div>
					<span class="arch-node-label">Anki</span>
				</div>
				<div class="arch-connector">
					<div class="arch-line"></div>
					<ArrowRight size={16} strokeWidth={2} class="arch-arrow" />
				</div>
				<div class="arch-node arch-node-center">
					<div class="arch-node-icon">
						<Layers size={20} strokeWidth={1.5} />
					</div>
					<span class="arch-node-label">AnkiMCP</span>
				</div>
				<div class="arch-connector">
					<div class="arch-line"></div>
					<ArrowRight size={16} strokeWidth={2} class="arch-arrow" />
				</div>
				<div class="arch-node">
					<div class="arch-node-icon">
						<Zap size={20} strokeWidth={1.5} />
					</div>
					<span class="arch-node-label">Claude</span>
				</div>
			</div>
			<p class="arch-caption">
				AnkiMCP runs as an Anki addon, exposing your collection via SSE to any MCP-compatible AI
				assistant.
			</p>
		</div>

		<!-- Quick Start Cell -->
		<div class="bento-cell cell-quickstart">
			<h3 class="qs-title">Quick Start</h3>
			<div class="qs-steps">
				<div class="qs-step">
					<div class="qs-pill">
						<span class="qs-num">1</span>
						<Download size={16} strokeWidth={1.5} />
						<span class="qs-text">Install addon in Anki</span>
					</div>
				</div>
				<div class="qs-line"></div>
				<div class="qs-step">
					<div class="qs-pill">
						<span class="qs-num">2</span>
						<Settings size={16} strokeWidth={1.5} />
						<span class="qs-text">Add MCP config</span>
					</div>
				</div>
				<div class="qs-line"></div>
				<div class="qs-step">
					<div class="qs-pill">
						<span class="qs-num">3</span>
						<Play size={16} strokeWidth={1.5} />
						<span class="qs-text">Ask Claude anything</span>
					</div>
				</div>
			</div>
		</div>

		<!-- CTA Cell -->
		<div class="bento-cell cell-cta">
			<div class="cta-inner">
				<a href="https://github.com/shivros/ankimcp" class="cta-btn cta-primary">
					Get Started
					<ArrowRight size={18} strokeWidth={2} />
				</a>
				<a href="/docs" class="cta-btn cta-secondary">
					View Docs
				</a>
			</div>
		</div>
	</div>
</div>

<style>
	/* ========================================
	   Page & Grid Foundation
	   ======================================== */
	.bento-page {
		max-width: 1120px;
		margin: 0 auto;
		padding: 2rem 1rem 4rem;
	}

	.bento-grid {
		display: grid;
		grid-template-columns: repeat(3, 1fr);
		gap: 1rem;
	}

	/* ========================================
	   Base Cell Styling
	   ======================================== */
	.bento-cell {
		background: #ffffff;
		border-radius: 1.25rem;
		border: 1px solid rgba(0, 0, 0, 0.06);
		padding: 2rem;
		transition:
			transform 0.35s cubic-bezier(0.22, 1, 0.36, 1),
			box-shadow 0.35s cubic-bezier(0.22, 1, 0.36, 1),
			opacity 0.6s cubic-bezier(0.22, 1, 0.36, 1);
		opacity: 0;
		transform: translateY(24px);
	}

	:global(.bento-cell.cell-visible) {
		opacity: 1;
		transform: translateY(0);
	}

	.bento-cell:hover {
		transform: translateY(-3px) scale(1.005);
		box-shadow:
			0 12px 40px rgba(0, 0, 0, 0.06),
			0 2px 8px rgba(0, 0, 0, 0.04);
	}

	:global(.bento-cell.cell-visible:hover) {
		transform: translateY(-3px) scale(1.005);
	}

	/* ========================================
	   Hero Cell
	   ======================================== */
	.cell-hero {
		grid-column: 1 / -1;
		padding: 4.5rem 3rem;
		background: linear-gradient(135deg, #fef9f3 0%, #fdf2f8 40%, #ede9fe 100%);
		text-align: center;
		position: relative;
		overflow: hidden;
	}

	.cell-hero::before {
		content: '';
		position: absolute;
		inset: 0;
		background: radial-gradient(
			ellipse 60% 50% at 50% 40%,
			rgba(99, 102, 241, 0.04) 0%,
			transparent 70%
		);
		pointer-events: none;
	}

	.hero-inner {
		position: relative;
		z-index: 1;
	}

	.hero-title {
		font-size: clamp(4rem, 6vw + 1rem, 8rem);
		font-weight: 200;
		letter-spacing: -0.05em;
		line-height: 1;
		margin: 0 0 1rem;
		background: linear-gradient(135deg, #1a1a2e 30%, #6366f1 70%, #06b6d4 100%);
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		background-clip: text;
	}

	.hero-tagline {
		font-size: clamp(1.1rem, 1.5vw, 1.5rem);
		color: #64748b;
		margin: 0;
		font-weight: 400;
		letter-spacing: -0.01em;
	}

	/* ========================================
	   Feature Cells
	   ======================================== */
	.cell-feature {
		display: flex;
		flex-direction: column;
		gap: 0.75rem;
	}

	.cell-feature-1 {
		background: linear-gradient(160deg, #ffffff 60%, #ede9fe 100%);
		border-color: rgba(99, 102, 241, 0.1);
	}

	.cell-feature-2 {
		background: linear-gradient(160deg, #ffffff 60%, #d1fae5 100%);
		border-color: rgba(6, 182, 212, 0.1);
		min-height: 220px;
	}

	.cell-feature-3 {
		background: linear-gradient(160deg, #ffffff 60%, #ffedd5 100%);
		border-color: rgba(249, 115, 22, 0.1);
	}

	.feature-icon {
		width: 56px;
		height: 56px;
		border-radius: 16px;
		display: flex;
		align-items: center;
		justify-content: center;
		flex-shrink: 0;
	}

	.icon-lavender {
		background: #ede9fe;
		color: #6366f1;
	}

	.icon-mint {
		background: #d1fae5;
		color: #059669;
	}

	.icon-peach {
		background: #ffedd5;
		color: #ea580c;
	}

	.feature-name {
		font-size: 1.125rem;
		font-weight: 650;
		color: #1a1a2e;
		margin: 0;
		letter-spacing: -0.01em;
	}

	.feature-desc {
		font-size: 0.925rem;
		line-height: 1.55;
		color: #64748b;
		margin: 0;
	}

	/* ========================================
	   Code Preview Cell
	   ======================================== */
	.cell-code {
		grid-column: span 2;
		padding: 0;
		overflow: hidden;
		background: #0f0f14;
		border-color: rgba(99, 102, 241, 0.15);
		position: relative;
	}

	.cell-code::after {
		content: '';
		position: absolute;
		inset: 0;
		border-radius: 1.25rem;
		border: 1px solid transparent;
		pointer-events: none;
		animation: code-glow 4s ease-in-out infinite;
	}

	@keyframes code-glow {
		0%,
		100% {
			box-shadow: inset 0 0 20px rgba(99, 102, 241, 0.04);
		}
		50% {
			box-shadow: inset 0 0 30px rgba(99, 102, 241, 0.1);
		}
	}

	.code-header {
		display: flex;
		align-items: center;
		gap: 6px;
		padding: 0.875rem 1.5rem;
		background: #1a1a24;
		border-bottom: 1px solid rgba(255, 255, 255, 0.06);
	}

	.code-dot {
		width: 10px;
		height: 10px;
		border-radius: 50%;
	}

	.code-dot.red {
		background: #ff5f57;
	}
	.code-dot.yellow {
		background: #febc2e;
	}
	.code-dot.green {
		background: #28c840;
	}

	.code-filename {
		margin-left: 0.75rem;
		font-size: 0.8rem;
		color: rgba(255, 255, 255, 0.4);
		font-family: var(--font-mono);
	}

	.code-block {
		margin: 0;
		padding: 1.5rem;
		background: transparent;
		border: none;
		border-radius: 0;
		font-size: 0.875rem;
		line-height: 1.7;
		color: #e2e8f0;
		font-family: var(--font-mono);
		overflow-x: auto;
		box-shadow: none;
	}

	.code-block code {
		font-family: var(--font-mono);
	}

	:global(.json-key) {
		color: #7dd3fc;
	}

	:global(.json-str) {
		color: #86efac;
	}

	:global(.json-brace) {
		color: #94a3b8;
	}

	/* ========================================
	   Stats Cell
	   ======================================== */
	.cell-stats {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		gap: 0;
		text-align: center;
		background: linear-gradient(160deg, #ffffff 40%, #f0f9ff 100%);
	}

	.stat {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 0.25rem;
		padding: 0.75rem 0;
	}

	.stat-number {
		font-size: 2.5rem;
		font-weight: 700;
		letter-spacing: -0.04em;
		color: #1a1a2e;
		line-height: 1;
	}

	.stat-label {
		font-size: 0.8rem;
		text-transform: uppercase;
		letter-spacing: 0.08em;
		color: #94a3b8;
		font-weight: 500;
	}

	.stat-divider {
		width: 32px;
		height: 1px;
		background: rgba(0, 0, 0, 0.08);
	}

	.stat-icon {
		color: #6366f1;
		display: flex;
		align-items: center;
		justify-content: center;
		margin-bottom: 0.125rem;
	}

	.stat-label-sse {
		font-size: 0.75rem;
		text-transform: uppercase;
		letter-spacing: 0.08em;
		color: #94a3b8;
		font-weight: 500;
	}

	/* ========================================
	   Architecture Cell
	   ======================================== */
	.cell-arch {
		grid-column: span 2;
		text-align: center;
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 1.5rem;
		padding: 2.5rem 2rem;
		background: linear-gradient(160deg, #ffffff 50%, #faf5ff 100%);
	}

	.arch-title {
		font-size: 1rem;
		font-weight: 600;
		color: #94a3b8;
		text-transform: uppercase;
		letter-spacing: 0.1em;
		margin: 0;
	}

	.arch-flow {
		display: flex;
		align-items: center;
		gap: 0;
		justify-content: center;
		flex-wrap: nowrap;
	}

	.arch-node {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 0.625rem;
	}

	.arch-node-icon {
		width: 56px;
		height: 56px;
		border-radius: 16px;
		background: #f8fafc;
		border: 2px solid #e2e8f0;
		display: flex;
		align-items: center;
		justify-content: center;
		color: #475569;
		transition: all 0.3s;
	}

	.arch-node-center .arch-node-icon {
		background: linear-gradient(135deg, #6366f1, #818cf8);
		border-color: #6366f1;
		color: #ffffff;
		box-shadow: 0 4px 20px rgba(99, 102, 241, 0.25);
	}

	.arch-node-label {
		font-size: 0.825rem;
		font-weight: 600;
		color: #475569;
		letter-spacing: -0.01em;
	}

	.arch-connector {
		display: flex;
		align-items: center;
		padding: 0 0.5rem;
		margin-bottom: 1.5rem;
		color: #94a3b8;
		position: relative;
	}

	.arch-line {
		width: 48px;
		height: 0;
		border-top: 2px dashed #cbd5e1;
		animation: dash-flow 1.5s linear infinite;
	}

	@keyframes dash-flow {
		0% {
			stroke-dashoffset: 0;
		}
		100% {
			stroke-dashoffset: -20;
		}
	}

	.arch-caption {
		font-size: 0.85rem;
		color: #94a3b8;
		max-width: 480px;
		line-height: 1.5;
		margin: 0;
	}

	/* ========================================
	   Quick Start Cell
	   ======================================== */
	.cell-quickstart {
		grid-column: 1 / -1;
		text-align: center;
		padding: 2.5rem 2rem;
	}

	.qs-title {
		font-size: 1rem;
		font-weight: 600;
		color: #94a3b8;
		text-transform: uppercase;
		letter-spacing: 0.1em;
		margin: 0 0 2rem;
	}

	.qs-steps {
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 0;
		flex-wrap: nowrap;
	}

	.qs-step {
		flex-shrink: 0;
	}

	.qs-pill {
		display: inline-flex;
		align-items: center;
		gap: 0.625rem;
		padding: 0.75rem 1.5rem;
		background: #f8fafc;
		border: 1px solid #e2e8f0;
		border-radius: 999px;
		transition: all 0.3s;
	}

	.qs-pill:hover {
		background: #f0f9ff;
		border-color: #bae6fd;
		box-shadow: 0 2px 12px rgba(6, 182, 212, 0.08);
	}

	.qs-num {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 24px;
		height: 24px;
		border-radius: 50%;
		background: #6366f1;
		color: white;
		font-size: 0.75rem;
		font-weight: 700;
		flex-shrink: 0;
	}

	.qs-text {
		font-size: 0.875rem;
		font-weight: 500;
		color: #334155;
		white-space: nowrap;
	}

	.qs-line {
		width: 48px;
		height: 0;
		border-top: 1px solid #cbd5e1;
		flex-shrink: 0;
	}

	:global(.qs-pill svg) {
		color: #6366f1;
		flex-shrink: 0;
	}

	/* ========================================
	   CTA Cell
	   ======================================== */
	.cell-cta {
		grid-column: 1 / -1;
		padding: 2rem 2.5rem;
		background: linear-gradient(-45deg, #6366f1, #818cf8, #06b6d4, #6366f1);
		background-size: 300% 300%;
		animation: cta-gradient 8s ease infinite;
		border: none;
	}

	@keyframes cta-gradient {
		0% {
			background-position: 0% 50%;
		}
		50% {
			background-position: 100% 50%;
		}
		100% {
			background-position: 0% 50%;
		}
	}

	.cta-inner {
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 1rem;
	}

	.cta-btn {
		display: inline-flex;
		align-items: center;
		gap: 0.5rem;
		padding: 0.875rem 2rem;
		border-radius: 0.75rem;
		font-weight: 600;
		font-size: 0.95rem;
		text-decoration: none;
		transition: all 0.25s cubic-bezier(0.22, 1, 0.36, 1);
	}

	.cta-primary {
		background: white;
		color: #4f46e5;
	}

	.cta-primary:hover {
		background: #f8fafc;
		transform: translateY(-2px);
		box-shadow: 0 8px 24px rgba(0, 0, 0, 0.15);
		text-decoration: none;
	}

	.cta-secondary {
		background: rgba(255, 255, 255, 0.15);
		color: white;
		border: 1px solid rgba(255, 255, 255, 0.3);
		backdrop-filter: blur(4px);
	}

	.cta-secondary:hover {
		background: rgba(255, 255, 255, 0.25);
		transform: translateY(-2px);
		text-decoration: none;
	}

	/* ========================================
	   Dark Mode
	   ======================================== */
	:global([data-theme='dark']) .bento-cell {
		background: #1a1a24;
		border-color: rgba(255, 255, 255, 0.06);
	}

	:global([data-theme='dark']) .bento-cell:hover {
		box-shadow:
			0 12px 40px rgba(0, 0, 0, 0.3),
			0 2px 8px rgba(0, 0, 0, 0.2);
	}

	:global([data-theme='dark']) .cell-hero {
		background: linear-gradient(135deg, #1a1a24 0%, #1e1b2e 40%, #151525 100%);
	}

	:global([data-theme='dark']) .cell-hero::before {
		background: radial-gradient(
			ellipse 60% 50% at 50% 40%,
			rgba(99, 102, 241, 0.08) 0%,
			transparent 70%
		);
	}

	:global([data-theme='dark']) .hero-title {
		background: linear-gradient(135deg, #e2e8f0 30%, #818cf8 70%, #22d3ee 100%);
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		background-clip: text;
	}

	:global([data-theme='dark']) .hero-tagline {
		color: #94a3b8;
	}

	:global([data-theme='dark']) .cell-feature-1 {
		background: linear-gradient(160deg, #1a1a24 60%, #2e1065 100%);
		border-color: rgba(99, 102, 241, 0.15);
	}

	:global([data-theme='dark']) .cell-feature-2 {
		background: linear-gradient(160deg, #1a1a24 60%, #064e3b 100%);
		border-color: rgba(6, 182, 212, 0.15);
	}

	:global([data-theme='dark']) .cell-feature-3 {
		background: linear-gradient(160deg, #1a1a24 60%, #431407 100%);
		border-color: rgba(249, 115, 22, 0.15);
	}

	:global([data-theme='dark']) .icon-lavender {
		background: rgba(99, 102, 241, 0.15);
		color: #a5b4fc;
	}

	:global([data-theme='dark']) .icon-mint {
		background: rgba(16, 185, 129, 0.15);
		color: #6ee7b7;
	}

	:global([data-theme='dark']) .icon-peach {
		background: rgba(249, 115, 22, 0.15);
		color: #fdba74;
	}

	:global([data-theme='dark']) .feature-name {
		color: #e2e8f0;
	}

	:global([data-theme='dark']) .feature-desc {
		color: #94a3b8;
	}

	:global([data-theme='dark']) .cell-code {
		background: #0a0a10;
		border-color: rgba(99, 102, 241, 0.2);
	}

	:global([data-theme='dark']) .code-header {
		background: #111118;
	}

	:global([data-theme='dark']) .cell-stats {
		background: linear-gradient(160deg, #1a1a24 40%, #0f172a 100%);
	}

	:global([data-theme='dark']) .stat-number {
		color: #e2e8f0;
	}

	:global([data-theme='dark']) .stat-label,
	:global([data-theme='dark']) .stat-label-sse {
		color: #64748b;
	}

	:global([data-theme='dark']) .stat-divider {
		background: rgba(255, 255, 255, 0.06);
	}

	:global([data-theme='dark']) .cell-arch {
		background: linear-gradient(160deg, #1a1a24 50%, #1e1b2e 100%);
	}

	:global([data-theme='dark']) .arch-title,
	:global([data-theme='dark']) .qs-title {
		color: #64748b;
	}

	:global([data-theme='dark']) .arch-node-icon {
		background: #1e1e2e;
		border-color: #334155;
		color: #94a3b8;
	}

	:global([data-theme='dark']) .arch-node-center .arch-node-icon {
		background: linear-gradient(135deg, #6366f1, #818cf8);
		border-color: #6366f1;
		color: #ffffff;
	}

	:global([data-theme='dark']) .arch-node-label {
		color: #94a3b8;
	}

	:global([data-theme='dark']) .arch-line {
		border-top-color: #334155;
	}

	:global([data-theme='dark']) .arch-connector {
		color: #475569;
	}

	:global([data-theme='dark']) .arch-caption {
		color: #64748b;
	}

	:global([data-theme='dark']) .cell-quickstart {
		background: #1a1a24;
	}

	:global([data-theme='dark']) .qs-pill {
		background: #1e1e2e;
		border-color: #334155;
	}

	:global([data-theme='dark']) .qs-pill:hover {
		background: #252540;
		border-color: #475569;
	}

	:global([data-theme='dark']) .qs-text {
		color: #cbd5e1;
	}

	:global([data-theme='dark']) .qs-line {
		border-top-color: #334155;
	}

	:global([data-theme='dark']) .cell-cta {
		background: linear-gradient(-45deg, #4338ca, #6366f1, #0e7490, #4338ca);
		background-size: 300% 300%;
	}

	/* ========================================
	   Responsive: Tablet (2 columns)
	   ======================================== */
	@media (max-width: 900px) {
		.bento-grid {
			grid-template-columns: repeat(2, 1fr);
		}

		.cell-hero {
			padding: 3.5rem 2rem;
		}

		.cell-code {
			grid-column: span 2;
		}

		.cell-stats {
			grid-column: span 2;
			flex-direction: row;
			gap: 1.5rem;
			padding: 1.5rem;
		}

		.stat-divider {
			width: 1px;
			height: 40px;
		}

		.cell-arch {
			grid-column: span 2;
		}

		.qs-steps {
			flex-wrap: wrap;
			gap: 0.5rem;
		}

		.qs-line {
			display: none;
		}
	}

	/* ========================================
	   Responsive: Mobile (1 column)
	   ======================================== */
	@media (max-width: 600px) {
		.bento-page {
			padding: 1rem 0.75rem 3rem;
		}

		.bento-grid {
			grid-template-columns: 1fr;
			gap: 0.75rem;
		}

		.cell-hero {
			padding: 3rem 1.5rem;
			grid-column: 1;
		}

		.hero-title {
			font-size: clamp(3rem, 12vw, 5rem);
		}

		.cell-code {
			grid-column: 1;
		}

		.cell-stats {
			grid-column: 1;
			flex-direction: column;
			gap: 0;
		}

		.stat-divider {
			width: 32px;
			height: 1px;
		}

		.cell-arch {
			grid-column: 1;
		}

		.arch-flow {
			flex-direction: column;
			gap: 0.25rem;
		}

		.arch-connector {
			transform: rotate(90deg);
			padding: 0.25rem 0;
			margin-bottom: 0;
		}

		.cell-quickstart {
			grid-column: 1;
		}

		.qs-steps {
			flex-direction: column;
			align-items: stretch;
			gap: 0;
		}

		.qs-pill {
			width: 100%;
			justify-content: center;
		}

		.qs-line {
			width: 0;
			height: 16px;
			border-top: none;
			border-left: 1px solid #cbd5e1;
			align-self: center;
		}

		.cell-cta {
			grid-column: 1;
		}

		.cta-inner {
			flex-direction: column;
		}

		.cta-btn {
			width: 100%;
			justify-content: center;
		}

		.cell-feature {
			padding: 1.5rem;
		}

		.cell-feature-2 {
			min-height: auto;
		}

		.bento-cell {
			padding: 1.5rem;
		}
	}
</style>
