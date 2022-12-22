<script>
	import { setContext } from 'svelte';
	import Navbar from '$lib/components/Navbar.svelte';
	import ExpenseList from '$lib/components/ExpensesList.svelte';
	import expensesData from '../expenses';
	import Totals from '$lib/components/Totals.svelte';
	import ExpenseForm from '$lib/components/ExpenseForm.svelte';

	let expenses = [...expensesData];

	//reactive
	$: total = expenses.reduce((acc, curr) => {
		console.log(acc, curr);
		return (acc += curr.amount);
	}, 0);

	//function
	function removeExpense(id) {
		expenses = expenses.filter((item) => item.id !== id);
	}

	//context
	setContext('remove', removeExpense);

	function clearExpenses() {
		expenses = [];
	}
</script>

<svelte:head>
	<title>Budget Calculator</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<Navbar />
<main class="content">
	<ExpenseForm />
	<Totals title="total expenses" {total} />
	<ExpenseList {expenses} />
	<button type="button" class="btn btn-primary btn-block" on:click={clearExpenses}
		>clear expenses</button
	>
</main>
