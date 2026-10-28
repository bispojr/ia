<template>
  <div :class="['prolog-repl', hasProgramSlot ? 'two-col' : 'one-col']">

    <!-- Left column: editable knowledge base (only shown when program is provided) -->
    <div v-if="hasProgramSlot" class="kb-col">
      <div class="section-label">Base de Conhecimento</div>
      <textarea
        v-model="programText"
        class="kb-input"
        spellcheck="false"
      />
    </div>

    <!-- Right column (or full width): query + output -->
    <div class="query-col">
      <div class="section-label">Consulta</div>
      <div class="code-area">
        <span class="prompt">?-</span>
        <textarea
          v-model="code"
          class="code-input"
          rows="2"
          spellcheck="false"
        />
      </div>
      <button class="run-btn" :disabled="running" @click="runQuery">
        {{ running ? 'Executando...' : 'Executar' }}
      </button>
      <div v-if="outputLines.length > 0" class="output-area">
        <pre :class="['output-text', { 'output-error': outputIsError }]">{{ displayOutput }}</pre>
        <button v-if="hasMore && !running" class="next-btn" @click="nextSolution">
          <span class="next-label">;</span> próxima solução
        </button>
      </div>
    </div>

  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'

const props = defineProps({
  initialCode: {
    type: String,
    default: ''
  },
  program: {
    type: String,
    default: ''
  }
})

function normalizeQuery(raw) {
  let q = raw.trim()
  if (q.startsWith('?-')) q = q.slice(2).trim()
  if (q.endsWith('.')) q = q.slice(0, -1).trim()
  return q
}

// Keep period in the textarea — it is part of Prolog syntax
const code = ref(props.initialCode.trim())
const programText = ref(props.program ? props.program.trim() : '')
const hasProgramSlot = computed(() => !!props.program && props.program.trim().length > 0)
const outputLines = ref([])   // each entry is one answer string
const outputIsError = ref(false)
const hasMore = ref(false)
const running = ref(false)
const tauReady = ref(false)
const activeSession = ref(null)
const activeCaptured = ref(null)
const bufferedNext = ref(null)  // pre-fetched next answer text, or null

// Display: all lines except the last are followed by " ;"
const displayOutput = computed(() =>
  outputLines.value.map((line, i) =>
    i < outputLines.value.length - 1 ? line + ' ;' : line
  ).join('\n')
)

function loadTauProlog() {
  return new Promise((resolve, reject) => {
    if (window.pl) { resolve(); return }
    const script = document.createElement('script')
    script.src = 'https://cdn.jsdelivr.net/npm/tau-prolog@0.3.4/modules/core.js'
    script.onload = resolve
    script.onerror = reject
    document.head.appendChild(script)
  })
}

onMounted(async () => {
  try {
    await loadTauProlog()
    tauReady.value = true
  } catch {
    outputLines.value = ['[Erro: não foi possível carregar o Tau Prolog]']
  }
})

function makeSession() {
  const pl = window.pl
  const session = pl.create()
  let captured = ''
  const captureStream = new pl.type.Stream(
    {
      put(text) { captured += text; return true },
      flush() { return true }
    },
    'append', 'user_output', 'text', false, 'reset'
  )
  session.streams['user_output'] = captureStream
  session.current_output = captureStream
  session.thread.session.current_output = captureStream
  // Returns a function that flushes and returns whatever was captured since last call
  const getCaptured = () => { const c = captured; captured = ''; return c }
  return { session, getCaptured }
}

function formatAnswer(answer, getCaptured) {
  const pl = window.pl
  const captured = getCaptured()
  if (answer === false) return null  // no (more) solutions
  const formatted = pl.format_answer(answer) || 'true'
  return captured ? captured + '\n' + formatted : formatted
}

