<script lang="ts">
	import type { Component } from 'svelte';
	import * as Icons from '$lib/phosphor';
	import { TEMPLATES } from '$lib/core/templates.svelte';
	import autoAnimate from '@formkit/auto-animate';
	import FileCopy from '$lib/flowbite/icons/FileCopy.svelte'
	import EasingControl from '$lib/components/EasingControl.svelte';

	const ICONS = Icons as unknown as Record<string, Component>;
	const VARIANTS = ['regular', 'light', 'fill'];
	const allIcons = Object.keys(ICONS);

	function variantKind(v: string): 'fill' | 'line' {
		return v === 'fill' ? 'fill' : 'line';
	}

	let template = $state('draw');
	let size = $state(28);
	let trigger = $state<'hover' | 'mount'>('hover');
	let loop = $state(false);
	let speed = $state(1);
	let easing = $state<string | null>(null);
	let variant = $state('regular');
	let search = $state('');
	let limit = $state(120);
	let selected = $state<string | null>(null);
	let copied = $state(false);
	let usageBar = $state<HTMLElement | undefined>();

	const filtered = $derived(
		search.trim()
			? allIcons.filter((n) => n.toLowerCase().includes(search.trim().toLowerCase()))
			: allIcons
	);
	const shown = $derived(filtered.slice(0, limit));

	const templatesForVariant = $derived(
		TEMPLATES.filter((t) => !t.for || t.for === variantKind(variant))
	);

	$effect(() => {
		if (!templatesForVariant.some((t) => t.id === template)) {
			template = templatesForVariant[0]?.id ?? 'pop';
		}
	});

	$effect(() => {
		search;
		variant;
		limit = 120;
		selected = null;
	});

	$effect(() => {
		function onWindowClick(e: MouseEvent) {
			if (!selected) return;
			if (usageBar?.contains(e.target as Node)) return;
			selected = null;
		}
		window.addEventListener('click', onWindowClick);
		return () => window.removeEventListener('click', onWindowClick);
	});

	$effect(() => {
		function onKeydown(e: KeyboardEvent) {
			if (e.key === 'Escape' && selected) selected = null;
		}
		window.addEventListener('keydown', onKeydown);
		return () => window.removeEventListener('keydown', onKeydown);
	});

	function usageSnippet(icon: string): string {
		const v = variant === 'regular' ? '' : ` variant="${variant}"`;
		const tr = trigger === 'mount' ? ' trigger="mount"' : '';
		const sp = speed !== 1 ? ` speed={${speed}}` : '';
		const ez = easing ? ` easing="${easing}"` : '';
		const l = loop ? ' loop' : '';
		return `<${icon} template="${template}"${v}${tr}${sp}${ez} size={${size}}${l} />`;
	}

	async function copyUsage(): Promise<void> {
		if (!selected) return;
		await navigator.clipboard.writeText(usageSnippet(selected));
		copied = true;
		setTimeout(() => (copied = false), 1500);
	}
</script>

<svelte:head>
	<title>Phosphor · svelte-animated-icon</title>
</svelte:head>

<div class="page-header">
	<h1 class="ls-tightx">Phosphor</h1>
	<p class="col2 text-md">
		{allIcons.length} icons · regular, light, and fill variants · hover to animate.
	</p>
</div>
<section class="display-grid">
	<div class="controls box gap16">
		<div class="usage-bar box" class:active={selected} bind:this={usageBar} use:autoAnimate>
			{#if selected}
			<div class="row gap8 ycenter xright">
				<button class="icon-btn" onclick={copyUsage}>
					<FileCopy template="draw" speed={2.25} easing="cubic-bezier(0.32, 0, 0.67, 0)" size={20} />
					<span class="col-theme">{copied ? 'Copied!' : ''}</span>
				</button>
			</div>
			<pre class="language-svelte"><code class="language-svelte"
					>{usageSnippet(selected)}</code
				></pre>
			{/if}
		</div>
		<div class="box gap4 variants-box">
			<span class="text-sm col2 font20 w500">Icon Variant:</span>
			<div class="row gap4">
				{#each VARIANTS as v (v)}
					<button
						class="standard-btn small"
						class:active={variant === v}
						onclick={() => (variant = v)}
					>
						{v}
					</button>
				{/each}
			</div>
		</div>
		<div class="box gap4 search-box">
			<input
				class="control-search"
				type="search"
				placeholder="Search {filtered.length} icons…"
				bind:value={search}
			/>
		</div>
		<div class="box gap4 templates-box">
			<span class="text-sm col2 font20 w500">Select Animated Type:</span>
			<select class="styled-select" bind:value={template}>
				{#each templatesForVariant as t (t.id)}
					<option value={t.id}>{t.label}</option>
				{/each}
			</select>
		</div>
		<div class="border-box box gap16">
		<div class="box sliders-box gap8">
			<label class="slider row ycenter gap8">
				<span class="text-sm col2 font20 w500">Size</span>
				<input type="range" min="12" max="64" step="4" bind:value={size} />
				<span class="text-sm col2 font20 w500">{size}px</span>
			</label>
			<label class="slider row ycenter gap8">
				<span class="text-sm col2 font20 w500">Speed</span>
				<input type="range" min="0.25" max="3" step="0.25" bind:value={speed} />
				<span class="text-sm col2 font20 w500">{speed}x</span>
			</label>
		</div>
		<div class="row checkboxes-box gap16">
			<label class="row ycenter gap4">
				<input
					type="checkbox"
					checked={trigger === 'mount'}
					onchange={(e) => (trigger = e.currentTarget.checked ? 'mount' : 'hover')}
				/>
				<span class="text-sm col2 font20 w500 tt-u">Autoplay</span>
			</label>
			<label class="row ycenter gap4">
				<input type="checkbox" bind:checked={loop} />
				<span class="text-sm col2 font20 w500 tt-u">Loop</span>
			</label>
		</div>
		</div>
		<div class="box easing-box gap4">
			<EasingControl bind:value={easing} />
		</div>
	</div>
	<div class="display-area">
		<div class="icon-grid" use:autoAnimate>
			{#each shown as name (name)}
				{@const Icon = ICONS[name]}
				<button
					class="icon-cell gap8"
					class:active={selected === name}
					title={name}
					onclick={(e) => {
						e.stopPropagation();
						selected = name;
					}}
				>
					<Icon {variant} {template} {size} {trigger} {loop} {speed} {easing} />
					<span class="text-xs col3">{name}</span>
				</button>
			{/each}
		</div>
		{#if shown.length < filtered.length}
			<div class="row xcenter">
				<button class="standard-btn" onclick={() => (limit += 120)}>
					Load more ({filtered.length - shown.length} remaining)
				</button>
			</div>
		{/if}
	</div>
</section>
