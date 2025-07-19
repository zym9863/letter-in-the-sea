<script lang="ts">
  import { createEventDispatcher } from 'svelte';
  import { fade, fly } from 'svelte/transition';
  const dispatch = createEventDispatcher();
  
  let content = '';
  let isSending = false;
  
  function handleSend() {
    if (content.trim() && !isSending) {
      isSending = true;
      // Trigger send animation
      setTimeout(() => {
        dispatch('send', content);
        content = '';
        isSending = false;
      }, 1500);
    }
  }
  
  function handleCancel() {
    if (!isSending) {
      dispatch('cancel');
    }
  }
  
  function adjustTextareaHeight(event: Event) {
    const textarea = event.target as HTMLTextAreaElement;
    textarea.style.height = 'auto';
    textarea.style.height = textarea.scrollHeight + 'px';
  }
</script>

<div class="write-container" in:fade={{ duration: 300 }}>
  <div class="paper" class:sending={isSending}>
    <h2 class="page-title">写一封信给大海</h2>
    
    <div class="letter-content">
      <textarea
        bind:value={content}
        placeholder="在这里写下你想说的话...
        
可以是无法言说的心事
可以是深夜的思绪
可以是想要放下的过往

这封信将永远地漂流在数字海洋中
也许某天，会有人读到你的故事..."
        disabled={isSending}
        on:input={adjustTextareaHeight}
      ></textarea>
    </div>
    
    <div class="actions">
      <button 
        class="btn cancel-btn" 
        on:click={handleCancel}
        disabled={isSending}
      >
        返回
      </button>
      
      <button 
        class="btn send-btn" 
        on:click={handleSend}
        disabled={!content.trim() || isSending}
      >
        {#if isSending}
          正在投递...
        {:else}
          投进大海
        {/if}
      </button>
    </div>
  </div>
  
  {#if isSending}
    <div class="sending-animation" transition:fade={{ duration: 1000 }}>
      <div class="bottle" out:fly={{ y: -200, duration: 1500 }}>
        🍾
      </div>
      <div class="ripples"></div>
    </div>
  {/if}
</div>

<style>
  .write-container {
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 1rem;
  }
  
  .paper {
    background: linear-gradient(135deg, rgba(255, 255, 255, 0.9), rgba(255, 255, 255, 0.8));
    backdrop-filter: blur(20px) saturate(180%);
    -webkit-backdrop-filter: blur(20px) saturate(180%);
    border: 1px solid rgba(255, 255, 255, 0.3);
    border-radius: 20px;
    padding: 2.5rem;
    max-width: 650px;
    width: 100%;
    max-height: 85vh;
    display: flex;
    flex-direction: column;
    box-shadow: 
      0 20px 40px rgba(0, 0, 0, 0.15),
      inset 0 1px 0 rgba(255, 255, 255, 0.5);
    transition: all 0.4s cubic-bezier(0.23, 1, 0.320, 1);
  }
  
  .paper.sending {
    transform: scale(0.9);
    opacity: 0.5;
  }
  
  .page-title {
    color: var(--ocean-dark);
    margin-bottom: 2rem;
    text-align: center;
    font-size: 2rem;
    font-weight: 600;
    letter-spacing: 0.05rem;
    position: relative;
    text-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  
  .page-title::after {
    content: '';
    position: absolute;
    bottom: -8px;
    left: 50%;
    transform: translateX(-50%);
    width: 80px;
    height: 3px;
    background: linear-gradient(90deg, transparent, var(--ocean-medium), transparent);
    border-radius: 2px;
  }
  
  .letter-content {
    flex: 1;
    margin-bottom: 1.5rem;
    min-height: 300px;
  }
  
  textarea {
    width: 100%;
    min-height: 320px;
    padding: 1.5rem;
    border: 2px solid rgba(107, 182, 255, 0.2);
    border-radius: 12px;
    font-size: 1.15rem;
    line-height: 1.8;
    resize: none;
    font-family: inherit;
    color: #333;
    background: rgba(255, 255, 255, 0.6);
    backdrop-filter: blur(10px);
    transition: all 0.3s ease;
    box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.05);
  }
  
  textarea:focus {
    outline: none;
    border-color: var(--ocean-medium);
    background: rgba(255, 255, 255, 0.8);
    box-shadow: 
      inset 0 2px 4px rgba(0, 0, 0, 0.05),
      0 0 0 3px rgba(107, 182, 255, 0.1);
  }
  
  textarea::placeholder {
    color: #999;
    font-style: italic;
  }
  
  .actions {
    display: flex;
    gap: 1rem;
    justify-content: flex-end;
  }
  
  .btn {
    padding: 1rem 2.5rem;
    border: none;
    border-radius: 30px;
    font-size: 1.05rem;
    cursor: pointer;
    transition: all 0.3s cubic-bezier(0.23, 1, 0.320, 1);
    font-weight: 500;
    letter-spacing: 0.02rem;
    position: relative;
    overflow: hidden;
  }
  
  .btn:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
  
  .cancel-btn {
    background: linear-gradient(135deg, #f5f5f5, #e0e0e0);
    color: #666;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.08);
  }
  
  .cancel-btn:hover:not(:disabled) {
    background: linear-gradient(135deg, #e8e8e8, #d0d0d0);
    transform: translateY(-2px);
    box-shadow: 0 6px 20px rgba(0, 0, 0, 0.12);
  }
  
  .send-btn {
    background: linear-gradient(135deg, var(--ocean-medium), var(--ocean-deep));
    color: white;
    box-shadow: 0 4px 15px rgba(107, 182, 255, 0.3);
  }
  
  .send-btn::before {
    content: '';
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.3), transparent);
    transition: left 0.5s ease;
  }
  
  .send-btn:hover:not(:disabled) {
    background: linear-gradient(135deg, var(--ocean-deep), var(--ocean-dark));
    transform: translateY(-3px);
    box-shadow: 
      0 8px 25px rgba(107, 182, 255, 0.4),
      0 0 20px rgba(107, 182, 255, 0.2);
  }
  
  .send-btn:hover:not(:disabled)::before {
    left: 100%;
  }
  
  .sending-animation {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 100;
    pointer-events: none;
  }
  
  .bottle {
    font-size: 5rem;
    filter: drop-shadow(0 4px 8px rgba(0, 0, 0, 0.3));
    animation: bob 1s ease-in-out infinite;
  }
  
  @keyframes bob {
    0%, 100% { transform: translateY(0) rotate(-5deg); }
    50% { transform: translateY(-10px) rotate(5deg); }
  }
  
  .ripples {
    position: absolute;
    bottom: -20px;
    left: 50%;
    transform: translateX(-50%);
    width: 100px;
    height: 100px;
    border-radius: 50%;
    border: 2px solid rgba(255, 255, 255, 0.5);
    animation: ripple 2s ease-out infinite;
  }
  
  @keyframes ripple {
    0% {
      width: 50px;
      height: 50px;
      opacity: 1;
    }
    100% {
      width: 150px;
      height: 150px;
      opacity: 0;
    }
  }
  
  @media (max-width: 768px) {
    .paper {
      padding: 1.5rem;
    }
    
    .page-title {
      font-size: 1.5rem;
    }
    
    textarea {
      font-size: 1rem;
      min-height: 250px;
    }
    
    .actions {
      flex-direction: column-reverse;
    }
    
    .btn {
      width: 100%;
    }
  }
</style>
