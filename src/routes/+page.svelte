<script>
	async function getYesNo() {
		const res = await fetch('https://yesno.wtf/api');
		const text = await res.json();
		if (res.ok) {
			return text;
		} else {
			throw new Error(text);
		}
	}
	let promise = getYesNo();

	function handleClick() {
		promise = getYesNo();
	}
</script>

<div class="content">
	<h1>気まぐれおてんき予報</h1>
	<div class="btn-wrapper">
		<button
			on:click={() => {
				handleClick();
			}}>明日は雨かな？</button
		>
	</div>
	<div class="text-area">
		<p>明日の天気は・・・</p>
		{#await promise}
			<p class="load">Loading...</p>
		{:then answer}
			{#if answer.answer === 'yes'}
				<p class="ans">雨だよ☔</p>
			{:else}
				<p class="ans">晴れだよ🌞</p>
			{/if}
		{:catch error}
			<p class="ans">嵐だよ🍃</p>
		{/await}
	</div>
</div>

<style>
	.btn-wrapper {
		text-align: center;
		margin-bottom: 32px;
	}
	button {
		border: 1px solid #674188;
		border-radius: 10px;
		padding: 8px;
		width: 220px;
		margin: 0 auto;
		color: white;
		background-color: #674188;
	}
	button:hover {
		color: #fffbf5;
		background-color: #c3acd0;
		cursor: pointer;
	}
	h1 {
		color: #674188;
	}
	.content {
		padding: 20px;
	}
	.text-area {
		width: 80%;
		margin: 0 auto;
	}
	.ans,
	.load {
		text-align: center;
	}
	.ans {
		font-size: 24px;
		text-decoration: underline; /* 下線 */
		text-decoration-thickness: 0.5em; /* 線の太さ */
		text-decoration-color: rgba(255, 228, 0, 0.4); /* 線の色 */
		text-underline-offset: -0.2em; /* 線の位置。テキストに重なるようにやや上部にする */
		text-decoration-skip-ink: none; /* 下線と文字列が重なる部分でも下線が省略されない（線が途切れない） */
	}
</style>
