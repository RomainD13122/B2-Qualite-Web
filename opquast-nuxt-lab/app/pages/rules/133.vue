<script setup>
import { getRuleById } from '~/data/rules'

const ruleId = 133
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
              Démonstration de la cohérence : tous les liens présents dans le
              paragraphe (corps de texte) partagent strictement le même style
              (couleur indigo + soulignement léger).
            </div>

            <div class="rounded-xl border border-zinc-800 bg-zinc-950 p-6">
              <h3 class="text-zinc-100 font-semibold mb-3">
                Rapport d'audit de sécurité
              </h3>
              <p class="text-zinc-300 text-sm leading-7">
                L'analyse des journaux a révélé plusieurs anomalies. Nous avons
                d'abord examiné les
                <a
                  href="#"
                  class="text-indigo-400 underline decoration-indigo-400/30 hover:decoration-indigo-400 transition"
                  >règles du pare-feu périmétrique</a
                >
                pour vérifier les ports ouverts. Ensuite, nous avons audité la
                <a
                  href="#"
                  class="text-indigo-400 underline decoration-indigo-400/30 hover:decoration-indigo-400 transition"
                  >politique de mots de passe</a
                >
                des comptes administrateurs. Enfin, une vérification croisée avec
                les
                <a
                  href="#"
                  class="text-indigo-400 underline decoration-indigo-400/30 hover:decoration-indigo-400 transition"
                  >logs d'accès SSH</a
                >
                a permis d'identifier une tentative d'intrusion.
              </p>
            </div>
          </div>

          <div v-else>
            <pre
              class="rounded-xl bg-zinc-950 p-5 overflow-x-auto text-sm text-zinc-100"
            >
<code>&lt;!-- Utilisation d'une classe utilitaire identique pour tous les liens de même nature --&gt;
&lt;style&gt;
  .link-text {
    @apply text-indigo-400 underline decoration-indigo-400/30 hover:decoration-indigo-400;
  }
&lt;/style&gt;

&lt;p&gt;
  Nous avons examiné les 
  &lt;a href=&quot;#&quot; class=&quot;link-text&quot;&gt;règles du pare-feu&lt;/a&gt;, 
  puis la 
  &lt;a href=&quot;#&quot; class=&quot;link-text&quot;&gt;politique de mots de passe&lt;/a&gt; 
  et enfin les 
  &lt;a href=&quot;#&quot; class=&quot;link-text&quot;&gt;logs SSH&lt;/a&gt;.
&lt;/p&gt;</code>
</pre>
            <p class="mt-3 text-xs text-zinc-500">
              Cette cohérence permet à l'œil de scanner rapidement le texte et
              d'identifier immédiatement les ressources liées sans effort cognitif.
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