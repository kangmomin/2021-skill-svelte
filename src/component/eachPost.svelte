<script>
    import { onMount } from 'svelte';
    export let postId
    let tocken, post
    let reply = "", refReply = ""
    let isGood = false
    let replyData = {
        reply: "",
        refReply: ""
    }
    
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
        getGood()
        getReply()
    })
    
    async function getReply() {
        let res = await fetch("https://koldin.myddns.me:8080/reply/" + postId)
        replyData = await res.json()
    }
    
    async function getGood() {
        let res = await fetch("https://koldin.myddns.me:8080/good/" + postId, {
            method:"POST",
            body: JSON.stringify({tocken})
        })
        let goodData = await res.json()
        isGood = false
        if(goodData.isGood > 0) isGood = true
    }
    
    async function addGood() {
        let res = await fetch("https://koldin.myddns.me:8080/add-good/" + postId, {
            method:"POST",
            body: JSON.stringify({ tocken })
        })
        getGood()
    }
    
    async function deleteGood() {
        let res = await fetch("https://koldin.myddns.me:8080/delete-good/" + postId, {
            method:"POST",
            body: JSON.stringify({ tocken })
        })
        getGood()
    }
    
    async function deletePost() {
        let answer = prompt("정말로 삭제하시겠습니까? '네'", "")
        if (answer !== '네') return 0
        let res = await fetch("https://koldin.myddns.me:8080/delete-post?id=" + postId, {
            method: "POST",
            body: JSON.stringify({ tocken })
        })
        
        alert("삭제하였습니다.")
        location.href = "/"
    }
    
    async function writeReply(refReplyId=0) {
        let description = ""
        if (refReplyId == 0) {
            description = reply
            if (reply.length < 1)
                return alert("빈 댓글은 작성할 수 없습니다.")
        } else {
            description = refReply
            if (refReply.length < 1)
                return alert("빈 댓글은 작성할 수 없습니다.")
        }

        let res = await fetch("https://koldin.myddns.me:8080/reply", {
            method: "POST",
            body: JSON.stringify({
                tocken,
                description,
                postId: Number(postId),
                refReplyId
            })
        })
        
        reply = ""
        refReply = ""
        
        alert("작성되었습니다.")
        closeWriteRefReply()
        callReply()
    }
    
    function showWriteRefReply(type, idx, tag = 0) {
        let replyArea = document.querySelectorAll(type)
        closeWriteRefReply()
        replyArea[idx].style.display = "block"
        if (type == ".writeRefReplyArea") refReply = `to USER-${tag}-`
    }
    
    function closeWriteRefReply() {
        reply = ""
        refReply = ""
        let replyArea = document.querySelectorAll(".refReplyWriter")
        let writeRefReply = document.querySelectorAll(".writeRefReplyArea")
        for (let i = 0; i < replyArea.length; i++) 
            replyArea[i].style.display = "none"
        for (let i = 0; i < writeRefReply.length; i++) 
            writeRefReply[i].style.display = "none"
    }
    
    async function callReply() {
        let res = await fetch("https://koldin.myddns.me:8080/reply/" + postId)
        replyData = await res.json()
    }
