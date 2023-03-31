<script>
	import Router from "svelte-spa-router"
	import {ExampleCamps} from './stores/ExampleCamps';

	import {onMount, onDestroy} from "svelte"
	import Navbar from "./components/Navbar.svelte"
	import Home from "./pages/Home.svelte"
    import SearchResults from "./components/searchResults.svelte";
	// Import the functions you need from the SDKs you need
	import { initializeApp } from "firebase/app";
	import { getAnalytics } from "firebase/analytics";
	import Login from "./pages/login.svelte"
// TODO: Add SDKs for Firebase products that you want to use
// https://firebase.google.com/docs/web/setup#available-libraries

// Your web app's Firebase configuration
// For Firebase JS SDK v7.20.0 and later, measurementId is optional
	const firebaseConfig = {
	apiKey: "AIzaSyA8XWvQBSMD6z40etxPsMqf19SSkI6GirI",
	authDomain: "camps-f0abf.firebaseapp.com",
	projectId: "camps-f0abf",
	storageBucket: "camps-f0abf.appspot.com",
	messagingSenderId: "903786744191",
	appId: "1:903786744191:web:ea174177e51eb9d6b1f24a",
	measurementId: "G-Y5F19JPC9M"
	};

	// Initialize Firebase
	const app = initializeApp(firebaseConfig);
	const analytics = getAnalytics(app);

	let camps = [];

	const unsubscribeCamps = ExampleCamps.subscribe(data => {
		camps = data;
	})



	onDestroy(() => {
		unsubscribeCamps();

	})
	const logout = async(auth0) => {
		await auth0.logout({
			returnTo: window.location.origin
		})
	}

	const handleLogout = async() => {
		if (!auth0) {return}
		await logout(auth0);
		isLoggedIn = await auth0.isAuthenticated()
	}

</script>

<main>
	<div class="app">
		<Navbar/>
		<Router routes={{
			"/":Home,
			"/search/:query":SearchResults,
			"/login":Login
		}}/>
	</div>

</main>

<style>
	.app {
		position: relative;
		background: linear-gradient(to bottom right, #ca01e1, #e50301);
		min-height: 98.5vh;
		background-size: cover;
	}
</style>