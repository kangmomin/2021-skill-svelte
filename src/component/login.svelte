<script>
    import {Link} from "svelte-routing"
    let id = ""
    let password = ""
    
    async function login() {
        if(id.length < 1 || password.length < 1) {
            return alert("아이디 혹은 패스워드는 필수 기입칸입니다.")
        }
        
        let res = await fetch("http://koldin.myddns.me:3101/login", {
            method: "post",
            credentials: 'include',
            body: JSON.stringify({
                "accountId": id,
                "accountPassword": password
            })
        })
        
        res = await res.json()

        if (res.err) {
            switch (res.message) {
                case "id is wrong":
                    alert("존재하지 않는 아이디입니다.")
                    return
                    
                case "password is wrong":
                    alert("잘못된 비밀번호입니다.")
                    return
            }
        }

        document.cookie = "tocken=" + res.tocken
        location.href = "/"
    }
</script>

<div class="login"> 
    <div class="logo">
        <img src="../img/bb963f70a53da7f53e08ae6c28e0f2dd5963a909179a89eb42ad47e202c2281515a57a5b6eb427ae452c38e44f1ff8b76ae7b8a52ae50a9ce7cee9201d676d07d844da82ba298baa50b8326eedfd9ef718efdf.jpg" alt="">
    </div> <!--로고자리-->
    <div class="id">
        <input type="text" id="id" bind:value={id} placeholder="아이디"> <!--아이디-->
    </div>
    <div class="password">
        <input type="password" id="password" bind:value={password} placeholder="비밀번호"> <!--비밀번호-->
    </div>
    <div class="controlBtn">
        <Link to="sign-up">
            <input type="button" value="회원가입"> <!--회원가입 버튼-->
        </Link>
        <input type="button" value="로그인" on:click="{login}"> <!--로그인 버튼-->
    </div>
</div>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300&display=swap');
    * {
        margin: 0;
        padding: 0;
        font-family: 'Noto Sans KR', sans-serif;
    }
    .login {
        width: 1000px;
        height: 500px;
        background-color: #cdcdcd;
        margin:200px auto;
        border-radius: 50px;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }
    .logo {
        width: 75px;
        height: 75px;
        background-color: white;
        border-radius: 50%;
        margin-bottom: 30px;
    }
    .logo img {
        width: 75px;
        height: 75px;
        border-radius: 50%;
    }
    .id input {
        width: 500px;
        height: 40px;
        font-size: 35px;
    }
    .password input {
        width: 500px;
        height: 40px;
        font-size: 35px;
        margin: 20px 0;
    }
    .controlBtn input {
        width: 240px;
        height: 40px;
        color: white;
        background-color: #3A38B2;
        border: 0;
        margin: 10px;
        font-size: 28px;
    }
</style>