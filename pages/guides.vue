<template>
  <div class="min-h-screen bg-gray-50">
    <!-- Header -->
    <header class="bg-white shadow-sm border-b">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4">
        <div class="flex justify-between items-center">
          <h1 class="text-3xl font-bold text-gray-900">Expression Guides</h1>
          <UButton to="/" variant="outline">Back to Training</UButton>
        </div>
      </div>
    </header>

    <!-- Main Content -->
    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
      <!-- Expression Cards -->
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        <div
          v-for="expression in expressionGuides"
          :key="expression.name"
          class="bg-white rounded-lg shadow-lg overflow-hidden hover:shadow-xl transition-shadow"
        >
          <!-- Expression Header -->
          <div :class="expression.headerClass" class="px-6 py-4">
            <h3 class="text-xl font-bold text-white capitalize">{{ expression.name }}</h3>
          </div>

          <!-- Expression Content -->
          <div class="px-6 py-4">
            <p class="text-gray-600 mb-4">{{ expression.description }}</p>
            
            <!-- Key Indicators -->
            <div class="space-y-3">
              <div>
                <h4 class="font-semibold text-gray-800 mb-2">👁️ Eyes & Eyebrows</h4>
                <ul class="text-sm text-gray-600 space-y-1">
                  <li v-for="indicator in expression.eyes" :key="indicator" class="flex items-start">
                    <span class="text-blue-500 mr-2">•</span>
                    {{ indicator }}
                  </li>
                </ul>
              </div>

              <div>
                <h4 class="font-semibold text-gray-800 mb-2">👄 Mouth & Lips</h4>
                <ul class="text-sm text-gray-600 space-y-1">
                  <li v-for="indicator in expression.mouth" :key="indicator" class="flex items-start">
                    <span class="text-green-500 mr-2">•</span>
                    {{ indicator }}
                  </li>
                </ul>
              </div>

              <div v-if="expression.other.length > 0">
                <h4 class="font-semibold text-gray-800 mb-2">🎭 Other Features</h4>
                <ul class="text-sm text-gray-600 space-y-1">
                  <li v-for="indicator in expression.other" :key="indicator" class="flex items-start">
                    <span class="text-purple-500 mr-2">•</span>
                    {{ indicator }}
                  </li>
                </ul>
              </div>
            </div>

            <!-- Micro-expression Notes -->
            <div class="mt-4 p-3 bg-yellow-50 border border-yellow-200 rounded-lg">
              <h5 class="font-semibold text-yellow-800 text-sm mb-1">⚡ Micro-expression Tips:</h5>
              <p class="text-xs text-yellow-700">{{ expression.microTips }}</p>
            </div>
          </div>
        </div>
      </div>

      <!-- General Tips Section -->
      <div class="mt-12 bg-white rounded-lg shadow-lg p-8">
        <h2 class="text-2xl font-bold text-gray-900 mb-6">General Tips for Micro-Expression Recognition</h2>
        
        <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
          <div>
            <h3 class="text-lg font-semibold text-gray-800 mb-3">🎯 Focus Areas</h3>
            <ul class="space-y-2 text-gray-600">
              <li class="flex items-start">
                <span class="text-blue-500 mr-2">•</span>
                Watch the upper face (eyes and eyebrows) - they're harder to control consciously
              </li>
              <li class="flex items-start">
                <span class="text-blue-500 mr-2">•</span>
                Pay attention to asymmetry - genuine emotions are often symmetrical
              </li>
              <li class="flex items-start">
                <span class="text-blue-500 mr-2">•</span>
                Look for brief flashes (1/25th to 1/5th of a second)
              </li>
              <li class="flex items-start">
                <span class="text-blue-500 mr-2">•</span>
                Notice timing - micro-expressions happen before conscious control kicks in
              </li>
            </ul>
          </div>

          <div>
            <h3 class="text-lg font-semibold text-gray-800 mb-3">⚡ Practice Tips</h3>
            <ul class="space-y-2 text-gray-600">
              <li class="flex items-start">
                <span class="text-green-500 mr-2">•</span>
                Start with longer durations and gradually decrease
              </li>
              <li class="flex items-start">
                <span class="text-green-500 mr-2">•</span>
                Practice one expression type at a time initially
              </li>
              <li class="flex items-start">
                <span class="text-green-500 mr-2">•</span>
                Use the replay button to study expressions you missed
              </li>
              <li class="flex items-start">
                <span class="text-green-500 mr-2">•</span>
                Don't get discouraged - even experts get 70-80% accuracy
              </li>
            </ul>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
