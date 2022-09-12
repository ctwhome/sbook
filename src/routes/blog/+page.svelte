<script lang="ts">
  // Gets the metadata of the mdfile directly
  const posts = Object.values(import.meta.globEager("./*.md"))
</script>

<div class="container mx-auto">

  <h2 class="text-2xl ">
    All blogposts here:
  </h2>

  {#each posts as { metadata: { title, slug } } }
    <div class="mt-2">
      <a sveltekit:prefetch class="hover:underline" href="blog/{slug}">{title}</a>
    </div>
  {/each}
</div>
