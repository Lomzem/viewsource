<script lang="ts">
  import Button from "$lib/components/ui/button/button.svelte"
  import Input from "$lib/components/ui/input/input.svelte"
  import { enhance } from "$app/forms"
  import { IconLoader2 } from "@tabler/icons-svelte"
  let fetchData: Promise<string> | undefined = $state()
</script>

<main class="grid h-dvh place-content-center place-items-center p-8">
  <h1 class="text-center font-heading text-5xl font-black text-pretty">put the html in the bag</h1>
  <form
    class="mt-8 flex w-full flex-wrap gap-2"
    method="POST"
    use:enhance={({ cancel, formData }) => {
      cancel()

      const url = formData.get("url")?.toString()
      if (!url) return
      fetchData = fetch(url).then((res) => res.text())
    }}
  >
    <Input
      class="grow overflow-clip py-8 text-xl"
      type="url"
      name="url"
      autofocus
      required
      placeholder="https://"
    />
    <Button class="grow cursor-pointer py-8 text-2xl" type="submit">View Source</Button>
  </form>
  {#await fetchData}
    <IconLoader2 size={64} class="mt-8 animate-spin text-foreground" />
    <p class="mt-2 text-2xl">lemme get that for you :)</p>
  {/await}
</main>
