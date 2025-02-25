<!-- main page for this app -->
<script lang="ts">
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
			<input
				type="text"
				name="add"
				id="add"
				placeholder="Add todo"
				class="w-full rounded-lg bg-zinc-700 p-4 focus:outline-none"
			/>
			<select name="category" id="category" class="rounded-lg bg-zinc-700 p-4 focus:outline-none">
				<option value="work">Work</option>
				<option value="life">Life</option>
				<option value="game">Game</option>
			</select>
		</div>
		{@render task('Do something better', 'Life', 'February 25, 2025 08:30 PM')}
		<!-- Create a list of task and respect of the category -->
		{@render task('Create a simple todo app', 'Work', 'February 25, 2025 10:30 AM')}
		{@render task('Write report', 'Work', 'February 25, 2025 11:30 AM')}
		{@render task('Get daily rewards', 'Game', 'February 25, 2025 12:30 PM')}
	</div>
</div>

<!-- Reusable snippets -->
{#snippet task(task: string, category: string, datetime: string)}
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
