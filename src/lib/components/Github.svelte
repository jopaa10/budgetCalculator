<script>
	import { onMount } from 'svelte';

	let users = [];
	let loading = true;

	onMount(async () => {
		let usersData = await fetch(`https://api.github.com/users`);
		let githubUsers = await usersData.json();
		users = githubUsers;
		loading = false;
	});
</script>

{#if loading}
	<h2>Loading ...</h2>
{:else}
	<section>
		{#each users as user}
			<article class="user">
				<img src={user.avatar_url} alt={user.login} />
				<div class="user-info">
					<h3>User: {user.login}</h3>
					<a href={user.html_url} class="btn-primary" target="blank"> Github url </a>
				</div>
			</article>
		{/each}
	</section>
{/if}

<style>
	h2 {
		text-align: center;
	}
</style>
