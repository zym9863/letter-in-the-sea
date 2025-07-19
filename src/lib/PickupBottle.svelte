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
    width: 100%;
    height: 100%;
    position: relative;
  }

  .bottle {
    font-size: 5rem;
    margin-bottom: 2rem;
    filter: drop-shadow(0 4px 8px rgba(0, 0, 0, 0.3));
    animation: floating 3s ease-in-out infinite;
  }
  
  @keyframes floating {
    0%, 100% { transform: translateY(0) rotate(-5deg); }
    50% { transform: translateY(-20px) rotate(5deg); }
  }

  .letter-display {
    background: linear-gradient(135deg, rgba(255, 255, 255, 0.95), rgba(255, 255, 255, 0.85));
    backdrop-filter: blur(20px) saturate(180%);
    -webkit-backdrop-filter: blur(20px) saturate(180%);
    border: 1px solid rgba(255, 255, 255, 0.3);
    padding: 2.5rem;
    border-radius: 20px;
    max-width: 650px;
    width: 100%;
    box-shadow: 
      0 20px 40px rgba(0, 0, 0, 0.15),
      inset 0 1px 0 rgba(255, 255, 255, 0.5);
    margin-bottom: 2rem;
    animation: slideUp 0.6s ease-out;
  }
  
  @keyframes slideUp {
    from {
      opacity: 0;
      transform: translateY(30px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
  
  .letter-display h2 {
    color: var(--ocean-dark);
    font-size: 1.8rem;
    font-weight: 600;
    margin-bottom: 1rem;
    letter-spacing: 0.05rem;
  }

  .timestamp {
    font-size: 1rem;
    color: var(--ocean-medium);
    margin-bottom: 1.5rem;
    font-style: italic;
    letter-spacing: 0.02rem;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 0.5rem;
  }
  
  .timestamp::before {
    content: '🕰️';
    font-style: normal;
  }

  .content {
    padding: 1.5rem;
    color: #333;
    text-align: left;
    white-space: pre-wrap;
    background: rgba(255, 255, 255, 0.5);
    border-radius: 12px;
    border: 1px solid rgba(107, 182, 255, 0.1);
    min-height: 200px;
    font-size: 1.1rem;
    line-height: 1.8;
    letter-spacing: 0.02rem;
  }

  .empty {
    padding: 3rem;
    background: linear-gradient(135deg, rgba(255, 255, 255, 0.95), rgba(255, 255, 255, 0.85));
    backdrop-filter: blur(20px) saturate(180%);
    -webkit-backdrop-filter: blur(20px) saturate(180%);
    border: 1px solid rgba(255, 255, 255, 0.3);
    border-radius: 20px;
    box-shadow: 
      0 20px 40px rgba(0, 0, 0, 0.15),
      inset 0 1px 0 rgba(255, 255, 255, 0.5);
    animation: pulse 2s ease-in-out infinite;
  }
  
  .empty p {
    color: var(--ocean-medium);
    font-size: 1.2rem;
    letter-spacing: 0.02rem;
    margin: 0;
  }
  
  @keyframes pulse {
    0%, 100% { transform: scale(1); }
    50% { transform: scale(1.02); }
  }

  .back-btn {
    background: linear-gradient(135deg, var(--ocean-medium), var(--ocean-deep));
    color: white;
    border: none;
    padding: 1rem 2.5rem;
    font-size: 1.05rem;
    font-weight: 500;
    letter-spacing: 0.02rem;
    border-radius: 30px;
    cursor: pointer;
    transition: all 0.3s cubic-bezier(0.23, 1, 0.320, 1);
    box-shadow: 0 4px 15px rgba(107, 182, 255, 0.3);
    position: relative;
    overflow: hidden;
  }
  
  .back-btn::before {
    content: '';
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.3), transparent);
    transition: left 0.5s ease;
  }

  .back-btn:hover {
    background: linear-gradient(135deg, var(--ocean-deep), var(--ocean-dark));
    transform: translateY(-3px);
    box-shadow: 
      0 8px 25px rgba(107, 182, 255, 0.4),
      0 0 20px rgba(107, 182, 255, 0.2);
  }
  
  .back-btn:hover::before {
    left: 100%;
  }

  @media (max-width: 768px) {
    .letter-display {
      width: 100%;
    }
  }
</style>
