<script lang="ts">
	import CodeBlock from '$lib/components/CodeBlock.svelte';
	import { Settings, Database, Shield, Zap, ArrowLeft } from 'lucide-svelte';
</script>

<svelte:head>
	<title>Documentation - AnkiMCP</title>
	<meta
		name="description"
		content="Complete documentation for AnkiMCP - capabilities, configuration, and API reference"
	/>
</svelte:head>

<div class="docs-container">
	<nav class="docs-nav">
		<a href="/" class="back-link">
			<ArrowLeft class="h-4 w-4" />
			Back to Home
		</a>
		<h1 class="docs-nav-title">Documentation</h1>
		<ul class="nav-list">
			<li><a href="#overview">Overview</a></li>
			<li><a href="#installation">Installation</a></li>
			<li><a href="#configuration">Configuration</a></li>
			<li><a href="#capabilities">Capabilities</a></li>
			<li><a href="#api-reference">API Reference</a></li>
			<li><a href="#security">Security</a></li>
			<li><a href="#troubleshooting">Troubleshooting</a></li>
		</ul>
	</nav>

	<main class="docs-content">
		<section id="overview" class="section">
			<h2 class="section-heading">Overview</h2>
			<p class="mb-4">
				AnkiMCP is an Anki addon that bridges your flashcard collection with AI assistants through
				the Model Context Protocol (MCP). Unlike traditional REST APIs, MCP provides a standardized
				way for AI tools to access and interact with your data safely and efficiently.
			</p>

			<div class="feature-comparison mb-6">
				<h3 class="mb-4 text-xl font-semibold">AnkiMCP vs AnkiConnect</h3>
				<div class="grid gap-6 md:grid-cols-2">
					<div class="comparison-card">
						<h4 class="mb-2 font-semibold text-green-600">AnkiMCP Advantages</h4>
						<ul class="list-inside list-disc space-y-1 text-sm">
							<li>Built for AI assistant integration</li>
							<li>MCP protocol standardization</li>
							<li>Fine-grained permission system</li>
							<li>Automatic server lifecycle management</li>
							<li>Type-safe tool definitions</li>
						</ul>
					</div>
					<div class="comparison-card">
						<h4 class="mb-2 font-semibold text-blue-600">AnkiConnect Use Cases</h4>
						<ul class="list-inside list-disc space-y-1 text-sm">
							<li>Browser extensions (Yomichan, etc.)</li>
							<li>Direct HTTP API integration</li>
							<li>Legacy application support</li>
							<li>Custom automation scripts</li>
						</ul>
					</div>
				</div>
			</div>
		</section>

		<section id="installation" class="section">
			<h2 class="section-heading">Installation</h2>

			<div class="installation-methods">
				<div class="method-card">
					<h3 class="mb-3 text-xl font-semibold">Method 1: AnkiWeb (Recommended)</h3>
					<ol class="mb-4 list-inside list-decimal space-y-2">
						<li>Open Anki and go to <code>Tools → Add-ons → Get Add-ons...</code></li>
						<li>Enter the addon code: <code class="addon-code">1513864660</code></li>
						<li>Click OK and restart Anki</li>
						<li>The MCP server will start automatically when you open a profile</li>
					</ol>
					<div class="note">
						<p>
							<strong>Tip:</strong> You can also browse the addon listing at
							<a href="https://ankiweb.net/shared/info/1513864660" class="link"
								>ankiweb.net/shared/info/1513864660</a
							> to verify the code before installing.
						</p>
					</div>
				</div>

				<div class="method-card">
					<h3 class="mb-3 text-xl font-semibold">Method 2: Manual Installation</h3>
					<ol class="list-inside list-decimal space-y-2">
						<li>
							Download the latest release from <a
								href="https://github.com/shivros/ankimcp/releases"
								class="link">GitHub</a
							>
						</li>
						<li>Extract the archive</li>
						<li>
							Copy the <code>src/ankimcp</code> folder to your Anki addons directory:
							<ul class="mt-2 ml-6 list-inside list-disc space-y-1">
								<li><strong>Windows:</strong> <code>%APPDATA%\\Anki2\\addons21\\</code></li>
								<li>
									<strong>macOS:</strong> <code>~/Library/Application Support/Anki2/addons21/</code>
								</li>
								<li><strong>Linux:</strong> <code>~/.local/share/Anki2/addons21/</code></li>
							</ul>
						</li>
						<li>Restart Anki</li>
					</ol>
				</div>
			</div>
		</section>

		<section id="configuration" class="section">
			<h2 class="section-heading">Configuration</h2>

			<div class="config-section">
				<h3 class="mb-3 text-xl font-semibold">
					<Settings class="mr-2 inline h-5 w-5" />
					Anki Addon Settings
				</h3>
				<p class="mb-4">Configure AnkiMCP through Anki's addon configuration:</p>
				<ol class="mb-4 list-inside list-decimal space-y-2">
					<li>Go to <code>Tools → Add-ons</code></li>
					<li>Select AnkiMCP and click <code>Config</code></li>
					<li>Modify settings as needed</li>
				</ol>

				<div class="config-example">
					<h4 class="mb-2 font-semibold">Configuration Options</h4>
					<CodeBlock language="JSON" label="ankimcp.json">
						{`{
  "host": "localhost",
  "port": 4473,
  "mode": "denylist",
  "global_permissions": {
    "read": true,
    "write": true,
    "delete": false
  },
  "deck_allowlist": [],
  "deck_denylist": ["PrivateDeck"],
  "protected_decks": ["Default", "ImportantDeck"],
  "protected_tags": [],
  "readonly_tags": ["readonly"],
  "note_type_permissions": {
    "allow_create": true,
    "allow_modify": false,
    "allowed_types": []
  }
}`}
					</CodeBlock>
				</div>
			</div>

			<div class="config-section">
				<h3 class="mb-3 text-xl font-semibold">MCP Host Configuration</h3>
				<p class="mb-4">Configure your MCP host to connect to AnkiMCP:</p>

				<div class="host-configs">
					<div class="host-config">
						<h4 class="mb-2 font-semibold">Claude Desktop / Claude Code</h4>
						<p class="mb-2 text-sm">
							Add to your MCP configuration (e.g., <code>.mcp.json</code> or
							<code>claude_desktop_config.json</code>):
						</p>
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

					<div class="host-config">
						<h4 class="mb-2 font-semibold">Other MCP Hosts</h4>
						<p class="mb-2 text-sm">For custom MCP implementations, connect to:</p>
						<ul class="list-inside list-disc text-sm">
							<li><strong>URL:</strong> <code>http://localhost:4473/sse</code></li>
							<li><strong>Protocol:</strong> MCP over SSE (Server-Sent Events)</li>
						</ul>
					</div>
				</div>
			</div>
		</section>

		<section id="capabilities" class="section">
			<h2 class="section-heading">
				<Zap class="section-heading-icon" />
				Capabilities
			</h2>
			<p class="mb-6">
				AnkiMCP provides comprehensive access to your Anki collection through these MCP tools:
			</p>

			<div class="capabilities-grid">
				<div class="capability-card">
					<Database class="mb-3 h-8 w-8 text-blue-600" />
					<h3 class="mb-2 text-lg font-semibold">Deck Management</h3>
					<ul class="capability-list">
						<li><code>list_decks</code> - Get all available decks</li>
						<li><code>get_deck_info</code> - Get detailed deck information</li>
						<li><code>create_deck</code> - Create new decks</li>
						<li><code>update_deck</code> - Rename or update deck properties</li>
						<li><code>delete_deck</code> - Remove decks (with protection)</li>
					</ul>
				</div>

				<div class="capability-card">
					<Database class="mb-3 h-8 w-8 text-green-600" />
					<h3 class="mb-2 text-lg font-semibold">Note Operations</h3>
					<ul class="capability-list">
						<li><code>search_notes</code> - Search using Anki syntax</li>
						<li><code>get_note</code> - Get detailed note information</li>
						<li><code>get_cards_for_note</code> - Get cards for a note</li>
						<li><code>create_note</code> - Add new notes/cards</li>
						<li><code>update_note</code> - Modify existing notes</li>
						<li><code>delete_note</code> - Remove a note</li>
					</ul>
				</div>

				<div class="capability-card">
					<Database class="mb-3 h-8 w-8 text-purple-600" />
					<h3 class="mb-2 text-lg font-semibold">Note Types</h3>
					<ul class="capability-list">
						<li><code>list_note_types</code> - List available note types</li>
						<li><code>create_note_type</code> - Create custom note types</li>
					</ul>
				</div>

				<div class="capability-card">
					<Database class="mb-3 h-8 w-8 text-orange-600" />
					<h3 class="mb-2 text-lg font-semibold">Statistics & Settings</h3>
					<ul class="capability-list">
						<li><code>get_review_stats</code> - Learning progress data</li>
						<li><code>get_permissions</code> - View permission settings</li>
					</ul>
				</div>
			</div>
		</section>

		<section id="api-reference" class="section">
			<h2 class="section-heading">API Reference</h2>
			<p class="mb-6">Detailed reference for each MCP tool available in AnkiMCP:</p>

			<div class="api-reference">
				<div class="api-method">
					<h3 class="api-method-name">list_decks</h3>
					<p class="api-description">Returns all available decks with their IDs and card counts.</p>
					<div class="api-example">
						<h4>Response Example:</h4>
						<CodeBlock language="JSON" label="list_decks">
							{`[
  {
    "id": 1234567890,
    "name": "Japanese::Vocabulary",
    "card_count": 892,
    "is_filtered": false
  }
]`}
						</CodeBlock>
					</div>
				</div>

				<div class="api-method">
					<h3 class="api-method-name">search_notes</h3>
					<p class="api-description">Search notes using Anki's powerful search syntax.</p>
					<div class="api-parameters">
						<h4>Parameters:</h4>
						<ul>
							<li>
								<code>query</code> (string) - Anki search query (e.g., "deck:Japanese tag:vocabulary")
							</li>
							<li>
								<code>limit</code> (number, optional) - Maximum results to return (default: 50)
							</li>
						</ul>
					</div>
					<div class="api-example">
						<h4>Response Example:</h4>
						<CodeBlock language="JSON" label="search_notes">
							{`[
  {
    "id": 1234567890,
    "model_name": "Basic",
    "fields": {
      "Front": "こんにちは",
      "Back": "Hello"
    },
    "tags": ["vocabulary", "greetings"],
    "card_count": 1
  }
]`}
						</CodeBlock>
					</div>
				</div>

				<div class="api-method">
					<h3 class="api-method-name">create_note</h3>
					<p class="api-description">Create a new note in the specified deck.</p>
					<div class="api-parameters">
						<h4>Parameters:</h4>
						<ul>
							<li><code>deck_name</code> (string) - Target deck name</li>
							<li><code>model_name</code> (string) - Note type/model name</li>
							<li><code>fields</code> (object) - Field values as key-value pairs</li>
							<li><code>tags</code> (array, optional) - Tags to add to the note</li>
						</ul>
					</div>
					<div class="api-example">
						<h4>Response Example:</h4>
						<CodeBlock language="JSON" label="create_note">
							{`{
  "id": 1234567890,
  "model_name": "Basic",
  "fields": {
    "Front": "Hello",
    "Back": "World"
  },
  "tags": ["test"],
  "card_count": 1
}`}
						</CodeBlock>
					</div>
				</div>

				<div class="api-method">
					<h3 class="api-method-name">get_deck_info</h3>
					<p class="api-description">Get detailed information about a specific deck.</p>
					<div class="api-parameters">
						<h4>Parameters:</h4>
						<ul>
							<li><code>deck_name</code> (string) - Name of the deck</li>
						</ul>
					</div>
					<div class="api-example">
						<h4>Response Example:</h4>
						<CodeBlock language="JSON" label="get_deck_info">
							{`{
  "id": 1234567890,
  "name": "Japanese::Vocabulary",
  "card_count": 892,
  "new_count": 45,
  "learning_count": 12,
  "review_count": 100,
  "is_filtered": false
}`}
						</CodeBlock>
					</div>
				</div>

				<div class="api-method">
					<h3 class="api-method-name">update_deck</h3>
					<p class="api-description">Update a deck's properties (rename or set description).</p>
					<div class="api-parameters">
						<h4>Parameters:</h4>
						<ul>
							<li><code>deck_name</code> (string) - Current name of the deck</li>
							<li><code>new_name</code> (string, optional) - New name for the deck</li>
							<li><code>description</code> (string, optional) - New description</li>
						</ul>
					</div>
				</div>

				<div class="api-method">
					<h3 class="api-method-name">delete_deck</h3>
					<p class="api-description">
						Delete a deck and all its cards. Cannot delete protected decks.
					</p>
					<div class="api-parameters">
						<h4>Parameters:</h4>
						<ul>
							<li><code>deck_name</code> (string) - Name of the deck to delete</li>
						</ul>
					</div>
				</div>

				<div class="api-method">
					<h3 class="api-method-name">get_review_stats</h3>
					<p class="api-description">Get review statistics for a deck or the entire collection.</p>
					<div class="api-parameters">
						<h4>Parameters:</h4>
						<ul>
							<li><code>deck_name</code> (string, optional) - Deck name, or omit for all decks</li>
						</ul>
					</div>
					<div class="api-example">
						<h4>Response Example:</h4>
						<CodeBlock language="JSON" label="get_review_stats">
							{`{
  "deck_name": "All Decks",
  "total_cards": 5000,
  "new_cards": 1000,
  "learning_cards": 50,
  "review_cards": 500,
  "mature_cards": 3450
}`}
						</CodeBlock>
					</div>
				</div>

				<div class="api-method">
					<h3 class="api-method-name">list_note_types</h3>
					<p class="api-description">List all available note types (models) with their fields.</p>
					<div class="api-example">
						<h4>Response Example:</h4>
						<CodeBlock language="JSON" label="list_note_types">
							{`[
  {
    "id": 1234567890,
    "name": "Basic",
    "fields": ["Front", "Back"],
    "templates": ["Card 1"],
    "field_count": 2,
    "template_count": 1
  }
]`}
						</CodeBlock>
					</div>
				</div>

				<div class="api-method">
					<h3 class="api-method-name">get_permissions</h3>
					<p class="api-description">Get current permission settings.</p>
					<div class="api-example">
						<h4>Response Example:</h4>
						<CodeBlock language="JSON" label="get_permissions">
							{`{
  "mode": "denylist",
  "global_permissions": {
    "read": true,
    "write": true,
    "delete": true
  },
  "protected_decks": ["Default"],
  "deck_allowlist": [],
  "deck_denylist": []
}`}
						</CodeBlock>
					</div>
				</div>
			</div>
		</section>

		<section id="security" class="section">
			<h2 class="section-heading">
				<Shield class="section-heading-icon" />
				Security & Permissions
			</h2>
			<p class="mb-6">
				AnkiMCP includes comprehensive security features to protect your Anki collection:
			</p>

			<div class="security-features">
				<div class="security-feature">
					<h3 class="mb-2 text-lg font-semibold">Global Permissions</h3>
					<p class="mb-3">Set default permissions for all operations:</p>
					<ul class="list-inside list-disc space-y-1">
						<li><strong>Read:</strong> Allow viewing decks, notes, and statistics</li>
						<li><strong>Write:</strong> Allow creating and updating notes/decks</li>
						<li><strong>Delete:</strong> Allow removing notes and decks</li>
					</ul>
				</div>

				<div class="security-feature">
					<h3 class="mb-2 text-lg font-semibold">Deck Access Control</h3>
					<p class="mb-3">Control which decks are accessible using allowlist or denylist mode:</p>
					<CodeBlock language="JSON" label="deck_access" allowCopy={false}>
						{`"mode": "denylist",
"deck_allowlist": [],
"deck_denylist": ["PersonalDiary", "Private"]`}
					</CodeBlock>
					<p class="mt-2 text-sm text-gray-600">
						In <strong>denylist</strong> mode, all decks are accessible except those listed. In
						<strong>allowlist</strong> mode, only listed decks are accessible.
					</p>
				</div>

				<div class="security-feature">
					<h3 class="mb-2 text-lg font-semibold">Protected Decks</h3>
					<p class="mb-3">Add an extra layer of protection against accidental deletion:</p>
					<CodeBlock language="JSON" label="protected_decks" allowCopy={false}>
						{`"protected_decks": [
  "Default",
  "MedicalSchool"
]`}
					</CodeBlock>
					<p class="text-sm text-gray-600">
						Protected decks cannot be deleted even if delete permissions are enabled.
					</p>
				</div>

				<div class="security-feature">
					<h3 class="mb-2 text-lg font-semibold">Note Type Permissions</h3>
					<p class="mb-3">Control which note types can be used for creating notes:</p>
					<CodeBlock language="JSON" label="note_type_permissions" allowCopy={false}>
						{`"note_type_permissions": {
  "allow_create": true,
  "allow_modify": false,
  "allowed_types": []
}`}
					</CodeBlock>
					<p class="text-sm text-gray-600">
						When <code>allowed_types</code> is empty with denylist mode, all types are allowed.
					</p>
				</div>
			</div>
		</section>

		<section id="troubleshooting" class="section">
			<h2 class="section-heading">Troubleshooting</h2>

			<div class="troubleshooting-guide">
				<div class="issue">
					<h3 class="issue-title">MCP Server Not Starting</h3>
					<div class="issue-content">
						<p><strong>Symptoms:</strong> AI assistant can't connect to AnkiMCP</p>
						<p><strong>Solutions:</strong></p>
						<ul class="ml-4 list-inside list-disc space-y-1">
							<li>Ensure Anki is running with a profile loaded</li>
							<li>Check that the addon is enabled in Tools → Add-ons</li>
							<li>Verify port 4473 is not blocked by firewall</li>
							<li>Restart Anki and try again</li>
						</ul>
					</div>
				</div>

				<div class="issue">
					<h3 class="issue-title">Permission Denied Errors</h3>
					<div class="issue-content">
						<p>
							<strong>Symptoms:</strong> "Permission denied" when AI tries to create/modify content
						</p>
						<p><strong>Solutions:</strong></p>
						<ul class="ml-4 list-inside list-disc space-y-1">
							<li>Check addon configuration for global permissions</li>
							<li>Verify deck-specific permissions are not too restrictive</li>
							<li>Ensure the target deck isn't in the protected list</li>
						</ul>
					</div>
				</div>

				<div class="issue">
					<h3 class="issue-title">Connection Refused</h3>
					<div class="issue-content">
						<p>
							<strong>Symptoms:</strong> MCP host can't connect to
							<code>http://localhost:4473/sse</code>
						</p>
						<p><strong>Solutions:</strong></p>
						<ul class="ml-4 list-inside list-disc space-y-1">
							<li>Check if another application is using port 4473</li>
							<li>
								Verify the SSE endpoint is accessible: <code>curl http://localhost:4473/health</code
								>
							</li>
							<li>Try changing the port in addon configuration</li>
							<li>Check system firewall settings</li>
						</ul>
					</div>
				</div>
			</div>

			<div class="help-section">
				<h3 class="mb-2 text-lg font-semibold">Getting Help</h3>
				<p class="mb-3">If you're still experiencing issues:</p>
				<ul class="list-inside list-disc space-y-1">
					<li>
						Check the <a href="https://github.com/shivros/ankimcp/issues" class="link"
							>GitHub issues</a
						> for similar problems
					</li>
					<li>Create a new issue with detailed error messages and your configuration</li>
					<li>Include your Anki version and operating system</li>
				</ul>
			</div>
		</section>
	</main>
