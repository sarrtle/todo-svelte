<!-- main page for this app -->
<script lang="ts">
	// types for tasks
	type task = {
		task: string;
		category: string;
		datetime: string;
		timestamp: number;
	};

	// todo task name
	let task_value = $state<string>('');

	// todo task value
	let category_value = $state<string>('work');

	// todo task list
	let task_list = $state<task[]>([]);

	// when add todo
	function add_todo(_: SubmitEvent) {
		console.log(task_value, category_value);

		// get today date in this format: February 25, 2025 08:30 PM
		let date_object = new Date();
		let today: string = date_object.toLocaleString('en-US', {
			month: 'long',
			day: 'numeric',
			year: 'numeric',
			hour: 'numeric',
			minute: 'numeric',
			hour12: true
		});

		// create timestamp as number for the id
		let timestamp = date_object.getTime();

		task_list.push({
			task: task_value,
			category: category_value,
			datetime: today,
			timestamp: timestamp
		});
		console.log(task_list);
	}
</script>

<!-- Main container -->
<div class="flex h-screen flex-col gap-2 bg-zinc-900 p-8 font-sans text-slate-300">
	<!-- Greetings with exact date -->
	<div class="flex gap-4">
		<div class="flex flex-col items-center justify-center text-xl font-bold">
			<div>Feb</div>
			<div>25</div>
		</div>
		<div class="flex flex-col gap-2">
			<div class="text-3xl font-semibold">Good evening.</div>
			<div class="text-2xl font-semibold text-slate-300/60">What is your plan for today?</div>
		</div>
	</div>

	<!-- separate container for margin control -->
	<div class="flex flex-col gap-3 p-4">
		<!-- Add todo input -->
		<div class="flex gap-2">
			<form onsubmit={add_todo} class="w-full">
				<input
					type="text"
					name="add"
					id="add"
					placeholder="Add todo"
					class="w-full rounded-lg bg-zinc-700 p-4 focus:outline-none"
					bind:value={task_value}
				/>
			</form>
			<select
				bind:value={category_value}
				name="category"
				id="category"
				class="rounded-lg bg-zinc-700 p-4 focus:outline-none"
			>
				<option value="work">Work</option>
				<option value="life">Life</option>
				<option value="game">Game</option>
			</select>
		</div>
		<!-- {@render task_snippet('Do something better', 'Life', 'February 25, 2025 08:30 PM')} -->
		{#each task_list as task_data (task_data.timestamp)}
			{@render task_snippet(task_data.task, task_data.category, task_data.datetime)}
		{/each}
	</div>
</div>

<!-- Reusable snippets -->
{#snippet task_snippet(task: string, category: string, datetime: string)}
	<div class="flex items-center justify-between gap-4 rounded-lg bg-zinc-700 p-4">
		<div class="flex items-center gap-4">
			<div>
				<input type="checkbox" name="complete" id="complete" class="h-4 w-4" />
			</div>
			<div>
				<div class="text-xs text-slate-300/60">{datetime}</div>
				<div>{task}</div>
			</div>
		</div>
		<div class="text-slate-300/60">#{category}</div>
	</div>
{/snippet}
