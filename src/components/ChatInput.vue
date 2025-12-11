<script setup lang="ts">
import { ref } from 'vue'

const message = ref('')
const selectedModel = ref('Smart (GPT-5)')
const showModelMenu = ref(false)
const showOptionsMenu = ref(false)

const models = [
  { id: 1, name: 'Smart (GPT-5)', icon: '🎯' },
  { id: 2, name: 'Creative (Claude)', icon: '✨' },
  { id: 3, name: 'Fast (GPT-3.5)', icon: '⚡' }
]

const options = [
  { id: 1, name: 'Nueva conversación', icon: 'edit' },
  { id: 2, name: 'Cargar archivo', icon: 'upload' },
  { id: 3, name: 'Capturar pantalla', icon: 'camera' }
]

const suggestions = [
  'Escribir un primer borrador',
  'Obtener consejos',
  'Aprender algo nuevo',
  'Crear un plan',
  'Analizar datos'
]

const selectModel = (model: string) => {
  selectedModel.value = model
  showModelMenu.value = false
}

const selectOption = (optionName: string) => {
  console.log('Selected:', optionName)
  showOptionsMenu.value = false
}

const handleSuggestion = (suggestion: string) => {
  message.value = suggestion
}
</script>

<template>
  <div class="chat-container">
    <div class="chat-header">
      <h1 class="greeting">Buenos días, Juan</h1>
      <p class="subgreeting">¿En qué puedo ayudarte hoy?</p>
    </div>

    <div class="chat-input-wrapper">
      <div class="input-container">
        <div class="model-selector" @click="showModelMenu = !showModelMenu">
          <svg class="model-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <rect x="3" y="3" width="7" height="7" rx="1"/>
            <rect x="14" y="3" width="7" height="7" rx="1"/>
            <rect x="14" y="14" width="7" height="7" rx="1"/>
            <rect x="3" y="14" width="7" height="7" rx="1"/>
          </svg>
          <span>{{ selectedModel }}</span>
          <svg class="chevron" :class="{ open: showModelMenu }" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <polyline points="6 9 12 15 18 9"/>
          </svg>

          <transition name="dropdown">
            <div v-if="showModelMenu" class="dropdown-menu model-menu">
              <div
                v-for="model in models"
                :key="model.id"
                class="dropdown-item"
                @click.stop="selectModel(model.name)"
              >
                <span class="item-icon">{{ model.icon }}</span>
                <span>{{ model.name }}</span>
              </div>
            </div>
          </transition>
        </div>

        <input
          v-model="message"
          type="text"
          class="chat-input"
          placeholder="Pregunta cualquier cosa"
        />

        <div class="input-actions">
          <button class="action-btn" @click="showOptionsMenu = !showOptionsMenu">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <line x1="12" y1="5" x2="12" y2="19"/>
              <line x1="5" y1="12" x2="19" y2="12"/>
            </svg>
          </button>

          <button class="action-btn">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M21.44 11.05l-9.19 9.19a6 6 0 0 1-8.49-8.49l9.19-9.19a4 4 0 0 1 5.66 5.66l-9.2 9.19a2 2 0 0 1-2.83-2.83l8.49-8.48"/>
            </svg>
          </button>

          <button class="action-btn">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M12 1a3 3 0 0 0-3 3v8a3 3 0 0 0 6 0V4a3 3 0 0 0-3-3z"/>
              <path d="M19 10v2a7 7 0 0 1-14 0v-2"/>
              <line x1="12" y1="19" x2="12" y2="23"/>
              <line x1="8" y1="23" x2="16" y2="23"/>
            </svg>
          </button>
        </div>

        <transition name="dropdown">
          <div v-if="showOptionsMenu" class="dropdown-menu options-menu">
            <div
              v-for="option in options"
              :key="option.id"
              class="dropdown-item"
              @click="selectOption(option.name)"
            >
              <svg v-if="option.icon === 'edit'" class="item-svg-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"/>
                <path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"/>
              </svg>
              <svg v-else-if="option.icon === 'upload'" class="item-svg-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/>
                <polyline points="17 8 12 3 7 8"/>
                <line x1="12" y1="3" x2="12" y2="15"/>
              </svg>
              <svg v-else-if="option.icon === 'camera'" class="item-svg-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <rect x="2" y="2" width="20" height="20" rx="2" ry="2"/>
                <circle cx="12" cy="12" r="3"/>
                <path d="M16 2v4M8 2v4M2 8h20"/>
              </svg>
              <span>{{ option.name }}</span>
            </div>
          </div>
        </transition>
      </div>

      <div class="suggestions">
        <button
          v-for="(suggestion, index) in suggestions"
          :key="index"
          class="suggestion-btn"
          @click="handleSuggestion(suggestion)"
        >
          {{ suggestion }}
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.chat-container {
  width: 100%;
  max-width: 900px;
  margin: 0 auto;
  padding: 2rem;
}

