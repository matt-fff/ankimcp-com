<script lang="ts">
	import { Crosshair, CheckSquare, FileText, Cpu, ArrowRight } from 'lucide-svelte';

	let visible = $state(false);
	let sectionsRevealed = $state<Record<string, boolean>>({
		title: false,
		schematic: false,
		specs: false,
		procedure: false,
		revisions: false
	});

	$effect(() => {
		visible = true;
		const delays: [string, number][] = [
			['title', 200],
			['schematic', 600],
			['specs', 1000],
			['procedure', 1400],
			['revisions', 1800]
		];
		const timers: ReturnType<typeof setTimeout>[] = [];
		for (const [key, delay] of delays) {
			timers.push(
				setTimeout(() => {
					sectionsRevealed[key] = true;
				}, delay)
			);
		}
		return () => timers.forEach(clearTimeout);
	});

	const capabilities = [
		{ feature: 'Deck Mgmt', status: 'ACTIVE', description: 'CRUD operations on deck structures' },
		{ feature: 'Note Ops', status: 'ACTIVE', description: 'Search, create, modify notes' },
		{ feature: 'Permissions', status: 'ACTIVE', description: 'R/W/D granular control' },
		{ feature: 'Statistics', status: 'ACTIVE', description: 'Review analytics & progress' },
		{ feature: 'Note Types', status: 'ACTIVE', description: 'List & create note models' },
		{ feature: 'Tag Control', status: 'ACTIVE', description: 'Protected & readonly tags' }
	];

	const revisions = [
		{ rev: 'C', date: '2024-12', description: 'Added SSE transport support', author: 'MF' },
		{ rev: 'B', date: '2024-09', description: 'Permission system overhaul', author: 'MF' },
		{ rev: 'A', date: '2024-06', description: 'Initial release', author: 'MF' }
	];

	const installSteps = [
		{ ref: 'INST-001', text: 'Open Anki application and navigate to Tools > Add-ons > Get Add-ons' },
		{ ref: 'INST-002', text: 'Enter addon code: 1513864660' },
		{ ref: 'INST-003', text: 'Confirm installation and restart Anki client' },
		{ ref: 'INST-004', text: 'Verify MCP server starts on profile load (port 4473)' },
		{ ref: 'INST-005', text: 'Configure MCP host with connection payload (see below)' }
	];
</script>

<svelte:head>
	<title>AnkiMCP - Blueprint Specification</title>
	<meta
		name="description"
		content="AnkiMCP technical blueprint - Model Context Protocol bridge for Anki flashcard collections"
	/>
</svelte:head>

