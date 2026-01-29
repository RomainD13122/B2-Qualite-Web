<script setup>
import { getRuleById } from '~/data/rules'

const ruleId = 135
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
              Sur mobile ou tablette (pas de survol possible), les liens non différenciés sont invisibles. Testez la différence ci-dessous.
            </div>

            <div class="space-y-6">
              <div class="rounded-xl border border-red-900/30 bg-red-950/5 p-5 opacity-75">
                <div class="flex justify-between items-center mb-2">
                    <span class="text-xs font-medium text-red-400 uppercase">Mauvais (Lien invisible sans survol)</span>
                    <span class="text-[10px] text-zinc-500 border border-zinc-800 px-2 py-0.5 rounded">Simulez le mobile (pas de souris)</span>
                </div>
                <p class="text-zinc-300">
                  Pour sécuriser votre compte, activez le <a href="#" class="text-zinc-300 hover:text-blue-400 hover:underline cursor-pointer font-normal">MFA dans les paramètres</a> dès que possible.
                  (Essayez de trouver le lien sans passer la souris dessus !)
                </p>
              </div>

              <div class="rounded-xl border border-green-900/30 bg-green-950/5 p-5">
                <div class="mb-2 text-xs font-medium text-green-400 uppercase">Bon (Lien visible par défaut)</div>
                <p class="text-zinc-300">
                  Pour sécuriser votre compte, activez le <a href="#" class="text-blue-400 underline decoration-blue-400/50 hover:text-blue-300">MFA dans les paramètres</a> dès que possible.
                </p>
              </div>
            </div>
          </div>

          <div v-else>
            <pre
              class="rounded-xl bg-zinc-950 p-5 overflow-x-auto text-sm text-zinc-100"
            >
<code>&lt;!-- ❌ MAUVAIS : Le lien a la même couleur que le texte --&gt;
&lt;!-- Il ne change qu'au survol (hover), ce qui ne marche pas sur mobile --&gt;
&lt;a href=&quot;#&quot; class=&quot;text-zinc-300 hover:text-blue-400&quot;&gt;
  MFA dans les paramètres
&lt;/a&gt;

&lt;!-- ✅ BON : Le lien se distingue par la couleur ET/OU le soulignement --&gt;
&lt;!-- Accessible immédiatement, quel que soit le périphérique --&gt;
&lt;a href=&quot;#&quot; class=&quot;text-blue-400 underline&quot;&gt;
  MFA dans les paramètres
&lt;/a&gt;</code>
</pre>
            <p class="mt-3 text-xs text-zinc-500">
              Note : Le contraste de couleur doit être d'au moins 3:1 par rapport au texte environnant si le soulignement n'est pas utilisé.
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