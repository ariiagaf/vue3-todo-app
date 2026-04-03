<script setup lang="ts">
import { ref } from 'vue'

const count = ref(0)

const increment = () => {
      count.value++
}

const decrement = () => {
  count.value--
}

const reset = () => {
  count.value = 0
}

const getCountColor = (): string => {
  if (count.value > 0) return '#10b981'
  if (count.value < 0) return '#ef4444'
  return '#ffffff'
}

</script>

<template>
    <div class="counter-container">
        <div class="counter-card">
            <h2 class="counter-title">Counter App</h2>
            <div class="counter-display">
            <span class="counter-value" :style="{ color: getCountColor() }">
                {{ count }}
            </span>
            </div>
            <div class="counter-buttons">
                <button class="button button-decrement" @click="decrement">−</button>
                <button class="button button-reset" @click="reset">⟳</button>
                <button class="button button-increment" @click="increment">+</button>
            </div>
        </div>
    </div>
</template>

<style scoped>
    .counter-container {
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 2rem;
        min-height: 500px;
    }

    .counter-card {
        background: linear-gradient(135deg, #a855f7 0%, #3b82f6 100%);
        border-radius: 20px;
        padding: 3rem;
        box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
        max-width: 500px;
        width: 100%;
        text-align: center;
    }

    .counter-title {
        color: white;
        font-size: 1.875rem;
        font-weight: 700;
        margin: 0 0 2rem 0;
        text-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    }

    .counter-display {
        background: rgba(255, 255, 255, 0.15);
        border-radius: 15px;
        padding: 3rem 2rem;
        margin-bottom: 2rem;
        backdrop-filter: blur(10px);
        border: 1px solid rgba(255, 255, 255, 0.2);
    }

    .counter-value {
        font-size: 4rem;
        font-weight: 800;
        display: block;
        transition: all 0.3s ease;
        letter-spacing: -2px;
    }

    .counter-buttons {
        display: flex;
        gap: 1rem;
        justify-content: center;
    }

    .button {
        width: 60px;
        height: 60px;
        border: none;
        border-radius: 12px;
        font-size: 1.5rem;
        font-weight: 700;
        cursor: pointer;
        transition: all 0.3s ease;
        color: white;
        display: flex;
        align-items: center;
        justify-content: center;
        box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
    }

    .button:hover {
        transform: translateY(-3px);
        box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
    }

    .button:active {
        transform: translateY(-1px);
    }

    .button-increment {
        background-color: #10b981;
        flex: 1;
    }

    .button-increment:hover {
        background-color: #059669;
    }

    .button-decrement {
        background-color: #ef4444;
        flex: 1;
    }

    .button-decrement:hover {
        background-color: #dc2626;
    }

    .button-reset {
        background-color: rgba(255, 255, 255, 0.2);
        border: 2px solid rgba(255, 255, 255, 0.4);
    }

    .button-reset:hover {
        background-color: rgba(255, 255, 255, 0.3);
        border-color: rgba(255, 255, 255, 0.6);
    }

    @media (max-width: 480px) {
        .counter-card {
            padding: 2rem;
        }

        .counter-value {
            font-size: 3rem;
        }

        .button {
            width: 50px;
            height: 50px;
            font-size: 1.25rem;
        }

        .counter-buttons {
            gap: 0.75rem;
        }
    }
</style>
    