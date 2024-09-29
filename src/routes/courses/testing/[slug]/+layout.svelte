<script lang="ts">
	import { dev } from '$app/environment';
	import { page } from '$app/stores';
	import projectRoot from 'virtual:project-root';
</script>

{#if dev}
	<div
		class="my-4 flex flex-col gap-2 rounded-md border-2 border-primary-200 bg-primary-100 p-2 text-sm shadow-sm dark:border-primary-900 dark:bg-primary-800 dark:text-primary-200"
	>
		<a
			href="vscode://file/{projectRoot}/content{$page.url.pathname}.md"
			class="group block decoration-primary-500"
		>
			Open in <span class="font-semibold group-hover:underline">Visual Studio Code</span>. &rarr;
		</a>
		<a
			href="obsidian://open?value=content&file={$page.url.pathname}"
			class="group block decoration-primary-500"
		>
			Open in <span class="font-semibold group-hover:underline">Obsidian</span>. &rarr;
		</a>
	</div>
{/if}

<header>
	<h2>
		<a href="/courses/testing" class=" text-primary-600 dark:text-primary-200">
			Introduction to Testing
		</a>
	</h2>
</header>

<slot />
