<script>
	//Some components
	import Line from '../components/charts/Line.svelte'
	import Area from '../components/charts/Area.svelte'
	import Bars from '../components/charts/Bars.svelte'
	import Multiline from '../components/charts/Multiline.svelte'
	import Scatter from '../components/charts/Scatter.svelte'
	import ScatterCanvas from '../components/charts/ScatterCanvas.svelte'
	import Mapbox from '../components/maps/Mapbox.svelte'
	import Map from '../components/maps/Map.svelte'
	import Scroller from '@sveltejs/svelte-scroller'
  	import Sankey from '../components/charts/Sankey.svelte'
	import {curveMonotoneX} from 'd3-shape'
	import { Button, ButtonGroup, ButtonGroupItem } from 'svelte-materialify';

	//Test data
	import world from '../data/world.json';
	import cases from '../data/covid-cases.json';
	import weather from '../data/weather.json';
	import weather2 from '../data/weather2.json';
	import weather3 from '../data/weather3.json';
	import sankeydata from '../data/sankey-data.js';

	import locale from '@reuters-graphics/d3-locale';
	import { geoWinkel3 } from 'd3-geo-projection';
	import { scaleQuantize } from 'd3-scale';
	import {extent} from 'd3-array';
	
	let color = '#5cc6b2';

	let index = 0, offset, progress;

	let scatterStep, otherStep;

	const loc = new locale('es');
	const format = {
		x: loc.formatTime('%b %e'),
        y: loc.format(',.1d'),
    }

	weather.forEach(d => d.time = new Date(d.time));
	weather2.forEach(d => d.time = new Date(d.time));

	const projection = geoWinkel3()
				.rotate([-11, 0])
				.precision(1);

	const palette = () => {
		const _extent = extent(cases.data, d => d.latest.cases)
		const max = _extent[0] > _extent[1] ? _extent[0] : _extent[1];
		const min = _extent[0] < _extent[1] ? _extent[0] : _extent[1];
		const d = (max-min)/9;

		return scaleQuantize()
				.range(['#ffe461', '#ffc755', '#fea94d', '#f68c4a', '#ea704a', '#da554e', '#c73a55', '#ae1f5f', '#90006c'])
				.domain([... Array(9)].map((_d, i) => min + d * i))
				.nice();
	}

	const steps = [
		{center: [-7.889722,42.782222], zoom:7.5},
		{center: [0,42.782222], zoom:7},
		{center: [-7.889722,41.782222], zoom:6.5}
	]

	const points = [...new Array(2500)]
        .map(d => (
            {
				coords: [
					{ x: Math.random() * 400,
					y: Math.random() * 400,
					width: Math.random() * 16,
					height: Math.random() * 16 },
					{ x: Math.random() * 400,
					y: Math.random() * 400,
					width: Math.random() * 16,
					height: Math.random() * 16 }
				]
        }))

	const otherPoints = [...new Array(800)]
        .map(d => (
            {
				coords: [
					{ x: Math.random() * 400,
					y: Math.random() * 400,
					r: Math.random() * 16 },
					{ x: Math.random() * 400,
					y: Math.random() * 400,
					r: Math.random() * 16 }
				]
        }))
</script>

	<Multiline 
		data={weather2}
		options={
			{
				key:{x: 'time', y: ['ny1', 'sf1', 'au1']},
				format: format,
				color: ['#fc0', '#036', '#f0c'],
				layout: 'col',
				title:'Title',
				desc:'Description',
				curve: curveMonotoneX
			}
		}
	/>

	<Line 
		data={weather}
		options={
			{
				key:{x: 'time', y: 'value'},
				format: format,
				color: color,
				layout: 'col',
				title:'Title',
				desc:'Description',
				curve: curveMonotoneX
			}
		}
	/>

	<Sankey
		data={sankeydata}
		colorNodes={d => '#00bbff'}
		colorLinks={d => '#00bbff35'}
		layout='col'
	/>

	<Area 
		data={weather}
		options={
			{
				key:{x: 'time', y: 'value'},
				format: format,
				color: color,
				layout: 'col',
				title:'Title',
				desc:'Description',
				curve: curveMonotoneX
			}
		}
	/>

	<div class='col'>
		<p>This is an example of how you can do smooth transitions. It uses canvas so you can do a few thousand elements. Instead of the buttons triggering which step is in view, you can use the scroll ...  </p>
		<ButtonGroup activeClass="red white-text" >
			<ButtonGroupItem on:click={() => scatterStep = 0}>Arrange like so</ButtonGroupItem>
			<ButtonGroupItem on:click={() => scatterStep = 1}>Rearrange again</ButtonGroupItem>
		</ButtonGroup>
	</div>
	<ScatterCanvas
		data={points}
		layout='wide'
		step={scatterStep}
		mark='square'
	/>

	<div class='col'>
		<p>This is an example of how you can do smooth transitions. It uses canvas so you can do a few thousand elements. Instead of the buttons triggering which step is in view, you can use the scroll ...  </p>
		<Button on:click={() => otherStep = 0} outlined>
			Arrange like so
		</Button>
		<Button on:click={() => otherStep = 1} outlined>
			Rearrange again
		</Button>
	</div>
	<ScatterCanvas
		data={otherPoints}
		layout='wide'
		step={otherStep}
		mark='circle'
	/>

	<Bars 
		data={weather}
		options={
			{
				key:{x: 'time', y: 'value'},
				format: format,
				color: color,
				layout: 'col',
				title:'Title',
				desc:'Description'
			}
		}
	/>

	<Scatter 
		data={weather3}
		options={
			{
				key:{x: 'pressure', y: 'temperatureHigh', size:'moonPhase'},
				format: format,
				color: color,
				layout: 'col',
				title:'Title',
				desc:'Description'
			}
		}
	/>
	
	<!-- <Scroller top={0} bottom={1} bind:index bind:offset bind:progress>
	<div slot="background">
		<Mapbox 
			options={
				{style:'mapbox://styles/fndvit/ckc1oqzq94nh91imn76jqxcha',
				scrollZoom:false,
				center: [-7.889722,42.782222],
				zoom: 6.5}
			}
			steps={steps}
			index={index}
			accessToken='pk.eyJ1IjoiZm5kdml0IiwiYSI6ImNrYzBzYjhkMDBicG4yc2xrbnMzNXVoeDIifQ.mrdvw_7AIeOwa5IgHLaHJg'
			layout='fs'
		/>
	</div>
	<div slot="foreground">
		<section><p class='col'>This is the first section.</p></section>
		<section><p class='col'>This is the second section.</p></section>
		<section><p class='col'>This is the third section.</p></section>
	</div>
	</Scroller> -->

	<Map 
		data={cases.data}
		map={{json:world, features:'countries'}}
		options={
			{
				scale:palette(),
				projection:projection,
				join:{data:'geoid', map:'alpha3'},
				value:'latest.cases',
				legend:{title: '', format: ''},
				layout:'wide'
			}
		}
	/>
