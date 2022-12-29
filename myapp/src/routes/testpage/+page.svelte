<script>
import Header from "../../components/Header.svelte";
import {questiondata_store,choosingans_store} from "../../store/stores";
import {onMount} from 'svelte';
import Footer from "../../Components/Testfooter.svelte"
let all_questions =[];
let choosen_answer=[];
let current_question = 0;

onMount(async()=>{
    $choosingans_store=choosen_answer;
    const response=await fetch('data/question.json');
    all_questions = await response.json();
    questiondata_store.set(all_questions);
});
</script>

<div class="mycontainer">
    <Header/>
    <div class="container mt-3">
        {#each all_questions as data, i}
        {#if current_question===i}
        <div>
            <span class="h4">
            {i+1} . {JSON.parse(data.content_text).question}
            </span>
            <div class="d-flex flex-column mt-4 text-primary font-weight-bold">
                {#each JSON.parse(data.content_text).answers as answers, j}
                <div class="d-flex align-items-center p-2">
                    <label class="w-100 pointer font-weight-bold">
                        <input type="radio" class="m-2"
                        value={answers.answer}
                        name="radio"
                        id="radio{j}"
                        bind:group={choosen_answer[i]}/>

                        {@html answers.answer}
                    </label>
                </div>
                {/each}
            </div>
        </div>
        {/if}
        {/each}
        
    </div>
</div>

<div class="position-fixed w-50  bg-dark text-white" style="bottom:15px;right:15px">
    <Footer
    {current_question}
    on:prevPage={() => (current_question = current_question - 1)}
    on:nextPage={() => (current_question = current_question + 1)}
    on:updateQues={(event) => {
		current_question = event.detail;
		}}/>
</div>

<style>
    .mycontainer{
        min-width: 828px !important;
    }
</style>