<div class="blueprint-page" class:visible>
	<!-- Grid background overlay -->
	<div class="grid-overlay"></div>

	<!-- Corner registration marks -->
	<div class="reg-mark reg-top-left">+</div>
	<div class="reg-mark reg-top-right">+</div>
	<div class="reg-mark reg-bottom-left">+</div>
	<div class="reg-mark reg-bottom-right">+</div>

	<!-- Fold marks -->
	<div class="fold-mark fold-left-mid"></div>
	<div class="fold-mark fold-right-mid"></div>
	<div class="fold-mark fold-top-mid"></div>
	<div class="fold-mark fold-bottom-mid"></div>

	<div class="blueprint-content">
		<!-- TITLE BLOCK -->
		<section class="title-block" class:section-visible={sectionsRevealed.title}>
			<div class="title-block-inner">
				<div class="title-row title-main">
					<div class="title-field">
						<span class="field-label">PROJECT</span>
						<span class="field-value title-name">AnkiMCP</span>
					</div>
					<div class="title-stamp">
						<div class="stamp-circle">
							<span>APPROVED</span>
						</div>
					</div>
				</div>
				<div class="title-row title-meta">
					<div class="title-field">
						<span class="field-label">REVISION</span>
						<span class="field-value">1.0</span>
					</div>
					<div class="title-field">
						<span class="field-label">DATE</span>
						<span class="field-value">2024</span>
					</div>
					<div class="title-field">
						<span class="field-label">STATUS</span>
						<span class="field-value status-prod">PRODUCTION</span>
					</div>
					<div class="title-field">
						<span class="field-label">DWG NO.</span>
						<span class="field-value">DWG-AMCP-001</span>
					</div>
				</div>
				<div class="title-row title-desc">
					<div class="title-field full-width">
						<span class="field-label">DESCRIPTION</span>
						<span class="field-value"
							>Model Context Protocol Bridge for Anki Flashcard Collections</span
						>
					</div>
				</div>
				<div class="title-row title-sub-desc">
					<div class="title-field full-width">
						<span class="field-label">ABSTRACT</span>
						<span class="field-value muted"
							>Server-Sent Events (SSE) transport layer enabling AI assistants to interface with Anki
							collections via standardized JSON-RPC messaging protocol</span
						>
					</div>
				</div>
			</div>
		</section>

		<!-- SYSTEM SCHEMATIC -->
		<section class="schematic-section" class:section-visible={sectionsRevealed.schematic}>
			<div class="section-header">
				<Crosshair size={16} />
				<span>SYSTEM ARCHITECTURE SCHEMATIC</span>
				<span class="header-ref">REF: ARCH-001</span>
			</div>

			<div class="schematic-diagram">
				<!-- Dimension line top -->
				<div class="dimension-line-top">
					<div class="dim-mark dim-mark-left"></div>
					<div class="dim-line-h"></div>
					<div class="dim-label-top">FULL SYSTEM SPAN</div>
					<div class="dim-mark dim-mark-right"></div>
				</div>

				<div class="schematic-nodes">
					<!-- Node: ANKI CLIENT -->
					<div class="schematic-node">
						<div class="node-callout node-callout-left">
							<span class="callout-dot"></span>
							<span class="callout-line"></span>
							<span class="callout-text">Desktop App<br />Qt Framework</span>
						</div>
						<div class="node-box">
							<div class="node-icon">
								<FileText size={20} />
							</div>
							<div class="node-label">ANKI CLIENT</div>
							<div class="node-sub">addon host</div>
						</div>
					</div>

					<!-- Connection 1 -->
					<div class="connection">
						<div class="conn-line">
							<div class="conn-arrow">
								<ArrowRight size={14} />
							</div>
						</div>
						<div class="conn-labels">
							<span class="conn-protocol">SSE</span>
							<span class="conn-detail">Server-Sent Events</span>
						</div>
						<!-- Dimension line for connection -->
						<div class="conn-dimension">
							<span class="dim-end-mark"></span>
							<span class="dim-measurement">localhost</span>
							<span class="dim-end-mark"></span>
						</div>
					</div>

					<!-- Node: MCP SERVER -->
					<div class="schematic-node node-primary">
						<div class="node-box">
							<div class="node-icon">
								<Cpu size={20} />
							</div>
							<div class="node-label">MCP SERVER</div>
							<div class="node-port">:4473</div>
							<div class="node-sub">ankimcp addon</div>
						</div>
						<div class="node-callout node-callout-bottom">
							<span class="callout-dot"></span>
							<span class="callout-line callout-line-v"></span>
							<span class="callout-text">JSON-RPC 2.0<br />over SSE transport</span>
						</div>
					</div>

					<!-- Connection 2 -->
					<div class="connection">
						<div class="conn-line">
							<div class="conn-arrow">
								<ArrowRight size={14} />
							</div>
						</div>
						<div class="conn-labels">
							<span class="conn-protocol">JSON-RPC</span>
							<span class="conn-detail">MCP Protocol</span>
						</div>
						<div class="conn-dimension">
							<span class="dim-end-mark"></span>
							<span class="dim-measurement">bidirectional</span>
							<span class="dim-end-mark"></span>
						</div>
					</div>

					<!-- Node: AI ASSISTANT -->
					<div class="schematic-node">
						<div class="node-callout node-callout-right">
							<span class="callout-dot"></span>
							<span class="callout-line"></span>
							<span class="callout-text">Claude, GPT<br />or any MCP host</span>
						</div>
						<div class="node-box">
							<div class="node-icon">
								<Cpu size={20} />
							</div>
							<div class="node-label">AI ASSISTANT</div>
							<div class="node-sub">MCP host</div>
						</div>
					</div>
				</div>

				<!-- NOTE callout -->
				<div class="note-callout">
					<span class="note-label">NOTE:</span>
					<span class="note-text"
						>Server auto-starts when Anki profile is loaded. All communication occurs over
						localhost.</span
					>
				</div>
			</div>
		</section>

		<!-- SPECIFICATIONS TABLE -->
		<section class="specs-section" class:section-visible={sectionsRevealed.specs}>
			<div class="section-header">
				<Crosshair size={16} />
				<span>CAPABILITY SPECIFICATIONS</span>
				<span class="header-ref">REF: SPEC-001</span>
			</div>

			<div class="specs-table-wrapper">
				<table class="specs-table">
					<thead>
						<tr>
							<th>ITEM</th>
							<th>FEATURE</th>
							<th>STATUS</th>
							<th>DESCRIPTION</th>
						</tr>
					</thead>
					<tbody>
						{#each capabilities as cap, i}
							<tr>
								<td class="item-num">{String(i + 1).padStart(2, '0')}</td>
								<td class="feature-name">{cap.feature}</td>
								<td class="status-cell">
									<span class="status-dot"></span>
									{cap.status}
								</td>
								<td class="desc-cell">{cap.description}</td>
							</tr>
						{/each}
					</tbody>
				</table>
			</div>

			<div class="spec-notes">
				<div class="note-callout">
					<span class="note-label">NOTE:</span>
					<span class="note-text"
						>All capabilities subject to permission configuration. Delete operations require explicit
						enablement.</span
					>
				</div>
			</div>
		</section>

		<!-- INSTALLATION PROCEDURE -->
		<section class="procedure-section" class:section-visible={sectionsRevealed.procedure}>
			<div class="section-header">
				<CheckSquare size={16} />
				<span>PROCEDURE: SYSTEM INSTALLATION</span>
				<span class="header-ref">REF: PROC-001</span>
			</div>

			<div class="procedure-steps">
				{#each installSteps as step, i}
					<div class="procedure-step">
						<div class="step-checkbox"></div>
						<div class="step-number">STEP {i + 1}</div>
						<div class="step-ref">REF: {step.ref}</div>
						<div class="step-text">{step.text}</div>
					</div>
				{/each}
			</div>

			<div class="config-payload">
				<div class="payload-header">
					<span class="payload-label">CONFIGURATION PAYLOAD</span>
					<span class="payload-ref">REF: CFG-001</span>
				</div>
				<pre class="payload-code">{`{
  "mcpServers": {
    "ankimcp": {
      "type": "sse",
      "url": "http://localhost:4473/sse"
    }
  }
}`}</pre>
				<div class="payload-footer">
					<span class="note-label">TARGET:</span>
					<span class="note-text"
						>claude_desktop_config.json | .mcp.json | MCP host configuration</span
					>
				</div>
			</div>

			<!-- Additional config detail -->
			<div class="config-payload" style="margin-top: 1rem;">
				<div class="payload-header">
					<span class="payload-label">ADDON CONFIGURATION (OPTIONAL)</span>
					<span class="payload-ref">REF: CFG-002</span>
				</div>
				<pre class="payload-code">{`{
  "host": "localhost",
  "port": 4473,
  "mode": "denylist",
  "global_permissions": {
    "read": true,
    "write": true,
    "delete": false
  },
  "deck_denylist": ["PrivateDeck"],
  "protected_decks": ["Default"]
}`}</pre>
				<div class="payload-footer">
					<span class="note-label">ACCESS:</span>
					<span class="note-text">Tools > Add-ons > AnkiMCP > Config</span>
				</div>
			</div>
		</section>

		<!-- REVISION NOTES -->
		<section class="revisions-section" class:section-visible={sectionsRevealed.revisions}>
			<div class="section-header">
				<Crosshair size={16} />
				<span>REVISION HISTORY</span>
				<span class="header-ref">DWG-AMCP-001</span>
			</div>

			<table class="revision-table">
				<thead>
					<tr>
						<th>REV</th>
						<th>DATE</th>
						<th>DESCRIPTION</th>
						<th>AUTHOR</th>
					</tr>
				</thead>
				<tbody>
					{#each revisions as rev}
						<tr>
							<td class="rev-letter">{rev.rev}</td>
							<td>{rev.date}</td>
							<td>{rev.description}</td>
							<td>{rev.author}</td>
						</tr>
					{/each}
				</tbody>
			</table>

			<div class="blueprint-footer">
				<div class="footer-left">
					<span class="footer-item">SCALE: NTS</span>
					<span class="footer-item">UNITS: METRIC</span>
					<span class="footer-item">SHEET: 1 OF 1</span>
				</div>
				<div class="footer-right">
					<div class="stamp-circle stamp-small">
						<span>v1.0</span>
					</div>
				</div>
			</div>
		</section>
	</div>
</div>

<style>
	/* =========================================================
	   BLUEPRINT PAGE - Technical Schematic Aesthetic
	   ========================================================= */

	:global(body:has(.blueprint-page)) {
		background: #0a1628 !important;
		background-image: none !important;
	}

	.blueprint-page {
		--bp-bg: #0a1628;
		--bp-grid-thin: rgba(0, 180, 255, 0.06);
		--bp-grid-thick: rgba(0, 180, 255, 0.12);
		--bp-text: #e0f0ff;
		--bp-accent: #00b4ff;
		--bp-orange: #ff6b35;
		--bp-border: rgba(0, 180, 255, 0.3);
		--bp-muted: rgba(224, 240, 255, 0.4);
		--bp-surface: rgba(0, 180, 255, 0.04);
		--bp-font: var(--font-mono), 'Courier New', Courier, monospace;

		position: relative;
		min-height: 100vh;
		background: var(--bp-bg);
		color: var(--bp-text);
		font-family: var(--bp-font);
		font-size: 0.875rem;
		line-height: 1.5;
		overflow-x: hidden;
		opacity: 0;
		transition: opacity 0.8s ease;
	}

	.blueprint-page.visible {
		opacity: 1;
	}

	/* Grid overlay */
	.grid-overlay {
		position: fixed;
		inset: 0;
		pointer-events: none;
		z-index: 0;
		background-image:
			linear-gradient(var(--bp-grid-thin) 1px, transparent 1px),
			linear-gradient(90deg, var(--bp-grid-thin) 1px, transparent 1px),
			linear-gradient(var(--bp-grid-thick) 1px, transparent 1px),
			linear-gradient(90deg, var(--bp-grid-thick) 1px, transparent 1px);
		background-size:
			40px 40px,
			40px 40px,
			200px 200px,
			200px 200px;
		opacity: 0;
		animation: gridFadeIn 1.5s ease 0.3s forwards;
	}

	@keyframes gridFadeIn {
		to {
			opacity: 1;
		}
	}

	/* Registration marks */
	.reg-mark {
		position: fixed;
		font-family: var(--bp-font);
		font-size: 1.5rem;
		color: rgba(0, 180, 255, 0.25);
		z-index: 1;
		pointer-events: none;
		line-height: 1;
	}

	.reg-top-left {
		top: 16px;
		left: 16px;
	}
	.reg-top-right {
		top: 16px;
		right: 16px;
	}
	.reg-bottom-left {
		bottom: 16px;
		left: 16px;
	}
	.reg-bottom-right {
		bottom: 16px;
		right: 16px;
	}

	/* Fold marks */
	.fold-mark {
		position: fixed;
		background: rgba(0, 180, 255, 0.15);
		z-index: 1;
		pointer-events: none;
	}

	.fold-left-mid,
	.fold-right-mid {
		width: 12px;
		height: 1px;
		top: 50%;
	}

	.fold-left-mid {
		left: 0;
	}
	.fold-right-mid {
		right: 0;
	}

	.fold-top-mid,
	.fold-bottom-mid {
		width: 1px;
		height: 12px;
		left: 50%;
	}

	.fold-top-mid {
		top: 0;
	}
	.fold-bottom-mid {
		bottom: 0;
	}

	/* Content container */
	.blueprint-content {
		position: relative;
		z-index: 2;
		max-width: 960px;
		margin: 0 auto;
		padding: 3rem 2rem 4rem;
		display: flex;
		flex-direction: column;
		gap: 2.5rem;
	}

	/* Section reveal animation */
	section {
		opacity: 0;
		transform: translateY(12px);
		transition:
			opacity 0.6s ease,
			transform 0.6s ease;
	}

	section.section-visible {
		opacity: 1;
		transform: translateY(0);
	}

	/* =========================================================
	   SECTION HEADER
	   ========================================================= */

	.section-header {
		display: flex;
		align-items: center;
		gap: 0.5rem;
		padding-bottom: 0.5rem;
		border-bottom: 1px solid var(--bp-border);
		margin-bottom: 1.25rem;
		font-size: 0.8rem;
		text-transform: uppercase;
		letter-spacing: 0.08em;
		color: var(--bp-accent);
	}

	.header-ref {
		margin-left: auto;
		color: var(--bp-muted);
		font-size: 0.7rem;
	}

	/* =========================================================
	   TITLE BLOCK
	   ========================================================= */

	.title-block-inner {
		border: 2px solid var(--bp-border);
		display: flex;
		flex-direction: column;
	}

	.title-row {
		display: flex;
		border-bottom: 1px solid rgba(0, 180, 255, 0.2);
	}

	.title-row:last-child {
		border-bottom: none;
	}

	.title-field {
		display: flex;
		flex-direction: column;
		padding: 0.6rem 1rem;
		border-right: 1px solid rgba(0, 180, 255, 0.2);
		flex: 1;
	}

	.title-field:last-child {
		border-right: none;
	}

	.title-field.full-width {
		flex: 1;
	}

	.field-label {
		font-size: 0.65rem;
		text-transform: uppercase;
		letter-spacing: 0.1em;
		color: var(--bp-muted);
		margin-bottom: 0.15rem;
	}

	.field-value {
		color: var(--bp-text);
		font-size: 0.85rem;
	}

	.field-value.muted {
		color: var(--bp-muted);
		font-size: 0.8rem;
	}

	.title-name {
		font-size: 2rem;
		font-weight: 700;
		letter-spacing: 0.15em;
		color: var(--bp-accent);
	}

	.title-main {
		align-items: center;
	}

	.title-main .title-field {
		flex: 1;
	}

	.status-prod {
		color: #4ade80;
	}

	/* Stamp */
	.title-stamp {
		display: flex;
		align-items: center;
		justify-content: center;
		padding: 0.75rem 1.5rem;
	}

	.stamp-circle {
		width: 64px;
		height: 64px;
		border: 2px solid var(--bp-accent);
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 0.6rem;
		text-transform: uppercase;
		letter-spacing: 0.08em;
		color: var(--bp-accent);
		transform: rotate(-12deg);
		position: relative;
	}

	.stamp-circle::before {
		content: '';
		position: absolute;
		inset: 3px;
		border: 1px solid rgba(0, 180, 255, 0.3);
		border-radius: 50%;
	}

	.stamp-small {
		width: 44px;
		height: 44px;
		font-size: 0.65rem;
		font-weight: 700;
	}

	/* =========================================================
	   SYSTEM SCHEMATIC
	   ========================================================= */

	.schematic-diagram {
		padding: 1.5rem 0;
	}

	/* Dimension line */
	.dimension-line-top {
		display: flex;
		align-items: center;
		justify-content: center;
		margin-bottom: 1.25rem;
		position: relative;
		padding: 0 2rem;
	}

	.dim-line-h {
		flex: 1;
		height: 1px;
		background: var(--bp-muted);
		position: relative;
		animation: dimLineDraw 1s ease 0.8s both;
		transform-origin: left;
	}

	@keyframes dimLineDraw {
		from {
			transform: scaleX(0);
		}
		to {
			transform: scaleX(1);
		}
	}

	.dim-mark {
		width: 1px;
		height: 10px;
		background: var(--bp-muted);
	}

	.dim-label-top {
		position: absolute;
		top: -14px;
		left: 50%;
		transform: translateX(-50%);
		font-size: 0.6rem;
		color: var(--bp-muted);
		letter-spacing: 0.1em;
		white-space: nowrap;
	}

	/* Nodes layout */
	.schematic-nodes {
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 0;
		flex-wrap: wrap;
	}

	.schematic-node {
		position: relative;
		flex: 0 0 auto;
	}

	.node-box {
		border: 1.5px solid var(--bp-border);
		padding: 1rem 1.25rem;
		text-align: center;
		background: rgba(0, 180, 255, 0.03);
		min-width: 130px;
		position: relative;
	}

	.node-primary .node-box {
		border-color: var(--bp-accent);
		background: rgba(0, 180, 255, 0.06);
		box-shadow:
			0 0 20px rgba(0, 180, 255, 0.08),
			inset 0 0 20px rgba(0, 180, 255, 0.03);
	}

	.node-icon {
		color: var(--bp-accent);
		margin-bottom: 0.4rem;
		display: flex;
		justify-content: center;
	}

	.node-label {
		font-size: 0.75rem;
		font-weight: 700;
		letter-spacing: 0.1em;
		text-transform: uppercase;
		color: var(--bp-text);
	}

	.node-port {
		font-size: 0.85rem;
		color: var(--bp-orange);
		font-weight: 700;
		margin-top: 0.1rem;
	}

	.node-sub {
		font-size: 0.6rem;
		color: var(--bp-muted);
		text-transform: uppercase;
		letter-spacing: 0.08em;
		margin-top: 0.25rem;
	}

	/* Callouts */
	.node-callout {
		position: absolute;
		display: flex;
		align-items: center;
		gap: 0.25rem;
		pointer-events: none;
	}

	.node-callout-left {
		right: calc(100% + 8px);
		top: 50%;
		transform: translateY(-50%);
		flex-direction: row-reverse;
	}

	.node-callout-right {
		left: calc(100% + 8px);
		top: 50%;
		transform: translateY(-50%);
	}

	.node-callout-bottom {
		top: calc(100% + 8px);
		left: 50%;
		transform: translateX(-50%);
		flex-direction: column;
	}

	.callout-dot {
		width: 4px;
		height: 4px;
		border-radius: 50%;
		background: var(--bp-accent);
		flex-shrink: 0;
	}

	.callout-line {
		width: 24px;
		height: 1px;
		background: rgba(0, 180, 255, 0.3);
		border-style: dashed;
		flex-shrink: 0;
	}

	.callout-line-v {
		width: 1px;
		height: 20px;
	}

	.callout-text {
		font-size: 0.6rem;
		color: var(--bp-muted);
		line-height: 1.3;
		white-space: nowrap;
	}

	/* Connections */
	.connection {
		display: flex;
		flex-direction: column;
		align-items: center;
		padding: 0 0.5rem;
		min-width: 80px;
		flex: 0 0 auto;
	}

	.conn-line {
		width: 100%;
		height: 1.5px;
		background: var(--bp-border);
		position: relative;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.conn-arrow {
		color: var(--bp-accent);
		position: absolute;
		right: -2px;
		display: flex;
	}

	.conn-labels {
		display: flex;
		flex-direction: column;
		align-items: center;
		margin-top: 0.35rem;
		gap: 0.1rem;
	}

	.conn-protocol {
		font-size: 0.7rem;
		font-weight: 700;
		color: var(--bp-accent);
		letter-spacing: 0.08em;
	}

	.conn-detail {
		font-size: 0.55rem;
		color: var(--bp-muted);
		letter-spacing: 0.06em;
	}

	.conn-dimension {
		display: flex;
		align-items: center;
		gap: 0.3rem;
		margin-top: 0.5rem;
	}

	.dim-end-mark {
		width: 1px;
		height: 6px;
		background: var(--bp-muted);
	}

	.dim-measurement {
		font-size: 0.55rem;
		color: var(--bp-muted);
		letter-spacing: 0.06em;
	}

	/* Note callout box */
	.note-callout {
		border: 1px dashed rgba(0, 180, 255, 0.25);
		padding: 0.5rem 0.75rem;
		margin-top: 1.25rem;
		display: flex;
		gap: 0.5rem;
		align-items: baseline;
		font-size: 0.7rem;
	}

	.note-label {
		color: var(--bp-orange);
		font-weight: 700;
		letter-spacing: 0.06em;
		flex-shrink: 0;
	}

	.note-text {
		color: var(--bp-muted);
	}

	/* =========================================================
	   SPECIFICATIONS TABLE
	   ========================================================= */

	.specs-table-wrapper {
		overflow-x: auto;
		-webkit-overflow-scrolling: touch;
	}

	.specs-table {
		width: 100%;
		border-collapse: collapse;
		font-size: 0.8rem;
		min-width: 500px;
	}

	.specs-table th {
		text-align: left;
		padding: 0.5rem 0.75rem;
		border: 1px solid var(--bp-border);
		background: rgba(0, 180, 255, 0.06);
		font-size: 0.7rem;
		letter-spacing: 0.1em;
		color: var(--bp-accent);
		font-weight: 600;
	}

	.specs-table td {
		padding: 0.45rem 0.75rem;
		border: 1px solid rgba(0, 180, 255, 0.15);
		color: var(--bp-text);
	}

	.specs-table tbody tr:hover {
		background: rgba(0, 180, 255, 0.04);
	}

	.item-num {
		color: var(--bp-muted);
		text-align: center;
		width: 3rem;
	}

	.feature-name {
		font-weight: 600;
		letter-spacing: 0.04em;
		text-transform: uppercase;
		white-space: nowrap;
	}

	.status-cell {
		display: flex;
		align-items: center;
		gap: 0.4rem;
		color: #4ade80;
		font-weight: 600;
		font-size: 0.75rem;
		letter-spacing: 0.06em;
	}

	.status-dot {
		width: 6px;
		height: 6px;
		border-radius: 50%;
		background: #4ade80;
		flex-shrink: 0;
		animation: statusPulse 2.5s ease-in-out infinite;
	}

	@keyframes statusPulse {
		0%,
		100% {
			opacity: 1;
			box-shadow: 0 0 0 0 rgba(74, 222, 128, 0.4);
		}
		50% {
			opacity: 0.7;
			box-shadow: 0 0 6px 2px rgba(74, 222, 128, 0.2);
		}
	}

	.desc-cell {
		color: var(--bp-muted);
	}

	.spec-notes {
		margin-top: 0.75rem;
	}

	/* =========================================================
	   INSTALLATION PROCEDURE
	   ========================================================= */

	.procedure-steps {
		display: flex;
		flex-direction: column;
		gap: 0.5rem;
		margin-bottom: 1.5rem;
	}

	.procedure-step {
		display: grid;
		grid-template-columns: 20px auto auto 1fr;
		gap: 0.75rem;
		align-items: baseline;
		padding: 0.5rem 0;
		border-bottom: 1px solid rgba(0, 180, 255, 0.08);
	}

	.procedure-step:last-child {
		border-bottom: none;
	}

	.step-checkbox {
		width: 14px;
		height: 14px;
		border: 1.5px solid var(--bp-border);
		flex-shrink: 0;
		margin-top: 1px;
	}

	.step-number {
		font-size: 0.7rem;
		font-weight: 700;
		color: var(--bp-accent);
		letter-spacing: 0.08em;
		white-space: nowrap;
	}

	.step-ref {
		font-size: 0.6rem;
		color: var(--bp-muted);
		letter-spacing: 0.06em;
		white-space: nowrap;
	}

	.step-text {
		color: var(--bp-text);
		font-size: 0.8rem;
	}

	/* Configuration payload */
	.config-payload {
		border: 2px dashed rgba(0, 180, 255, 0.25);
		background: rgba(0, 180, 255, 0.02);
	}

	.payload-header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 0.5rem 0.75rem;
		border-bottom: 1px solid rgba(0, 180, 255, 0.15);
	}

	.payload-label {
		font-size: 0.7rem;
		font-weight: 700;
		color: var(--bp-accent);
		letter-spacing: 0.08em;
	}

	.payload-ref {
		font-size: 0.6rem;
		color: var(--bp-muted);
	}

	.payload-code {
		margin: 0;
		padding: 0.75rem 1rem;
		background: transparent;
		border: none;
		border-radius: 0;
		color: var(--bp-text);
		font-family: var(--bp-font);
		font-size: 0.8rem;
		line-height: 1.6;
		overflow-x: auto;
	}

	.payload-footer {
		padding: 0.4rem 0.75rem;
		border-top: 1px solid rgba(0, 180, 255, 0.15);
		display: flex;
		gap: 0.5rem;
		align-items: baseline;
		font-size: 0.65rem;
	}

	/* =========================================================
	   REVISION TABLE
	   ========================================================= */

	.revision-table {
		width: 100%;
		border-collapse: collapse;
		font-size: 0.75rem;
		margin-bottom: 1.5rem;
	}

	.revision-table th {
		text-align: left;
		padding: 0.4rem 0.75rem;
		border: 1px solid var(--bp-border);
		background: rgba(0, 180, 255, 0.06);
		font-size: 0.65rem;
		letter-spacing: 0.1em;
		color: var(--bp-accent);
	}

	.revision-table td {
		padding: 0.4rem 0.75rem;
		border: 1px solid rgba(0, 180, 255, 0.15);
		color: var(--bp-muted);
	}

	.rev-letter {
		font-weight: 700;
		color: var(--bp-text);
		text-align: center;
	}

	/* Blueprint footer */
	.blueprint-footer {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding-top: 1rem;
		border-top: 1px solid var(--bp-border);
	}

	.footer-left {
		display: flex;
		gap: 1.5rem;
	}

	.footer-item {
		font-size: 0.6rem;
		color: var(--bp-muted);
		letter-spacing: 0.1em;
		text-transform: uppercase;
	}

	/* =========================================================
	   RESPONSIVE
	   ========================================================= */

	@media (max-width: 768px) {
		.blueprint-content {
			padding: 2rem 1rem 3rem;
			gap: 2rem;
		}

		.title-name {
			font-size: 1.5rem;
		}

		.title-meta {
			flex-wrap: wrap;
		}

		.title-meta .title-field {
			flex: 1 1 45%;
		}

		/* Schematic goes vertical on mobile */
		.schematic-nodes {
			flex-direction: column;
			gap: 0;
		}

		.connection {
			transform: rotate(90deg);
			padding: 0.75rem 0;
			min-width: 60px;
		}

		.conn-labels,
		.conn-dimension {
			transform: rotate(-90deg);
		}

		.dimension-line-top {
			display: none;
		}

		/* Hide callouts on small screens */
		.node-callout-left,
		.node-callout-right {
			display: none;
		}

		.node-callout-bottom {
			position: relative;
			top: auto;
			left: auto;
			transform: none;
			margin-top: 0.5rem;
			flex-direction: row;
		}

		/* Procedure steps simplify */
		.procedure-step {
			grid-template-columns: 20px 1fr;
			grid-template-rows: auto auto;
		}

		.step-number {
			grid-column: 2;
		}

		.step-ref {
			display: none;
		}

		.step-text {
			grid-column: 1 / -1;
		}

		/* Registration marks closer */
		.reg-mark {
			font-size: 1rem;
		}

		.footer-left {
			flex-wrap: wrap;
			gap: 0.75rem;
		}

		/* Title stamp smaller */
		.stamp-circle {
			width: 48px;
			height: 48px;
			font-size: 0.5rem;
		}
	}

	@media (max-width: 480px) {
		.title-main {
			flex-direction: column;
			align-items: stretch;
		}

		.title-stamp {
			border-top: 1px solid rgba(0, 180, 255, 0.2);
			justify-content: flex-end;
			padding: 0.5rem 1rem;
		}

		.schematic-node .node-box {
			min-width: 100px;
			padding: 0.75rem 1rem;
		}

		.section-header {
			font-size: 0.7rem;
		}

		.section-header span:first-of-type {
			overflow: hidden;
			text-overflow: ellipsis;
			white-space: nowrap;
		}
	}
</style>
