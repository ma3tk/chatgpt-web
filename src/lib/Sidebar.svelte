<script lang="ts">
  import { addChat, clearChats } from "./Storage.svelte";
  import { exportAsMarkdown } from "./Export.svelte";
  import type { Chat } from "./Types.svelte";

  export let activeChatId: number;
  export let sortedChats: Chat[];
  export let apiKey: string;
</script>

<aside class="menu">
  <p class="menu-label">チャット一覧</p>
  <ul class="menu-list">
    {#if sortedChats.length === 0}
      <li>チャットはまだありません</li>
      <li>
        <a
                href={"#"}
                class="panel-block"
                class:is-disabled={!apiKey}
                on:click|preventDefault={() => {
            activeChatId = addChat();
          }}><span class="greyscale mr-2">➕</span> 新規チャット</a
        >
      </li>
    {:else}
      <li>
        <ul>
          {#each sortedChats as chat}
            <li>
              <a
                href={"#"}
                class:is-disabled={!apiKey}
                class:is-active={activeChatId === chat.id}
                on:click|preventDefault={() => (activeChatId = chat.id)}>{chat.name || `Chat ${chat.id}`}</a
              >
            </li>
          {/each}
        </ul>
      </li>
    {/if}
  </ul>
  <p class="menu-label">アクション</p>
  <ul class="menu-list">
    <li>
      <a
        href={"#"}
        class="panel-block"
        class:is-disabled={!apiKey}
        on:click|preventDefault={() => {
          activeChatId = addChat();
        }}><span class="greyscale mr-2">➕</span> 新規チャット</a
      >
    </li>
    <li>
      <a
        href={"#"}
        class="panel-block"
        class:is-disabled={!apiKey}
        on:click|preventDefault={() => {
          if(confirm("本当にチャットを全て削除してもよろしいですか？")) {
            clearChats();
            activeChatId = null;
          }
        }}><span class="greyscale mr-2">🗑️</span> チャットを全て削除する</a
      >
    </li>
    {#if activeChatId}
      <li>
        <a
          href={"#"}
          class="panel-block"
          class:is-disabled={!apiKey}
          on:click|preventDefault={() => {
            exportAsMarkdown(activeChatId);
          }}><span class="greyscale mr-2">📥</span> チャットのエクスポート</a
        >
      </li>
    {/if}
    <li>
      <a
              href={"#"}
              class="panel-block"
              class:is-disabled={!apiKey}
              class:is-active={!activeChatId}
              on:click|preventDefault={() => {
          activeChatId = null;
        }}><span class="greyscale mr-2">🔑</span> API keyの設定</a
      >
    </li>
  </ul>
</aside>
