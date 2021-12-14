<script>
    import { onMount } from 'svelte';
    export let postId
    let post
    let tocken
    
    if (document.cookie.split("; ").find((row) => row.startsWith("tocken"))) 
        tocken = document.cookie.split("; ").find((row) => row.startsWith("tocken")).split("=")[1]
        
    onMount(async () => {
        let res = await fetch("https://koldin.myddns.me:8080/post/" + postId, {
            method: "POST",
            body: JSON.stringify({
                tocken
            })
        })
        post = await res.json()
    })
    
    // async function loadPostInfo() {
    //     res = await fetch("http://koldin.myddns.me:3101/post/" + postId)
    //     post = await res.json()
    // }
</script>
<div class="descriptionArea"> <!--제목과 작성자 작성시간 표시되는곳-->
    {#if post}
        <div class="descriptionTitle"> <!--제목-->
            <p>{post.description}</p> 
        </div>
        <div class="descriptionInfoArea"> <!--작성자 정보-->
            <div class="descriptionId"> <!--장성자 아이디-->
                USER-{post.ownerId}
            </div>
            <div class="descriptionInfomation"> <!--조회수 추천수등 -->
                <p>조회수 {post.view}</p>
                <p>추천 {post.good}</p>
                <p>댓글 {post.replyCount}</p>
                <p>작성일 {post.created}</p>
            </div>
        </div>
        <div class="description"> <!--메인 내용-->
            <p>
                {post.description}
            </p>
            <div class="descriptionBtn">
                {#if tocken.length > 1}
                    <input class="likeBtn" type="button" value="추천"> <!--추천 버튼-->
                    <input class="hateBtn" type="button" value="비추천"> <!--비추천 버튼-->
                {/if}
            </div>
        </div>
    {:else}
        <h1 id="loading">Now Loading...</h1>
    {/if}
    <div class="replyArea"> <!--댓글영역-->
        <div class="replyBox"> <!--댓글 테두리-->
            <div class="replyInfo"><!--댓글 작성자 정보-->
                <p class="id"> <!--댓글 작성자 아이디-->
                    US***
                </p>
                <p class="date"> <!--댓글 작성일-->
                    21-12-08 04:49
                </p>
            </div>
            <div class="replyText"> <!--댓글 내용-->
                print("hello World"); ㄹㅇㅋㅋ
            </div>
            <div class="otherBtn"> <!--신고 답글달기버튼-->
                <input type="button" href="#" class="report" value="신고"> <!--신고버튼-->
                <input type="button" href="#" class="ref-replyBtn" value="답글달기"> <!--대댓글버튼-->
            </div>
        </div>
        <div class="refReply"> <!--대댓글-->
            <div class="replyInfo">
                <p class="id">
                   <span>ㄴ</span> US***
                </p>
                <p class="date">
                    21-12-08 04:49
                </p>
            </div>
            <div class="replyText">
                잘못 달았누
            </div>
            <div class="otherBtn">
                <input type="button" href="#" class="reportBtn" value="신고">
                <input type="button" href="#" class="ref-replyBtn" value="답글달기">
            </div>
        </div>
        <div class="replyBox">
            <div class="replyInfo">
                <p class="id">
                    US***
                </p>
                <p class="date">
                    21-12-08 04:49
                </p>
            </div>
            <div class="replyText">
                print("hello World"); ㄹㅇㅋㅋ
            </div>
            <div class="otherBtn">
                <input type="button" href="#" class="report" value="신고">
                <input type="button" href="#" class="ref-replyBtn" value="답글달기">
            </div>
        </div>
        <div class="replyBox">
            <div class="replyInfo">
                <p class="id">
                    US***
                </p>
                <p class="date">
                    21-12-08 04:49
                </p>
            </div>
            <div class="replyText">
                print("hello World"); ㄹㅇㅋㅋ
            </div>
            <div class="otherBtn">
                <input type="button" href="#" class="report" value="신고">
                <input type="button" href="#" class="ref-replyBtn" value="답글달기">
            </div>
        </div>
    </div>
</div>



<style>
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300&display=swap');
* {
    margin: 0;
    padding: 0;
    font-family: 'Noto Sans KR', sans-serif;
}

#loading {
    text-align: center;
    margin-top: 50%;
}

.replyBox {
    display: flex;
    flex-direction: column;
    padding: 10px;
    border: 1px solid #3A38B2;
    margin-top: 10px;
}
.replyBox:first-child {
    margin-top: 25px;
}
.replyInfo {
    display: flex;
}
.id {
    font-weight: bold;
    margin-right: 20px;
}
.date {
    color: #999999;
}
.otherBtn input {
    border: 0;
    color: #949494;
    background-color: rgba(0, 0, 0, 0);
}
.otherBtn input:hover {
    cursor: pointer;
}
.refReply {
    background-color: #ebebeb;
    border: 1px solid #3A38B2;
    padding: 10px;
    padding-left: 40px;
}
.replyInfo span {
    color: #cdcdcd;
}
.descriptionArea {
    width: 1200px;
    height: 85px;
    margin: 0 auto;
    background-color: #3A38B2;
}
.descriptionTitle {
    width: 1100px;
    margin: 0 auto;
    color: white;
    font-size: 35px;
    border-bottom:  2px solid white;
}
.descriptionInfoArea {
    width: 1100px;
    margin: 0 auto;
    color: white;
    font-size: 18px;
    display: flex;
    justify-content: space-between;
}
.descriptionInfomation {
    display: flex;
}
.descriptionInfomation p {
    margin: 0 10px;
}
.description p {
    padding: 50px;
    font-size: 18px;
}
.descriptionBtn {
    display: flex;
    justify-content: center;
    border-bottom: 2px solid rgba(128, 128, 128, 0.541);
}
.descriptionBtn input {
    width: 85px;
    height: 50px;
    background-color: white;
    border: 2px solid #3455CB;
    border-radius: 10px;
    margin: 5px;
    margin-bottom: 40px;
}
.descriptionBtn input:hover {
    cursor: pointer;
}
</style>