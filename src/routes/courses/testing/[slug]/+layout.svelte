<script lang="ts">
	import { dev } from '$app/environment';
	import { page } from '$app/stores';
	import projectRoot from 'virtual:project-root';
</script>

{#if dev}
	<a
		class="my-4 flex rounded-md border-2 border-primary-200 bg-primary-100 p-2 text-sm font-semibold shadow-sm transition-colors hover:border-primary-300 hover:bg-primary-200 dark:border-primary-900 dark:bg-primary-800 dark:text-primary-200"
		href="vscode://file/{projectRoot}/content{$page.url.pathname}.md"
	>
		<span class="block">Open in Visual Studio Code.</span>
		<span class="block text-primary-800">&rarr;</span>
	</a>
{/if}

<header>
	<h2>
		<a href="/courses/testing" class=" text-primary-600 dark:text-primary-200">
			Introduction to Testing
		</a>
	</h2>
</header>

<slot />
