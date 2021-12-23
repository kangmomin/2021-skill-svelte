<script>
    let description = "", title = "", avatar, files

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
        
        if(res.ok)
          return location.href = "/"
        console.log(res.json())
    }
    
    async function imageUpload() {
      let data = files
        let form = new FormData()
        form.append("image", data)
        
        let res = await fetch("https://koldin.myddns.me:8080/image", {
            method: "post",
            body: form
        })
        
        let path = await res.json()
        let authImgPath = path.imgPath
        
        let des = document.querySelector(".mainContent")
        des.innerHTML += `<img src='https://koldin.myddns.me:8080/public/${path.imgPath}' style="max-width: 100%">`
    }

    const onFileSelected = (e)=>{
      files = e.target.files[0]
      let reader = new FileReader()
      reader.readAsDataURL(files)
      reader.onload = e => {
        avatar = e.target.result
      }
    }
</script>

<!--메인영역-->
<div class="main">
    <input type="text" placeholder="제목" class="title" bind:value="{title}">
    <div class="post"> <!--작성할게시글 영역-->
        <div class="mainContent" bind:innerHTML={description} contenteditable="true"></div>
        <div>
          <input class="uploadArea" type="file" accept="image/*" on:change={(e) => {onFileSelected(e)}} placeholder="이미지 업로드 인풋창">
          <input class="uploadBtn" type="button" on:click="{imageUpload}" value="이미지 업로드">
        </div>
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
.post {
  width: 1100px;
  height: 800px;
  border-bottom: 2px solid #cdcdcd;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.mainContent {
  width: 1030px;
  height: 680px;
  border: 2px solid #7689cd;
  border-radius: 20px;
  padding: 10px;
  overflow: auto;
}
.uploadArea {
  width: 300px;
  margin-left: 585px;
  border: 2px solid #7689cd;
  padding: 10px;
  margin-top: 20px;
}
.uploadBtn {
  width: 120px;
  margin-left: 20px;
  border: 2px solid #7689cd;
  padding: 10px;
  border-radius: 10px;
  background-color: white;
  margin-top: 20px;
}
.writeBtn {
  width: 120px;
  margin-top: 20px;
  padding: 10px;
  border: 2px solid #7689cd;
  background-color: white;
  border-radius: 10px;
  margin-left: 72%;
}
.title {
  width: 1030px;
  height: 20px;
  border: 2px solid #7689cd;
  padding: 20px;
  border-radius: 10px;
}
</style>