</script>
<div class="descriptionArea"> <!--제목과 작성자 작성시간 표시되는곳-->
    {#if post}
        <div class="descriptionTitle"> <!--제목-->
            <p>{post.title}</p> 
        </div>
        <div class="descriptionInfoArea"> <!--작성자 정보-->
            <div class="descriptionId"> <!--장성자 아이디-->
                USER-{post.ownerId}
            </div>
            <div class="descriptionInfomation"> <!--조회수 추천수등 -->
                {#if post.isOwner == true}
                    <p on:click={deletePost}>삭제</p>
                {/if}
                <p>조회수 {post.view}</p>
                <p>추천 {post.good}</p>
                <p>댓글 {post.replyCount}</p>
                <p>작성일 {post.created}</p>
            </div>
        </div>
        <div class="description"> <!--메인 내용-->
            <p>
                {@html post.description}
            </p>
            <div class="descriptionBtn">
                {#if tocken.length > 1}
                    {#if isGood}
                        <input class="likeBtn" id="clickedGoodBtn" type="button" value="추천" on:click="{deleteGood}"> <!--추천 버튼-->
                    {:else}
                        <input class="likeBtn" type="button" value="추천" on:click="{addGood}"> <!--추천 버튼-->
                    {/if}
                    <!-- <input class="hateBtn" type="button" value="비추천"> 비추천 버튼 -->
                {/if}
            </div>
        </div>
    {:else}
        <h1 id="loading">Now Loading...</h1>
    {/if}
    
    {#if tocken.length > 1}
        <div class="writeReply">
            <textarea bind:value={reply}></textarea> <!--댓글 작성할내용-->
            <input type="button" value="작성" on:click="{writeReply}"> <!--댓글 작성버튼-->
        </div>
    {/if}
    
    <div class="replyArea"> <!--댓글영역-->
        {#if replyData.reply}
            {#each replyData.reply as {id, created, ownerId, description}, i}
                <div class="replyBox"> <!--댓글 테두리-->
                    <div class="replyInfo"><!--댓글 작성자 정보-->
                        <p class="id"> <!--댓글 작성자 아이디-->
                            USER-{ownerId}
                        </p>
                        <p class="date"> <!--댓글 작성일-->
                            {created}
                        </p>
                    </div>
                    <div class="replyText"> <!--댓글 내용-->
                        {description}
                    </div>
                    <div class="otherBtn"> <!--신고 답글달기버튼-->
                        <input type="button" class="report" value="신고"> <!--신고버튼-->
                        <input type="button" class="ref-replyBtn" value="답글달기"
                            on:click={() => showWriteRefReply(".refReplyWriter", i)}> <!--대댓글버튼-->
                    </div>
                </div>
                <div class="refReplyWriter">
                    <div class="writeRefreply">
                        <div class="writeRefreplyArea" contenteditable="true" bind:innerHTML="{refReply}"></div>
                        <input type="button" value="작성" on:click={() => writeReply(id)}>
                    </div>
                </div>
                {#if replyData.refReply}
                {#each replyData.refReply as {created, ownerId, description, refReplyId}, i}
                {#if refReplyId == id}
                <div class="refReply"> <!--대댓글-->
                    <div class="replyInfo">
                        <p class="id">
                           <span>ㄴ</span> USER-{ownerId}
                        </p>
                        <p class="date">
                            {created}
                        </p>
                    </div>
                    <div class="replyText">
                        {description}
                    </div>
                    <div class="otherBtn">
                        <input type="button" class="reportBtn" value="신고">
                        <input type="button" class="ref-replyBtn" value="답글달기"
                        on:click={() => showWriteRefReply(".writeRefReplyArea", i, ownerId)}>
                    </div>
                </div>
                <div class="writeRefReplyArea">
                    <div class="writeRefreply">
                        <div class="writeRefreplyArea" bind:innerHTML="{refReply}" contenteditable="true"></div>
                        <input type="button" value="작성" on:click={() => writeReply(id)}>
                    </div>
                </div>
                {/if}
                {/each}
                {/if}
            {/each}
        {/if}
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

#clickedGoodBtn {
    width: 85px;
    height: 50px;
    background-color: #cdcdcd;
    border: 2px solid #3455CB;
    border-radius: 10px;
    margin: 5px;
}

.descriptionBtn input:hover {
    cursor: pointer;
}
.writeReply {
  width: 1200px;
  height: 160px;
  margin-top: 15px;
}
.writeReply textarea {
  width: 1200px;
  height: 100px;
  padding: 10px;
  resize: none;
}
.writeReply input {
  width: 65px;
  height: 35px;
  background-color: white;
  border: 1px solid #3a38b2;
  border-radius: 6px;
  float: right;
}

.refReply {
  background-color: #ebebeb;
  border: 1px solid #3a38b2;
  padding: 10px;
  padding-left: 40px;
}
.refReply span {
  color: #cdcdcd;
}

.writeRefreply {
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 10px;
  border: 1px solid #3a38b2;
  height: 90px;
  background-color: #ebebeb;
}
.writeRefreplyArea {
  width: 1118px;
  height: 40px;
  border: 0.5px solid #3a38b2;
  background-color: white;
  border-radius: 5px;
  padding: 5px;
  margin-left: 45px;
}
.writeRefreply input {
  width: 65px;
  height: 35px;
  background-color: white;
  border: 1px solid #3a38b2;
  border-radius: 6px;
  margin-top: 5px;
  margin-left: 94.5%;
}
.writeRefReplyArea {
    display: none;
}
.refReplyWriter {
    display: none;
}
</style>