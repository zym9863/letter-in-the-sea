[**English**] | [中文](./README.md)

# Letter in the Sea

A warm digital message-in-a-bottle application, allowing emotions to drift with the waves.

## 🌊 Project Introduction

"Letter in the Sea" is a web application built with Svelte + TypeScript + Vite. Users can write their thoughts, feelings, or memories they wish to leave behind as letters tossed into a digital sea. They can also randomly pick up letters left by others, experiencing stories and emotions from strangers.

### Core Features

- **Send a Letter** - Write your thoughts and let them drift in the digital sea
- **Pickup Echoes** - Randomly read letters from others, experiencing different life stories
- **Local Storage** - All letters are saved locally in the browser to protect privacy
- **Timestamp** - Automatically records the delivery time of each letter
- **Elegant Animations** - Smooth page transitions and delivery animations

## 🚀 Quick Start

### Install Dependencies

```bash
# Using pnpm (recommended)
pnpm install

# Or using npm
npm install
```

### Development Environment

```bash
# Start the development server
pnpm dev

# Or
npm run dev
```

### Build Project

```bash
# Build for production
pnpm build

# Or
npm run build
```

### Preview Build

```bash
# Preview production build
pnpm preview

# Or
npm run preview
```

## 📁 Project Structure

```
letter-in-the-sea/
├── src/
│   ├── lib/
│   │   ├── WriteLetter.svelte    # Write letter component
│   │   ├── PickupBottle.svelte   # Pickup letter component
│   │   └── Counter.svelte        # Counter component (example)
│   ├── types/
│   │   └── letter.ts             # TypeScript type definitions
│   ├── assets/                   # Static resources
│   ├── App.svelte               # Main app component
│   ├── main.ts                  # App entry point
│   └── app.css                  # Global styles
├── public/                      # Public resources
├── index.html                   # HTML entry
└── package.json                 # Project configuration
```

## 🎨 Technical Features

- **Svelte 5** - Using the latest version of the Svelte framework
- **TypeScript** - Full type support
- **Vite** - Fast development build tool
- **Responsive Design** - Adapts to mobile and desktop
- **Animation Effects** - Using Svelte built-in transition animations
- **Local Storage** - Data saved with localStorage

## 💡 Use Cases

- Record unspoken thoughts
- Share late-night musings
- Let go of past memories
- Feel the warmth of strangers
- Find emotional resonance

## 🛠️ Development Guide

### Recommended IDE Configuration

- [VS Code](https://code.visualstudio.com/) + [Svelte](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode)

### Type Checking

```bash
# Run Svelte and TypeScript type checks
pnpm check

# Or
npm run check
```

## 📝 License

This project is for learning and personal use only.

---

> Wrote a letter and tossed it into the sea, letting emotions drift with the waves.