</div>

<style>
	.docs-container {
		--docs-surface: color-mix(in srgb, var(--color-bg-2) 95%, transparent);
		--docs-surface-elevated: color-mix(in srgb, var(--color-bg-2) 88%, var(--color-bg-1) 12%);
		--docs-code-surface: color-mix(in srgb, var(--color-bg-2) 80%, #000 20%);
		--docs-border: color-mix(in srgb, var(--color-text) 12%, transparent);
		--docs-border-strong: color-mix(in srgb, var(--color-text) 16%, transparent);
		--docs-muted: color-mix(in srgb, var(--color-text) 70%, transparent);
		--code-surface: color-mix(in srgb, var(--docs-surface) 96%, transparent);
		--code-border: var(--docs-border-strong);
		--code-header: color-mix(in srgb, var(--docs-surface-elevated) 94%, transparent);
		--code-accent: var(--color-theme-1);
		display: grid;
		grid-template-columns: minmax(0, 260px) 1fr;
		gap: 2rem;
		max-width: 1200px;
		margin: 0 auto;
		padding: 2rem 2rem 4rem;
	}

	:global([data-theme='dark']) .docs-container {
		--docs-surface: color-mix(in srgb, var(--color-bg-2) 88%, #000 12%);
		--docs-surface-elevated: color-mix(in srgb, var(--color-bg-1) 88%, #000 12%);
		--docs-code-surface: color-mix(in srgb, var(--color-bg-1) 78%, #000 22%);
		--docs-border: color-mix(in srgb, var(--color-text) 18%, transparent);
		--docs-border-strong: color-mix(in srgb, var(--color-text) 24%, transparent);
		--docs-muted: color-mix(in srgb, var(--color-text) 65%, transparent);
		--code-surface: color-mix(in srgb, var(--docs-surface) 90%, #000 10%);
		--code-border: color-mix(in srgb, var(--color-text) 28%, transparent);
		--code-header: color-mix(in srgb, var(--docs-surface-elevated) 88%, #000 12%);
		--code-accent: color-mix(in srgb, var(--color-theme-2) 60%, var(--color-theme-1));
	}

	.docs-nav {
		position: sticky;
		top: 5.5rem;
		align-self: start;
		background: var(--docs-surface-elevated);
		border: 1px solid var(--docs-border);
		border-radius: 0.875rem;
		padding: 1.5rem;
		box-shadow: 0 18px 45px color-mix(in srgb, #000 12%, transparent);
		display: grid;
		gap: 1.25rem;
	}

	:global([data-theme='dark']) .docs-nav {
		box-shadow: 0 18px 45px color-mix(in srgb, #000 32%, transparent);
	}

	.docs-nav-title {
		font-size: 1.5rem;
		font-weight: 700;
		color: color-mix(in srgb, var(--color-text) 94%, transparent);
		margin: 0;
		line-height: 1.25;
		text-wrap: balance;
		overflow-wrap: anywhere;
	}

	.back-link {
		display: inline-flex;
		align-items: center;
		gap: 0.5rem;
		color: var(--docs-muted);
		text-decoration: none;
		font-size: 0.875rem;
		font-weight: 600;
		transition: color 0.2s ease;
	}

	.back-link:hover {
		color: var(--color-theme-1);
	}

	.nav-list {
		list-style: none;
		padding: 0;
		margin: 0;
		display: grid;
		gap: 0.35rem;
	}

	.nav-list a {
		color: var(--docs-muted);
		text-decoration: none;
		padding: 0.35rem 0.5rem;
		display: block;
		border-radius: 0.5rem;
		font-weight: 600;
		transition:
			color 0.2s ease,
			background 0.2s ease;
	}

	.nav-list a:hover,
	.nav-list a:focus-visible {
		color: var(--color-theme-1);
		background: color-mix(in srgb, var(--color-theme-2) 18%, transparent);
	}

	.docs-content {
		display: flex;
		flex-direction: column;
		gap: 3rem;
	}

	.section {
		margin-bottom: 0;
		padding-bottom: 2.5rem;
		border-bottom: 1px solid var(--docs-border);
		scroll-margin-top: 6.75rem;
	}

	.section:last-child {
		border-bottom: none;
	}

	.section-heading {
		display: flex;
		align-items: center;
		gap: 0.75rem;
		font-size: clamp(1.9rem, 2vw + 1.3rem, 2.4rem);
		font-weight: 700;
		margin: 0 0 1.75rem 0;
		letter-spacing: -0.01em;
		color: color-mix(in srgb, var(--color-text) 92%, var(--color-theme-1) 8%);
	}

	.section-heading::after {
		content: '';
		flex: 1;
		height: 2px;
		background: linear-gradient(
			to right,
			color-mix(in srgb, var(--color-theme-1) 55%, transparent),
			transparent
		);
		border-radius: 999px;
		margin-left: 1rem;
		min-width: 3rem;
	}

	.section-heading-icon {
		width: 1.85rem;
		height: 1.85rem;
		flex-shrink: 0;
		color: var(--color-theme-1);
	}

	.security-features {
		display: grid;
		gap: 1.5rem;
		grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
	}

	.feature-comparison,
	.method-card,
	.host-config,
	.capability-card,
	.api-method,
	.security-feature,
	.help-section {
		background: var(--docs-surface);
		border: 1px solid var(--docs-border);
		border-radius: 0.9rem;
		padding: 1.5rem;
		box-shadow: 0 18px 38px color-mix(in srgb, #000 10%, transparent);
	}

	.comparison-card {
		background: var(--docs-surface-elevated);
		border: 1px solid var(--docs-border);
		border-radius: 0.75rem;
		padding: 1.5rem;
		display: flex;
		flex-direction: column;
		gap: 0.75rem;
	}

	.installation-methods {
		display: grid;
		gap: 1.5rem;
		grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
	}

	.addon-code {
		background: color-mix(in srgb, var(--color-theme-2) 16%, var(--docs-surface));
		color: color-mix(in srgb, var(--color-theme-2) 70%, white);
		padding: 0.35rem 0.75rem;
		border-radius: 0.5rem;
		font-weight: 600;
		letter-spacing: 0.02em;
	}

	.note {
		background: color-mix(in srgb, #fbbf24 18%, var(--docs-surface));
		border: 1px solid color-mix(in srgb, #f59e0b 40%, var(--docs-border));
		padding: 1rem 1.25rem;
		border-radius: 0.75rem;
	}

	.config-section {
		display: flex;
		flex-direction: column;
		gap: 1.25rem;
	}

	.config-section + .config-section {
		margin-top: 2.5rem;
	}

	.config-example {
		background: var(--docs-surface-elevated);
		border: 1px solid var(--docs-border);
		border-radius: 0.75rem;
		padding: 1.25rem;
	}

	.host-configs {
		display: grid;
		gap: 1.25rem;
	}

	.capabilities-grid {
		display: grid;
		gap: 1.5rem;
		grid-template-columns: minmax(0, 1fr);
	}

	.capability-card {
		box-shadow: 0 20px 45px color-mix(in srgb, #000 8%, transparent);
	}

	.capability-list {
		list-style: none;
		padding: 0;
		margin: 0;
		display: grid;
		gap: 0.75rem;
	}

	.capability-list li {
		font-size: 0.95rem;
		border-bottom: 1px solid var(--docs-border);
		padding-bottom: 0.75rem;
	}

	.capability-list li:last-child {
		border-bottom: none;
		padding-bottom: 0;
	}

	.capability-list code {
		background: color-mix(in srgb, var(--color-theme-2) 14%, transparent);
		color: color-mix(in srgb, var(--color-theme-2) 70%, var(--color-text));
		padding: 0.15rem 0.45rem;
		border-radius: 999px;
		font-weight: 600;
	}

	.api-reference {
		display: grid;
		gap: 2rem;
	}

	.api-method-name {
		font-family: var(--font-mono);
		font-size: 1.25rem;
		font-weight: 700;
		color: var(--color-theme-1);
		margin-bottom: 0.5rem;
	}

	.api-description {
		color: var(--docs-muted);
		margin-bottom: 1rem;
	}

	.api-parameters h4,
	.api-example h4 {
		font-weight: 600;
		margin-bottom: 0.5rem;
		font-size: 0.9rem;
		color: color-mix(in srgb, var(--color-text) 78%, transparent);
	}

	.api-parameters ul {
		list-style: none;
		padding: 0;
		margin: 0 0 1rem 0;
		display: grid;
		gap: 0.5rem;
	}

	.api-parameters li {
		font-size: 0.95rem;
		color: var(--docs-muted);
	}

	.troubleshooting-guide {
		display: grid;
		gap: 1.5rem;
	}

	.issue {
		border: 1px solid color-mix(in srgb, #ef4444 35%, var(--docs-border));
		border-radius: 0.9rem;
		background: color-mix(in srgb, #ef4444 10%, var(--docs-surface));
		overflow: hidden;
	}

	.issue-title {
		background: color-mix(in srgb, #ef4444 14%, var(--docs-surface-elevated));
		padding: 1rem 1.5rem;
		margin: 0;
		font-size: 1.1rem;
		font-weight: 600;
		color: color-mix(in srgb, #ef4444 70%, var(--color-text));
		border-bottom: 1px solid color-mix(in srgb, #ef4444 25%, var(--docs-border));
	}

	.issue-content {
		padding: 1.25rem 1.5rem 1.5rem;
		display: grid;
		gap: 0.75rem;
	}

	.help-section {
		background: color-mix(in srgb, var(--color-theme-2) 12%, var(--docs-surface));
		border: 1px solid color-mix(in srgb, var(--color-theme-2) 30%, var(--docs-border));
		margin-top: 1.75rem;
	}

	.link {
		color: var(--color-theme-1);
		text-decoration: none;
		font-weight: 600;
	}

	.link:hover,
	.link:focus-visible {
		text-decoration: underline;
	}

	code {
		background: color-mix(in srgb, var(--color-bg-2) 80%, transparent);
		padding: 0.15rem 0.45rem;
		border-radius: 0.5rem;
		font-family: var(--font-mono);
		font-size: 0.9em;
	}

	@media (min-width: 900px) {
		.capabilities-grid {
			grid-template-columns: repeat(2, minmax(0, 1fr));
		}
	}

	@media (max-width: 1100px) {
		.docs-container {
			grid-template-columns: minmax(0, 220px) 1fr;
		}
	}

	@media (max-width: 900px) {
		.docs-container {
			grid-template-columns: 1fr;
			padding: 1.75rem 1.25rem 3rem;
			gap: 1.5rem;
		}

		.docs-nav {
			position: relative;
			top: 0;
			display: flex;
			flex-direction: column;
			gap: 1.25rem;
			box-shadow: 0 12px 26px color-mix(in srgb, #000 12%, transparent);
		}
	}

	@media (max-width: 640px) {
		.docs-container {
			padding: 1.25rem 1rem 2.5rem;
		}

		.capabilities-grid,
		.installation-methods {
			grid-template-columns: 1fr;
		}

		.section-heading {
			flex-direction: column;
			align-items: flex-start;
			gap: 0.5rem;
		}

		.section-heading::after {
			margin-left: 0;
			width: 100%;
		}
	}
</style>
