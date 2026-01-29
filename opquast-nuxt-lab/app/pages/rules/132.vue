<script setup>
import { getRuleById } from '~/data/rules'

const ruleId = 132
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
              Comparaison entre un lien non explicite (pattern souvent utilisé en phishing) et un lien explicite (bonne pratique).
            </div>

            <div class="grid gap-6 md:grid-cols-2">
              <div class="rounded-xl border border-red-900/50 bg-red-950/10 p-5">
                <div class="flex items-center gap-2 mb-3 text-red-400">
                  <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><line x1="15" y1="9" x2="9" y2="15"/><line x1="9" y1="9" x2="15" y2="15"/></svg>
                  <span class="font-semibold text-sm">À éviter (Ambigu)</span>
                </div>
                <p class="text-sm text-zinc-300 leading-relaxed">
                  Une vulnérabilité critique a été détectée dans le module SSH.
                  Pour en savoir plus sur le correctif, 
                  <a href="#" class="text-blue-400 underline hover:text-blue-300">cliquez ici</a>.
                </p>
              </div>

              <div class="rounded-xl border border-emerald-900/50 bg-emerald-950/10 p-5">
                <div class="flex items-center gap-2 mb-3 text-emerald-400">
                  <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
                  <span class="font-semibold text-sm">Recommandé (Explicite)</span>
                </div>
                <p class="text-sm text-zinc-300 leading-relaxed">
                  Une vulnérabilité critique a été détectée dans le module SSH.
                  Veuillez consulter la 
                  <a href="#" class="text-blue-400 underline hover:text-blue-300">note de version du patch CVE-2024-34</a>.
                </p>
              </div>
            </div>
          </div>

          <div v-else>
            <pre
              class="rounded-xl bg-zinc-950 p-5 overflow-x-auto text-sm text-zinc-100"
            >
<code>&lt;!-- MAUVAIS : "cliquez ici" force l'utilisateur à lire tout le contexte --&gt;
&lt;p&gt;
  Pour en savoir plus sur le correctif, 
  &lt;a href=&quot;patch-note.html&quot;&gt;cliquez ici&lt;/a&gt;.
&lt;/p&gt;

&lt;!-- BON : Le lien est compréhensible même hors contexte (lecteur d'écran) --&gt;
&lt;p&gt;
  Veuillez consulter la 
  &lt;a href=&quot;patch-note.html&quot;&gt;note de version du patch CVE-2024-34&lt;/a&gt;.
&lt;/p&gt;</code>
</pre>
            <p class="mt-3 text-xs text-zinc-500">
              En cybersécurité, des liens explicites permettent aussi de vérifier la destination avant de cliquer, réduisant les risques d'ingénierie sociale.
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