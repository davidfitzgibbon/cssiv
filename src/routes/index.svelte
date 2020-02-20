<script context="module">
	export function preload({ params, query }) {
		return this.fetch(`https://raw.githubusercontent.com/Fyrd/caniuse/master/data.json`).then(r => r.json()).then(data => {
			let cats = data.cats.CSS.reverse();
			let items = [];
			let browsers =  {
				"ie": {"6":"2006","7":"2009","8":"2011","9":"2012","10":"2013","11":"2015","5.5":"2001"},
				"edge": {"12":"2015","13":"2015","14":"2016","15":"2017","16":"2017","17":"2018","18":"2020","79":"2020","80":"2020"},
				"firefox": {"2":"2006","3":"2008","4":"2011","5":"2011","6":"2011","7":"2011","8":"2011","9":"2011","10":"2012","11":"2012","12":"2012","13":"2012","14":"2012","15":"2012","16":"2012","17":"2012","18":"2013","19":"2013","20":"2013","21":"2013","22":"2013","23":"2013","24":"2013","25":"2013","26":"2013","27":"2014","28":"2014","29":"2014","30":"2014","31":"2014","32":"2014","33":"2014","34":"2014","35":"2015","36":"2015","37":"2015","38":"2015","39":"2015","40":"2015","41":"2015","42":"2015","43":"2015","44":"2016","45":"2016","46":"2016","47":"2016","48":"2016","49":"2016","50":"2016","51":"2017","52":"2017","53":"2017","54":"2017","55":"2017","56":"2017","57":"2017","58":"2018","59":"2018","60":"2018","61":"2018","62":"2018","63":"2018","64":"2018","65":"2019","66":"2019","67":"2019","68":"2019","69":"2019","70":"2019","71":"2019","72":"2020","73":"2020","74":"2020","3.5":"2009","3.6":"2010"},
				"chrome": {"4":"2010","5":"2010","6":"2010","7":"2010","8":"2010","9":"2011","10":"2011","11":"2011","12":"2011","13":"2011","14":"2011","15":"2011","16":"2011","17":"2012","18":"2012","19":"2012","20":"2012","21":"2012","22":"2012","23":"2012","24":"2013","25":"2013","26":"2013","27":"2013","28":"2013","29":"2013","30":"2013","31":"2013","32":"2014","33":"2014","34":"2014","35":"2014","36":"2014","37":"2014","38":"2014","39":"2014","40":"2015","41":"2015","42":"2015","43":"2015","44":"2015","45":"2015","46":"2015","47":"2015","48":"2016","49":"2016","50":"2016","51":"2016","52":"2016","53":"2016","54":"2016","55":"2016","56":"2017","57":"2017","58":"2017","59":"2017","60":"2017","61":"2017","62":"2017","63":"2017","64":"2018","65":"2018","66":"2018","67":"2018","68":"2018","69":"2018","70":"2018","71":"2018","72":"2019","73":"2019","74":"2019","75":"2019","76":"2019","77":"2019","78":"2019","79":"2019","80":"2020","81":"2020","82":"2020","83":"2020"},
				"safari": {"4":"2009","5":"2010","6":"2012","7":"2013","8":"2014","9":"2015","10":"2016","11":"2017","12":"2018","13":"2019","3.1":"2008","3.2":"2008","5.1":"2011","6.1":"2013","7.1":"2014","9.1":"2016","10.1":"2017","11.1":"2018","12.1":"2019"},
				"opera": {"9":"2006","9.5-9.6":"2007","10.0-10.1":"2009","10.5":"2009","10.6":"2010","11":"2010","11.1":"2010","11.5":"2010","11.6":"2011","12":"2011","12.1":"2012","15":"2013","16":"2013","17":"2013","18":"2013","19":"2013","20":"2014","21":"2014","22":"2014","23":"2014","24":"2014","25":"2014","26":"2014","27":"2015","28":"2015","29":"2015","30":"2015","31":"2015","32":"2015","33":"2015","34":"2015","35":"2016","36":"2016","37":"2016","38":"2016","39":"2016","40":"2016","41":"2016","42":"2016","43":"2017","44":"2017","45":"2017","46":"2017","47":"2017","48":"2017","49":"2017","50":"2018","51":"2018","52":"2018","53":"2018","54":"2018","55":"2018","56":"2018","57":"2018","58":"2019","60":"2019","62":"2019","63":"2019","64":"2019","65":"2019","66":"2020"},
				"ios_saf": {"3.2":"2010","4.0-4.1":"2010","4.2-4.3":"2010","5.0-5.1":"2011","6.0-6.1":"2012","7.0-7.1":"2013","8":"2014","8.1-8.4":"2014","9.0-9.2":"2015","9.3":"2016","10.0-10.2":"2016","10.3":"2017","11.0-11.2":"2017","11.3-11.4":"2018","12.0-12.1":"2018","12.2-12.4":"2019","13.0-13.1":"2019","13.2":"2019","13.3":"2019"},
				"android": {"2.1":"2009","2.2":"2010","2.3":"2010","3":"2011","4":"2011","4.1":"2012","4.2-4.3":"2012","4.4":"2013","4.4.3-4.4.4":"2013"},
				"bb": {"7":"2011","10":"2013"},
				"op_mob": {"10":"2009","11":"2011","11.1":"2011","11.5":"2011","12":"2012","12.1":"2012"},
				"and_chr": {"79":"2019"},
				"and_ff": {"68":"2019"},
				"ie_mob": {"10":"2012","11":"2014"},
				"and_uc": {"12.12":"2020"},
				"samsung": {"4":"2016","5.0-5.4":"2016","6.2-6.4":"2017","7.2-7.4":"2018","8.2":"2018","9.2":"2019","10.1":"2019"},
				"and_qq": {"1.2":"2010"},
				"kaios": {"2.5":"2016"}
			};
			for (var key in data.data) {
				if (data.data.hasOwnProperty(key)) {
					var item = data.data[key];

					// get categories
					let hasCSSCat = false;
					for ( let cat of item.categories ) {
						if ( cats.indexOf(cat) > -1 ) hasCSSCat = true;
					}
					if ( hasCSSCat ) {
						var yearsCounter = {};
						var years = [];

						for ( let browser in browsers) {
							for( let version in browsers[browser] ) {
								if (item.stats[browser][version] == "y") {
									if ( !yearsCounter.hasOwnProperty(browsers[browser][version])) {
										yearsCounter[browsers[browser][version]]=0;
									}
									yearsCounter[browsers[browser][version]]++;
									if ( yearsCounter[browsers[browser][version]] == 2) {
										years.push(browsers[browser][version]);
									}
								}
							}
						}

						item.years = years.sort();
						if(years.length) {
							items.push(item);
						}
					}
				}
			}
			items.sort(function(a, b) {
				var A = a.years[0];
				var B = b.years[0];

				if (A < B) {
					return 1;
				}
				if (A > B) {
					return -1;
				}
				return 0;
			})
			return { data:data, items:items };
		});
	}
