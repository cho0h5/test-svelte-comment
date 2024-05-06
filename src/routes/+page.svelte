<script>
	let comments = (async () => {
		const res = await fetch('http://home.cho0h5.org:8080/comment');
		return await res.json();
	})();

	let content = '';

	async function send() {
		const res = await fetch('http://home.cho0h5.org:8080/comment/' + content);
		if (res.ok) {
			content = '';
			comments = (async () => {
				const res = await fetch('http://home.cho0h5.org:8080/comment');
				return await res.json();
			})();
		}
	}
</script>

<svelte:head>
	<title>Home</title>
	<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bulma@1.0.0/css/bulma.min.css">
</svelte:head>

<section class="hero is-link">
	<div class="hero-body">
			<h1 class="title">
				이슈많음
			</h1>
</section>

<section class="section">
	<form class="field">
		<label class="label">
			댓글
			<input class="input" bind:value={content} placeholder="hello~">
		</label>
		<button class="button is-link" on:click={send}>작성</button>
	</form>
</section>

<section class="section">
	{#await comments}
		<p>loading...</p>
	{:then comments}
	<table class="table">
		<tbody>
		{#each comments.reverse() as comment}
				<tr><td>{comment.content}</td></tr>
		{/each}
		</tbody>
	</table>
	{:catch error}
		<p>{error.message}</p>
	{/await}
</section>