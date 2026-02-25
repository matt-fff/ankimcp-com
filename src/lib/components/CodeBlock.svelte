<script lang="ts" module>
	function highlightJson(raw: string): string {
		return raw
			.replace(/&/g, '&amp;')
			.replace(/</g, '&lt;')
			.replace(/>/g, '&gt;')
			.replace(
				/"([^"\\]*(\\.[^"\\]*)*)"(\s*:)/g,
				'<span class="hl-key">"$1"</span>$3'
			)
			.replace(
				/:\s*"([^"\\]*(\\.[^"\\]*)*)"/g,
				(match) => {
					const colonPart = match.slice(0, match.indexOf('"'));
					const value = match.slice(match.indexOf('"'));
					return `${colonPart}<span class="hl-string">${value}</span>`;
				}
			)
			.replace(
				/:\s*(true|false)/g,
				': <span class="hl-bool">$1</span>'
			)
			.replace(
				/:\s*(\d+(?:\.\d+)?)/g,
				': <span class="hl-number">$1</span>'
			)
			.replace(
				/:\s*(null)/g,
				': <span class="hl-null">$1</span>'
			)
			.replace(
				/([{}[\]])/g,
				'<span class="hl-brace">$1</span>'
			);
	}
</script>

<script lang="ts">
	import { Copy, Check } from 'lucide-svelte';

	let {
		label = '',
		language = '',
		allowCopy = true,
		children
	} = $props<{
		label?: string;
		language?: string;
		allowCopy?: boolean;
		children?: any;
	}>();

	let copied = $state(false);
	let codeEl = $state<HTMLElement | undefined>(undefined);
	let copyTimer: ReturnType<typeof setTimeout> | undefined;

	async function handleCopy() {
		if (!codeEl) return;
		const text = codeEl.innerText.trimEnd();
		try {
			await navigator.clipboard.writeText(text);
			copied = true;
			clearTimeout(copyTimer);
			copyTimer = setTimeout(() => {
				copied = false;
			}, 2000);
		} catch (error) {
			console.error('Failed to copy code', error);
		}
	}

	let highlighted = $derived.by(() => {
		if (!codeEl) return '';
		const raw = codeEl.innerText;
		if (!language) return '';
		if (language.toLowerCase() === 'json') {
			return highlightJson(raw);
		}
		return '';
	});

	$effect(() => {
		if (highlighted && codeEl) {
			codeEl.innerHTML = highlighted;
		}
	});

	$effect(() => {
		return () => clearTimeout(copyTimer);
	});
</script>

<div class="code-block" data-language={language ? language.toLowerCase() : null}>
	{#if label || language || allowCopy}
		<div class="code-block__meta">
			<div class="code-block__info">
				<div class="titlebar-dots">
					<span class="dot dot-red"></span>
					<span class="dot dot-yellow"></span>
					<span class="dot dot-green"></span>
				</div>
				{#if label}
					<span class="code-block__label">{label}</span>
				{/if}
				{#if language}
					<span class="code-block__language">{language}</span>
				{/if}
			</div>
			{#if allowCopy}
				<button
					type="button"
					class="code-block__copy"
					class:copied
					onclick={handleCopy}
					aria-live="polite"
				>
					{#if copied}
						<Check size={13} />
						Copied
					{:else}
						<Copy size={13} />
						Copy
					{/if}
				</button>
			{/if}
		</div>
	{/if}
	<div class="code-block__scroll">
		<pre><code bind:this={codeEl}>{@render children?.()}</code></pre>
	</div>
</div>

<style>
	.code-block {
		border: 1px solid rgba(255, 255, 255, 0.08);
		border-radius: 0.9rem;
		background: rgba(255, 255, 255, 0.04);
		backdrop-filter: blur(12px);
		-webkit-backdrop-filter: blur(12px);
		overflow: hidden;
		display: flex;
		flex-direction: column;
	}

	.code-block__meta {
		display: flex;
		justify-content: space-between;
		align-items: center;
		gap: 1rem;
		padding: 0.65rem 1rem;
		background: rgba(255, 255, 255, 0.03);
		border-bottom: 1px solid rgba(255, 255, 255, 0.06);
		font-size: 0.85rem;
	}

	.code-block__info {
		display: flex;
		align-items: center;
		gap: 0.75rem;
		flex-wrap: wrap;
	}

	.titlebar-dots {
		display: flex;
		gap: 6px;
	}

	.dot {
		width: 10px;
		height: 10px;
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

	.code-block__label {
		font-weight: 600;
		font-size: 0.8rem;
		font-family: var(--font-mono);
		color: rgba(255, 255, 255, 0.4);
		letter-spacing: 0.02em;
	}

	.code-block__language {
		font-family: var(--font-mono);
		font-size: 0.7rem;
		letter-spacing: 0.08em;
		text-transform: uppercase;
		background: rgba(129, 140, 248, 0.12);
		border: 1px solid rgba(129, 140, 248, 0.2);
		color: rgba(129, 140, 248, 0.8);
		border-radius: 999px;
		padding: 0.1rem 0.45rem;
	}

	.code-block__copy {
		display: inline-flex;
		align-items: center;
		gap: 0.3rem;
		border: 1px solid rgba(255, 255, 255, 0.1);
		border-radius: 0.4rem;
		background: rgba(255, 255, 255, 0.06);
		color: rgba(255, 255, 255, 0.5);
		font-size: 0.72rem;
		font-weight: 600;
		padding: 0.2rem 0.6rem;
		cursor: pointer;
		transition: all 0.2s ease;
	}

	.code-block__copy:hover,
	.code-block__copy:focus-visible {
		background: rgba(255, 255, 255, 0.12);
		color: rgba(255, 255, 255, 0.85);
		border-color: rgba(255, 255, 255, 0.18);
		outline: none;
	}

	.code-block__copy.copied {
		background: rgba(34, 197, 94, 0.15);
		border-color: rgba(34, 197, 94, 0.25);
		color: #86efac;
	}

	.code-block__scroll {
		overflow-x: auto;
	}

	pre {
		margin: 0;
		padding: 1rem 1.25rem;
		font-family: var(--font-mono);
		font-size: 0.88rem;
		line-height: 1.65;
		color: rgba(255, 255, 255, 0.8);
		background: transparent;
		border: none;
		border-radius: 0;
		box-shadow: none;
	}

	code {
		display: block;
		white-space: pre;
	}

	/* Syntax highlighting tokens */
	:global(.hl-key) {
		color: #67e8f9;
	}

	:global(.hl-string) {
		color: #86efac;
	}

	:global(.hl-bool) {
		color: #c4b5fd;
	}

	:global(.hl-number) {
		color: #fbbf24;
	}

	:global(.hl-null) {
		color: #f87171;
		font-style: italic;
	}

	:global(.hl-brace) {
		color: rgba(255, 255, 255, 0.5);
	}
</style>
