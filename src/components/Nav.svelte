<script>
	export let segment;
</script>

<nav>
	<a class:selected='{segment === undefined}' href='.'>CSS IV</a>
	<a class:selected='{segment === "about"}' href='about'>What is CSS IV</a>
</nav>


<style>
	nav {
		border-bottom: 1px solid rgba(255,62,0,0.1);
		font-weight: 300;
		padding: 0 1em;
		text-align: center;
	}

	.selected {
		position: relative;
	}
	.selected:after {
		content: "";
		position: absolute;
		bottom: 0;
		left: 0;
		width: 100%;
		background: rgb(255,62,0);
		height: 5px;
	}

	a {
		text-decoration: none;
		padding: 1em 0.5em;
		display: inline;
		line-height: 3rem;
	}
</style>