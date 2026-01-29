<script setup>
import { getRuleById } from '~/data/rules'

const ruleId = 134
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
              Comparaison sur un message d'avertissement de sécurité. L'utilisateur s'attend naturellement à ce que tout texte souligné soit interactif.
            </div>

            <div class="space-y-6">
              <div class="rounded-xl border border-zinc-800 bg-zinc-950 p-5">
                <div class="mb-2 text-xs font-medium text-red-400 uppercase tracking-wider">Mauvaise pratique</div>
                <div class="p-4 bg-zinc-900 rounded border border-zinc-800">
                    <p class="text-zinc-300">
                        <span class="underline decoration-zinc-400 cursor-default">IMPORTANT : Ne partagez jamais votre mot de passe, même avec le support IT.</span>
                        Si vous avez un doute, contactez le RSSI.
                    </p>
                    <p class="mt-2 text-xs text-zinc-500 italic">
                        (L'utilisateur essaie de cliquer sur "Important..." mais rien ne se passe)
                    </p>
                </div>
              </div>

              <div class="rounded-xl border border-zinc-800 bg-zinc-950 p-5">
                <div class="mb-2 text-xs font-medium text-green-400 uppercase tracking-wider">Bonne pratique</div>
                <div class="p-4 bg-zinc-900 rounded border border-zinc-800">
                    <p class="text-zinc-300">
                        <strong class="font-bold text-amber-400">IMPORTANT :</strong> Ne partagez jamais votre mot de passe, même avec le support IT.
                        Si vous avez un doute, 
                        <a href="#" class="text-blue-400 underline hover:text-blue-300">contactez le RSSI</a>.
                    </p>
                </div>
              </div>
            </div>
          </div>

          <div v-else>
            <pre
              class="rounded-xl bg-zinc-950 p-5 overflow-x-auto text-sm text-zinc-100"
            >
<code>&lt;!-- ❌ MAUVAIS : L'utilisateur pense que c'est un lien --&gt;
&lt;p&gt;
  &lt;u&gt;IMPORTANT : Ne donnez jamais votre mot de passe.&lt;/u&gt;
&lt;/p&gt;

&lt;!-- ✅ BON : On utilise le gras ou la couleur pour l'emphase --&gt;
&lt;!-- Le soulignement est réservé exclusivement à la balise &lt;a&gt; --&gt;
&lt;p&gt;
  &lt;strong class=&quot;text-amber-400&quot;&gt;IMPORTANT :&lt;/strong&gt; 
  Ne donnez jamais votre mot de passe.
  &lt;br&gt;
  En cas de doute, 
  &lt;a href=&quot;#&quot; class=&quot;underline text-blue-400&quot;&gt;ouvrez un ticket&lt;/a&gt;.
&lt;/p&gt;</code>
</pre>
            <p class="mt-3 text-xs text-zinc-500">
              Cette convention est fondamentale pour l'affordance : la capacité d'un objet (ici le texte) à suggérer sa propre utilisation.
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