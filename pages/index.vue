<template>
  <div class="min-h-screen bg-gray-50">
    <!-- Header with Scores -->
    <header class="bg-white shadow-sm border-b">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4">
        <div class="flex justify-between items-center">
          <h1 class="text-3xl font-bold text-gray-900">Micro Expression Trainer</h1>
          <UButton to="/guides" variant="outline">View Guides</UButton>
        </div>
        
        <!-- Score Display -->
        <div class="mt-4 grid grid-cols-7 gap-4">
          <div v-for="expression in expressions" :key="expression" 
               class="text-center bg-gray-100 rounded-lg p-3">
            <div class="text-sm font-medium text-gray-600 capitalize">{{ expression }}</div>
            <div class="text-2xl font-bold" :class="getScoreColor(scores[expression])">
              {{ scores[expression] }}
            </div>
          </div>
        </div>
      </div>
    </header>

    <!-- Main Training Area -->
    <main class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
      <!-- Image Display Area -->
      <div class="bg-white rounded-lg shadow-lg p-8 mb-8">
        <div class="flex justify-center items-center mb-6">
          <div class="relative w-48 h-32 bg-gray-200 border-1 border-gray-400 rounded-lg overflow-hidden">
            <img
              v-if="currentImage"
              :src="currentImage"
              alt="Facial expression"
              style="height: 600px"
            />
            <div v-else class="flex items-center justify-center h-full text-gray-500 text-sm text-center p-2" style="height: 600px; height: 600px">
              Click "Start Test" to begin
            </div>
          </div>
        </div>

        <!-- Controls -->
        <div class="space-y-6">
          <!-- Speed Control -->
          <div class="bg-gray-50 p-4 rounded-lg">
            <label class="block text-sm font-medium text-gray-700 mb-2">
              Flash Duration: {{ flashDuration }}ms
            </label>
            <input
              v-model="flashDuration"
              type="range"
              :min="40"
              :max="600"
              :step="10"
              class="w-full h-2 bg-red-100 rounded-lg cursor-pointer slider"
            />
          </div>

          <!-- Action Buttons -->
          <div class="flex justify-center space-x-4">
            <UButton 
              @click="startTest" 
              :disabled="isPlaying"
              size="lg"
              color="primary"
            >
              Start Test
            </UButton>
            <UButton 
              @click="replay" 
              :disabled="!hasPlayed || isPlaying"
              size="lg"
              variant="outline"
            >
              Replay
            </UButton>
          </div>

          <!-- Expression Buttons -->
          <div v-if="hasPlayed" class="grid grid-cols-3 gap-4">
            <UButton
              v-for="expression in expressions"
              :key="expression"
              @click="makeGuess(expression)"
              :disabled="hasGuessed"
              :color="getButtonColor(expression)"
              :variant="hasGuessed && expression === correctAnswer ? 'solid' : 'outline'"
              size="lg"
              class="capitalize"
            >
              {{ expression }}
            </UButton>
          </div>

          <!-- Feedback -->
          <div v-if="feedback" class="text-center p-4 rounded-lg" :class="feedbackClass">
            <p class="text-lg font-semibold">{{ feedback }}</p>
            <UButton @click="nextRound" class="mt-2" color="primary">
              Next Round
            </UButton>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
const expressions = ['anger', 'disgust', 'fear', 'contempt', 'happy', 'sad', 'surprised']

// Reactive state
const currentImage = ref('')
const flashDuration = ref(400)
const isPlaying = ref(false)
const hasPlayed = ref(false)
const hasGuessed = ref(false)
const correctAnswer = ref('')
const userGuess = ref('')
const feedback = ref('')
const currentActor = ref(null)

// Scores stored in localStorage
const scores = ref({
  anger: 0,
  disgust: 0,
  fear: 0,
  contempt: 0,
  happy: 0,
  sad: 0,
  surprised: 0,
})

// Load scores from localStorage on mount
onMounted(() => {
  const savedScores = localStorage.getItem('microExpressionScores')
  if (savedScores) {
    scores.value = JSON.parse(savedScores)
  }
})

