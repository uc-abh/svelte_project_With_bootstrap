<script>
	import { tweened } from 'svelte/motion';
	let original = 5* 60; 
	let timer = tweened(original);
	setInterval(() => {
		if ($timer > 0) $timer--;
	}, 1000);
	$: minutes = Math.floor($timer / 60);
	$: seconds = Math.floor($timer - minutes * 60);
</script>
{#if minutes == 0 && seconds == 0}
	<div
		class="position-fixed top-50 start-50 d-flex justify-content-center align-items-center w-100 h-100"
		style="transform: translate(-50%, -50%);background-color: rgba(0, 0, 0, 0.3);"
	>
		<div class="p-5 rounded text-center bg-white" style="min-width: 400px;">
			<h5 class="text-dark bg-white">Your time is over</h5>
			<a href="/result"><button class="btn btn-dark">See Your Result</button></a>
		</div>
	</div>
	<p class="m-0">
		
	</p>
{:else}
	
		{#if minutes.toString().length > 1}
			{minutes}:
		{:else}
			0{minutes}:
		{/if}
		{#if seconds.toString().length > 1}
			{seconds}
		{:else}
			0{seconds}
		{/if}
	
{/if}
