<script>
	import NumberFlow from '@number-flow/svelte';

	/** Unsigned magnitude; shown as −mag and +mag side by side */
	let mag = $state(9);

	const MAX = Number.MAX_SAFE_INTEGER;

	function delta(d) {
		const n = mag + d;
		if (n < 0) mag = 0;
		else if (n > MAX) mag = MAX;
		else mag = n;
	}
</script>

<main>
	<h1>NumberFlow: negative vs positive (same magnitude)</h1>
	<p class="hint">
		This repo’s <code>number-flow</code> includes a <strong>patch</strong> for digit-wrap on negative values (see
		<code>getDelta</code> in <code>packages/number-flow/src/lite.ts</code>). With the fix, <strong>−mag</strong> and
		<strong>+mag</strong> should both use the same calm one-tick wraps when you step ±1 / ±10. On upstream
		<strong>0.6.x</strong>, the negative column spins wrong in <strong>both</strong> directions across 9↔0 (see
		<a href="https://github.com/barvian/number-flow/issues/184">barvian/number-flow#184</a> and the CodeSandbox in
		that issue for a build against npm).
	</p>

	<div class="controls">
		<span class="mag-label">Magnitude: <strong>{mag}</strong></span>
		<button type="button" onclick={() => delta(-1)} disabled={mag <= 0}>−1</button>
		<button type="button" onclick={() => delta(1)}>+1</button>
		<button type="button" class="secondary" onclick={() => delta(-10)} disabled={mag < 10}>−10</button>
		<button type="button" class="secondary" onclick={() => delta(10)}>+10</button>
	</div>

	<div class="pair">
		<section class="panel panel--neg" aria-label="Negative value">
			<h2>Negative (−mag)</h2>
			<p class="sub">Should match positive after patch (try +1 / −1 around 9↔10)</p>
			<p class="value"><NumberFlow value={-mag} /></p>
		</section>
		<section class="panel panel--pos" aria-label="Positive value">
			<h2>Positive (+mag)</h2>
			<p class="sub">Reference column</p>
			<p class="value"><NumberFlow value={mag} /></p>
		</section>
	</div>
</main>

<style>
	main {
		font-family: system-ui, sans-serif;
		max-width: 52rem;
		margin: 2rem auto;
		padding: 0 1rem;
	}
	.hint {
		color: #444;
		line-height: 1.5;
		margin-bottom: 1.25rem;
	}
	.hint a {
		color: inherit;
	}
	.controls {
		display: flex;
		flex-wrap: wrap;
		align-items: center;
		gap: 0.5rem 0.75rem;
		margin-bottom: 1.5rem;
		padding: 0.75rem 0;
		border-top: 1px solid #ddd;
		border-bottom: 1px solid #ddd;
	}
	.mag-label {
		margin-right: 0.5rem;
	}
	button {
		cursor: pointer;
		padding: 0.35rem 0.75rem;
		font-size: 1rem;
	}
	button:disabled {
		opacity: 0.45;
		cursor: not-allowed;
	}
	button.secondary {
		font-size: 0.9rem;
	}
	.pair {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 1rem;
	}
	@media (max-width: 640px) {
		.pair {
			grid-template-columns: 1fr;
		}
	}
	.panel {
		border: 1px solid #ddd;
		border-radius: 0.5rem;
		padding: 1rem 1.25rem;
	}
	.panel h2 {
		margin: 0 0 0.25rem;
		font-size: 1.1rem;
	}
	.sub {
		margin: 0 0 0.5rem;
		font-size: 0.85rem;
		color: #666;
	}
	.value {
		font-size: 2.25rem;
		font-weight: 600;
		margin: 0;
	}
</style>
