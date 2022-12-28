<script>
	export let show = false;
</script>

{#if show}
	<div class="position-fixed top-50 start-50 d-flex justify-content-center align-items-center w-100 h-100" style="transform: translate(-50%, -50%);background-color: rgba(0, 0, 0, 0.3);">
		<div class="p-5 rounded text-center bg-white" style="max-width: 400px;">
			<h5 class="text-dark bg-white">Do you want to end the test?</h5>
			<div class="buttons bg-white">
				<button class="btn btn-danger" on:click|self={() => (show = false)}>NO</button>
				<a href="/result">
					<button class="btn btn-danger">YES</button>
				</a>
			</div>
		</div>
	</div>
{/if}

<style>
</style>