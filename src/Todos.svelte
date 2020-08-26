<script>
  import TodoItem from './TodoItem.svelte'
  import FilterButton from './FilterButton.svelte'

  let newTodoTitle = '';
  let currentFilter = 'all';
  let uid = 1;

  let filterOptions = [
    'all',
    'active',
    'completed'
  ]

  let todos = [
    {
      id: uid++,
      title: 'first todo',
      completed: false
    },
    {
      id: uid++,
      title: 'second todo',
      completed: false
    },
    {
      id: uid++,
      title: 'third todo',
      completed: false
    },
  ];

  function addTodo(event) {
    if (event.key === 'Enter') {
      const todo = {
        id: uid++,
        completed: false,
        title: newTodoTitle
      }
      todos = [...todos, todo ];

      newTodoTitle = '';

    }
  }

  $: todosRemaining = filteredTodos.filter(todo => !todo.completed).length
  $: filteredTodos = currentFilter === 'all' ? todos : currentFilter === 'completed'
    ? todos.filter(todo => todo.completed)
    : todos.filter(todo => !todo.completed)

  function checkAllTodos(event) {
    todos.map(todo => todo.completed = event.target.checked)
    todos = todos;
  }

  function updateFilter(newFilter) {
    currentFilter = newFilter;
  }

  function clearCompleted() {
    todos = todos.filter(todo => !todo.completed);
  }

  function handleDeleteTodo(event) {
    todos = todos.filter(todo => todo.id !== event.detail.id);
  }

  function handleUpdateFilter(event) {
    updateFilter(event.detail.filterOption)
  }

  function handleToggleComplete(event) {
    const todoIndex = todos.findIndex(todo => todo.id === event.detail.id);
    const updatedTodo = {...todos[todoIndex], completed: !todos[todoIndex].completed}

    todos = [
      ...todos.slice(0, todoIndex),
      updatedTodo,
      ...todos.slice(todoIndex+1)
    ]
  }

</script>

<style>
  .container {
    max-width: 800px;
    margin: 10px auto;
  }
  .todo-input {
    width: 100%;
    padding: 10px, 20px;
    font-size: 18px;
    margin-bottom: 20px;
  }

  .inner-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 16px;
    border-top: 1px solid lightgrey;
    padding-top: 15px;
    margin-bottom: 13px;
  }

  .inner-container-input {
    margin-right: 12px;
  }

  button {
    font-size: 14px;
    background-color: white;
    appearance: none;
  }

  button:hover {
    background: lightseagreen;
    color: white;
  }

  button:focus {
    outline: none;
  }

  .active {
    background: lightskyblue;

  }

</style>

<div class='container'>
  <h2>Todos</h2>
  <input on:keydown={addTodo} bind:value={newTodoTitle} type="text" class="todo-input" placeholder="insert todo item ..."/>

  {#each filteredTodos as todo}
    <div class='todo-item'>
      <TodoItem {...todo} on:deleteTodo={handleDeleteTodo} on:toggleComplete={handleToggleComplete}/>
    </div>
  {/each}

  <div class="inner-container">
    <div>
      <label>
        <input type="checkbox" class="inner-container-input" on:change={checkAllTodos}/>Check All</label>
        <div>
          {todosRemaining} items left
        </div>
    </div>
  </div>

  <div class="inner-container">
    <div>
      {#each filterOptions as filterOption}
        <FilterButton {filterOption} {currentFilter} on:updateFilter={handleUpdateFilter} />
      {/each}
    </div>
    <div>
      <button on:click={clearCompleted}>Clear Completed</button>
    </div>
  </div>
</div>
