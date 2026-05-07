<script lang="ts">
  import Button from "$lib/components/ui/button/button.svelte"
  import Input from "$lib/components/ui/input/input.svelte"
  import { enhance } from "$app/forms"
  import { IconBrandGithubFilled, IconLoader2 } from "@tabler/icons-svelte"
  import CodeBlock from "./CodeBlock.svelte"

  let fetchData: Promise<string> | undefined = $state()
</script>

<header>
  <a
    href="https://github.com/Lomzem/viewsource"
    target="_blank"
    rel="noreferrer"
    aria-label="View on GitHub"
    class="mt-4 ml-4 flex gap-1 text-muted-foreground transition hover:text-foreground"
  >
    <IconBrandGithubFilled />
    <span class="self-baseline-last">view on github</span>
  </a>
</header>

<main class="grid min-h-dvh grid-cols-1 place-content-center p-8">
  <h1 class="text-center font-heading text-5xl font-black text-pretty">put the html in the bag</h1>
  <form
    class="mt-8 flex w-full flex-col gap-3 place-self-center sm:w-3xl sm:flex-row sm:items-center"
    method="POST"
    use:enhance={({ cancel, formData }) => {
      cancel()

      const url = formData.get("url")?.toString()
      if (!url) return
      fetchData = fetch(url).then((res) => res.text())
    }}
  >
    <Input
      class="flex-1 overflow-clip py-8 text-xl md:text-2xl
      "
      type="url"
      name="url"
      autofocus
      required
      placeholder="https://"
    />
    <Button class="w-full cursor-pointer py-8 text-2xl sm:w-auto" type="submit">View Source</Button>
  </form>

  {#if fetchData}
    {#await fetchData}
      <IconLoader2 size={64} class="mt-8 animate-spin place-self-center text-foreground" />
      <p class="mt-2 place-self-center text-2xl">lemme get that for you :)</p>
    {:then html}
      <CodeBlock code={html} />
    {/await}
  {/if}
</main>
