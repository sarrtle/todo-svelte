<!-- main page for this app -->
<script lang="ts">
	// types for tasks
	type task = {
		task: string;
		category: string;
		datetime: string;
		timestamp: number;
	};

	// types for date greetings
	type date_greeting_type = {
		month: string;
		day: string;
		greeting: string;
		daytime: string;
	};

	// get the current date
	let date: string = new Date().toLocaleString('en-US', {
		month: 'short',
		day: 'numeric'
	});
	let date_greeting: date_greeting_type = {
		month: date.split(' ')[0],
		day: date.split(' ')[1],
		greeting:
			new Date().getHours() > 12
				? 'Good morning.'
				: new Date().getHours() > 18
					? 'Good afternoon.'
					: 'Good evening.',
		daytime: new Date().getHours() < 18 ? 'today' : 'evening'
	};

	// todo task name
	let task_value = $state<string>('');

	// todo task value
	let category_value = $state<string>('work');

	// todo task list
	let task_list = $state<task[]>([]);

	// when add todo
	function add_todo(event: SubmitEvent) {
		event.preventDefault();

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

		// clear todo input
		task_value = '';
	}
</script>

<div class="flex h-screen w-full items-center justify-center bg-zinc-900">
	<!-- Main container -->
	<div
		class="flex h-screen flex-col gap-2 p-4 font-sans text-slate-300 md:max-w-2xl md:min-w-2xl lg:max-w-4xl lg:min-w-3xl"
	>
		<!-- Greetings with exact date -->
		<div class="flex gap-4">
			<div class="flex flex-col items-center justify-center text-xl font-bold">
				<div>{date_greeting.month}</div>
				<div>{date_greeting.day}</div>
			</div>
			<div class="flex flex-col gap-2">
				<div class="text-3xl font-semibold">{date_greeting.greeting}</div>
				<div class="text-2xl font-semibold text-slate-300/60">
					What is your plan for {date_greeting.daytime}?
				</div>
			</div>
		</div>

		<!-- separate container for margin control -->
		<div class="flex max-h-[calc(100vh-200px)] flex-col gap-3 p-2">
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
			<!-- Task list scrollable -->
			<div class="flex flex-col gap-3 overflow-y-scroll">
				<!-- {@render task_snippet('Do something better', 'Life', 'February 25, 2025 08:30 PM')} -->
				{#each task_list as task_data (task_data.timestamp)}
					{@render task_snippet(task_data.task, task_data.category, task_data.datetime)}
				{/each}
			</div>
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
</div>
