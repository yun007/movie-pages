<script>
    export let movie;
    export let imgUrl;
    export let current;
    export let totalPage;
</script>

<div class="slide_wrap">
    {#await movie}
    <p>...Loading</p>
    {:then movie } 
    <!-- <div id="carouselExampleIndicators" class="carousel slide" data-bs-ride="carousel"> -->
        <div id="carouselExampleCaptions" class="carousel slide" data-bs-ride="carousel">   
    
        <div class="carousel-indicators" style="display:none">
        {#each movie as item, index}
        <button type="button" data-bs-target="#carouselExampleIndicators" data-bs-slide-to="{index}" class="current {index == 0 ? 'active' : ''}" aria-label="Slide {index+1}"></button>
        {/each}      
        </div>
    <div class="carousel-inner">    
        {#each movie as item, index}

        <div class="carousel-item {index == 0 ? 'active' : ''}" style="background-image:linear-gradient( rgba(0, 0, 0, 0.0), rgba(0, 0, 0, 1) ), url({imgUrl+item.backdrop_path})">
            <div class="img-box">
                <img src={imgUrl+item.backdrop_path} class="d-block w-100" alt="...">
            </div>
        <div class="carousel-caption d-none d-md-block">
            <h3 class="movie_title">{item.original_title}</h3>
            <p class="overview">{item.overview == 0 ? '요약 정보없음' : item.overview}</p>
        </div>      
        </div>
        
        {/each}     
        <div class="current_page">{current} / {totalPage}</div>
    </div>
    <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="prev">
        <span class="carousel-control-prev-icon" aria-hidden="true"></span>
        <span class="visually-hidden">Previous</span>
    </button>
    <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="next">
        <span class="carousel-control-next-icon" aria-hidden="true"></span>
        <span class="visually-hidden">Next</span>
    </button>
    </div>  
    {/await}
</div>
<style>
    .slide_wrap{background:rgba(0,0,0,0.7)}    
    .carousel-indicators [data-bs-target]{border-radius: 50%; width:10px;height:10px}
    .carousel-caption{width:100%;left:0;right:0;top:50%;transform:translateY(-50%);bottom:initial;background: rgba(0,0,0,0.5);z-index:5}
    .movie_title{font-size:40px}
    .carousel-inner,.carousel-item{min-height:680px}
    .img-box{width:1280px;position:absolute;top:0;left:50%;transform: translateX(-50%)}

    .overview{padding:10px 200px;text-align:left;word-break: keep-all;}
    .current_page{position:absolute;bottom:10px;right:20px;color:#fff;font-weight:bold}
    .carousel-control-prev,.carousel-control-next{z-index:10}
    .carousel-control-prev:active,.carousel-control-next:active{background:none}
    
</style>