<script lang="ts">
  import { onMount, createEventDispatcher } from 'svelte';
  import type { Letter } from '../types/letter';
  import { fade } from 'svelte/transition';

  export let letter: Letter | null;
  const dispatch = createEventDispatcher();
  const currentDate = new Date().toISOString();
  let daysAgo = '';

  // 计算信件的时间
  $: if (letter) {
    const letterDate = new Date(letter.timestamp);
    const now = new Date();
    const diffTime = Math.abs(now.getTime() - letterDate.getTime());
    const diffMinutes = Math.floor(diffTime / (1000 * 60));
    const diffHours = Math.floor(diffTime / (1000 * 60 * 60));
    const diffDays = Math.floor(diffTime / (1000 * 60 * 60 * 24));
    
    if (diffMinutes < 1) {
      daysAgo = '刚刚';
    } else if (diffMinutes < 60) {
      daysAgo = `${diffMinutes} 分钟前`;
    } else if (diffHours < 24) {
      daysAgo = `${diffHours} 小时前`;
    } else if (diffDays === 1) {
      daysAgo = '昨天';
    } else {
      daysAgo = `${diffDays} 天前`;
    }
  }
</script>

<div class="pickup-container" in:fade={{ duration: 300 }}>
  {#if letter}
    <div class="bottle">
      🍾
    </div>
    <div class="letter-display">
      <h2>你拾起了一封信...</h2>
      <p class="timestamp">{daysAgo}</p>
      <div class="content">
        <p>{letter.content}</p>
      </div>
    </div>
  {:else}
    <div class="empty">
      <p>海中当前没有信件，过会儿再来看看吧！</p>
    </div>
  {/if}

  <button class="back-btn" on:click={() => dispatch('back')}>返回</button>
</div>

<style>
  .pickup-container {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    padding: 2rem;
    text-align: center;
    color: #fff;
  }

  .bottle {
    font-size: 4rem;
    margin-bottom: 1.5rem;
  }

  .letter-display {
    background: rgba(255, 255, 255, 0.9);
    padding: 1.5rem;
    border-radius: 10px;
    max-width: 600px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
    margin-bottom: 1.5rem;
  }

  .timestamp {
    font-size: 0.9rem;
    color: #666;
    margin-top: 0.5rem;
    font-style: italic;
  }

  .content {
    padding: 1rem 0;
    color: #333;
    text-align: left;
    white-space: pre-wrap;
  }

  .empty {
    padding: 2rem;
    background: rgba(255, 255, 255, 0.9);
    border-radius: 10px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
  }

  .back-btn {
    background: #4682B4;
    color: white;
    border: none;
    padding: 0.8rem 2rem;
    font-size: 1rem;
    border-radius: 25px;
    cursor: pointer;
    transition: background 0.3s ease;
  }

  .back-btn:hover {
    background: #5691C3;
  }

  @media (max-width: 768px) {
    .letter-display {
      width: 100%;
    }
  }
</style>
