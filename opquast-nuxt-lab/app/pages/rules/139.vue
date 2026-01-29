<script setup>
import { getRuleById } from '~/data/rules'

const ruleId = 139
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
              Différence entre un lien vague et un lien explicite qui prévient l'utilisateur de l'action système (ouverture du client mail).
            </div>

            <div class="space-y-6">
              <div class="rounded-xl border border-zinc-800 bg-zinc-950 p-5">
                <p class="text-xs text-red-400 font-semibold uppercase mb-2">Mauvais (L'utilisateur ne sait pas ce qui va se passer)</p>
                <div class="p-4 bg-zinc-900 rounded border border-zinc-800">
                    <p class="text-zinc-300">
                        Vous avez détecté une activité suspecte ?
                        <a href="mailto:soc@company.com" class="text-blue-400 underline">Contactez-nous</a>.
                    </p>
                </div>
              </div>

              <div class="rounded-xl border border-zinc-800 bg-zinc-950 p-5">
                <p class="text-xs text-green-400 font-semibold uppercase mb-2">Bon (Action explicite)</p>
                <div class="p-4 bg-zinc-900 rounded border border-zinc-800">
                    <p class="text-zinc-300 mb-3">
                        Vous avez détecté une activité suspecte ?
                    </p>
                    <a href="mailto:soc@company.com" class="inline-flex items-center gap-2 px-4 py-2 bg-zinc-800 hover:bg-zinc-700 border border-zinc-600 rounded text-zinc-100 transition-colors text-sm">
                        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/><polyline points="22,6 12,13 2,6"/></svg>
                        Envoyer un mail à l'équipe SOC
                    </a>
                </div>
              </div>
            </div>
          </div>

          <div v-else>
            <pre
              class="rounded-xl bg-zinc-950 p-5 overflow-x-auto text-sm text-zinc-100"
            >
<code>&lt;!-- ❌ Mauvais : "Contactez-nous" masque le protocole mailto: --&gt;
&lt;a href=&quot;mailto:soc@company.com&quot;&gt;Contactez-nous&lt;/a&gt;


&lt;!-- ✅ Bon : Le libellé annonce l'action d'envoi d'email --&gt;
&lt;a href=&quot;mailto:soc@company.com&quot;&gt;
  &lt;svg&gt;...&lt;/svg&gt;
  Envoyer un mail à l'équipe SOC
&lt;/a&gt;</code>
</pre>
            <p class="mt-3 text-xs text-zinc-500">
              Cela s'applique aussi aux liens <code>webcal:</code> (ajouter au calendrier) ou <code>skype:</code>.
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