<script>
	import { Button } from "smelte";
	import { TextField } from "smelte";
	import { onMount } from "svelte";
	// import { YoutubeDataAPI } from "youtube-v3-api";
	const API_KEY = "AIzaSyAXl6KBB0aJ1zFGJoQVzl45aXXpySJt8eQ";
	let gapiReady = false,
		mounted = false;

	onMount(() => {
		mounted = true;

		if (gapiReady) {
			loadClient();
		}
		if (gapi.client) {
			gapi.load("client:auth2", function () {
				gapi.auth2.init({
					client_id:
						"765839078612-0pfbtcgjduc7di75ook1i6i0ldtcdoou.apps.googleusercontent.com",
				});
			});
		}
		if(gapi.auth2){
			authenticate()
		}
	});
	function loadClient() {
		let x = gapi.client.setApiKey(API_KEY);
		console.log(`return of gap.client.setApiKey: ${x}`);
		return gapi.client
			.load(
				"https://www.googleapis.com/discovery/v1/apis/youtube/v3/rest"
			)
			.then(
				function () {
					console.log("GAPI client loaded for API");
				},
				function (err) {
					console.error("Error loading GAPI client for API", err);
				}
			);
	}

	function gapiLoaded() {
		gapiReady = true;
	}

	function loadGapiClient() {
		gapi.load("client:auth2", function () {
			gapi.auth2.init({
				client_id:
					"765839078612-0pfbtcgjduc7di75ook1i6i0ldtcdoou.apps.googleusercontent.com",
			});
		});
	}

	/**
	 * Sample JavaScript code for youtube.channels.list
	 * See instructions for running APIs Explorer code samples locally:
	 * https://developers.google.com/explorer-help/guides/code_samples#javascript
	 */

	function authenticate() {
		return gapi.auth2
			.getAuthInstance()
			.signIn({
				scope: "https://www.googleapis.com/auth/youtube.readonly",
			})
			.then(
				function () {
					console.log("Sign-in successful");
				},
				function (err) {
					console.error("Error signing in", err);
				}
			);
	}

	// Make sure the client is loaded and sign-in is complete before calling this method.
	function execute() {
		return gapi.client.youtube.channels.list({}).then(
			function (response) {
				// Handle the results here (response.result has the parsed body).
				console.log("Response", response);
			},
			function (err) {
				console.error("Execute error", err);
			}
		);
	}

	function test() {
		console.log(`testtttty!`);
	}
</script>

<svelte:head>
	<title>YouTube Saver</title>
	<script
		src="https://apis.google.com/js/api.js"
		on:load={gapiLoaded}></script>
</svelte:head>

<h1>YouTube Saver</h1>

<p>
	Archive, curate, and organize any YouTube content: videos, channels,
	playlists, comments, and metadata.
</p>
<h6 class="mt-6">Keyword Search</h6>

<div class="py-2">
	<TextField label="Search" />
	<Button on:click={test}>Button</Button>
</div>

<h6 class="mb-3 mt-6">Auth Client (YouTube API)</h6>
<div class="py-2">
	<!-- <button onclick={() => (authenticate().then(loadClient))}>Auth</button> -->
	<Button light on:click={() => authenticate().then(loadClient)}>Auth</Button>
</div>

<h6 class="mb-3 mt-6">Run query (YouTube API)</h6>
<div class="py-2">
	<Button dark on:click={() => execute()}>Execute</Button>
</div>

<h6 class="mb-3 mt-6">Block</h6>
<div class="py-2">
	<Button color="alert" dark block>Button</Button>
</div>

<h6 class="mb-3 mt-6">Outlined</h6>
<div class="py-2">
	<Button color="secondary" light block outlined>Button</Button>
</div>

<h6 class="mb-3 mt-6">Text</h6>
<div class="py-2">
	<Button text>Button</Button>
</div>

<h6 class="mb-3 mt-6">Disabled</h6>
<div class="py-2">
	<Button block disabled>Button</Button>
</div>

<h6 class="mb-3 mt-6">
	FAB <a href="https://material.io/components/buttons-floating-action-button/"
		>(Floating action button)</a
	>
</h6>
<div class="py-2">
	<Button color="alert" icon="change_history" />
</div>

<h6 class="mb-3 mt-6">Fab flat</h6>
<div class="py-2">
	<Button color="error" icon="change_history" text light flat />
</div>
