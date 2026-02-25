<script lang="ts">
	import { BookOpen, Sparkles, Layers, FileText, Shield, BarChart3, Github, FileCode } from 'lucide-svelte';

	let heroVisible = $state(false);
	let heroSubVisible = $state(false);
	let problemWords = $state<boolean[]>([]);
	let bridgeVisible = $state(false);
	let capabilities = $state<boolean[]>([false, false, false, false]);
	let codeVisible = $state(false);
	let ctaVisible = $state(false);

	const problemText = [
		'Managing thousands of flashcards',
		'is powerful.',
		'But it\'s manual.',
		'Repetitive.',
		'Isolated from your AI tools.'
	];

	const capabilityItems = [
		{
			num: '01',
			title: 'Deck Management',
			desc: 'Create, update, and organize your decks with AI assistance.',
			icon: Layers
		},
		{
			num: '02',
			title: 'Note Operations',
			desc: 'Search, create, and modify flashcards through natural conversation.',
			icon: FileText
		},
		{
			num: '03',
			title: 'Permission Control',
			desc: 'Fine-grained access rules protect your collection.',
			icon: Shield
		},
		{
			num: '04',
			title: 'Real-time Statistics',
			desc: 'Review progress and learning analytics on demand.',
			icon: BarChart3
		}
	];

	const mcpConfig = `{
  "mcpServers": {
    "ankimcp": {
      "type": "sse",
      "url": "http://localhost:4473/sse"
    }
  }
}`;

	function observe(node: HTMLElement, callback: (visible: boolean) => void) {
		const observer = new IntersectionObserver(
			(entries) => {
				entries.forEach((entry) => {
					if (entry.isIntersecting) {
						callback(true);
					}
				});
			},
			{ threshold: 0.2 }
		);
		observer.observe(node);
		return {
			destroy() {
				observer.disconnect();
			}
		};
	}

	function heroAction(node: HTMLElement) {
		return observe(node, () => {
			heroVisible = true;
			setTimeout(() => {
				heroSubVisible = true;
			}, 1200);
		});
	}

	function problemAction(node: HTMLElement) {
		return observe(node, () => {
			problemWords = [];
			problemText.forEach((_, i) => {
				setTimeout(() => {
					problemWords = [...problemWords, true];
				}, i * 400);
			});
		});
	}

	function bridgeAction(node: HTMLElement) {
		return observe(node, () => {
			bridgeVisible = true;
		});
	}

	function capabilityAction(node: HTMLElement) {
		const idx = parseInt(node.dataset.idx || '0');
		return observe(node, () => {
			setTimeout(() => {
				capabilities = capabilities.map((c, i) => (i === idx ? true : c));
			}, idx * 200);
		});
	}

	function codeAction(node: HTMLElement) {
		return observe(node, () => {
			codeVisible = true;
		});
	}

	function ctaAction(node: HTMLElement) {
		return observe(node, () => {
			ctaVisible = true;
		});
	}
</script>

<svelte:head>
	<title>AnkiMCP - Your flashcards just got an AI brain</title>
	<meta
		name="description"
		content="AnkiMCP bridges your Anki flashcard collection with AI assistants through the Model Context Protocol."
	/>
</svelte:head>

