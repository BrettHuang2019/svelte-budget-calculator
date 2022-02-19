<!-- Javascript / Logic -->
<script>
  import { setContext, onMount, afterUpdate } from 'svelte'

  import Github from './Github.svelte'
  import GithubAwait from './GithubAwait.svelte'

  // components
  import Navbar from './Navbar.svelte'
  import ExpensesList from './ExpensesList.svelte'
  import Totals from './Totals.svelte'
  import ExpenseForm from './ExpenseForm.svelte'
  import Modal from './Modal.svelte'

  // data
  import Expense from './Expense.svelte'
  // variables
  let expenses = []

  // set editing variables
  let setName = ''
  let setAmount = null
  let setID = null

  // toggle form variables
  let isFormOpen = false

  // reactive
  $: isEditing = setID ? true : false
  $: total = expenses.reduce((acc, curr) => {
    return (acc = acc += curr.amount)
  }, 0)

  // functions
  function showForm() {
    isFormOpen = true
  }

  function hideForm() {
    isFormOpen = false
  }

  function removeExpense(id) {
    expenses = expenses.filter((item) => item.id !== id)
  }

  function clearExpenses() {
    expenses = []
  }

  function addExpense({ name, amount }) {
    console.log(name, amount)
    // amount = parseInt(amount);
    let expense = {
      id: Math.random() * Date.now(),
      name,
      amount,
    }
    expenses = [expense, ...expenses]
  }

  function setModifiedExpense(id) {
    console.log(id)
    let expense = expenses.find((item) => item.id === id)
    setID = expense.id
    setName = expense.name
    setAmount = expense.amount
    if (!isFormOpen) isFormOpen = true
  }

  function editExpense({ name, amount }) {
    expenses = expenses.map((item) => {
      if (item.id === setID) {
        item.name = name
        item.amount = amount
      }
      return item
    })
    setName = ''
    setAmount = null
    setID = null
  }

  // context
  setContext('remove', removeExpense)
  setContext('modify', setModifiedExpense)

  // local storage
  function setLocalStorage() {
    localStorage.setItem('expenses', JSON.stringify(expenses))
  }
  onMount(() => {
    expenses = localStorage.getItem('expenses')
      ? JSON.parse(localStorage.getItem('expenses'))
      : []
  })
  afterUpdate(()=>{
    console.log("afterUpdate");
    setLocalStorage()
  })
</script>

<!-- CSS / Styling -->

<!-- HTML -->
<Navbar {showForm} />

<GithubAwait />

<!-- <main class="content">
  {#if isFormOpen}
  <Modal>
    <ExpenseForm
    {addExpense}
    name={setName}
    amount={setAmount}
    {isEditing}
    {editExpense}
    {hideForm}
    />
  </Modal>
  {/if}
  <Totals title="total expenses" {total} />
  <ExpensesList {expenses} />

  <button class="btn btn-primary btn-block" on:click={clearExpenses}>
    clear expense
  </button>
</main> -->


