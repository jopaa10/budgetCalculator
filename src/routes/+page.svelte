<script>
	import { setContext, onMount, afterUpdate } from 'svelte';
	import Navbar from '$lib/components/Navbar.svelte';
	import ExpenseList from '$lib/components/ExpensesList.svelte';
	//import expensesData from '../expenses';
	import Totals from '$lib/components/Totals.svelte';
	import ExpenseForm from '$lib/components/ExpenseForm.svelte';
	import Modal from '$lib/components/Modal.svelte';
	//import Github from '$lib/components/Github.svelte';
	import GithubAwait from '$lib/components/Github.svelte';

	//let expenses = [...expensesData];
	let expenses = [];

	//toggle form variables
	let isFormOpen = false;

	//reactive
	$: isEditing = setId ? true : false;

	$: total = expenses.reduce((acc, curr) => {
		console.log(acc, curr);
		return (acc += curr.amount);
	}, 0);

	//set editing variables
	let setName = '';
	let setAmount = null;
	let setId = null;

	//function
	function showForm() {
		isFormOpen = true;
	}

	function hideForm() {
		isFormOpen = false;
		setAmount = null;
		setName = '';
		setId = null;
	}

	function removeExpense(id) {
		expenses = expenses.filter((item) => item.id !== id);
	}

	function addExpense() {
		let expense = { id: Math.random() * Date.now(), name: setName, amount: setAmount };
		expenses = [expense, ...expenses];
	}

	function setModifiedExpense(id) {
		let expense = expenses.find((item) => item.id === id);

		setId = expense.id;
		setName = expense.name;
		setAmount = expense.amount;
		showForm();
	}

	function editExpense() {
		expenses = expenses.map((item) => {
			return item.id === setId ? { ...item, name: setName, amount: setAmount } : { ...item };
		});
	}

	//context
	setContext('remove', removeExpense);
	setContext('modify', setModifiedExpense);

	//local storage
	function setLocalStorage() {
		localStorage.setItem('expenses', JSON.stringify(expenses));
	}

	function clearExpenses() {
		expenses = [];
	}

	function handleSubmit() {
		if (isEditing) {
			editExpense();
		} else {
			addExpense();
		}

		setId = null;
		setAmount = null;
		setName = '';
		hideForm();
	}

	onMount(() => {
		expenses = localStorage.getItem('expenses') ? JSON.parse(localStorage.getItem('expenses')) : [];
	});

	afterUpdate(() => {
		setLocalStorage();
	});
</script>

<svelte:head>
	<title>Budget Calculator</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<Navbar {showForm} />
<main class="content">
	<GithubAwait />
	<!-- {#if isFormOpen}
		<Modal>
			<ExpenseForm
				{hideForm}
				bind:name={setName}
				bind:amount={setAmount}
				{isEditing}
				{handleSubmit}
			/>
		</Modal>
	{/if}
	<Totals title="total expenses" {total} />
	<ExpenseList {expenses} />
	<button type="button" class="btn btn-primary btn-block" on:click={clearExpenses}
		>clear expenses</button
	> -->
</main>
