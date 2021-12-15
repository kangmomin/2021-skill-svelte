<script>
    import {Link} from "svelte-routing"
    
    let tocken
    if (document.cookie.split("; ").find((row) => row.startsWith("tocken"))) 
        tocken = document.cookie.split("; ").find((row) => row.startsWith("tocken")).split("=")[1]
    function deleteTocken() {
        document.cookie = "tocken="
        if (document.cookie.split("; ").find((row) => row.startsWith("tocken"))) 
            tocken = document.cookie.split("; ").find((row) => row.startsWith("tocken")).split("=")[1]
        location.href = "/"
    }
</script>

<div class="header"> <!--헤더-->
    <Link to="/">
        <div class="logo">
            <img src="../img/bb963f70a53da7f53e08ae6c28e0f2dd5963a909179a89eb42ad47e202c2281515a57a5b6eb427ae452c38e44f1ff8b76ae7b8a52ae50a9ce7cee9201d676d07d844da82ba298baa50b8326eedfd9ef718efdf.jpg" alt="">
        </div><!--로고자리-->
    </Link>
    <div class="headerBtn">
        <input class="searchBtn" type="button" value="검색"> <!--검색버튼-->
        {#if tocken}
        <Link to="write-post">
            <input class="loginBtn" type="button" value="글 작성"><!--글작성 버튼-->
        </Link>
        <input class="loginBtn" type="button" value="로그아웃" on:click="{deleteTocken}"><!--글작성 버튼-->
        <div class="vector"></div> <!--프로필사진-->
        {:else}
        <Link to="login">
            <input class="loginBtn" type="button" value="로그인"><!--로그인버튼-->
        </Link >
        {/if}
        
    </div>
</div>

<style>
* {
    margin: 0;
    padding: 0;
}

.header {
    width: 100%;
    height: 75px;
    background-color: #789AF1;
    display: flex;
    align-items: center;
    justify-content: space-between;
}
.logo {
    width: 60px;
    height: 60px;
    border-radius: 50%;
    background-color: white;
    margin-left: 30px;
}
.logo img {
    width: 60px;
    height: 60px;
    border-radius: 50%;
}
.searchBtn {
    width: 70px;
    height: 45px;
    border: 0;
    border-radius: 4px;
    background-color: #7689CD;
    color: white;
    font-size: 20px;
    margin-right: 20px;
}
.loginBtn {
    width: 100px;
    height: 45px;
    border: 0;
    border-radius: 4px;
    background-color: #7689CD;
    color: white;
    font-size: 20px;
    margin-right: 20px;
}
.headerBtn input:hover {
    cursor: pointer;
}
</style>