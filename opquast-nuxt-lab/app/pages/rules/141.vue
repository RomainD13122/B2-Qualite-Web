<script setup>
import { getRuleById } from '~/data/rules'

const ruleId = 141
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
              Ouverture de documents légaux (PDF) ou de liens externes : l'utilisateur doit être averti pour ne pas être surpris par le nouvel onglet.
            </div>

            <div class="rounded-xl border border-zinc-800 bg-zinc-950 p-6 space-y-8">
              
              <div>
                <h3 class="text-sm text-zinc-500 font-medium mb-2 uppercase">Mention textuelle</h3>
                <p class="text-zinc-300">
                  Veuillez lire et signer la 
                  <a href="#" target="_blank" class="text-blue-400 hover:underline">
                    Charte Informatique (PDF - 2Mo)
                    <span class="sr-only">ouvre une nouvelle fenêtre</span>
                  </a>.
                </p>
                <p class="text-xs text-zinc-500 mt-1">
                  (Ici, l'avertissement est caché visuellement mais lu par les lecteurs d'écran via "sr-only", car le contexte PDF suffit souvent visuellement).
                </p>
              </div>

              <div class="h-px bg-zinc-900 w-full"></div>

              <div>
                <h3 class="text-sm text-zinc-500 font-medium mb-2 uppercase">Indicateur visuel (Icône)</h3>
                <p class="text-zinc-300 flex items-center gap-2">
                  Partenaire de certification :
                  <a href="https://www.ssi.gouv.fr/" target="_blank" rel="noopener noreferrer" class="inline-flex items-center gap-1.5 text-zinc-200 hover:text-white px-3 py-2 bg-zinc-900 border border-zinc-800 rounded transition-colors group">
                    <img src="https://upload.wikimedia.org/wikipedia/fr/thumb/3/37/ANSSI_logo.svg/1200px-ANSSI_logo.svg.png" alt="" class="h-4 w-auto grayscale group-hover:grayscale-0 transition-all opacity-70 group-hover:opacity-100">
                    Site de l'ANSSI
                    <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="text-zinc-500 group-hover:text-zinc-300"><path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/><polyline points="15 3 21 3 21 9"/><line x1="10" y1="14" x2="21" y2="3"/></svg>
                    <span class="sr-only">(nouvelle fenêtre)</span>
                  </a>
                </p>
              </div>

            </div>
          </div>

          <div v-else>
            <pre
              class="rounded-xl bg-zinc-950 p-5 overflow-x-auto text-sm text-zinc-100"
            >
<code>&lt;!-- Sécurité : rel="noopener noreferrer" est CRITIQUE avec target="_blank" --&gt;
&lt;!-- Accessibilité : Ajouter une mention textuelle ou une icône --&gt;

&lt;a href=&quot;charte.pdf&quot; target=&quot;_blank&quot; rel=&quot;noopener noreferrer&quot;&gt;
  Charte Informatique
  &lt;!-- Option A : Texte caché pour screen reader --&gt;
  &lt;span class=&quot;sr-only&quot;&gt;(nouvelle fenêtre)&lt;/span&gt;
  
  &lt;!-- Option B : Icône visible --&gt;
  &lt;svg class=&quot;icon-new-window&quot; aria-hidden=&quot;true&quot;&gt;...&lt;/svg&gt;
&lt;/a&gt;</code>
</pre>
            <p class="mt-3 text-xs text-zinc-500">
              Le <code>rel="noopener"</code> empêche la nouvelle page d'avoir accès à l'objet <code>window.opener</code>, bloquant ainsi les attaques de type "Reverse Tabnabbing" où un site malveillant redirige la page d'origine vers une fausse page de login.
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

<style scoped>
/* Classe utilitaire pour cacher du texte visuellement mais le garder pour les lecteurs d'écran */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}
</style>