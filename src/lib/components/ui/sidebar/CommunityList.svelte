<script lang="ts">
  import Button from '$lib/components/input/Button.svelte'
  import Avatar from '$lib/components/ui/Avatar.svelte'
  import type { Community } from 'lemmy-js-client'
  import { flip } from 'svelte/animate'
  import { expoOut } from 'svelte/easing'
  import { slide, fade } from 'svelte/transition'

  export let items: Community[]
  export let expanded: boolean
  export let hidden: boolean
</script>

{#each items.sort( (a, b) => a.title.localeCompare(b.title) ) as follow (follow.id)}
  <div
    class="inline-flex w-full"
    class:hidden={hidden}
    animate:fade={{ duration: 250, easing: expoOut }}
  >
  
    <Button
      class="hover:bg-slate-200 w-full h-max {expanded ? '' : '!p-1.5'}"
      color="tertiary"
      alignment="left"
      href="/c/{follow.name}@{new URL(follow.actor_id).hostname}"
    >
      <div class="flex-none">
        <Avatar
          url={follow.icon}
          alt={follow.name}
          title={follow.title}
          width={20}
          slot="icon"
        />
      </div>
      <span class="max-w-full break-words" class:hidden={!expanded}>
        {follow.title}
      </span>
    </Button>
  </div>
{/each}