// Save scores to localStorage whenever they change
watch(scores, (newScores) => {
  localStorage.setItem('microExpressionScores', JSON.stringify(newScores))
}, { deep: true })

// Computed properties
const feedbackClass = computed(() => {
  if (!feedback.value) return ''
  return userGuess.value === correctAnswer.value 
    ? 'bg-green-100 text-green-800 border border-green-200'
    : 'bg-red-100 text-red-800 border border-red-200'
})

// Methods
const getScoreColor = (score) => {
  if (score > 0) return 'text-green-600'
  if (score < 0) return 'text-red-600'
  return 'text-gray-600'
}

const getButtonColor = (expression) => {
  if (!hasGuessed.value) return 'primary'
  if (expression === correctAnswer.value) return 'green'
  if (expression === userGuess.value && expression !== correctAnswer.value) return 'red'
  return 'gray'
}

const getRandomExpression = () => {
  return expressions[Math.floor(Math.random() * expressions.length)]
}

const getRandomActor = () => {
  // Assuming actors are numbered 0-99, adjust range as needed
  return Math.floor(Math.random() * 18)
}

const getImagePath = (expression, actorId) => {
  const expressionName = expression === 'neutral' ? 'Neutral' : 
    expression.charAt(0).toUpperCase() + expression.slice(1)
  return `/data/actors/${actorId}/${expressionName}.jpg`
}

const startTest = async () => {
  if (isPlaying.value) {
    console.log("Already playing")
    return
  }
  
  isPlaying.value = true
  hasPlayed.value = false
  hasGuessed.value = false
  feedback.value = ''
  userGuess.value = ''
  
  // Select random expression and actor
  correctAnswer.value = getRandomExpression()
  currentActor.value = getRandomActor()
  
  // Show neutral image first
  currentImage.value = getImagePath('neutral', currentActor.value)
  
  
  // Wait a moment, then flash the expression
  await new Promise(resolve => setTimeout(resolve, 1000))
  
  // Flash the expression
  currentImage.value = getImagePath(correctAnswer.value, currentActor.value)
  
  // Return to neutral after flash duration
  await new Promise(resolve => setTimeout(resolve, flashDuration.value))
  currentImage.value = getImagePath('neutral', currentActor.value)
  
  isPlaying.value = false
  hasPlayed.value = true
}

const replay = async () => {
  if (!correctAnswer.value || isPlaying.value || !currentActor.value) return
  
  isPlaying.value = true
  
  // Show neutral image first
  currentImage.value = getImagePath('neutral', currentActor.value)
  
  // Wait a moment, then flash the expression
  await new Promise(resolve => setTimeout(resolve, 1000))
  
  // Flash the expression
  currentImage.value = getImagePath(correctAnswer.value, currentActor.value)
  
  // Return to neutral after flash duration
  await new Promise(resolve => setTimeout(resolve, flashDuration.value))
  currentImage.value = getImagePath('neutral', currentActor.value)
  
  isPlaying.value = false
}

const makeGuess = async (expression) => {
  if (hasGuessed.value) return
  
  hasGuessed.value = true
  userGuess.value = expression
  
  if (expression === correctAnswer.value) {
    feedback.value = 'Correct! Well done!'
    scores.value[expression]++
  } else {
    feedback.value = `Incorrect. The correct answer was ${correctAnswer.value}.`
    scores.value[expression]--
  }
  
  // Wait 1.5 seconds to show feedback, then automatically go to next round
  await new Promise(resolve => setTimeout(resolve, 800))
  nextRound()
}

const nextRound = async () => {
  hasPlayed.value = false
  hasGuessed.value = false
  feedback.value = ''
  userGuess.value = ''
  correctAnswer.value = ''
  currentImage.value = ''
  currentActor.value = null
  
  // Wait 50ms before starting next test
  await new Promise(resolve => setTimeout(resolve, 1000))
  startTest()
}
</script>
