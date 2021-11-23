<script>
    import{ afterUpdate, beforeUpdate, onDestroy, onMount } from "svelte";
// import { HtmlTag, onMount } from "svelte/internal";
import Nested from "../components/Nested.svelte";
let name= '';
let yes = false;
let options = [
    {v:1, t:'서울'},
    {v:2, t:'뉴욕'},
    {v:3, t:'베이징'}
]
let selectedOpt = 1;


let title = "Main";
    let imgSrc = "https://img.freepik.com/free-vector/hello-word-memphis-background_136321-401.jpg?size=626&ext=jpg"
    let src = "https://img.freepik.com/free-vector/hello-word-memphis-background_136321-401.jpg?size=626&ext=jpg"
    let htmlName = '<strong>힘써.</strong>'
    
    let count= 0;
    function countPlus(){
        count = ++count
    }
    function countMin(){    
        if(count===0){
            return
        }else{
            count = --count
        }
    }
    
    $ : sum = count * 1000;
    $ : if(count>5){ alert('최대 추가는 5개입니다.'); count = 5}
    
    let arrData =[
        {item:'돈까스', price: '3000원' },
        {item:'피자', price: '5000원' },
        {item:'스파게티', price: '1000원' },
    ]

    /* LifeCycle */
    //onMount 화면이 열릴때
    //beforeUpdate
    //afterUpdate
    //onDestory 화면 벗어날때
    
    onMount(()=>{
        console.log('a')
    })

    let seconds = 0;
    let interVal = window.setInterval(()=>{
        ++seconds 
    }, 1000)

    onDestroy(()=>{
        window.clearInterval(interVal)
    });

    beforeUpdate(()=>{
        console.log('before')
    });

    afterUpdate(()=>{
        console.log('after')
    })


</script>
<h1>Timmer :: {seconds}</h1>
<h2>form Data bind</h2>
<input type="text" bind:value={name}>
<p>안녕~ {name}</p>

<label for="check">
    <input type="checkbox" id="check" bind:checked={yes} /> check
</label>

{#if yes}
<div>체크감사.</div>
{:else}
<div>체크해줭</div>
{/if}


<h2>셀렉트 박스</h2>
<select bind:value={selectedOpt}>
    {#each options as opt}
        <option value={opt.v}>{opt.t}</option>
    {/each}
</select>
<p>선택은 {selectedOpt}</p>

<h1>Home</h1>
<h2>Hello this is {title}</h2>

<!-- 데이터 랜더링 -->
<table>
    {#each arrData as {item,price}, i}
    <tr>
        <td>{item}</td>
        <td>{price}</td>
    </tr>
    {/each}
</table>

<button on:click={countPlus}>추가</button>
<button on:click={countMin}>감소</button>
<!-- if 문 -->
{#if count>0}
    <div><span>{count}</span> 개</div>
{:else}
    <div>추가를 누르면 수량이 표기됩니다.</div>
{/if} 



<div>Total : {sum} 원</div>
<div><img src={imgSrc} alt=""></div>
<div><img {src} alt=""></div>
<Nested name={title} />
<div>{@html htmlName}</div>
<strong>Strong</strong>


<style>

</style>