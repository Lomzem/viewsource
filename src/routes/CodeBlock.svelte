<script lang="ts">
  import { IconLoader2 } from "@tabler/icons-svelte"
  import { createHighlighterCore } from "shiki/core"
  import { createJavaScriptRegexEngine } from "shiki/engine/javascript"

  let { code }: { code: string } = $props()
  let outputRef: HTMLOutputElement | undefined = $state()
  $effect(() => {
    if (outputRef) outputRef.scrollIntoView({ behavior: "smooth" })
  })

  const highlighter = createHighlighterCore({
    themes: [import("@shikijs/themes/catppuccin-mocha")],
    langs: [import("@shikijs/langs/html")],
    engine: createJavaScriptRegexEngine(),
  })

  const highlightedHTML = highlighter.then((highlighter) =>
    highlighter.codeToHtml(code, {
      lang: "html",
      theme: "catppuccin-mocha",
      transformers: [
        {
          pre(hast) {
            this.addClassToHast(
              hast,
              "text-md p-4 max-h-full overflow-auto max-w-full rounded-lg lg:text-lg",
            )
          },
        },
      ],
    }),
  )
</script>

{#await highlightedHTML}
  <IconLoader2 size={64} class="mt-8 animate-spin place-self-center text-foreground" />
  <p class="mt-2 place-self-center text-2xl">gimme a sec, i'm highlighting</p>
{:then html}
  <output bind:this={outputRef} class="mt-8 max-h-[90svh] w-full">
    {@html html}
  </output>
{/await}
