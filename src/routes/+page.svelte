<script lang="ts">
	import CodeBlock from '$lib/components/CodeBlock.svelte';
	import { CheckCircle, Download, Code, Shield } from 'lucide-svelte';
</script>

<svelte:head>
	<title>AnkiMCP - AI-Powered Anki Integration</title>
	<meta
		name="description"
		content="Connect your Anki collection to AI assistants through the Model Context Protocol (MCP)"
	/>
</svelte:head>

<section class="hero">
	<div class="container">
		<h1 class="mb-6 text-5xl font-bold">AnkiMCP</h1>
		<p class="mx-auto mb-8 max-w-2xl text-xl text-gray-600">
			Expose your Anki collection to AI assistants via MCP, enabling them to help you study, create
			cards, and analyze your learning progress.
		</p>
		<div class="flex justify-center gap-4">
			<a href="#quickstart" class="btn btn-primary">Get Started</a>
			<a href="/docs" class="btn btn-secondary">Documentation</a>
		</div>
	</div>
</section>

<section class="features">
	<div class="container">
		<h2 class="features-heading">Why AnkiMCP?</h2>
		<div class="grid gap-8 md:grid-cols-3">
			<div class="feature-card">
				<Code class="mb-4 h-12 w-12 text-blue-600" />
				<h3 class="mb-2 text-xl font-semibold">MCP Protocol</h3>
				<p class="text-gray-600">
					Built on the Model Context Protocol, providing standardized AI integration with Claude
					Desktop and other MCP hosts.
				</p>
			</div>
			<div class="feature-card">
				<Shield class="mb-4 h-12 w-12 text-green-600" />
				<h3 class="mb-2 text-xl font-semibold">Permission Control</h3>
				<p class="text-gray-600">
					Fine-grained permissions let you control exactly what AI assistants can read, write, or
					modify in your collection.
				</p>
			</div>
			<div class="feature-card">
				<CheckCircle class="mb-4 h-12 w-12 text-purple-600" />
				<h3 class="mb-2 text-xl font-semibold">Full API Coverage</h3>
				<p class="text-gray-600">
					Access decks, notes, cards, and review statistics. Create, update, and manage your Anki
					content programmatically.
				</p>
			</div>
		</div>
	</div>
</section>

<section id="quickstart" class="quickstart">
	<div class="container">
		<h2 class="mb-12 text-center text-3xl font-bold">Quick Start</h2>

		<div class="quickstart-steps">
			<div class="step">
				<div class="step-number">1</div>
				<h3 class="mb-3 text-xl font-semibold">Install AnkiMCP</h3>
				<div class="prose prose-gray max-w-none">
					<p class="mb-4">Install the addon in Anki using one of these methods:</p>

					<div class="mb-4 rounded-lg bg-gray-50 p-4">
						<h4 class="mb-2 font-semibold">Option A: From AnkiWeb</h4>
						<ol class="list-inside list-decimal space-y-2">
							<li>In Anki, go to <code>Tools → Add-ons → Get Add-ons...</code></li>
							<li>
								Enter code: <code class="rounded bg-gray-200 px-2 py-1 font-mono">1513864660</code>
							</li>
							<li>Click OK and restart Anki</li>
						</ol>
					</div>

					<div class="rounded-lg bg-gray-50 p-4">
						<h4 class="mb-2 font-semibold">Option B: From GitHub</h4>
						<ol class="list-inside list-decimal space-y-2">
							<li>
								Download from <a
									href="https://github.com/matt-fff/ankimcp"
									class="text-blue-600 hover:underline">GitHub</a
								>
							</li>
							<li>Copy <code>src/ankimcp</code> to your Anki addons folder</li>
							<li>Restart Anki</li>
						</ol>
					</div>
				</div>
			</div>

			<div class="step">
				<div class="step-number">2</div>
				<h3 class="mb-3 text-xl font-semibold">Configure MCP Host</h3>
				<div class="prose prose-gray max-w-none">
					<p class="mb-4">Add AnkiMCP to your MCP host configuration:</p>

					<div class="mb-4 rounded-lg bg-gray-50 p-4">
						<h4 class="mb-2 font-semibold">For Claude Desktop / Claude Code</h4>
						<p class="mb-2">Add to your MCP configuration:</p>
						<CodeBlock language="JSON" label="mcp.json">
							{`{
  "mcpServers": {
    "ankimcp": {
      "type": "sse",
      "url": "http://localhost:4473/sse"
    }
  }
}`}
						</CodeBlock>
					</div>
				</div>
			</div>

			<div class="step">
				<div class="step-number">3</div>
				<h3 class="mb-3 text-xl font-semibold">Start Using</h3>
				<div class="prose prose-gray max-w-none">
					<p class="mb-4">
						The MCP server starts automatically when you open an Anki profile. Your AI assistant can
						now:
					</p>
					<ul class="list-inside list-disc space-y-2">
						<li>List and search your decks</li>
						<li>Create new cards from conversations</li>
						<li>Review your learning statistics</li>
						<li>Update existing notes</li>
					</ul>

					<div class="mt-4 rounded-lg bg-blue-50 p-4">
						<p class="text-sm">
							<strong>Try it:</strong> Ask Claude "What Anki decks do I have?" or "Create a flashcard
							for [topic]"
						</p>
					</div>
				</div>
			</div>
		</div>
	</div>
