<script>
	import Navbar from '$lib/components/Navbar.svelte';
	import ExpenseList from '$lib/components/ExpensesList.svelte';
	import expensesData from '../expenses';

	let expenses = [...expensesData];
</script>

<svelte:head>
	<title>Budget Calculator</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<Navbar />
<main class="content">
	<ExpenseList {expenses} />
</main>
