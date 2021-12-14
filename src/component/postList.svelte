<script>
import { Link } from 'svelte-routing'
import { onMount } from 'svelte';

//get query String
const urlParams = new URLSearchParams(window.location.search)
const page = urlParams.get("page")
const search = urlParams.get("search") || null

let controller = [
    {
        name : "추천순",
        style: "categoryBtn",
        sort: "good",
        isClick: true
    },
    {
        name : "조회순",
        style: "categoryBtn",
        sort: "view",
        isClick : false
    },
    {
        name : "등록순",
        style: "categoryBtn",
        sort: "insert",
        isClick: false
    }
]


let links = `http://koldin.myddns.me:3101/post?page=${page}`
let posts = false
if(search != null) links += `&search=${search}`
onMount(async () => {
    const res = await fetch(links)
    posts = await res.json()
})

async function pageLoader(page) {
    let links = `http://koldin.myddns.me:3101/post?page=${page}`
    let posts = false
    if(search != null) links += `&search=${search}`
    
    const res = await fetch(links)
    posts = await res.json()
}

async function sortData(_sort) {
    for (let i = 0; i < controller.length; i++) {
        controller[i].isClick = false
        if (controller[i].sort == _sort) 
            controller[i].isClick = true
    }
    if (_sort == "insert") _sort = "id"
    let links = `http://koldin.myddns.me:3101/post?page=${page}&sort=${_sort}`
    if(search != null) links += `&search=${search}`

    const res = await fetch(links)
    posts = await res.json()
}
</script>

<div class="info"> <!--아이디 제목 등 글자-->
    <p class="id">아이디</p>
    <p class="title">제목</p>
    <p class="date">작성일</p>
    <p class="view">조회수</p>
    <p class="like">추천수</p>
</div>
{#if posts}
    <li class="main"> <!--메인 내용-->
        {#each posts.data as {id, title, created, view, good}}
            <Link to="/post/{id}">
                <ul class="Group">
                    <p class="id">{id}</p> 
                    <p class="title">{title}</p> 
                    <p class="date">{created}</p> 
                    <p class="view">{view}</p> 
                    <p class="like">{good}</p>
                </ul>
            </Link>
        {/each}
    </li>
    <div class="controllerBtn"> <!--컨트롤 버튼들-->
        <div>
            {#each controller as {name, style, sort, isClick}}
                {#if isClick == true}
                    <input class="clickedBtn" type="button" value={name}>
                {:else}
                    <input class={style} type="button" value={sort, name} on:click={() => sortData(sort)}>
                {/if}
            {/each}
        </div>
        <div class="rectangle">
            <input type="button" value="<" class="pageNav"> <!--이전페이지-->
        {#if posts.lastPage > 0}
            {#each Array(posts.lastPage) as _, i}
                {#if posts.nowPage == i+1}
                    <input type="button" value="{i+1}" id="clickedPageNav">
                {:else}
                    <input type="button" value="{i+1}" class="pageNav" on:click="{pageLoader(i+1)}">
                {/if}
            {/each}
            {/if}
            <input type="button" value=">" class="pageNav"> <!--다음페이지-->
        </div>
    </div>
{:else}
    <h2 id="loading">Now Loading...</h2>
{/if}
<style>
* {
    margin: 0;
    padding: 0;
}

#loading {
    text-align: center;
}

.info {
    display: flex;
    width: 1180px;
    margin: 50px auto 10px;
    font-size: 18px;
    justify-content: space-between;
    padding: 10px;
}
.id {
    width: 10%;
    margin-left: 10px;
}
.title {
    width: 50%
}
.date {
    width: 15%;
}
.view {
   width: 15%;
}
.like {
    width: 10%;
    text-align: center;
}
.main {
    width: 1200px;
    margin: 10px auto;
    list-style: none;
}
.Group {
    display: flex;
    justify-content: space-between;
    border: 2px solid #789AF1;
    border-radius: 10px;
    padding: 10px;
    margin: 15px 0;
}
.controllerBtn {
    width: 1200px;
    margin: 10px auto;
    display: flex;
    align-items: center;
    justify-content: space-between;
}
.controllerBtn > div > input:hover {
    cursor: pointer;
}
.controllerBtn > div :first-child {
    margin: 0;
}
.categoryBtn {
    margin-left: 10px;
    width: 90px;
    height: 37px;
    border: 2px solid #789AF1;
    background-color: white;
    border-radius: 10px;
}
.clickedBtn {
    margin-left: 10px;
    width: 90px;
    height: 37px;
    border: 2px solid #789AF1;
    background-color: #EBEBEB;
    border-radius: 10px;
}
.rectangle {
    height: 50px;
    display: flex;
    align-items: center;
}
#clickedPageNav {
    background-color: #EBEBEB;
    border: 2px solid #789AF1;
    width: 37px;
    height: 37px;
    border: 2px solid #789AF1;
}
.pageNav{
    border: 2px solid #789AF1;
    width: 37px;
    height: 37px;
    background-color: white;
    border: 2px solid #789AF1;
}
.rectangle input:first-child {
    border: 2px solid #789AF1;
    background-color: white;
    border-radius: 10px;
    margin-right: 10px;
}
.rectangle input:last-child {
    border: 2px solid #789AF1;
    background-color: white;
    border-radius: 10px;
    margin-left: 10px;
}
</style>