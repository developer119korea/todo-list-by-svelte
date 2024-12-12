<script lang="ts">
	import { Accordion, AccordionItem, Avatar } from '@skeletonlabs/skeleton';
	import '../app.css';

	interface Todo {
		id: number;
		text: string;
		completed: boolean;
		isEditing?: boolean;
	}

	let todos: Todo[] = ['첫번째 할일', '두번째 할일'].map((text, index) => ({
		id: index + 1,
		text,
		completed: false
	}));

	let newTodo: string = '';
	let activeTab: 'active' | 'completed' = 'active';

	function addTodo(): void {
		if (newTodo.trim()) {
			const id = todos.length ? todos[todos.length - 1].id + 1 : 1;
			todos = [...todos, { id, text: newTodo.trim(), completed: false }];
			newTodo = '';
		}
	}

	function handleKeydown(e: KeyboardEvent): void {
		if (e.key === 'Enter' && !e.isComposing) {
			addTodo();
		}
	}

	function updateTodo(id: number, newText: string): void {
		todos = todos.map((todo) => {
			if (todo.id === id && newText.trim()) {
				return { ...todo, text: newText.trim() };
			}
			return todo;
		});
	}

	function toggleTodo(id: number): void {
		todos = todos.map((todo) => {
			if (todo.id === id) {
				return { ...todo, completed: !todo.completed };
			}
			return todo;
		});
	}

	function deleteTodo(id: number): void {
		todos = todos.filter((todo) => todo.id !== id);
	}

	function setActiveTab(tab: 'active' | 'completed'): void {
		activeTab = tab;
	}
</script>

<main class="max-w-2xl mx-auto p-4 mt-8 bg-gray-100 min-h-screen">
	<h1 class="text-3xl font-bold text-gray-800 mb-6">미리알림</h1>

    <span class="badge variant-filled">Badge</span>

    <Accordion>
        <AccordionItem open>
            <svelte:fragment slot="lead">(icon)</svelte:fragment>
            <svelte:fragment slot="summary">(summary)</svelte:fragment>
            <svelte:fragment slot="content">(content)</svelte:fragment>
        </AccordionItem>
        <AccordionItem>
            <svelte:fragment slot="lead">(icon)</svelte:fragment>
            <svelte:fragment slot="summary">(summary)</svelte:fragment>
            <svelte:fragment slot="content">(content)</svelte:fragment>
        </AccordionItem>
        <!-- ... -->
    </Accordion>

    <Avatar src="https://images.unsplash.com/photo-1617296538902-887900d9b592?ixid=M3w0Njc5ODF8MHwxfGFsbHx8fHx8fHx8fDE2ODc5NzExMDB8&ixlib=rb-4.0.3&w=128&h=128&auto=format&fit=crop" width="w-32" rounded="rounded-full" />

	<!-- Tab Navigation -->
	<div class="flex mb-4">
		<button
			on:click={() => setActiveTab('active')}
			class={`px-4 py-2 rounded-t-lg ${activeTab === 'active' ? 'bg-white text-blue-500' : 'bg-gray-200 text-gray-500'}`}
		>
			할일들
		</button>
		<button
			on:click={() => setActiveTab('completed')}
			class={`px-4 py-2 rounded-t-lg ${activeTab === 'completed' ? 'bg-white text-blue-500' : 'bg-gray-200 text-gray-500'}`}
		>
			완료됨
		</button>
	</div>

	<ul class="bg-white shadow overflow-hidden sm:rounded-md divide-y divide-gray-200">
		{#each todos as todo (todo.id)}
			{#if activeTab === 'active' && !todo.completed}
				<li class="px-6 py-4 flex items-center gap-4">
					<label class="relative inline-flex items-center cursor-pointer">
						<input
							type="checkbox"
							checked={todo.completed}
							on:change={() => toggleTodo(todo.id)}
							class="sr-only peer"
						/>
						<div
							class="w-5 h-5 bg-white border-2 border-gray-300 rounded-full peer-checked:bg-blue-500 peer-checked:border-blue-500"
						></div>
					</label>
					<input
						type="text"
						value={todo.text}
						on:input={(e) => updateTodo(todo.id, e.currentTarget.value)}
						class={`flex-1 bg-transparent focus:outline-none focus:ring-0 border-0 ${
							todo.completed ? 'text-gray-400 line-through' : 'text-gray-700'
						}`}
					/>
					<button
						on:click={() => deleteTodo(todo.id)}
						aria-label="할일 삭제"
						class="opacity-0 group-hover:opacity-100 text-gray-400 hover:text-red-500 transition-all duration-200"
					>
						삭제
					</button>
				</li>
			{/if}
			{#if activeTab === 'completed' && todo.completed}
				<li class="px-6 py-4 flex items-center gap-4">
					<label class="relative inline-flex items-center cursor-pointer">
						<input
							type="checkbox"
							checked={todo.completed}
							on:change={() => toggleTodo(todo.id)}
							class="sr-only peer"
						/>
						<div
							class="w-5 h-5 bg-white border-2 border-gray-300 rounded-full peer-checked:bg-blue-500 peer-checked:border-blue-500"
						></div>
					</label>
					<input
						type="text"
						value={todo.text}
						on:input={(e) => updateTodo(todo.id, e.currentTarget.value)}
						class={`flex-1 bg-transparent focus:outline-none focus:ring-0 border-0 ${
							todo.completed ? 'text-gray-400 line-through' : 'text-gray-700'
						}`}
					/>
					<button
						on:click={() => deleteTodo(todo.id)}
						aria-label="할일 삭제"
						class="opacity-0 group-hover:opacity-100 text-gray-400 hover:text-red-500 transition-all duration-200"
					>
						삭제
					</button>
				</li>
			{/if}
		{/each}

		{#if activeTab === 'active'}
			<li class="px-6 py-4">
				<div class="flex items-center gap-4">
					<div class="w-5 h-5 rounded-full border-2 border-dashed border-gray-300"></div>
					<input
						type="text"
						bind:value={newTodo}
						placeholder="새로운 미리알림"
						on:keydown={handleKeydown}
						on:blur={addTodo}
						class="flex-1 bg-transparent focus:outline-none focus:ring-0 border-0 text-gray-700 placeholder-gray-400 text-base"
					/>
				</div>
			</li>
		{/if}
	</ul>
</main>
