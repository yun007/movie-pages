<script>
    
    // import axios from 'axios'
    // import { onMount } from "svelte";
    import MainSlide from '../components/MainSlide.svelte';
    export let imgUrl;
    // let pgNum = 1;
	
    // let non = [];
    // let gettingQuote = false;

    // onMount(() => {
    //     getRandomQuote();
    // });

    // async function getRandomQuote() {
    //     gettingQuote = true;
    //     const res = await fetch("https://api.themoviedb.org/3/movie/popular?api_key=3c730caf1510cc2b2fe0fe392970b6f1&language=ko-kr=2");
    //     const json = await res.json();
    //     non = [...non, ...json.results];
    //     gettingQuote = false;
    //     console.log(non)
    // }

    

    // axios.get('https://api.themoviedb.org/3/movie/popular?api_key=3c730caf1510cc2b2fe0fe392970b6f1&language=ko-kr&page=1').then(response => {
    //     console.log(response);
    // });
    // const imgUrl = 'https://image.tmdb.org/t/p/original';
    let page = 1
    let totalPage;
    
    let movie = [];
    let testMovie = [];
    let current = 1;

    let popInfo = [];
    let actorInfo = [];
    let popSwitch = true;
    let popId;

   fetch(`https://api.themoviedb.org/3/movie/popular?api_key=3c730caf1510cc2b2fe0fe392970b6f1&language=ko-kr=${page}`)
    .then(response => response.json())
    .then(data => {
      console.log(data)

      movie = [...data.results];
      testMovie = [...testMovie, data] 
      totalPage = data.results.length;  
     // thumb1 = data.results[0].backdrop_path;
       
      //console.log(totalPage,thumb1, movie)
      let mySwiper = document.getElementById('carouselExampleCaptions');
      mySwiper.addEventListener('slide.bs.carousel', function (e) {
        current = e.to
        ++current;    
      })            
    })
    
  fetch('https://api.themoviedb.org/3/person/popular?api_key=3c730caf1510cc2b2fe0fe392970b6f1&language=ko-kr=1')
    .then(response => response.json())
    .then(data => {
      console.log(data)
      popInfo = [data.results[0],data.results[1],data.results[2]]
      console.log(popInfo)
      console.log(popInfo[0].id, popInfo[1].id, popInfo[2].id)
      
      // https://api.themoviedb.org/3/person/{person_id}?api_key=<<api_key>>&language=en-US
    })

    function info(_personal){
      fetch(`https://api.themoviedb.org/3/person/${_personal}?api_key=3c730caf1510cc2b2fe0fe392970b6f1&language=ko-kr=1`)
          .then(response => response.json())
          .then(data => {
            actorInfo = [data.birthday, data.name, data.place_of_birth]
            console.log(actorInfo.name, data)
      })
    }

    function popToggle(_idx,_personal){
      info(_personal)
      popId = _idx
      console.log(_idx, _personal)
    }

    $:{
      console.log(actorInfo)
    }

</script>

<MainSlide movie={movie} imgUrl={imgUrl} current={current} totalPage={totalPage} />



