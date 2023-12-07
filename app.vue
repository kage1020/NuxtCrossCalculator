<template>
  <main class="grid place-items-center w-screen h-screen overflow-hidden bg-stone-200 dark:bg-stone-900 font-inter">
    <div class="absolute top-8 right-8">
      <ThemeSwitcher />
    </div>
    <div class="rounded-lg border border-stone-200 bg-white text-stone-950 shadow-sm dark:border-stone-800 dark:bg-stone-950 dark:text-stone-50">
      <div class="flex flex-col space-y-1.5 p-6">Nuxt Calculator</div>
      <div class="p-6 pt-0">
        <div class="grid grid-cols-4 gap-4">
          <div class="col-span-4 h-10 rounded p-2 bg-stone-100 dark:bg-stone-800 flex items-center justify-end tracking-wider">
            <template v-for="f in formula">
              <UIcon :name="f.icon" v-if="f.icon" class="w-5 h-5" />
              <span v-if="!f.icon">{{ f.label }}</span>
            </template>
          </div>
          <UButton v-for="l in labels" @click="handlePush(l)" :class="['inline-flex items-center justify-center whitespace-nowrap rounded-md font-medium ring-offset-white transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-stone-950 focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50 dark:ring-offset-stone-950 dark:focus-visible:ring-stone-300 bg-stone-100 text-stone-900 hover:bg-stone-100/80 dark:bg-stone-800 dark:text-stone-50 dark:hover:bg-stone-800/80 h-10 px-4 py-2 text-lg', l.row === 1 ? 'row-span-1' : 'row-span-2 h-full', l.col === 1 ? 'col-span-1' : 'col-span-2']">
            <UIcon :name="l.icon" v-if="l.icon" class="w-6 h-6" />
            <span v-if="!l.icon">{{ l.label }}</span>
          </UButton>
        </div>
      </div>
    </div>
  </main>
</template>

<script setup lang="ts">
useHead({
  title: 'Cross Calculator',
  meta: [
    {
      name: 'description',
      content: 'Cross Framework Calculator',
    },
  ],
  link: [
    {
      rel: 'preconnect',
      href: 'https://fonts.googleapis.com',
    },
    {
      rel: 'preconnect',
      href: 'https://fonts.gstatic.com',
    },
    {
      rel: 'stylesheet',
      href: 'https://fonts.googleapis.com/css2?family=Inter&display=swap',
    },
  ]
})

const labels = [
  { label: 'C', icon: null, row: 1, col: 1 },
  { label: '/', icon: 'i-lucide-divide', row: 1, col: 1 },
  { label: '*', icon: 'i-lucide-x', row: 1, col: 1 },
  { label: '-', icon: 'i-lucide-minus', row: 1, col: 1 },
  { label: 7, icon: null, row: 1, col: 1 },
  { label: 8, icon: null, row: 1, col: 1 },
  { label: 9, icon: null, row: 1, col: 1 },
  { label: '+', icon: 'i-lucide-plus', row: 2, col: 1 },
  { label: 4, icon: null, row: 1, col: 1 },
  { label: 5, icon: null, row: 1, col: 1 },
  { label: 6, icon: null, row: 1, col: 1 },
  { label: 1, icon: null, row: 1, col: 1 },
  { label: 2, icon: null, row: 1, col: 1 },
  { label: 3, icon: null, row: 1, col: 1 },
  { label: '=', icon: 'i-lucide-equal', row: 2, col: 1 },
  { label: 0, icon: null, row: 1, col: 2 },
  { label: '.', icon: null, row: 1, col: 1 },
]

type Label = {
  label: string | number
  icon: string | null
  row: number
  col: number
}

const formula = useState<Label[]>('formula', () => [])

const handlePush = (label: Label) => {
  if (label.label === 'C') {
    formula.value = []
    return
  }
  else if (formula.value.length !== 0 && label.label === '=') {
    const result = eval(formula.value.map((f) => f.label).join(''))
    formula.value = [{ label: result.toString(), icon: null, row: 1, col: 1 }]
    return
  }
  if (formula.value.length === 0 && typeof label.label !== 'number') return
  if (formula.value.length === 1 && formula.value[0].label === 0 && typeof label.label === 'number') {
    formula.value = [label]
    return
  }
  formula.value = [...formula.value, label]
}
</script>