<!-- SECTION 1: Opening Frame -->
<section class="section-hero" use:heroAction>
	<div class="hero-grid">
		{#each Array(200) as _, i}
			<div
				class="hero-dot"
				class:hero-dot-visible={heroVisible}
				style="animation-delay: {Math.random() * 3}s; opacity: {Math.random() * 0.3 + 0.05}"
			></div>
		{/each}
	</div>

	<div class="hero-content">
		<h1 class="hero-title" class:hero-title-visible={heroVisible}>AnkiMCP</h1>
		<p class="hero-subtitle" class:hero-subtitle-visible={heroSubVisible}>
			Your flashcards just got an AI brain.
		</p>
	</div>
</section>

<!-- SECTION 2: The Problem -->
<section class="section-problem" use:problemAction>
	<div class="problem-content">
		{#each problemText as line, i}
			<span
				class="problem-line"
				class:problem-line-visible={problemWords[i]}
				style="transition-delay: {i * 0.15}s"
			>
				{line}
				{#if i < problemText.length - 1}{' '}{/if}
			</span>
		{/each}
	</div>
</section>

<!-- SECTION 3: The Bridge -->
<section class="section-bridge" use:bridgeAction>
	<div class="bridge-container" class:bridge-container-visible={bridgeVisible}>
		<div class="bridge-node bridge-node-left" class:bridge-node-visible={bridgeVisible}>
			<div class="bridge-icon-wrapper bridge-icon-anki">
				<BookOpen size={36} strokeWidth={1.5} />
			</div>
			<span class="bridge-label">Anki</span>
		</div>

		<div class="bridge-beam-wrapper">
			<div class="bridge-beam" class:bridge-beam-active={bridgeVisible}>
				<div class="bridge-beam-pulse"></div>
				<div class="bridge-beam-pulse bridge-beam-pulse-2"></div>
				<div class="bridge-beam-pulse bridge-beam-pulse-3"></div>
			</div>
		</div>

		<div class="bridge-node bridge-node-right" class:bridge-node-visible={bridgeVisible}>
			<div class="bridge-icon-wrapper bridge-icon-claude">
				<Sparkles size={36} strokeWidth={1.5} />
			</div>
			<span class="bridge-label">Claude</span>
		</div>
	</div>

	<p class="bridge-tagline" class:bridge-tagline-visible={bridgeVisible}>
		AnkiMCP bridges the gap.
	</p>
</section>

<!-- SECTION 4: Capabilities Reveal -->
<section class="section-capabilities">
	{#each capabilityItems as cap, i}
		{@const Icon = cap.icon}
		<div
			class="capability-row"
			class:capability-row-visible={capabilities[i]}
			class:capability-from-left={i % 2 === 0}
			class:capability-from-right={i % 2 !== 0}
			data-idx={i}
			use:capabilityAction
		>
			<span class="capability-number">{cap.num}</span>
			<div class="capability-info">
				<h3 class="capability-title">
					<Icon size={24} strokeWidth={1.5} class="capability-icon-inline" />
					{cap.title}
				</h3>
				<p class="capability-desc">{cap.desc}</p>
			</div>
		</div>
	{/each}
</section>

<!-- SECTION 5: The Code -->
<section class="section-code" use:codeAction>
	<div class="code-glow"></div>
	<div class="code-window" class:code-window-visible={codeVisible}>
		<div class="code-titlebar">
			<div class="code-dot code-dot-red"></div>
			<div class="code-dot code-dot-yellow"></div>
			<div class="code-dot code-dot-green"></div>
			<span class="code-titlebar-text">mcp.json</span>
		</div>
		<pre class="code-body"><code>{mcpConfig}</code></pre>
	</div>
</section>

<!-- SECTION 6: Call to Action -->
<section class="section-cta" use:ctaAction>
	<div class="cta-content" class:cta-content-visible={ctaVisible}>
		<h2 class="cta-heading">Start in three steps.</h2>

		<ol class="cta-steps">
			<li class="cta-step">
				<span class="cta-step-num">1</span>
				<span class="cta-step-text">Install the addon from AnkiWeb (code <code>1513864660</code>)</span>
			</li>
			<li class="cta-step">
				<span class="cta-step-num">2</span>
				<span class="cta-step-text">Add the MCP config to Claude Desktop</span>
			</li>
			<li class="cta-step">
				<span class="cta-step-num">3</span>
				<span class="cta-step-text">Open Anki and start talking to your cards</span>
			</li>
		</ol>

		<div class="cta-buttons">
			<a href="https://github.com/shivros/ankimcp" class="cta-btn cta-btn-primary">
				<Github size={20} strokeWidth={2} />
				GitHub
			</a>
			<a href="/docs" class="cta-btn cta-btn-secondary">
				<FileCode size={20} strokeWidth={2} />
				Documentation
			</a>
		</div>
	</div>
</section>

<style>
	/* ============================================
	   GLOBAL PAGE OVERRIDES
	   ============================================ */
	:global(body) {
		background: #000 !important;
		background-image: none !important;
	}

	/* Hide the site header and footer on this cinematic page */
	:global(header) {
		background: rgba(0, 0, 0, 0.85) !important;
		border-bottom-color: rgba(255, 255, 255, 0.08) !important;
		backdrop-filter: blur(12px) !important;
	}

	:global(header .logo h1) {
		color: #fff !important;
	}

	:global(header nav a) {
		color: rgba(255, 255, 255, 0.5) !important;
	}

	:global(header nav a:hover) {
		color: #3b82f6 !important;
		background: rgba(59, 130, 246, 0.1) !important;
	}

	:global(header nav .active a) {
		color: #3b82f6 !important;
		background: rgba(59, 130, 246, 0.15) !important;
	}

	:global(header .theme-toggle) {
		background: rgba(255, 255, 255, 0.06) !important;
		border-color: rgba(255, 255, 255, 0.12) !important;
		color: rgba(255, 255, 255, 0.6) !important;
	}

	:global(header .github-btn) {
		background: #3b82f6 !important;
	}

	:global(.flex.min-h-screen.flex-col > main) {
		padding: 0 !important;
		max-width: 100% !important;
	}

	:global(footer) {
		background: #0a0506 !important;
		border-top-color: rgba(255, 255, 255, 0.08) !important;
	}

	:global(footer *) {
		color: rgba(255, 255, 255, 0.5) !important;
	}

	:global(footer h3),
	:global(footer a:hover) {
		color: #3b82f6 !important;
	}

	/* ============================================
	   SECTION 1: HERO / OPENING FRAME
	   ============================================ */
	.section-hero {
		min-height: 100vh;
		background: #000;
		display: flex;
		align-items: center;
		justify-content: center;
		position: relative;
		overflow: hidden;
	}

	.hero-grid {
		position: absolute;
		inset: 0;
		display: grid;
		grid-template-columns: repeat(20, 1fr);
		grid-template-rows: repeat(10, 1fr);
		gap: 0;
		pointer-events: none;
	}

	.hero-dot {
		width: 2px;
		height: 2px;
		background: rgba(255, 255, 255, 0.15);
		border-radius: 50%;
		place-self: center;
		opacity: 0;
		transition: opacity 2s ease;
	}

	.hero-dot-visible {
		animation: dotFade 4s ease-in-out infinite alternate;
	}

	@keyframes dotFade {
		0% {
			opacity: 0.03;
		}
		50% {
			opacity: 0.2;
		}
		100% {
			opacity: 0.05;
		}
	}

	.hero-content {
		position: relative;
		z-index: 1;
		text-align: center;
		padding: 2rem;
	}

	.hero-title {
		font-size: clamp(5rem, 10vw, 12rem);
		font-weight: 900;
		color: #fff;
		letter-spacing: -0.03em;
		line-height: 1;
		margin: 0;
		opacity: 0;
		transform: scale(0.95);
		transition:
			opacity 1.5s cubic-bezier(0.16, 1, 0.3, 1),
			transform 1.5s cubic-bezier(0.16, 1, 0.3, 1);
	}

	.hero-title-visible {
		opacity: 1;
		transform: scale(1);
	}

	.hero-subtitle {
		font-size: clamp(1rem, 2.5vw, 1.75rem);
		font-weight: 300;
		color: rgba(255, 255, 255, 0.5);
		margin: 2rem 0 0;
		line-height: 1.8;
		opacity: 0;
		transform: translateY(20px);
		transition:
			opacity 1s ease,
			transform 1s ease;
	}

	.hero-subtitle-visible {
		opacity: 1;
		transform: translateY(0);
	}

	/* ============================================
	   SECTION 2: THE PROBLEM
	   ============================================ */
	.section-problem {
		min-height: 100vh;
		background: #111;
		background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.85' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.03'/%3E%3C/svg%3E");
		display: flex;
		align-items: center;
		padding: 4rem clamp(2rem, 8vw, 10rem);
	}

	.problem-content {
		max-width: 900px;
	}

	.problem-line {
		font-size: clamp(1.75rem, 4vw, 3.5rem);
		font-weight: 300;
		color: #fff;
		line-height: 1.6;
		opacity: 0;
		transform: translateY(20px);
		transition:
			opacity 0.8s ease,
			transform 0.8s ease;
		display: inline;
	}

	.problem-line-visible {
		opacity: 1;
		transform: translateY(0);
	}

	/* Make key words stand out */
	.problem-line:nth-child(3) {
		color: rgba(255, 255, 255, 0.9);
		font-weight: 500;
	}

	.problem-line:nth-child(4) {
		color: rgba(245, 158, 11, 0.9);
		font-weight: 500;
	}

	.problem-line:nth-child(5) {
		color: rgba(59, 130, 246, 0.9);
		font-weight: 500;
	}

	/* ============================================
	   SECTION 3: THE BRIDGE
	   ============================================ */
	.section-bridge {
		min-height: 100vh;
		background: linear-gradient(180deg, #111 0%, #0f0a2e 100%);
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		gap: 3rem;
		padding: 4rem 2rem;
		overflow: hidden;
	}

	.bridge-container {
		display: flex;
		align-items: center;
		gap: 0;
		width: 100%;
		max-width: 700px;
		opacity: 0;
		transition: opacity 1s ease;
	}

	.bridge-container-visible {
		opacity: 1;
	}

	.bridge-node {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 1rem;
		flex-shrink: 0;
		opacity: 0;
		transition:
			opacity 0.8s ease,
			transform 0.8s ease;
	}

	.bridge-node-left {
		transform: translateX(-40px);
	}

	.bridge-node-right {
		transform: translateX(40px);
	}

	.bridge-node-visible {
		opacity: 1;
		transform: translateX(0);
	}

	.bridge-icon-wrapper {
		width: 80px;
		height: 80px;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
		border: 2px solid;
	}

	.bridge-icon-anki {
		color: #f59e0b;
		border-color: rgba(245, 158, 11, 0.4);
		background: rgba(245, 158, 11, 0.08);
	}

	.bridge-icon-claude {
		color: #3b82f6;
		border-color: rgba(59, 130, 246, 0.4);
		background: rgba(59, 130, 246, 0.08);
	}

	.bridge-label {
		font-size: 1.1rem;
		font-weight: 600;
		color: rgba(255, 255, 255, 0.8);
		letter-spacing: 0.05em;
		text-transform: uppercase;
	}

	.bridge-beam-wrapper {
		flex: 1;
		padding: 0 1.5rem;
		display: flex;
		align-items: center;
	}

	.bridge-beam {
		width: 100%;
		height: 3px;
		background: rgba(255, 255, 255, 0.06);
		border-radius: 2px;
		position: relative;
		overflow: hidden;
	}

	.bridge-beam-pulse {
		position: absolute;
		top: -1px;
		left: -30%;
		width: 30%;
		height: 5px;
		background: linear-gradient(90deg, transparent, #3b82f6, #f59e0b, transparent);
		border-radius: 3px;
		opacity: 0;
	}

	.bridge-beam-active .bridge-beam-pulse {
		animation: beamTravel 2s ease-in-out infinite;
		opacity: 1;
	}

	.bridge-beam-pulse-2 {
		animation-delay: 0.7s !important;
	}

	.bridge-beam-pulse-3 {
		animation-delay: 1.4s !important;
	}

	@keyframes beamTravel {
		0% {
			left: -30%;
			opacity: 0;
		}
		10% {
			opacity: 1;
		}
		90% {
			opacity: 1;
		}
		100% {
			left: 100%;
			opacity: 0;
		}
	}

	.bridge-tagline {
		font-size: clamp(1.25rem, 3vw, 2rem);
		font-weight: 300;
		color: rgba(255, 255, 255, 0.6);
		text-align: center;
		margin: 0;
		opacity: 0;
		transform: translateY(20px);
		transition:
			opacity 1s ease 0.5s,
			transform 1s ease 0.5s;
	}

	.bridge-tagline-visible {
		opacity: 1;
		transform: translateY(0);
	}

	/* ============================================
	   SECTION 4: CAPABILITIES REVEAL
	   ============================================ */
	.section-capabilities {
		min-height: 100vh;
		background: #0f0a2e;
		display: flex;
		flex-direction: column;
		justify-content: center;
		padding: 6rem clamp(2rem, 8vw, 10rem);
		gap: 4rem;
	}

	.capability-row {
		display: flex;
		align-items: center;
		gap: clamp(1.5rem, 3vw, 3rem);
		opacity: 0;
		transition:
			opacity 0.8s ease,
			transform 0.8s ease;
	}

	.capability-from-left {
		transform: translateX(-60px);
	}

	.capability-from-right {
		transform: translateX(60px);
		flex-direction: row-reverse;
		text-align: right;
	}

	.capability-row-visible {
		opacity: 1;
		transform: translateX(0) !important;
	}

	.capability-number {
		font-size: clamp(3rem, 6vw, 5rem);
		font-weight: 900;
		color: transparent;
		-webkit-text-stroke: 1.5px rgba(59, 130, 246, 0.5);
		line-height: 1;
		flex-shrink: 0;
		letter-spacing: -0.02em;
		font-variant-numeric: tabular-nums;
	}

	.capability-info {
		flex: 1;
		max-width: 500px;
	}

	.capability-title {
		font-size: clamp(1.25rem, 2.5vw, 2rem);
		font-weight: 700;
		color: #fff;
		margin: 0 0 0.5rem;
		display: flex;
		align-items: center;
		gap: 0.75rem;
	}

	.capability-from-right .capability-title {
		justify-content: flex-end;
	}

	:global(.capability-icon-inline) {
		color: #3b82f6;
		flex-shrink: 0;
	}

	.capability-desc {
		font-size: clamp(0.95rem, 1.5vw, 1.15rem);
		font-weight: 300;
		color: rgba(255, 255, 255, 0.5);
		line-height: 1.8;
		margin: 0;
	}

	/* ============================================
	   SECTION 5: THE CODE
	   ============================================ */
	.section-code {
		min-height: 80vh;
		background: #0a0618;
		display: flex;
		align-items: center;
		justify-content: center;
		position: relative;
		padding: 4rem 2rem;
		overflow: hidden;
	}

	.code-glow {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		width: 500px;
		height: 500px;
		background: radial-gradient(circle, rgba(99, 102, 241, 0.15) 0%, rgba(99, 102, 241, 0.05) 40%, transparent 70%);
		border-radius: 50%;
		pointer-events: none;
	}

	.code-window {
		position: relative;
		z-index: 1;
		width: 100%;
		max-width: 540px;
		border-radius: 12px;
		overflow: hidden;
		background: rgba(17, 17, 27, 0.95);
		border: 1px solid rgba(255, 255, 255, 0.08);
		box-shadow:
			0 0 80px rgba(99, 102, 241, 0.12),
			0 25px 60px rgba(0, 0, 0, 0.5);
		opacity: 0;
		transform: translateY(40px) scale(0.96);
		transition:
			opacity 1s cubic-bezier(0.16, 1, 0.3, 1),
			transform 1s cubic-bezier(0.16, 1, 0.3, 1);
	}

	.code-window-visible {
		opacity: 1;
		transform: translateY(0) scale(1);
	}

	.code-titlebar {
		display: flex;
		align-items: center;
		gap: 8px;
		padding: 14px 18px;
		background: rgba(255, 255, 255, 0.03);
		border-bottom: 1px solid rgba(255, 255, 255, 0.06);
	}

	.code-dot {
		width: 12px;
		height: 12px;
		border-radius: 50%;
	}

	.code-dot-red {
		background: #ff5f57;
	}

	.code-dot-yellow {
		background: #febc2e;
	}

	.code-dot-green {
		background: #28c840;
	}

	.code-titlebar-text {
		margin-left: auto;
		font-size: 0.8rem;
		color: rgba(255, 255, 255, 0.3);
		font-family: var(--font-mono);
	}

	.code-body {
		margin: 0;
		padding: 1.5rem 2rem;
		font-family: var(--font-mono);
		font-size: clamp(0.8rem, 1.5vw, 0.95rem);
		line-height: 1.7;
		color: rgba(255, 255, 255, 0.8);
		background: transparent !important;
		border: none !important;
		border-radius: 0 !important;
		box-shadow: none !important;
		overflow-x: auto;
	}

	.code-body code {
		background: transparent;
		padding: 0;
		border-radius: 0;
		font-family: var(--font-mono);
		color: rgba(255, 255, 255, 0.8);
	}

	/* ============================================
	   SECTION 6: CALL TO ACTION
	   ============================================ */
	.section-cta {
		min-height: 60vh;
		background: linear-gradient(180deg, #0a0618 0%, #1a0a0a 100%);
		display: flex;
		align-items: center;
		justify-content: center;
		padding: 4rem 2rem;
	}

	.cta-content {
		text-align: center;
		max-width: 600px;
		opacity: 0;
		transform: translateY(30px);
		transition:
			opacity 0.8s ease,
			transform 0.8s ease;
	}

	.cta-content-visible {
		opacity: 1;
		transform: translateY(0);
	}

	.cta-heading {
		font-size: clamp(2rem, 5vw, 3.5rem);
		font-weight: 900;
		color: #fff;
		margin: 0 0 3rem;
		letter-spacing: -0.02em;
		line-height: 1.2;
	}

	.cta-steps {
		list-style: none;
		padding: 0;
		margin: 0 0 3rem;
		display: flex;
		flex-direction: column;
		gap: 1.25rem;
	}

	.cta-step {
		display: flex;
		align-items: center;
		gap: 1.25rem;
		text-align: left;
	}

	.cta-step-num {
		width: 36px;
		height: 36px;
		border-radius: 50%;
		border: 1.5px solid rgba(59, 130, 246, 0.5);
		color: #3b82f6;
		font-weight: 700;
		font-size: 0.9rem;
		display: flex;
		align-items: center;
		justify-content: center;
		flex-shrink: 0;
	}

	.cta-step-text {
		font-size: clamp(0.95rem, 1.5vw, 1.1rem);
		font-weight: 300;
		color: rgba(255, 255, 255, 0.7);
		line-height: 1.6;
	}

	.cta-step-text code {
		background: rgba(59, 130, 246, 0.12);
		color: #3b82f6;
		padding: 0.15rem 0.5rem;
		border-radius: 4px;
		font-family: var(--font-mono);
		font-size: 0.9em;
		font-weight: 500;
	}

	.cta-buttons {
		display: flex;
		gap: 1rem;
		justify-content: center;
		flex-wrap: wrap;
	}

	.cta-btn {
		display: inline-flex;
		align-items: center;
		gap: 0.6rem;
		padding: 0.9rem 2rem;
		border-radius: 8px;
		font-weight: 600;
		font-size: 1rem;
		text-decoration: none;
		transition:
			transform 0.2s ease,
			box-shadow 0.2s ease,
			background 0.2s ease;
	}

	.cta-btn:hover {
		transform: translateY(-2px);
		text-decoration: none;
	}

	.cta-btn-primary {
		background: #3b82f6;
		color: #fff;
		box-shadow: 0 8px 30px rgba(59, 130, 246, 0.3);
	}

	.cta-btn-primary:hover {
		background: #2563eb;
		box-shadow: 0 12px 40px rgba(59, 130, 246, 0.4);
	}

	.cta-btn-secondary {
		background: rgba(255, 255, 255, 0.06);
		color: rgba(255, 255, 255, 0.8);
		border: 1px solid rgba(255, 255, 255, 0.12);
	}

	.cta-btn-secondary:hover {
		background: rgba(255, 255, 255, 0.1);
		border-color: rgba(255, 255, 255, 0.2);
	}

	/* ============================================
	   RESPONSIVE
	   ============================================ */
	@media (max-width: 640px) {
		.section-problem {
			padding: 4rem 1.5rem;
		}

		/* Bridge goes vertical on mobile */
		.bridge-container {
			flex-direction: column;
			gap: 0;
			max-width: 300px;
		}

		.bridge-node-left {
			transform: translateY(-30px);
		}

		.bridge-node-right {
			transform: translateY(30px);
		}

		.bridge-node-visible {
			transform: translateY(0) !important;
		}

		.bridge-beam-wrapper {
			padding: 1.5rem 0;
			width: 3px;
			height: 100px;
			flex: none;
		}

		.bridge-beam {
			width: 3px;
			height: 100%;
		}

		.bridge-beam-pulse {
			top: -30%;
			left: -1px;
			width: 5px;
			height: 30%;
			background: linear-gradient(180deg, transparent, #3b82f6, #f59e0b, transparent);
		}

		@keyframes beamTravel {
			0% {
				top: -30%;
				left: -1px;
				opacity: 0;
			}
			10% {
				opacity: 1;
			}
			90% {
				opacity: 1;
			}
			100% {
				top: 100%;
				left: -1px;
				opacity: 0;
			}
		}

		.bridge-icon-wrapper {
			width: 64px;
			height: 64px;
		}

		/* Capabilities stack vertically */
		.capability-row,
		.capability-from-right {
			flex-direction: column !important;
			text-align: center;
			gap: 0.75rem;
		}

		.capability-from-right .capability-title {
			justify-content: center;
		}

		.capability-from-left {
			transform: translateY(30px);
		}

		.capability-from-right {
			transform: translateY(30px);
		}

		.section-capabilities {
			padding: 4rem 1.5rem;
			gap: 3rem;
		}

		.cta-step {
			align-items: flex-start;
		}
	}

	@media (max-width: 420px) {
		.hero-title {
			font-size: clamp(3.5rem, 15vw, 5rem);
		}

		.problem-line {
			font-size: clamp(1.25rem, 5vw, 1.75rem);
		}
	}
</style>
