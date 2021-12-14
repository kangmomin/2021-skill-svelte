<script>
	import {Router, Route} from "svelte-routing"
	import Headers from './component/header.svelte'
	import Post from "./component/postList.svelte"
	import EachPost from "./component/eachPost.svelte"
	import Login from "./component/login.svelte"
	import SignUp from "./component/signUp.svelte"
	import WritePost from "./component/writePost.svelte";
	export let url = ""
</script>

<Router url={url} >
	<Route path="/">
		<Headers />
		<Post></Post>
	</Route>
	<Route path="post/:id" let:params >
		<Headers />
		<EachPost postId={params.id} />
	</Route>
	<Route path="write-post" >
		<Headers />
		<WritePost />
	</Route>
	<Route path="login" >
		<Login></Login>
	</Route>
	<Route path="sign-up">
		<SignUp></SignUp>
	</Route>
</Router>