const expressionGuides = [
  {
    name: 'anger',
    headerClass: 'bg-red-600',
    description: 'Characterized by furrowed brows, tense jaw, and narrowed eyes. Often the most recognizable micro-expression.',
    eyes: [
      'Eyebrows pulled down and together',
      'Upper eyelids tensed and lowered',
      'Eyes may appear smaller/narrowed',
      'Intense, direct gaze'
    ],
    mouth: [
      'Lips pressed together tightly',
      'Corners of mouth may turn down',
      'Jaw muscles tensed',
      'Sometimes lips may part slightly'
    ],
    other: [
      'Vertical lines between eyebrows',
      'Nostrils may flare slightly',
      'Overall facial tension'
    ],
    microTips: 'Look for the characteristic "V" shape formed by the eyebrows - this is the most reliable indicator and appears very quickly.'
  },
  {
    name: 'disgust',
    headerClass: 'bg-green-600',
    description: 'Shows rejection or revulsion. Often involves the nose and upper lip area prominently.',
    eyes: [
      'Upper eyelid may raise slightly',
      'Eyebrows may lower slightly',
      'Eyes may narrow',
      'Crow\'s feet may appear'
    ],
    mouth: [
      'Upper lip raised (especially center)',
      'Nose wrinkled',
      'Sometimes lower lip pushes up',
      'Mouth may open slightly'
    ],
    other: [
      'Cheeks raised',
      'Lines from nose to mouth deepen',
      'Head may pull back slightly'
    ],
    microTips: 'The raised upper lip and wrinkled nose happen almost simultaneously - even brief flashes show this combination clearly.'
  },
  {
    name: 'fear',
    headerClass: 'bg-purple-600',
    description: 'Characterized by wide eyes, raised eyebrows, and often an open mouth. Creates an "alert" appearance.',
    eyes: [
      'Eyebrows raised and pulled together',
      'Upper eyelids raised (wide eyes)',
      'Lower eyelids tensed',
      'Whites of eyes may show above iris'
    ],
    mouth: [
      'Mouth often opens',
      'Lips may be stretched horizontally',
      'Jaw may drop',
      'Sometimes lips tremble'
    ],
    other: [
      'Horizontal wrinkles across forehead',
      'Face may appear "frozen"',
      'Overall wide-eyed expression'
    ],
    microTips: 'The combination of raised eyebrows with wide eyes is key - this creates distinctive horizontal forehead lines even in brief flashes.'
  },
  {
    name: 'contempt',
    headerClass: 'bg-orange-600',
    description: 'Unique for being asymmetrical. Shows superiority or disdain, often with one-sided mouth movements.',
    eyes: [
      'May have slight squinting',
      'One eyebrow may raise slightly',
      'Eyes may narrow on one side',
      'Overall expression of looking "down" on something'
    ],
    mouth: [
      'One corner of mouth raised (half-smile)',
      'Asymmetrical lip movement',
      'May show slight sneer',
      'One side of face more active'
    ],
    other: [
      'Pronounced asymmetry',
      'One cheek may raise more',
      'Head may tilt slightly'
    ],
    microTips: 'Contempt is the only universally asymmetrical expression - look for the one-sided mouth raise, even if very brief.'
  },
  {
    name: 'happy',
    headerClass: 'bg-yellow-500',
    description: 'Genuine happiness involves both mouth and eyes. Look for "crow\'s feet" and raised cheeks.',
    eyes: [
      'Crow\'s feet wrinkles appear',
      'Eyes may "squint" slightly',
      'Lower eyelids may push up',
      'Genuine "sparkle" in eyes'
    ],
    mouth: [
      'Corners of mouth turn up',
      'May show teeth',
      'Upper lip may raise',
      'Smile appears effortless'
    ],
    other: [
      'Cheeks raised and rounded',
      'Nasolabial folds deepen',
      'Overall face "lifts"'
    ],
    microTips: 'True happiness shows in the eyes - look for crow\'s feet wrinkles, even in micro-expressions. Fake smiles only involve the mouth.'
  },
  {
    name: 'sad',
    headerClass: 'bg-blue-600',
    description: 'Shows loss or dejection. Often involves drooping features and inner eyebrow raising.',
    eyes: [
      'Inner corners of eyebrows raised',
      'Upper eyelids may droop slightly',
      'Eyes may appear "heavy"',
      'May have a distant look'
    ],
    mouth: [
      'Corners of mouth turn down',
      'Lower lip may push out slightly',
      'Mouth may appear "heavy"',
      'Lip corners pull down'
    ],
    other: [
      'Inner eyebrow corners create "sad" angle',
      'Overall "drooping" appearance',
      'Face may appear to "sink"'
    ],
    microTips: 'The inner eyebrow raise is the most reliable sad indicator - it creates an inverted "V" that\'s hard to fake and appears even in brief flashes.'
  }
]
</script>
