<script lang="ts">
  import { page } from '$app/stores'
  import Link from '$lib/components/input/Link.svelte'
  import MultiSelect from '$lib/components/input/MultiSelect.svelte'
  import Post from '../../../lib/components/lemmy/post/Post.svelte'
  import { goto } from '$app/navigation'
  import Card from '$lib/components/ui/StickyCard.svelte'
  import CommunityCard from '$lib/components/lemmy/community/CommunityCard.svelte'
  import Button from '$lib/components/input/Button.svelte'
  import { Color } from '$lib/ui/colors'
  import { fly } from 'svelte/transition'
  import { userSettings } from '$lib/settings.js'
  import Modal from '$lib/components/ui/modal/Modal.svelte'
  import Pageination from '$lib/components/ui/Pageination.svelte'
  import Avatar from '$lib/components/ui/Avatar.svelte'
  import Sort from '$lib/components/lemmy/Sort.svelte'
  import { fullCommunityName, searchParam } from '$lib/util.js'
  import { onDestroy, onMount } from 'svelte'
  import { setSessionStorage } from '$lib/session.js'
  import PostFeed from '$lib/components/lemmy/post/PostFeed.svelte'

  export let data

  let sidebar: boolean = false

  onMount(() => {
    setSessionStorage('lastSeenCommunity', {
      id: data.community.community_view.community.id,
      name: fullCommunityName(
        data.community.community_view.community.name,
        data.community.community_view.community.actor_id
      ),
    })
  })
</script>

<Modal bind:open={sidebar}>
  <span slot="title">About</span>
  <div class="mx-auto">
    <CommunityCard community_view={data.community.community_view} />
  </div>
</Modal>

<div class="flex flex-col md:flex-row gap-4 w-full">
  <div class="flex flex-col gap-3 sm:gap-4 max-w-full w-full min-w-0">
    <div class="flex flex-row gap-3 items-center">
      <Avatar
        width={48}
        url={data.community.community_view.community.icon}
        alt={data.community.community_view.community.name}
      />
      <div class="flex flex-col gap-0">
        <h1 class="font-bold text-xl">
          {data.community.community_view.community.title}
        </h1>
        <span class="dark:text-zinc-400 text-slate-600 text-sm">
          !{data.community.community_view.community.name}@{new URL(
            data.community.community_view.community.actor_id
          ).hostname}
        </span>
      </div>
    </div>
    <div class="xl:hidden">
      <Button color="secondary" on:click={() => (sidebar = !sidebar)}>
        About
      </Button>
    </div>
    <div class="flex flex-col sm:flex-row gap-4 max-w-full w-full">
      <Sort selected={data.sort} />
    </div>
    <PostFeed posts={data.posts.posts} />
    <Pageination
      page={data.page}
      on:change={(p) => searchParam($page.url, 'page', p.detail.toString())}
    />
  </div>
  <div class="hidden xl:block">
    <CommunityCard community_view={data.community.community_view} />
  </div>
</div>