</script>

<script>
	export let data;
	export let items;

	let statuses = [
		{
			"name":"rec",
			"active": true
		},
		{
			"name":"ietf",
			"active": true
		},
		{
			"name":"ls",
			"active": true
		},
		{
			"name":"pr",
			"active": true
		},
		{
			"name":"cr",
			"active": true
		},
		{
			"name":"wd",
			"active": true
		},
		{
			"name":"unoff",
			"active": true
		},
		{
			"name":"other",
			"active": true
		}
	];

	$: sorted = items;

	$: resort = function(e) {
		let type = e.target.id;
		
		if ( type == selected ) {
			dir = !dir;
		}
		if ( type == "title" ) {
			sorted = sorted.sort(function(a, b) {
				var A = a.title.toUpperCase();
				var B = b.title.toUpperCase();
				if (A < B) {
					return dir? 1 : -1;
				}
				if (A > B) {
					return dir? -1 : 1;
				}
				return 0;
			})
		}
		if ( type == "first" ) {
			sorted = sorted.sort(function(a, b) {
				var A = a.years[0];
				var B = b.years[0];

				if (A < B) {
					return dir? 1 : -1;
				}
				if (A > B) {
					return dir? -1 : 1;
				}
				return 0;
			})
		}
		if ( type == "status" ) {
			sorted = sorted.sort(function(a, b) {
				var A = a.status;
				var B = b.status;

				if (
					A != "rec" && B == "rec" ||
					A != "rec" && B == "ietf" ||
					A != "ietf" && B == "rec" ||
					A != "ietf" && B == "ietf" ||

					A == "ls" && B == "rec" ||
					A == "ls" && B == "ietf" ||

					A == "pr" && B == "rec" ||
					A == "pr" && B == "ietf" ||
					A == "pr" && B == "ls" ||

					A == "cr" && B == "rec" ||
					A == "cr" && B == "ietf" ||
					A == "cr" && B == "ls" ||
					A == "cr" && B == "pr" ||

					A == "wd" && B == "rec" ||
					A == "wd" && B == "ietf" ||
					A == "wd" && B == "ls" ||
					A == "wd" && B == "pr" ||
					A == "wd" && B == "cr" ||

					A == "unoff" && B != "unoff" ||
					A == "other"
				) {
					return dir? 1 : -1;
				}
				if (
					B != "rec" && A == "rec" ||
					B != "rec" && A == "ietf" ||
					B != "ietf" && A == "rec" ||
					B != "ietf" && A == "ietf" ||

					B == "ls" && A == "rec" ||
					B == "ls" && A == "ietf" ||

					B == "pr" && A == "rec" ||
					B == "pr" && A == "ietf" ||
					B == "pr" && A == "ls" ||

					B == "cr" && A == "rec" ||
					B == "cr" && A == "ietf" ||
					B == "cr" && A == "ls" ||
					B == "cr" && A == "pr" ||

					B == "wd" && A == "rec" ||
					B == "wd" && A == "ietf" ||
					B == "wd" && A == "ls" ||
					B == "wd" && A == "pr" ||
					B == "wd" && A == "cr" ||
					B == "unoff" && A != "unoff" ||
					B == "other"
				) {
					return dir? -1 : 1;
				}
				return 0;
			})
		}
		selected = type;
	}

	$: isNotFiltered = function(item) {
		var showable = false;

		for ( let status of statuses) {
			if ( status.name == item.status && status.active ) {
				showable = true;
			}
		}
		return showable;
	}

	let dir = true;

	let selected = "first";