<div class="card_box">
  <div class="wrapper">
      <div class="title">Popular Actor TOP3 </div>
      <div class="card_box">
        {#await popInfo}
          <div>...Loading..</div>
          {:then popInfo } 
          {#each popInfo as item, index}
          <div class="card_item {index == 0 ? 'half' : 'quarter'}">
              <div class="inner">
                  <div class="thumb" style="background-image: linear-gradient( rgba(0, 0, 0, 0.1), rgba(0, 0, 0, 1) ), url({imgUrl+item.known_for[0].backdrop_path});"></div>
                  <div class="info">
                      <!-- <a class="title" href="#pop" on:click={ index == index ? (e) => popSwitch = !popSwitch : ''}>{index+1}. {item.name}</a> -->
                      <!-- <a class="title" href="#pop" on:click={ ()=> popToggle(index)}>{index+1}. {item.name}</a> -->
                      <a class="title" href="#pop" on:click={ ()=> popToggle(index, item.id)}>{index+1}. {item.name}</a>
                      <div class="txt">
                        <ul>
                          <li class="best_movie">{item.known_for[0].title}</li>
                        </ul>
                      </div>
                      <div class="date">2021-07-06</div>
                  </div>
                  <div id="pop" class="pop_info {popId === index ? 'on' : ''}" data-pop-no='{index}' style="background-image: linear-gradient( rgba(0, 0, 0, 0.1), rgba(0, 0, 0, 0.5) ), url({imgUrl+item.profile_path});">
                    <ul class="actor_info">
                      <li>Name : {actorInfo[1]}</li>
                      <li>Birth-Day : {actorInfo[0]}</li>
                      <li>출생지 : {actorInfo[2]}</li>
                    </ul>
                    <button type="button" class="close_pop" on:click={() => popToggle(false)}>닫기</button>
                  </div>                                    
              </div>
          </div>
          {/each}
          {/await}
      </div>
  </div>
</div>


<style>
.card_box {
    margin-bottom: 30px;
    padding:30px 20px;
}
.card_box>.wrapper{
  max-width:1280px;
  margin:0 auto;
}
.card_box>.wrapper>.title {
    font-size: 30px;
    margin-bottom: 30px;
    font-weight: bold;
}

.card_box .card_box {
    margin: 0 -10px;
    padding:0
}

.card_box .card_box:after {
    clear: both;
    content: " ";
    display: table;
}

.card_box .card_box .card_item {
    float: left;
    padding: 0 10px;
    margin-bottom: 15px;
}

.card_box .card_box .card_item .inner {
    height: 464px;
    border: 1px solid #ddd;
    position: relative;
    padding: 25px;
}

.card_box .card_box .card_item .inner .pop_info{
  display:none;
  position:absolute;
  top:0;
  left:0;
  right:0;
  bottom:0;  
  padding:20px;
  background-size:contain;
  background-repeat:no-repeat;
  background-position:top center;
}

.card_box .card_box .card_item .inner .pop_info.on{
  display:block
}

.close_pop{
  position:absolute;
  top:20px;
  right:20px;
}

.card_box .card_box .card_item .inner .thumb {
    background-size: cover;
    background-repeat: no-repeat;
    background-position: center;
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    background-color:rgba(0,0,0,0.9);
}

.card_box .card_box .card_item .inner .info {
    position: relative;
    padding: 15px 0 15px;
    height: 100%;
}

.card_box .card_box .card_item .inner .info .title {
    cursor: pointer;
    display: inline-block;
    font-size: 20px;
    font-weight: bold;
    margin-bottom: 15px;
}

.card_box .card_box .card_item .inner .info .txt {
    margin-bottom: 15px;
    color: #666;
}

.card_box .card_box .card_item .inner .info .date {
    color: #666;
    position: absolute;
    bottom: 0;
    left: 0;
}

.card_box .card_box .card_item.half {
    width: 50%;
}

.card_box .card_box .card_item .inner {
    padding-top: 300px;
}

.card_box .card_box .card_item .inner .thumb {
    height: 300px;
}

.card_box .card_box .card_item.half .inner .info .info {
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
}

.card_box .card_box .card_item.quarter {
    width: 25%;
}




.actor_info{
  position:absolute;
  left:0;
  bottom:0;
  color:#fff
}

@media(max-width:991px) {
    .card_box .card_box .card_item.half {
        width: 100%;
    }

    .card_box .card_box .card_item.quarter {
        width: 50%;
    }

    .card_box .card_box .card_item.half .inner,
    .card_box .card_box .card_item.quarter .inner {
        padding-top: 240px;
        height: auto;
    }

    .card_box .card_box .card_item.half .inner .thumb,
    .card_box .card_box .card_item.quarter .inner .thumb {
        height: 240px;
    }


    .card_box .card_box .card_item.half .inner .info .info {
        white-space: normal;
        text-overflow: clip;
        overflow: visible;
    }
}

@media(max-width:767px) {
    .card_box .card_box .card_item.half {
        width: 100%;
    }

    .card_box .card_box .card_item.quarter {
        width: 100%;
    }
}
</style>

