<script lang="ts">
  import type { Alignment } from '$lib/components/ui/menu/menu.js'
  import { backOut, bounceOut, circOut, expoOut, sineOut } from 'svelte/easing'
  import { scale } from 'svelte/transition'

  export let open = false

  let clazz = ''
  export { clazz as class }

  export let itemsClass = ''

  export let absolute = false

  export let alignment: Alignment = 'bottom-left'

  let element: any

  export const toggleOpen = () => (open = !open)

  function getOriginClass(origin: Alignment) {
    switch (origin) {
      case 'top-right':
        return 'bottom-[100%] right-0 origin-bottom-right'
      case 'bottom-left':
        return 'top-[100%] origin-top-left'
      case 'top-left':
        return 'bottom-[100%] left-0 origin-bottom-left'
      case 'bottom-right':
        return 'top-[100%] right-0 origin-top-right'
      case 'top-center':
        return 'bottom-[100%] -left-[450%] origin-bottom'
      case 'bottom-center':
        return 'top-[100%] -left-[450%] origin-top'
      default:
        return 'bottom-[100%] origin-top-left'
    }
  }
</script>

<svelte:body
  on:click={(e) => {
    if (!element.contains(e.target)) {
      open = false
    }
  }}
/>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<div
  class="overflow-visible {absolute
    ? 'absolute'
    : 'relative'} cursor-auto {clazz} {itemsClass}"
  bind:this={element}
  tabindex="-1"
>
  <slot name="button" {toggleOpen} />
  {#if open}
    <menu
      transition:scale|local={{
        duration: 200,
        start: 0.95,
        easing: expoOut,
      }}
      class="list-none absolute max-h-96 z-30 overflow-auto {getOriginClass(
        alignment
      )}
          rounded-lg py-2 min-w-[16rem] my-2 flex flex-col shadow-sm
          bg-white dark:bg-zinc-900 backdrop-blur-2xl border border-slate-200 dark:border-zinc-800
          {$$props.containerClass}"
    >
      {#if open}
        <div on:click={toggleOpen} class="flex flex-col gap-0">
          <slot {toggleOpen} />
        </div>
      {/if}
    </menu>
  {/if}
</div>
