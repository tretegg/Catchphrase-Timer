<script lang="ts">
	import '../app.css';
	import { Github } from "lucide-svelte";
	let { children } = $props();
</script>

{@render children()}


<meta name="description" content="Online Catchphrase Timer">



<!-- Github button fixed at the bottom left corner -->
<a href="https://github.com/tretegg/Catchphrase-Timer" target="_blank">
  <button class="w-12 h-12 rounded-xl shadow-lg hover:shadow-xl transition-shadow duration-300 fixed bottom-4 left-4">
	<!-- Set the GitHub SVG icon color to white -->
	<Github class="w-6 h-6 text-white" />
  </button>
</a>

  
  