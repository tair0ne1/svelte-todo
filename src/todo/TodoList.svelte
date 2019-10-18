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

    function removeTodo(event) {
        todos = todos.filter(todo => todo.id !== event.detail.id);
    }

    function toggleComplete(event) {
        todos = todos.map((todo) => {
            if (todo.id === event.detail.id) {
                todo.completed = !todo.completed;

                return todo;
            }

            return todo;
        });
    }

    function checkAllTodos(event) {
        todos = todos.map(todo => todo = {id: todo.id, title: todo.title, completed: event.target.checked});
    }

    function updateFilter(newFilter) {
        currentFilter = newFilter;
    }

    function clearCompleted() {
        todos = todos.filter(todo => !todo.completed)
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
    }
    button:focus {
        outline: none;
    }
    .active {
        background: lightseagreen;
    }
</style>

<div class="container">
    <h2>Svelte Todo App</h2>
    <input type="text" class="todo-input" id="todoInput" placeholder="Insert some todo item"
           bind:value={newTodoTitle} on:keydown={addTodo} >
    
    {#each filteredTodos as todo}
        <div class="todo-item">
            <TodoItem {...todo} 
                on:removeTodo={removeTodo}
                on:toggleComplete={toggleComplete}
            />
        </div>
    {/each}

    <div class="inner-container">
        <div>
            <label for="checkTodos">
                <input type="checkbox" id="checkTodos" 
                class="inner-container-input"
                on:change={checkAllTodos}> Check All
            </label>
        </div>
        <div>
            {#if todosRemaining === 0}
                All todos completed!
            {:else}
                {todosRemaining} items left!
            {/if}
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