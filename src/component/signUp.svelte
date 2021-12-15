<script>
    let studentId = "", id = "", password = "", name = "", authImgPath = ""
    let files, avatar

    async function overlapChecker(type) {
        let link = "https://koldin.myddns.me:8080/overlap-check/" + type
        let body
        
        if (type === "student-id") {
            if (studentId.length < 5) return alert("학번을 적어주십시오.")
            body = JSON.stringify({
                "studentId": studentId
            })
        } else if (type === "id") {
            if (id.length < 1) return alert("아이디를 적어주십시오.")
            body = JSON.stringify({
                "id": id
            })
        }
        
        let res = await fetch(link, {
            method: "post",
            body: body
        })
        
        let data = await res.json()

        if (data.IsOverlap) return alert("사용이 불가능합니다.")
        alert("사용이 가능합니다.")
    }
    
    async function signUp() {
        if (!id.length || !name.length || !password.length || !studentId.length || !authImgPath.length) {
            return alert("모든 칸을 채워주십시오")
        }
        
        let body = JSON.stringify({ id, name, password, studentId, authImg: authImgPath})
        
        let res = await fetch("https://koldin.myddns.me:8080/sign-up", {
            method: "post",
            body
        })
        
        if (res) {
            alert("가입 완료")
            location.href = "/login"
            return
        }
        
        alert("something is wrong value")
    }
    
    async function imgUpload() {
        let data = files
        let form = new FormData()
        form.append("image", data)
        
        let res = await fetch("https://koldin.myddns.me:8080/upload", {
            method: "post",
            body: form
        })
        
        let path = await res.json()
        authImgPath = path.imgPath
        
        alert("업로드가 완료되었습니다")
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

<div class="root">
    <div class="main">
        <div class="info">
            <div class="id"><!--아이디-->
                <input type="text" placeholder="아이디" bind:value={id}>
                <input type="button" class="duplicate" on:click="{() => overlapChecker('id')}" value="중복체크"> <!--중복체크 버튼-->
            </div>
            <div class="password"> <!--비밀번호-->
                <input type="password" placeholder="비밀번호" bind:value={password}>
            </div>
        </div>
        <div class="otherInfo">
            <input type="text" placeholder="학번 ex)10101" bind:value={studentId}> <!--학번 입력창-->
            <input type="button" class="duplicate" value="중복체크" on:click="{() => overlapChecker('student-id')}">
            <input type="text" placeholder="이름" bind:value={name}> <!--이름 입력창-->
        </div>
        <div class="imgArea">
            <div class="connImg"> <!--삽인된 이미지 확인창?-->
                <img src="{avatar}" alt="" width="280" height="370">
            </div>
            <div class="controlBtn">
                학생증을 업로드 해주세요!
                <br>
                <input type="file" accept="image/*" placeholder="이미지 업로드"
                    on:change={(e) => {onFileSelected(e)}}> <!--파일머시기?-->
                <input type="button" value="이미지 삽입" on:click={imgUpload}> <!--이미지 삽입 버튼-->
            </div>
        </div>
        <br>
        <input type="button" value="회원가입" id="signUpBtn" on:click="{signUp}">
    </div>
</div>

<style>
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300&display=swap');
* {
    margin: 0;
    padding: 0;
    font-family: 'Noto Sans KR', sans-serif;
}

.main {
    width: 700px;
    height: 850px;
    border-radius: 30px;
    background-color: white;
    margin: 40px auto;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}
.id input:first-child {
    width: 350px;
    height: 30px;
    font-size: 28px;
    border-radius: 10px;
    padding: 5px;
}
.duplicate {
    width: 120px;
    height: 40px;
    font-size: 25px;
    border-radius: 10px;
    background-color: #7886FF;
    color: white;
    margin-left: 30px;
    border: 0;
}
.password input {
    width: 505px;
    height: 30px;
    font-size: 28px;
    border-radius: 10px;
    padding: 5px;
    margin-top: 20px;
}
.info {
    width: 520px; 
    height: 130px;
    margin: 0 auto;
    border-bottom: 2px solid #949494;
}
.otherInfo {
    width: 520px;
    margin: 0 auto;
}
.otherInfo input:first-child {
    width: 350px;
    height: 30px;
    font-size: 28px;
    border-radius: 10px;
    padding: 5px;
    margin-top: 20px;
}
.otherInfo input:nth-child(3) {
    width: 350px;
    height: 30px;
    font-size: 28px;
    border-radius: 10px;
    padding: 5px;
    margin-top: 20px;
}
.imgArea {
    width: 520px;
    height: 370px;
    margin-top: 20px;
    display: flex;
    flex-direction: row;
}
.connImg {
    width: 280px;
    height: 370px;
    border: 2px solid #001AFF;
}
.controlBtn {
    display: flex;
    flex-direction: column;
    margin-left: 30px;
    justify-content: end;
}
.controlBtn input:first-child {
    width: 210px;
    height: 40px;
    font-size: 25px;
    border-radius: 10px;
    background-color: white;
    color: black;
    padding: 5px;
    line-height: 20px;
    border: 2px solid #7886FF;
}

#signUpBtn {
    width: 210px;
    height: 40px;
    font-size: 25px;
    border-radius: 10px;
    background-color: white;
    color: black;
    padding: 5px;
    line-height: 20px;
    border: 2px solid #7886FF;
}

.controlBtn input:last-child {
    width: 140px;
    height: 40px;
    font-size: 25px;
    border-radius: 10px;
    background-color: #7886FF;
    color: white;
    margin-top: 20px;
    border: 0;
}
</style>