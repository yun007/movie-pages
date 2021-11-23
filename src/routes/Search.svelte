<script>
    export let imgUrl;
    let keyword;
    let toggle = true;
    let searchInfo = [];

    function check(){
     console.log('a')   
     if(this.value.length>0){
         this.value = ''
         keyword = '';
         console.log('내용있음')
         toggle = false;
         return
     }
    }
    function search(){
        console.log({keyword})
        apiSearch(keyword)
    }
    //https://api.themoviedb.org/3/search/movie?api_key=3c730caf1510cc2b2fe0fe392970b6f1&query=
    function apiSearch(_keyword){
        fetch(`https://api.themoviedb.org/3/search/movie?api_key=3c730caf1510cc2b2fe0fe392970b6f1&query=${_keyword}&language=ko-kr`)
            .then(response => response.json())
            .then(data => {
                console.log(data, data.results.length)
                searchInfo = [...data.results]
                if(data.results.length == 0){
                    alert('미안 그런영화 없다.... ㅜㅜ')
                }
        })    
    }
    $:{
        console.log(searchInfo)
    }
</script>

<div class="search_wrap">
    <h1>Movie Sraech</h1>
    <input type="text" bind:value={keyword} on:click={check}/> <button type="button" disabled={!keyword} on:click={search} >검색</button>
    <div id="result" class="result_box">
        {#await searchInfo}
        <p>wating....</p>
        {:then searchInfo } 

        <div class="cards card_box">
            <div class="wrapper">
                <div class="cards">
                    {#each searchInfo as item, index}
                    <div class="card {searchInfo.length == 1 ? 'single' : ''}" on:click={()=>{console.log(searchInfo.length)}}>
                        <div class="inner1">
                            <div class="inner2">                            
                                <div class="texts">
                                    <div class="img_box"><img src="{item.poster_path == null ? 'https://www.themoviedb.org/assets/2/apple-touch-icon-cfba7699efe7a742de25c28e08c38525f19381d31087c69e89d6bcb8e3c0ddfa.png' : imgUrl+searchInfo[index].poster_path}" alt="{searchInfo[index].original_title+'포스터'}" /></div>
                                    <span class="tag">언어 : {item.original_language}</span>
                                    <div class="title">{item.title} ({item.original_title})</div>
                                    <div class="desc">줄거리<br>{item.overview}</div>
                                    <div class="date">출시일 : {item.release_date}</div>
                                </div>
                            </div>
                        </div>
                    </div>
                    {/each}
                </div>
            </div>
        </div>        
        {/await}

    </div>
</div>
<style>
    .search_wrap{padding:20px}
    .result_box{min-height:559px}
    .result_box{margin:50px 0 0}
    .img_box{width:100%}
    .img_box img{width:100%;height:320px}

.card_box{
    max-width:1280px;
    margin:0 auto;
}    
.cards {
    margin-bottom: 30px;        
}

.cards>.wrapper .cards {
    display:flex;
    flex-wrap:wrap;
    align-items: stretch;
    justify-content:center;
    margin: 0 -15px;
    
}

.cards>.wrapper .cards:after {
    display: table;
    content: " ";
    clear: both;
}

.cards>.wrapper .cards .card {    
    max-width:300px;
    min-height:220px;
    margin-bottom: 15px;
    border:none;
    display:initial
}
.cards>.wrapper .cards .card.single {    
    max-width:640px;
}

.cards>.wrapper .cards .card.single .img_box{
    text-align: center;
    background-color: #f2f2f2;
}
.cards>.wrapper .cards .card.single .img_box img{
    width:220px
}

.cards>.wrapper .cards .card .inner1 {
    padding: 0 15px;
}

.cards>.wrapper .cards .card .inner1 .inner2 {
    background: #fff;
    border: 1px solid #ddd;
    min-height:510px;
}

.cards>.wrapper .cards .card .inner1 .inner2 .texts {
    padding: 15px;
}

.cards>.wrapper .cards .card .inner1 .inner2 .texts .tag {
    background: #146ece;
    color: #fff;
    font-size: 11px;
    padding: 2.5px 5px;

}

.cards>.wrapper .cards .card .inner1 .inner2 .texts .title {
    margin-top: 15px;
    margin-bottom: 10px;
    cursor: pointer;
    font-weight: bold;
}

.cards>.wrapper .cards .card .inner1 .inner2 .texts .desc {
    margin-top: 10px;
}

.cards>.wrapper .cards .card .inner1 .inner2 .texts .date {
    margin-top: 15px;
    font-size: 14px;
    color: #444;
}

@media(max-width:760px) {

    .img_box {
        text-align: center;
        background-color:#f2f2f2;
    }
    .img_box img{
        width:220px
    }
    .cards>.wrapper .cards .card {
        width: 100%;
        max-width:100%;
    }
}
@media(max-width:420px) {
    .img_box img{
        width:100%
    }
    .cards>.wrapper .cards .card {
        width: 100%;
    }
}

</style>