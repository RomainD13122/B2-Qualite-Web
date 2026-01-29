<script setup>
import { getRuleById } from '~/data/rules'

const ruleId = 136
const rule = getRuleById(ruleId)
const activeTab = ref('preview')
</script>

<template>
  <section v-if="rule" class="space-y-6">
    <header class="space-y-3">
      <button
        @click="$router.back()"
        class="inline-flex items-center gap-2 text-sm text-zinc-400 hover:text-zinc-200 transition"
      >
        ← Retour
      </button>
      <div class="text-sm text-zinc-400">Règle n° {{ rule.id }}</div>

      <h1
        class="text-2xl sm:text-3xl font-semibold tracking-tight text-zinc-100"
      >
        {{ rule.title }}
      </h1>

      <div class="text-base sm:text-sm tracking-tight text-zinc-300">
        {{ rule.description }}
      </div>

      <div class="flex flex-wrap gap-2">
        <span
          v-for="tag in rule.tags"
          :key="tag"
          class="text-xs rounded-full border border-zinc-800 bg-zinc-900/30 px-2.5 py-1 text-zinc-300"
        >
          {{ tag }}
        </span>
      </div>
    </header>

    <section class="rounded-2xl border border-zinc-800 bg-zinc-900/30 p-6">
      <h2 class="text-lg font-semibold tracking-tight text-zinc-100">
        Objectif
      </h2>
      <ul class="mt-1 list-disc pl-5 space-y-2 text-sm text-zinc-300">
        <li v-for="o in rule.objectives" :key="o">{{ o }}</li>
      </ul>
    </section>

    <section class="rounded-2xl border border-zinc-800 bg-zinc-900/30 p-6">
      <h2 class="text-lg font-semibold tracking-tight text-zinc-100">
        Mise en œuvre
      </h2>
      <p v-if="rule.implementationIntro" class="mt-3 text-sm text-zinc-400">
        {{ rule.implementationIntro }}
      </p>
      <ul class="mt-1 list-disc pl-5 space-y-2 text-sm text-zinc-300">
        <li v-for="x in rule.implementation" :key="x">{{ x }}</li>
      </ul>
    </section>

    <section class="rounded-2xl border border-zinc-800 bg-zinc-900/30 p-6">
      <h2 class="text-lg font-semibold tracking-tight text-zinc-100">
        Contrôle
      </h2>
      <ul class="mt-3 list-disc pl-5 space-y-2 text-sm text-zinc-300">
        <li v-for="c in rule.control" :key="c">{{ c }}</li>
      </ul>
    </section>

    <section class="space-y-4">
      <h2 class="text-lg font-semibold tracking-tight text-zinc-100">
        Exemples
      </h2>

      <div
        class="rounded-2xl border border-zinc-800 bg-zinc-900/30 overflow-hidden"
      >
        <div class="flex border-b border-zinc-800">
          <button
            @click="activeTab = 'preview'"
            :class="[
              'px-5 py-3 text-sm transition',
              activeTab === 'preview'
                ? 'text-zinc-100 border-b-2 border-zinc-100'
                : 'text-zinc-400 hover:text-zinc-200',
            ]"
          >
            Rendu
          </button>

          <button
            @click="activeTab = 'code'"
            :class="[
              'px-5 py-3 text-sm transition',
              activeTab === 'code'
                ? 'text-zinc-100 border-b-2 border-zinc-100'
                : 'text-zinc-400 hover:text-zinc-200',
            ]"
          >
            Code
          </button>
        </div>

        <div class="p-6">
          <div v-if="activeTab === 'preview'" class="space-y-6">
            <div class="text-sm text-zinc-400">
              Simulation visuelle : Le lien "Guide Linux" a déjà été cliqué (couleur violette), indiquant à l'utilisateur qu'il a déjà consulté cette ressource.
            </div>

            <div class="rounded-xl border border-zinc-800 bg-zinc-950 p-6">
              <h3 class="text-zinc-100 font-medium mb-4">Guides de durcissement (CIS Benchmarks)</h3>
              
              <ul class="space-y-4">
                <li class="flex items-start gap-3">
                  <div class="mt-1 w-1.5 h-1.5 rounded-full bg-blue-500"></div>
                  <div>
                    <a href="#" class="text-blue-400 hover:underline block font-medium">
                      Guide de durcissement Windows Server 2022
                    </a>
                    <p class="text-xs text-zinc-500 mt-0.5">Non consulté</p>
                  </div>
                </li>

                <li class="flex items-start gap-3">
                  <div class="mt-1 w-1.5 h-1.5 rounded-full bg-purple-500"></div>
                  <div>
                    <a href="#" class="text-purple-400 hover:underline block font-medium">
                      Guide de durcissement Ubuntu Linux 22.04 LTS
                    </a>
                    <p class="text-xs text-zinc-500 mt-0.5">Déjà consulté</p>
                  </div>
                </li>
              </ul>
            </div>
          </div>

          <div v-else>
            <pre
              class="rounded-xl bg-zinc-950 p-5 overflow-x-auto text-sm text-zinc-100"
            >
<code>/* CSS : Utilisation de la pseudo-classe :visited */

/* État par défaut (Non visité) */
a.doc-link {
  color: #60a5fa; /* Blue-400 */
  text-decoration: none;
}

/* État visité (Déjà cliqué) */
a.doc-link:visited {
  color: #c084fc; /* Purple-400 */
}

a.doc-link:hover {
  text-decoration: underline;
}</code>
</pre>
            <p class="mt-3 text-xs text-zinc-500">
              Note technique : Pour des raisons de confidentialité (protection de l'historique utilisateur), les navigateurs modernes limitent les propriétés CSS modifiables via <code>:visited</code> (principalement la couleur).
            </p>
          </div>
        </div>
      </div>
    </section>
  </section>

  <section v-else class="rounded-2xl border border-zinc-800 bg-zinc-900/30 p-6">
    <h1 class="text-lg font-semibold text-zinc-100">Règle introuvable</h1>
    <p class="mt-2 text-sm text-zinc-400">
      Vérifiez que la règle existe dans
      <code class="text-zinc-300">rules.json</code>.
    </p>
  </section>
</template>