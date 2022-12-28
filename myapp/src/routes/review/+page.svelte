<script>
	import Header from '../../components/Header.svelte';
	import Footer from '../../Components/Reviewfooter.svelte';
    import {onMount} from 'svelte';
    let question_no;
    let all_questions = [];
    onMount(async()=>{
        const response=await fetch('data/question.json');
        all_questions = await response.json();
        question_no = new URL(location.href).searchParams.get("qno");
    });
</script>
<div>
    <Header/>
    <div class="container ">
        {#each all_questions as data, i}
        {#if question_no-1===i}
        <div>
            <span class="h4">
            {i+1} . {JSON.parse(data.content_text).question}
            </span>
            <div class="d-flex flex-column mt-4 text-primary font-weight-bold">
                {#each JSON.parse(data.content_text).answers as answers, j}
                <div class="d-flex">
                    {#if answers.is_correct == 1}
                        <p class="h6 my-auto">{String.fromCharCode(65 + j)}</p>
                        <label class="w-100 m-2">
                            <input type="radio" checked />
                            {@html answers.answer}
                        </label>
                    {:else}
                        <p class="h6 my-auto">{String.fromCharCode(65 + j)}</p>
                        <label class="w-100 m-2">
                            <input type="radio" disabled />
                            {@html answers.answer}
                        </label>
                    {/if}
                </div>
                {/each}
<div class='mt-3 text-warning'>
        {@html JSON.parse(all_questions[question_no-1].content_text).explanation}
</div>
            </div>
        </div>
        {/if}
        {/each}
    </div>
</div>

<div class="position-fixed w-50  bg-dark text-white" style="bottom:15px;right:15px">
    <Footer
    {question_no}
		on:prevPage_rev={() => (question_no = parseInt(question_no) - 1)}
        on:nextPage_rev={() => (question_no = parseInt(question_no) + 1)}
        on:updateQues_rev={(event) => {
		question_no = event.detail;
		}}/>
</div>
<style>
    body{
        color:white;
    }
    :global(seq::before) {
        content: attr(no);
        text-transform: uppercase;
    }
</style>

