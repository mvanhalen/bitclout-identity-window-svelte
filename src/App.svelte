<script>
	import {onMount} from 'svelte'
	
	const identityUrl ='https://identity.bitclout.com'
	
	let identityWindow

	let user;
	let userPayload;
	let loggedIn = false;

		

	onMount(async ()=>{

		
		
	})
	
	function launchLogin(){
			const h = 1000;
			const w = 800;
			const y = window.outerHeight / 2 + window.screenY - h / 2;
			const x = window.outerWidth / 2 + window.screenX - w / 2;

			window.addEventListener('message', (event) => {
				console.log(event)
				//do we have data
				if(event.data && event.origin){
					//can we trust it
					if(event.origin ==='https://identity.bitclout.com'){
						
						//initialize
						if(event.data.method ==='initialize'){
							console.log('initialize')
						
							const data = {id:event.data.id,service: 'identity'};
							console.log(data)
							
							console.log(location.origin)
							identityWindow.postMessage(data,'*');
							//event.source.postMessage(data,location.origin)
						}

						//login
						if(event.data.method ==='login'){
							user = event.data.payload.publicKeyAdded;
							console.log(user);
							userPayload = event.data.payload.users[user];
							console.log(userPayload);

							loggedIn = true;
							identityWindow.close();

						}


					}
					
				}
			
			},false);
		

			identityWindow = window.open(identityUrl + '/log-in', null, `toolbar=no, width=${w}, height=${h}, top=${y}, left=${x}`);
		
			

		}
	
</script>

<main>
	{#if loggedIn}
		<p>{user}</p>

	{:else}
		<p><a on:click={launchLogin}>Login</a></p>
	{/if}
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	
	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>