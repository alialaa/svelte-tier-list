<script lang="ts">
	import tippy from '$lib/actions/tippy.svelte';
	import { ChevronDown, ChevronUp, Minus, Plus } from 'lucide-svelte';
	import { defaultTiers, type Tier, type TierImage } from './utils';

	let {
		images = $bindable([]),
		tiers = $bindable(defaultTiers)
	}: {
		images?: TierImage[];
		tiers?: Tier[];
	} = $props();

	$inspect(tiers);
</script>

<div class="tiers">
	{#each tiers as tier, index (tier.id)}
		<div class="tier">
			<div
				class="label"
				contenteditable="true"
				bind:innerText={tier.label}
				style:background-color={tier.color}
			></div>

			<div class="content"></div>
			<div class="options">
				<div class="option">
					<button
						disabled={index === 0}
						aria-label="Move Tier {tier.label} Up"
						use:tippy={() => ({ content: 'Move Up' })}
					>
						<ChevronUp />
					</button>
				</div>
				<div class="option">
					<button
						disabled={index === tiers.length - 1}
						aria-label="Move Tier {tier.label} Down"
						use:tippy={() => ({ content: 'Move Down' })}
					>
						<ChevronDown />
					</button>
				</div>

				<div class="option">
					<button
						aria-label="Add Tier Below Tier {tier.label}"
						use:tippy={() => ({ content: 'Add Tier Below' })}
					>
						<Plus />
					</button>
				</div>
				<div class="option">
					<button
						aria-label="Delete Tier {tier.label}"
						use:tippy={() => ({ content: 'Delete Tier' })}
					>
						<Minus />
					</button>
				</div>
				<div class="option" use:tippy={() => ({ content: 'Edit Color' })}>
					<input
						aria-label="Edit Tier {tier.label} Color"
						class="color-input"
						type="color"
						id={`${tier.id}-color`}
						bind:value={tier.color}
					/>
				</div>
			</div>
		</div>
	{/each}
</div>

<style lang="scss">
	* {
		box-sizing: border-box;
	}
	button,
	div,
	input {
		&:focus-visible {
			outline: 2px solid #ff3e00;
		}
	}
	.tiers {
		background-color: #000;
		.tier {
			display: flex;
			margin: 2px 0;
			background-color: #151515;
			container-type: inline-size;
			position: relative;
			&:hover,
			&:focus-within {
				.options {
					display: flex;
				}
			}
			.options {
				position: absolute;
				display: flex;
				inset-inline-end: 5px;
				top: 5px;
				align-items: center;
				display: none;
				.option {
					margin: 2px;
					button {
						background-color: transparent;
						border: none;
						padding: 0;
						width: 20px;
						height: 20px;
						cursor: pointer;
						:global(svg) {
							stroke: #ffffff;
							width: 20px;
						}
						&[disabled] {
							opacity: 0.4;
							cursor: not-allowed;
						}
					}
				}
				.color-input {
					width: 18px;
					height: 18px;
					cursor: pointer;
					padding: 0;
					margin: 0;
					&::-webkit-color-swatch-wrapper {
						padding: 0;
					}
					&::-webkit-color-swatch {
						border: none;
					}
				}
			}

			.label {
				min-width: 74px;
				width: 10%;
				aspect-ratio: 1;
				display: flex;
				justify-content: center;
				align-items: center;
				font-size: 5cqw;
				color: #111;
				position: relative;
			}
			.content {
				flex: 1;
				display: flex;
				flex-direction: row;
				flex-wrap: wrap;
				padding: 0 4px;
				align-items: center;
			}
		}
	}
</style>