.chat-header {
  text-align: center;
  margin-bottom: 3rem;
}

.greeting {
  font-size: 2.5rem;
  font-weight: 600;
  margin: 0;
  color: #1e293b;
  margin-bottom: 0.5rem;
}

.subgreeting {
  font-size: 1.25rem;
  color: #475569;
  margin: 0;
  font-weight: 400;
}

.chat-input-wrapper {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.input-container {
  position: relative;
  background: rgba(255, 255, 255, 0.9);
  backdrop-filter: blur(10px);
  border-radius: 16px;
  padding: 0.75rem 1rem;
  display: flex;
  align-items: center;
  gap: 1rem;
  box-shadow: 0 8px 32px rgba(59, 130, 246, 0.1);
  border: 1px solid rgba(147, 197, 253, 0.2);
}

.model-selector {
  position: relative;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.5rem 0.75rem;
  background: rgba(147, 197, 253, 0.1);
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s ease;
  white-space: nowrap;
}

.model-selector:hover {
  background: rgba(147, 197, 253, 0.2);
}

.model-icon {
  width: 18px;
  height: 18px;
  color: #3b82f6;
}

.model-selector span {
  font-size: 0.9rem;
  color: #1e293b;
}

.chevron {
  width: 16px;
  height: 16px;
  color: #64748b;
  transition: transform 0.2s ease;
}

.chevron.open {
  transform: rotate(180deg);
}

.chat-input {
  flex: 1;
  background: transparent;
  border: none;
  outline: none;
  color: #1e293b;
  font-size: 1rem;
  padding: 0.5rem;
}

.chat-input::placeholder {
  color: #cbd5e1;
}

.input-actions {
  display: flex;
  gap: 0.5rem;
}

.action-btn {
  width: 40px;
  height: 40px;
  border-radius: 8px;
  border: none;
  background: rgba(147, 197, 253, 0.1);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
}

.action-btn:hover {
  background: rgba(147, 197, 253, 0.2);
  transform: scale(1.05);
}

.action-btn svg {
  width: 20px;
  height: 20px;
  color: #3b82f6;
}

.dropdown-menu {
  position: absolute;
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(20px);
  border-radius: 12px;
  padding: 0.5rem;
  box-shadow: 0 12px 48px rgba(59, 130, 246, 0.15);
  border: 1px solid rgba(147, 197, 253, 0.2);
  z-index: 1000;
  min-width: 220px;
}

.model-menu {
  top: calc(100% + 8px);
  left: 0;
}

.options-menu {
  bottom: calc(100% + 8px);
  right: 0;
}

.dropdown-item {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.75rem 1rem;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s ease;
  color: #1e293b;
  font-size: 0.95rem;
}

.dropdown-item:hover {
  background: rgba(147, 197, 253, 0.1);
}

.item-icon {
  font-size: 1.2rem;
}

.item-svg-icon {
  width: 20px;
  height: 20px;
  color: #3b82f6;
  stroke-width: 2;
}

.suggestions {
  display: flex;
  flex-wrap: wrap;
  gap: 0.75rem;
  justify-content: center;
}

.suggestion-btn {
  padding: 0.75rem 1.5rem;
  background: rgba(147, 197, 253, 0.15);
  border: 1px solid rgba(147, 197, 253, 0.3);
  border-radius: 24px;
  color: #1e293b;
  font-size: 0.9rem;
  cursor: pointer;
  transition: all 0.2s ease;
  white-space: nowrap;
}

.suggestion-btn:hover {
  background: rgba(147, 197, 253, 0.25);
  border-color: rgba(147, 197, 253, 0.4);
  transform: translateY(-2px);
}

.dropdown-enter-active,
.dropdown-leave-active {
  transition: all 0.2s ease;
}

.dropdown-enter-from {
  opacity: 0;
  transform: translateY(-10px);
}

.dropdown-leave-to {
  opacity: 0;
  transform: translateY(-10px);
}

@media (max-width: 768px) {
  .greeting {
    font-size: 2rem;
  }

  .subgreeting {
    font-size: 1rem;
  }

  .model-selector span {
    font-size: 0.8rem;
  }

  .suggestions {
    gap: 0.5rem;
  }

  .suggestion-btn {
    padding: 0.6rem 1.2rem;
    font-size: 0.85rem;
  }
}
</style>
