<script>
    import Header from '../../Components/Header.svelte';
    import {
        questiondata_store,
        choosingans_store,
        correctanswer_store,
        chhosenanswerbystudent_store,
    } from '../../store/stores';
    let incorrect = 0;
    let percentage = 0;
    let correct = 0;
    let chhosenanswerbystudent_arr = [];
    let correctanswer_arr = [];
    $: unselected_ques=0
    $: for (let i = 0; i < $questiondata_store.length; i++) {
        let correct_index = 0;
        if ($choosingans_store[i]) {
            for (let j = 0; j < 4; j++) {
                if (JSON.parse($questiondata_store[i].content_text).answers[j].answer == $choosingans_store[i]) {
                    correct_index = j;
                }
            }
        } else {
            correct_index = null;
        }
        chhosenanswerbystudent_arr[i] = correct_index;
        $chhosenanswerbystudent_store[i]=correct_index;
    }
    $: for (let i = 0; i < $questiondata_store.length; i++) {
        let actualCorrect = 0;
        for (let j = 0; j < 4; j++) {
            if (JSON.parse($questiondata_store[i].content_text).answers[j].is_correct == '1') {
                actualCorrect = j;
            }
        }
        correctanswer_arr[i] = actualCorrect;
        $correctanswer_store[i]=actualCorrect
    }
    $: for (let i = 0; i < chhosenanswerbystudent_arr.length; i++) {
        if (chhosenanswerbystudent_arr[i] != null) {
            if (chhosenanswerbystudent_arr[i] == correctanswer_arr[i]) {
                correct = correct + 1;
                percentage = Math.round((correct / 11) * 100);
            } else {
                incorrect = incorrect + 1;
            }
        } else{
            unselected_ques+=1;
        }
    }
    const handle_click=(event)=>{
console.log(event);
    }
    
    </script>
    
    <div>
    <Header />
    <div class="container d-flex justify-content-center mt-4 flex-column align-items-center">
        <div class="d-flex" style="width: 800px;">
            <div
                class="d-flex flex-column align-items-center btn  bg-opacity-25 border rounded w-50"
              
            >
                <p style="color: white">{percentage} %</p>
                <h6>Result</h6>
            </div>
            <div
                class="d-flex flex-column align-items-center btn border rounded w-50  ms-3" >
                <p class="text-white">11</p>
                <h6>Total item</h6>
            </div>
            <div
                class="d-flex flex-column align-items-center btn border rounded w-50 ms-3">
                <p class="text-white">{correct}</p>
                <h6>Correct</h6>
            </div>
            <div
                class="d-flex flex-column align-items-center btn border rounded w-50 ms-3">
                <p class="text-white">{incorrect}</p>
                <h6>Incorrect</h6>
            </div>
            <div
                class="d-flex flex-column align-items-center btn border rounded w-50 ms-3" >
                <p class="text-white">{unselected_ques}</p>
                <h6>Unattempted</h6>
            </div>
        </div>
        <table class="table">
            <thead>
                <tr>
                    <th scope="col">S.No.</th>
                    <th scope="col">Questions</th>
                    <th scope="col">Options</th>
                    <th scope="col">Result</th>
                </tr>
            </thead>
            <tbody>
                {#each $questiondata_store as data, i}
                    <tr>
                        <th scope="row" class="text-white">{i + 1}</th>
                        <td>
                            <a  href = "review?qno={i+1}"
                                class="w-100 text-decoration-none text-white {i}"
                                on:click={handle_click} >
                                {data.snippet}
                            </a>
                        </td>
                        <td class="d-flex">
                            {#each JSON.parse(data.content_text).answers as _, j}
                                <p
                                    class="{`${
                                    correctanswer_arr[i] == j ? 'bg-success' : ''
                                    }`}   border d-flex justify-content-center align-items-center ms-2  text-white rounded"
                                    class:selected={correctanswer_arr[i] != chhosenanswerbystudent_arr[i] &&
                                    chhosenanswerbystudent_arr[i] == j ? true : false}
                                     style="width: 24px; height:24px" >
                                    {String.fromCharCode(65 + j)}
                                </p>
                            {/each}
                        </td>
                        <td>
                            {#if chhosenanswerbystudent_arr[i] == correctanswer_arr[i]}
                                <span>Correct</span>
                            {:else if chhosenanswerbystudent_arr[i] == null}
                                <span>Unattempted</span>
                            {:else}
                                <span>Incorrect</span>
                            {/if}
                        </td>
                    </tr>
                {/each}
            </tbody>
        </table>
    </div>
    </div>
    
    