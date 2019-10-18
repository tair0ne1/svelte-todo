<script>
    import TodoItem from './TodoItem.svelte';

    let newTodoTitle = '';
    let currentFilter = 'all';
    let nextId = 4;

    let filteredTodos = [];

    let todos = [
        {
            id: 1,
            title: 'My first todo',
            completed: false,
        },
        {
            id: 2,
            title: 'My second todo',
            completed: false,
        },
        {
            id: 3,
            title: 'My third todo',
            completed: false,
        },
    ];

    $: todosRemaining = filteredTodos.filter((todo) => !todo.completed).length;
    $: filteredTodos = currentFilter === 'all' ? todos :
                       currentFilter === 'completed' ? todos.filter((todo) => todo.completed) : 
                                                       todos.filter((todo) => !todo.completed);

    function addTodo(event) {
        if (event.key !== 'Enter') {
            return;
        }

        const newTodo = {
            id: nextId,
            title: newTodoTitle,
            completed: false,
        };

        todos = [...todos, newTodo];

        nextId++;
        newTodoTitle = '';
    }

    function removeTodo() {

    }

    function toggleComplete() {

    }
</script>

<div class="container">
    <img src={'/img/TaironeLogo.png'} alt="Logo">

    <h2>Svelte Todo App</h2>
    <input type="text" class="todo-input" id="todoInput" placeholder="Insert some todo item"
           bind:value={newTodoTitle} on:keydown={addTodo} >
    
    {#each filteredTodos as todo}
        <div class="todo-item">
            <TodoItem {...todo} 
                on:deleteTodo={removeTodo}
                on:toggleComplete={toggleComplete}
            />
        </div>
    {/each}

    <div class="inner-container">
        <div>
            <label for="checkTodos">Check All</label>
            <input type="checkbox" id="checkTodos" class="inner-container-input"
             on:change={checkAllTodos}>
        </div>
        <div>
            {todosRemaining} items left!
        </div>
    </div>

    <div class="inner-container">
        <div>
            <button 
                on:click={() => updateFilter('all')} 
                class:active="{currentFilter === 'all'}">All</button>
            <button 
                on:click={() => updateFilter('active')} 
                class:active="{currentFilter === 'active'}">Active</button>
            <button 
                on:click={() => updateFilter('completed')} 
                class:active="{currentFilter === 'completed'}">Completed</button>
        </div>
        <div>
            <button on:click={clearCompleted}>Clear Completed</button>
        </div>
    </div>

</div>