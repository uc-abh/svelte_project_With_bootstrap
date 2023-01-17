<script>
    import { createEventDispatcher } from 'svelte';
    import Sidebar from './Sidebar.svelte';
      const dispatch = createEventDispatcher();
    export let question_no;
      let sidebar_show = false;
      
      const prevPage = () => {
          dispatch('prevPage_rev');
      };
      
      const nextPage = () => {
          dispatch('nextPage_rev');
      };
      
      const displayQuesNum = (event) => {
          dispatch('updateQues_rev', parseInt(event.detail)+1);
      };
  </script>
  
  <div>
      <div class="d-flex justify-content-between p-2 align-items-center bg-white border border-dark text-white">
          
          
        <button on:click={() => (sidebar_show = !sidebar_show)} class="btn btn-dark" >
			List
		</button>
		<Sidebar bind:show={sidebar_show} on:displayQuesNum={displayQuesNum} />
	
		<button
			on:click={prevPage}
			class="btn btn-dark"
			disabled={parseInt(question_no) <= 1 ? true : false}>Previous</button>
      <button class="btn btn-dark">
				{parseInt(question_no)} of 11
        </button>
		<button
			on:click={nextPage}
			class="btn btn-dark"
			disabled={parseInt(question_no)> 10 ? true : false}>Next</button>
              <a class="bg-light" href="/">
            <button  class="btn btn-dark">Dashboard</button>
              </a>
              <a class="bg-light" href="/result">
             <button  class="btn btn-dark">Result</button>
              </a>
      </div>
  </div>

  