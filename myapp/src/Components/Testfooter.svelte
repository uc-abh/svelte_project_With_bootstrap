<script>
	import { createEventDispatcher } from 'svelte';
	import Confirmation from './Confirmation.svelte';
	import Sidebar from './Sidebar.svelte';
	import Timer from './Yourtime.svelte';
	const dispatch = createEventDispatcher();
	export let current_question;
	let sidebar_show = false;
	let confirm_show = false;

	const prevPage = () => {
		dispatch('prevPage');
	};

	const nextPage = () => {
		dispatch('nextPage');
	};

	const displayQuesNum = (event) => {
		dispatch('updateQues', event.detail);
	};
</script>

<div>
	<div
		class="d-flex justify-content-between p-2 align-items-center bg-white border border-dark text-white"
	>
		<button class="btn btn-dark">
			<Timer />
		</button>

		<button on:click={() => (sidebar_show = !sidebar_show)} class="btn btn-dark"> List </button>
		<Sidebar bind:show={sidebar_show} on:displayQuesNum={displayQuesNum} />

		<button
			on:click={prevPage}
			on:click={() => (sidebar_show = false)}
			class="btn btn-dark"
			disabled={current_question < 1 ? true : false}>Previous</button
		>
		<button class="btn btn-dark">
			{current_question + 1} of 11
		</button>
		<button
			on:click={nextPage}
			on:click={() => (sidebar_show = false)}
			class="btn btn-dark"
			disabled={current_question + 1 > 10 ? true : false}>Next</button
		>

		<button class="btn btn-dark" on:click={() => (confirm_show = !confirm_show)}>End Test</button>
	</div>
</div>
<Confirmation bind:show={confirm_show} />