</section>

<section class="cta">
	<div class="container text-center">
		<h2 class="mb-4 text-3xl font-bold">Ready to enhance your Anki experience?</h2>
		<p class="mb-8 text-xl text-gray-600">
			Join the growing community using AI to supercharge their spaced repetition learning.
		</p>
		<div class="flex justify-center gap-4">
			<a href="https://github.com/matt-fff/ankimcp" class="btn btn-primary">
				<Download class="mr-2 h-5 w-5" />
				View on GitHub
			</a>
			<a href="/docs" class="btn btn-secondary">Read the Docs</a>
		</div>
	</div>
</section>

<style>
	.hero {
		padding: 4rem 0;
		background:
			linear-gradient(
				135deg,
				color-mix(in srgb, var(--color-bg-1) 96%, transparent) 0%,
				color-mix(in srgb, var(--color-bg-2) 90%, transparent) 60%,
				color-mix(in srgb, var(--color-bg-2) 86%, transparent) 100%
			),
			radial-gradient(
				120% 120% at 15% 10%,
				color-mix(in srgb, var(--color-theme-1) 6%, transparent),
				transparent 60%
			);
	}

	:global([data-theme='dark']) .hero {
		background:
			linear-gradient(
				140deg,
				color-mix(in srgb, var(--color-bg-1) 92%, transparent) 0%,
				color-mix(in srgb, var(--color-bg-2) 88%, transparent) 55%,
				color-mix(in srgb, var(--color-bg-2) 84%, transparent) 100%
			),
			radial-gradient(
				130% 120% at 22% 18%,
				color-mix(in srgb, var(--color-theme-2) 10%, transparent),
				transparent 65%
			);
	}

	.container {
		max-width: 1200px;
		margin: 0 auto;
		padding: 0 1rem;
	}

	.btn {
		display: inline-flex;
		align-items: center;
		padding: 0.75rem 1.5rem;
		border-radius: 0.5rem;
		font-weight: 600;
		text-decoration: none;
		transition: all 0.2s;
	}

	.btn-primary {
		background: #3b82f6;
		color: white;
	}

	.btn-primary:hover {
		background: #2563eb;
	}

	.btn-secondary {
		background: #f3f4f6;
		color: #374151;
		border: 1px solid #e5e7eb;
	}

	.btn-secondary:hover {
		background: #e5e7eb;
	}

	.features {
		padding: 4rem 0;
		background: var(--color-bg-1);
	}

	.features-heading {
		position: relative;
		display: block;
		margin: 0 auto 3rem;
		padding-bottom: 0.75rem;
		font-size: clamp(2.1rem, 1.5vw + 1.9rem, 3rem);
		font-weight: 700;
		letter-spacing: -0.015em;
		text-align: center;
	}

	.features-heading::after {
		content: '';
		position: absolute;
		left: 50%;
		bottom: 0;
		transform: translateX(-50%);
		width: clamp(120px, 22vw, 220px);
		height: 0.25rem;
		border-radius: 999px;
		background: linear-gradient(
			90deg,
			color-mix(in srgb, var(--color-theme-1) 70%, transparent),
			transparent 80%
		);
	}

	.feature-card {
		background: var(--color-bg-2);
		padding: 2rem;
		border-radius: 0.75rem;
		box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
		border: 1px solid rgba(255, 255, 255, 0.1);
	}

	.quickstart {
		padding: 4rem 0;
	}

	.quickstart-steps {
		max-width: 800px;
		margin: 0 auto;
	}

	.step {
		position: relative;
		margin-bottom: 3rem;
		padding-left: 4rem;
	}

	.step-number {
		position: absolute;
		left: 0;
		top: 0;
		width: 3rem;
		height: 3rem;
		background: #3b82f6;
		color: white;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
		font-weight: bold;
		font-size: 1.25rem;
	}

	code {
		font-family: 'Courier New', monospace;
		font-size: 0.875em;
	}

	.cta {
		padding: 4rem 0;
		background: var(--color-bg-1);
	}
</style>