</script>

<svelte:head>
	<title>CSS IV ( Inferred Versions )</title>
</svelte:head>

<h1>CSS IV<br>( Inferred Versions )</h1>
<p><a rel="prefetch" href="about">Explanation</a></p>

<ul class=statuses>
	{#each statuses as status, index}
		<li><button class:active={status.active} on:click="{() => status.active = !status.active}">{status.name}</button></li>
	{/each}
</ul>

<table>
	<tr class={dir ? "up" : "down"}>
		<th id='title' class:selected={selected == 'title'} on:click={resort}>Title</th>
		<th id='first' class:selected={selected == 'first'} on:click={resort}>First release</th>
		<th id='status' class:selected={selected == 'status'} on:click={resort}>Status</th>
	</tr>

	{#each sorted as item, index}
		{#if selected == "first"}
			{#if index == 0 || item.years[0] !== sorted[index - 1].years[0]}
				<tr>
					<td class="year" colspan="3">{item.years[0]}</td>
				</tr>
			{/if}
		{/if}
		{#if isNotFiltered(item)}
		<tr>
			<td>{item.title}</td>
			<td>{item.years[0]}</td>
			<td><span class={item.status} title={data.statuses[item.status]}>{item.status}</span></td>
		</tr>
		{/if}
	{/each}
</table>

<style>
	* {
		text-align: center;
	}
	.statuses {
		display: flex;
		width: 100%;
		list-style: none;
		margin-top: 3rem;
		padding: 0 0 1rem;
		border-bottom: 1px solid #eee;
		margin-bottom: 1rem;
	}
	.statuses li {
		padding: 0;
		flex: 1;
		text-align: center;
	}
	.statuses button {
		padding: .5em 1em;
		border: 1px solid lightblue;
		border-radius: .5em;
		min-width: 4em;
		background: white;
		color: lightskyblue;
		cursor: pointer;
	}
	.statuses button:hover {
		border-color: #039ed0;
	}

	.statuses button.active {
		color: green;
		border-color: green;
		background: lightgreen;
	}

	table {
		text-align: center;
		border-collapse: collapse;
		width: 100%;
	}
	th:first-child,
	td:first-child {
		text-align: left;
	}

	table th {
		border-bottom: 1px solid #eee;
		padding: 0 2rem;
		position: relative;
	}
	th:hover {
		cursor: pointer;
	}
	td {
		border-bottom: 1px solid #f5f5f5;
	}

	.selected {
		background: wheat;
	}

	.up .selected:after,
	.down .selected:after {
		content: "";
		position: absolute;
		right: 1rem;
		top: 50%;
		margin-top: -0.2rem;
		width: 0;
		height: 0;
		border-left: .5rem solid transparent;
		border-right: .5rem solid transparent;
	}
	.up .selected:after {
		border-top: .5rem solid #666;
	}
	.down .selected:after {
		border-bottom: .5rem solid #666;
	}

	td.year {
		background: hsl(59.1, 86.7%, 73.1%);;
		color: #333;
		text-align: center;
		margin-top: 1rem;
	}
	
	.rec,
	.ietf,
	.ls{background:#007700}
	.pr{background:#387700}
	.cr{background:#777700}
	.wd{background:#770000}
	.unoff{background:#777777}

	span {
		background: #666;
		color: white;
		padding:0 1rem;
		width: 3rem;
		text-align: center;
		display: inline-block;
	}
</style>