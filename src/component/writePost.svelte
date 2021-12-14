<script>
    let description = "", title = ""

    async function writePost() {
        let tocken
        if (document.cookie.split("; ").find((row) => row.startsWith("tocken"))) 
            tocken = document.cookie.split("; ").find((row) => row.startsWith("tocken")).split("=")[1]
        else
            location.href = "/"

        let body = JSON.stringify({
            tocken,
            description,
            title
        })
        
        let res = await fetch("https://koldin.myddns.me:8080/post", {
            method: "post",
            body
        })
        
        location.href = "/"
        // let data = res.json()
    }
</script>

<!--메인영역-->
<div class="main">
    <input type="text" placeholder="제목" id="title" bind:value="{title}">
    <div class="post"> <!--작성할게시글 영역-->
        <textarea bind:value={description}></textarea>
        <!-- <input type="text" placeholder="이미지 업로드 인풋창">
        <input type="button" value="이미지 업로드"> 업로드버튼 -->
    </div>
    <input class="writeBtn" type="button" value="글 작성" on:click="{writePost}">
</div>
<!--메인end-->

<style>
.main {
  width: 1300px;
  margin: 25px auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
#title {
    width: 1030px;
    border: 2px solid #7689cd;
    border-radius: 20px;
    padding: 10px;
}
.post {
  width: 1100px;
  height: 700px;
  border-bottom: 2px solid #cdcdcd;
  display: flex;
}
.post textarea {
  width: 1030px;
  height: 680px;
  margin: 0 auto;
  border: 2px solid #7689cd;
  border-radius: 20px;
  padding: 10px;
}
/* .post > input:nth-child(2) {
  width: 300px;
  margin-left: 585px;
  border: 2px solid #7689cd;
  padding: 10px;
  margin-top: 20px;
}
.post > input:nth-child(3) {
  width: 120px;
  margin-left: 20px;
  border: 2px solid #7689cd;
  padding: 10px;
  border-radius: 10px;
  background-color: white;
  margin-top: 20px;
} */
.writeBtn {
  width: 120px;
  margin-top: 20px;
  padding: 10px;
  border: 2px solid #7689cd;
  background-color: white;
  border-radius: 10px;
  margin-left: 890px;
}

</style>