<script lang="ts">
  import { onMount } from 'svelte';
  import WriteLetter from './lib/WriteLetter.svelte';
  import PickupBottle from './lib/PickupBottle.svelte';
  import type { Letter } from './types/letter';
  
  let currentView: 'home' | 'write' | 'pickup' = 'home';
  let letters: Letter[] = [];
  
  // 从本地存储加载信件
  onMount(() => {
    const stored = localStorage.getItem('seaLetters');
    if (stored) {
      letters = JSON.parse(stored);
    }
  });
  
  // 保存信件到本地存储
  function saveLetters() {
    localStorage.setItem('seaLetters', JSON.stringify(letters));
  }
  
  // 投递信件
  function handleSendLetter(event: CustomEvent<string>) {
    const newLetter: Letter = {
      id: Date.now().toString(),
      content: event.detail,
      timestamp: new Date().toISOString()
    };
    letters = [...letters, newLetter];
    saveLetters();
    currentView = 'home';
  }
  
  // 获取随机信件
  function getRandomLetter(): Letter | null {
    if (letters.length === 0) return null;
    const randomIndex = Math.floor(Math.random() * letters.length);
    return letters[randomIndex];
  }
</script>

<main>
  <div class="ocean-background">
    {#if currentView === 'home'}
      <div class="home-container">
        <h1 class="title">海中信</h1>
        <p class="subtitle">写了信又丢进海里，让情感随波逐流</p>
        
        <div class="actions">
          <button class="action-btn write-btn" on:click={() => currentView = 'write'}>
            <span class="icon">✍️</span>
            <span>投信入海</span>
          </button>
          
          <button class="action-btn pickup-btn" on:click={() => currentView = 'pickup'}>
            <span class="icon">🌊</span>
            <span>拾取回音</span>
          </button>
        </div>
        
        <p class="letter-count">海中已有 {letters.length} 封信</p>
      </div>
    {/if}
    
    {#if currentView === 'write'}
      <WriteLetter 
        on:send={handleSendLetter}
        on:cancel={() => currentView = 'home'}
      />
    {/if}
    
    {#if currentView === 'pickup'}
      <PickupBottle 
        letter={getRandomLetter()}
        on:back={() => currentView = 'home'}
      />
    {/if}
  </div>
</main>

<style>
  :global(body) {
    margin: 0;
    padding: 0;
    overflow: hidden;
  }
  
  main {
    width: 100vw;
    height: 100vh;
    margin: 0;
    padding: 0;
  }
  
  .ocean-background {
    width: 100%;
    height: 100%;
    background: linear-gradient(180deg, #87CEEB 0%, #4682B4 50%, #1E3A8A 100%);
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    animation: wave 20s ease-in-out infinite;
  }
  
  @keyframes wave {
    0%, 100% { background-position: 0% 50%; }
    50% { background-position: 100% 50%; }
  }
  
  .home-container {
    text-align: center;
    color: white;
    padding: 2rem;
  }
  
  .title {
    font-size: 4rem;
    margin-bottom: 0.5rem;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
    animation: float 3s ease-in-out infinite;
  }
  
  @keyframes float {
    0%, 100% { transform: translateY(0px); }
    50% { transform: translateY(-10px); }
  }
  
  .subtitle {
    font-size: 1.2rem;
    margin-bottom: 3rem;
    opacity: 0.9;
    font-style: italic;
  }
  
  .actions {
    display: flex;
    gap: 2rem;
    justify-content: center;
    margin-bottom: 2rem;
  }
  
  .action-btn {
    background: rgba(255, 255, 255, 0.2);
    border: 2px solid rgba(255, 255, 255, 0.4);
    color: white;
    padding: 1.5rem 2.5rem;
    font-size: 1.2rem;
    border-radius: 50px;
    cursor: pointer;
    transition: all 0.3s ease;
    backdrop-filter: blur(10px);
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }
  
  .action-btn:hover {
    background: rgba(255, 255, 255, 0.3);
    transform: translateY(-2px);
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
  }
  
  .icon {
    font-size: 1.5rem;
  }
  
  .letter-count {
    font-size: 1rem;
    opacity: 0.8;
    margin-top: 2rem;
  }
  
  @media (max-width: 768px) {
    .title {
      font-size: 3rem;
    }
    
    .actions {
      flex-direction: column;
      gap: 1rem;
    }
    
    .action-btn {
      width: 100%;
      max-width: 300px;
    }
  }
</style>
