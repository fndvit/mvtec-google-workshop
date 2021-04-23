<script>
	import Menu from './components/common/Menu.svelte'
	import Test from './test/Test.svelte'
	import Footer from './components/common/Footer.svelte'

</script>

<main>
	<Menu />

	<Test />

	<Footer>
		<div slot="about">
			What's the point of this project?
		</div>
		<div slot="data">
			About the data ...
		</div>
		<div slot="team">
			Student, student, student ...
		</div>
	</Footer>
</main>

<style>
	main {
		padding: 1em;
		padding-bottom: 0;
		margin: 0 auto;
	}
</style>