function runQuery() {
  if (!tauReady.value) {
    outputLines.value = ['[Tau Prolog ainda não carregado]']
    return
  }

  if (!code.value.trim().endsWith('.')) {
    outputIsError.value = true
    outputLines.value = ['Erro: a sequência de objetivos deve encerrar com um ponto final "."']
    return
  }

  running.value = true
  outputIsError.value = false
  outputLines.value = []
  hasMore.value = false
  bufferedNext.value = null
  activeSession.value = null
  activeCaptured.value = null

  const { session, getCaptured } = makeSession()
  const goal = normalizeQuery(code.value)

  const getFirstAnswer = () => {
    session.query(goal + '.', {
      success() {
        session.answer((answer) => {
          const text = formatAnswer(answer, getCaptured)
          if (text === null) {
            running.value = false
            outputLines.value = ['false.']
            hasMore.value = false
          } else {
            // Peek: check if there is a next solution
            session.answer((peek) => {
              running.value = false
              const peekText = formatAnswer(peek, getCaptured)
              outputLines.value = [text]
              if (peekText === null) {
                hasMore.value = false
              } else {
                hasMore.value = true
                activeSession.value = session
                activeCaptured.value = getCaptured
                bufferedNext.value = peekText
              }
            })
          }
        })
      },
      error(err) {
        running.value = false
        outputLines.value = ['[Erro de sintaxe: ' + (err && err.args ? err.args[0] : err) + ']']
      }
    })
  }

  if (programText.value.trim()) {
    session.consult(programText.value.trim(), {
      success: getFirstAnswer,
      error(err) {
        running.value = false
        outputLines.value = ['[Erro ao carregar programa: ' + (err && err.args ? err.args[0] : err) + ']']
      }
    })
  } else {
    getFirstAnswer()
  }
}

function nextSolution() {
  if (!activeSession.value || running.value) return
  running.value = true

  const sess = activeSession.value
  const getCaptured = activeCaptured.value
  const current = bufferedNext.value
  bufferedNext.value = null

  outputLines.value.push(current)

  // Peek: check if there is yet another solution after this one
  sess.answer((peek) => {
    running.value = false
    const peekText = formatAnswer(peek, getCaptured || (() => ''))
    if (peekText === null) {
      hasMore.value = false
      activeSession.value = null
      activeCaptured.value = null
    } else {
      hasMore.value = true
      bufferedNext.value = peekText
    }
  })
}
</script>

<style scoped>
.prolog-repl {
  background: #1e1e2e;
  border: 1px solid #45475a;
  border-radius: 8px;
  padding: 16px;
  font-family: 'JetBrains Mono', 'Fira Code', monospace;
  margin: 12px 0;
}

.two-col {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 16px;
}

.one-col {
  display: block;
}

.kb-col {
  display: flex;
  flex-direction: column;
  border-right: 1px solid #45475a;
  padding-right: 16px;
}

.kb-input {
  flex: 1;
  background: transparent;
  border: none;
  color: #89dceb;
  font-family: inherit;
  font-size: 0.88em;
  line-height: 1.6;
  resize: none;
  outline: none;
  min-height: 100px;
  width: 100%;
}

.query-col {
  display: flex;
  flex-direction: column;
}

.section-label {
  color: #6c7086;
  font-size: 0.75em;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  margin-bottom: 6px;
}

.code-area {
  display: flex;
  align-items: flex-start;
  gap: 8px;
}

.prompt {
  color: #cba6f7;
  font-weight: bold;
  line-height: 1.5;
  padding-top: 4px;
  user-select: none;
  flex-shrink: 0;
}

.code-input {
  flex: 1;
  background: transparent;
  border: none;
  color: #cdd6f4;
  font-family: inherit;
  font-size: 0.95em;
  line-height: 1.5;
  resize: vertical;
  outline: none;
  width: 100%;
}

.run-btn {
  margin-top: 10px;
  margin-bottom: 2px;
  background: #89b4fa;
  color: #1e1e2e;
  border: none;
  border-radius: 5px;
  padding: 6px 20px;
  font-family: inherit;
  font-weight: bold;
  cursor: pointer;
  font-size: 0.88em;
  transition: background 0.15s;
}

.run-btn:hover:not(:disabled) {
  background: #b4befe;
}

.run-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.output-area {
  margin-top: 10px;
  border-top: 1px solid #45475a;
  padding-top: 10px;
}

.output-text {
  color: #a6e3a1;
  margin: 0;
  font-family: inherit;
  font-size: 0.93em;
  white-space: pre-wrap;
}

.output-error {
  color: #f38ba8;
}

.next-btn {
  margin-top: 8px;
  background: #313244;
  color: #f5c2e7;
  border: 1px solid #585b70;
  border-radius: 5px;
  padding: 5px 16px;
  font-family: inherit;
  font-size: 0.88em;
  cursor: pointer;
  transition: background 0.15s;
}

.next-btn:hover {
  background: #45475a;
}

.next-label {
  font-weight: bold;
  color: #cba6f7;
}
</style>
