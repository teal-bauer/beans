<script lang="ts">
	import type { Bean } from '$lib/beans.svelte';
	import { beansStore } from '$lib/beans.svelte';
	import BeanCard from './BeanCard.svelte';
	import BeanItem from './BeanItem.svelte';

	interface Props {
		bean: Bean;
		depth?: number;
		selectedId?: string | null;
		onSelect?: (bean: Bean) => void;
	}

	let { bean, depth = 0, selectedId = null, onSelect }: Props = $props();

	const children = $derived(beansStore.children(bean.id));

	function handleClick(e: MouseEvent) {
		e.stopPropagation();
		onSelect?.(bean);
	}
</script>

<div class="bean-item">
	<!-- svelte-ignore a11y_click_events_have_key_events -->
	<!-- svelte-ignore a11y_no_static_element_interactions -->
	<div onclick={handleClick}>
		<BeanCard {bean} variant="list" selected={selectedId === bean.id} onclick={() => onSelect?.(bean)} />
	</div>

	{#if children.length > 0}
		<div class="mt-1 ml-4 space-y-1 border-l border-border pl-2">
			{#each children as child (child.id)}
				<BeanItem bean={child} depth={depth + 1} {selectedId} {onSelect} />
			{/each}
		</div>
	{/if}
</div>
