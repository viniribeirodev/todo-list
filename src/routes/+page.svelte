<script lang="ts">
	import Card from '$cp/Card.svelte'
	import Container from '$cp/Container.svelte'
	import Head from '$cp/Head.svelte'
	import Title from '$cp/Title.svelte'
	import Icon from '@iconify/svelte'
	import ShortUniqueId from 'short-unique-id'
	import { flip } from 'svelte/animate'
	import { fade, fly } from 'svelte/transition'
	const uid = new ShortUniqueId({ length: 10 })
	type PropsList = {
		id: string
		content: string
		check: boolean
	}

	let list: PropsList[] = []

	const addItem = () => {
		if (!content) return
		list = [...list, { id: uid.rnd(), content, check: false }]
		content = ''
	}

	let content = ''
</script>

<Head title="Todo List" />
<Container>
	<Card>
		<Title>Minha lista</Title>
		<hr class="my-3" />
		<div class="flex items-center gap-3">
			<input
				type="text"
				class="w-full border rounded-full px-4 py-2 outline-none"
				bind:value={content}
			/>
			<button type="button" on:click={addItem}>
				<Icon
					icon="material-symbols:add"
					class="text-3xl text-indigo-400 duration-150 hover:opacity-70"
				/>
			</button>
		</div>
		<div class="grid grid-cols-1 gap-3 pt-4">
			{#if !list.length}
				<div
					class="text-center text-sm text-slate-600"
					in:fly={{ x: 100, duration: 400, delay: 400 }}
					out:fly={{ x: 100, duration: 400 }}
				>
					Nenhum item adicionado
				</div>
			{/if}
			{#each list as l, i (l.id)}
				<div
					class="flex w-full items-center justify-between border-b px-2 py-1 hover:bg-slate-50 duration-150"
					animate:flip
					in:fly={{ x: 100, duration: 400, delay: 400 }}
					out:fly={{ x: 100, duration: 400 }}
				>
					<div class="text-sm font-light text-slate-500" class:line-through={l.check}>
						{l.content}
					</div>
					<div class="flex">
						{#if l.check}
							<button
								type="button"
								in:fade
								on:click={() => {
									list = list.map((lm) => {
										if (lm.id === l.id) lm.check = false
										return lm
									})
								}}
							>
								<Icon
									icon="material-symbols:keyboard-return-rounded"
									class="text-xl text-slate-400 hover:opacity-80 duration-150"
								/>
							</button>
						{:else}
							<button
								type="button"
								in:fade
								on:click={() => {
									list = list.map((lm) => {
										if (lm.id === l.id) lm.check = true
										return lm
									})
								}}
							>
								<Icon
									icon="material-symbols:check-small"
									class="text-xl text-green-400 hover:opacity-80 duration-150"
								/>
							</button>
						{/if}
						<button type="button" on:click={() => (list = list.filter((lf) => lf.id !== l.id))}>
							<Icon
								icon="ic:outline-remove"
								class="text-xl text-red-500 hover:opacity-80 duration-150"
							/>
						</button>
					</div>
				</div>
			{/each}
		</div>
	</Card>
</Container>
