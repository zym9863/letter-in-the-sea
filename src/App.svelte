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
    <!-- Ocean layers for depth effect -->
    <div class="ocean-layer layer-1"></div>
    <div class="ocean-layer layer-2"></div>
    <div class="ocean-layer layer-3"></div>
    
    <!-- Floating particles -->
    <div class="particles">
      {#each Array(20) as _, i}
        <div class="particle" style="--i: {i}"></div>
      {/each}
    </div>
    
    <!-- Light rays from surface -->
    <div class="light-rays">
      {#each Array(5) as _, i}
        <div class="ray" style="--ray-i: {i}"></div>
      {/each}
    </div>
    
    {#if currentView === 'home'}
      <div class="home-container">
        <div class="title-wrapper">
          <h1 class="title">
            <span class="title-char">海</span>
            <span class="title-char">中</span>
            <span class="title-char">信</span>
          </h1>
          <div class="title-underline"></div>
        </div>
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
    background: radial-gradient(ellipse at top, var(--ocean-light) 0%, var(--ocean-medium) 25%, var(--ocean-deep) 50%, var(--ocean-dark) 75%, var(--ocean-abyss) 100%);
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    overflow: hidden;
  }
  
  /* Ocean depth layers */
  .ocean-layer {
    position: absolute;
    width: 200%;
    height: 200%;
    left: -50%;
    top: -50%;
    opacity: 0.3;
    mix-blend-mode: multiply;
  }
  
  .layer-1 {
    background: radial-gradient(circle at 20% 80%, transparent 0%, rgba(255, 255, 255, 0.1) 50%, transparent 100%);
    animation: drift 25s ease-in-out infinite;
  }
  
  .layer-2 {
    background: radial-gradient(circle at 80% 20%, transparent 0%, rgba(0, 100, 200, 0.1) 50%, transparent 100%);
    animation: drift 30s ease-in-out infinite reverse;
  }
  
  .layer-3 {
    background: radial-gradient(circle at 50% 50%, transparent 0%, rgba(0, 50, 150, 0.05) 50%, transparent 100%);
    animation: drift 35s ease-in-out infinite;
  }
  
  @keyframes drift {
    0%, 100% { transform: translate(0, 0) rotate(0deg); }
    33% { transform: translate(30px, -30px) rotate(120deg); }
    66% { transform: translate(-20px, 20px) rotate(240deg); }
  }
  
  /* Floating particles */
  .particles {
    position: absolute;
    width: 100%;
    height: 100%;
    pointer-events: none;
  }
  
  .particle {
    position: absolute;
    width: 4px;
    height: 4px;
    background: rgba(255, 255, 255, 0.6);
    border-radius: 50%;
    left: calc(var(--i) * 5%);
    animation: float-up calc(10s + var(--i) * 1s) linear infinite;
    animation-delay: calc(var(--i) * -0.5s);
  }
  
  @keyframes float-up {
    0% {
      transform: translateY(100vh) translateX(0);
      opacity: 0;
    }
    10% {
      opacity: 1;
    }
    90% {
      opacity: 1;
    }
    100% {
      transform: translateY(-100vh) translateX(100px);
      opacity: 0;
    }
  }
  
  /* Light rays */
  .light-rays {
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    pointer-events: none;
    overflow: hidden;
  }
  
  .ray {
    position: absolute;
    width: 2px;
    height: 100%;
    background: linear-gradient(to bottom, rgba(255, 255, 255, 0.2) 0%, transparent 50%);
    left: calc(20% + var(--ray-i) * 15%);
    transform-origin: top center;
    animation: sway calc(15s + var(--ray-i) * 2s) ease-in-out infinite;
    animation-delay: calc(var(--ray-i) * -1s);
  }
  
  @keyframes sway {
    0%, 100% { transform: rotate(-5deg); }
    50% { transform: rotate(5deg); }
  }
  
  .home-container {
    text-align: center;
    color: white;
    padding: 2rem;
    position: relative;
    z-index: 10;
  }
  
  .title-wrapper {
    position: relative;
    display: inline-block;
    margin-bottom: 1.5rem;
  }
  
  .title {
    font-family: 'ZCOOL XiaoWei', serif;
    font-size: 5rem;
    margin: 0;
    font-weight: 400;
    letter-spacing: 1rem;
    position: relative;
    display: flex;
    justify-content: center;
    gap: 0.5rem;
  }
  
  .title-char {
    display: inline-block;
    text-shadow: 
      0 0 20px rgba(255, 255, 255, 0.5),
      0 0 40px rgba(107, 182, 255, 0.5),
      0 0 60px rgba(44, 124, 229, 0.3),
      2px 2px 4px rgba(0, 0, 0, 0.5);
    animation: wave-text 3s ease-in-out infinite;
    animation-delay: calc(var(--char-index, 0) * 0.1s);
  }
  
  .title-char:nth-child(1) { --char-index: 0; }
  .title-char:nth-child(2) { --char-index: 1; }
  .title-char:nth-child(3) { --char-index: 2; }
  
  @keyframes wave-text {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(-15px); }
  }
  
  .title-underline {
    position: absolute;
    bottom: -10px;
    left: 50%;
    transform: translateX(-50%);
    width: 120%;
    height: 3px;
    background: linear-gradient(90deg, transparent, var(--ocean-light), transparent);
    opacity: 0.8;
    animation: shimmer 3s ease-in-out infinite;
  }
  
  @keyframes shimmer {
    0%, 100% { opacity: 0.8; transform: translateX(-50%) scaleX(0.8); }
    50% { opacity: 1; transform: translateX(-50%) scaleX(1); }
  }
  
  .subtitle {
    font-size: 1.3rem;
    margin-bottom: 3rem;
    opacity: 0.95;
    font-style: italic;
    font-weight: 300;
    letter-spacing: 0.1rem;
    text-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
    animation: fade-in 2s ease-out;
  }
  
  @keyframes fade-in {
    from { opacity: 0; transform: translateY(20px); }
    to { opacity: 0.95; transform: translateY(0); }
  }
  
  .actions {
    display: flex;
    gap: 2rem;
    justify-content: center;
    margin-bottom: 2rem;
  }
  
  .action-btn {
    background: linear-gradient(135deg, var(--glass), var(--glass-border));
    border: 1px solid var(--glass-border);
    color: white;
    padding: 1.5rem 3rem;
    font-size: 1.2rem;
    font-weight: 500;
    letter-spacing: 0.05rem;
    border-radius: 50px;
    cursor: pointer;
    transition: all 0.4s cubic-bezier(0.23, 1, 0.320, 1);
    backdrop-filter: blur(20px) saturate(180%);
    -webkit-backdrop-filter: blur(20px) saturate(180%);
    display: flex;
    align-items: center;
    gap: 0.8rem;
    position: relative;
    overflow: hidden;
    box-shadow: 
      0 8px 32px rgba(0, 0, 0, 0.1),
      inset 0 1px 0 rgba(255, 255, 255, 0.2);
  }
  
  .action-btn::before {
    content: '';
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
    transition: left 0.5s ease;
  }
  
  .action-btn:hover {
    background: linear-gradient(135deg, rgba(255, 255, 255, 0.15), rgba(255, 255, 255, 0.3));
    transform: translateY(-3px);
    box-shadow: 
      0 12px 40px rgba(0, 0, 0, 0.15),
      inset 0 1px 0 rgba(255, 255, 255, 0.3),
      0 0 30px rgba(107, 182, 255, 0.3);
  }
  
  .action-btn:hover::before {
    left: 100%;
  }
  
  .action-btn:active {
    transform: translateY(-1px);
  }
  
  .icon {
    font-size: 1.8rem;
    filter: drop-shadow(0 2px 4px rgba(0, 0, 0, 0.2));
    animation: icon-bounce 2s ease-in-out infinite;
  }
  
  .write-btn .icon { animation-delay: 0s; }
  .pickup-btn .icon { animation-delay: 0.5s; }
  
  @keyframes icon-bounce {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(-3px); }
  }
  
  .letter-count {
    font-size: 1.1rem;
    opacity: 0.9;
    margin-top: 3rem;
    padding: 0.8rem 2rem;
    background: var(--glass);
    border: 1px solid var(--glass-border);
    border-radius: 30px;
    backdrop-filter: blur(10px);
    display: inline-block;
    letter-spacing: 0.05rem;
    animation: pulse-glow 3s ease-in-out infinite;
  }
  
  @keyframes pulse-glow {
    0%, 100% { box-shadow: 0 0 20px rgba(107, 182, 255, 0.2); }
    50% { box-shadow: 0 0 30px rgba(107, 182, 255, 0.4); }
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
