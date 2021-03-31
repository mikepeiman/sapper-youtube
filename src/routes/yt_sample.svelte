<script>
	import { Button } from "smelte";
	import { TextField } from "smelte";
	import { onMount } from "svelte";

    const API_KEY = "AIzaSyAXl6KBB0aJ1zFGJoQVzl45aXXpySJt8eQ";
    const CLIENT_ID =
        "765839078612-0pfbtcgjduc7di75ook1i6i0ldtcdoou.apps.googleusercontent.com";
    let mounted = false,
        gapiLoaded = false;

    onMount(() => {
        if (gapiLoaded) {
            console.log(`GAPI loaded`);
            // authenticate().then(loadClient)
            gapi.load("client:auth2", function () {
                gapi.auth2.init({ client_id: CLIENT_ID });
            });
            // console.log(x)
        }
    });

    function loadGapi() {
        mounted = true;
        gapiLoaded = true;
        gapi.load("client:auth2", function () {
            gapi.auth2.init({ client_id: CLIENT_ID });
        });
    }

    function authenticate() {
        let auth = gapi.auth2;
        console.log(`gapi ${gapi}`, gapi);
        console.log(`gapi.auth2 ${auth}`);
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
    function loadClient() {
        gapi.client.setApiKey(API_KEY);
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
    // Make sure the client is loaded and sign-in is complete before calling this method.
    function execute() {
        return gapi.client.youtube.channels
            .list({
                part: ["snippet,contentDetails,statistics"],
                id: ["UC_x5XG1OV2P6uZZ5FSM9Ttw"],
            })
            .then(
                function (response) {
                    // Handle the results here (response.result has the parsed body).
                    console.log("Response", response);
                },
                function (err) {
                    console.error("Execute error", err);
                }
            );
    }
</script>

<svelte:head>
    <script src="https://apis.google.com/js/api.js" on:load={loadGapi}></script>
</svelte:head>

<h1>YouTube Saver</h1>

<p>
	Archive, curate, and organize any YouTube content: videos, channels,
	playlists, comments, and metadata.
</p>

<button onclick="{() => authenticate().then(loadClient)}">authorize and load</button>
<button onclick="{() => execute()}">execute</button>
