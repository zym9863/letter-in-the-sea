<script lang="ts">
  import { createEventDispatcher } from 'svelte';
  import { fade, fly } from 'svelte/transition';
  
  const dispatch = createEventDispatcher();
  
  let content = '';
  let isSending = false;
  
  function handleSend() {
    if (content.trim() && !isSending) {
      isSending = true;
      // 触发投递动画
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
      />
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
    background: rgba(255, 255, 255, 0.95);
    border-radius: 10px;
    padding: 2rem;
    max-width: 600px;
    width: 100%;
    max-height: 80vh;
    display: flex;
    flex-direction: column;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
    transition: all 0.3s ease;
  }
  
  .paper.sending {
    transform: scale(0.9);
    opacity: 0.5;
  }
  
  .page-title {
    color: #1E3A8A;
    margin-bottom: 1.5rem;
    text-align: center;
    font-size: 1.8rem;
  }
  
  .letter-content {
    flex: 1;
    margin-bottom: 1.5rem;
    min-height: 300px;
  }
  
  textarea {
    width: 100%;
    min-height: 300px;
    padding: 1rem;
    border: 2px solid #e0e0e0;
    border-radius: 8px;
    font-size: 1.1rem;
    line-height: 1.6;
    resize: none;
    font-family: inherit;
    color: #333;
    background: rgba(255, 255, 255, 0.8);
    transition: border-color 0.3s ease;
  }
  
  textarea:focus {
    outline: none;
    border-color: #4682B4;
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
    padding: 0.8rem 2rem;
    border: none;
    border-radius: 25px;
    font-size: 1rem;
    cursor: pointer;
    transition: all 0.3s ease;
    font-weight: 500;
  }
  
  .btn:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
  
  .cancel-btn {
    background: #e0e0e0;
    color: #666;
  }
  
  .cancel-btn:hover:not(:disabled) {
    background: #d0d0d0;
  }
  
  .send-btn {
    background: #4682B4;
    color: white;
  }
  
  .send-btn:hover:not(:disabled) {
    background: #5691C3;
    transform: translateY(-2px);
    box-shadow: 0 5px 15px rgba(70, 130, 180, 0.3);
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
    font-size: 4rem;
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
