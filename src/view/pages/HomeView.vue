<template>
  <div class="container mt-4">
    <div class="accordion" id="accordionExample">
      <div class="accordion-item">
        <h2 class="accordion-header" id="headingOne">
          <button
            class="py-2 accordion-button collapsed"
            type="button"
            data-bs-toggle="collapse"
            data-bs-target="#collapseOne"
            aria-expanded="false"
            aria-controls="collapseOne"
          >
            Parole Extra
          </button>
        </h2>
        <div
          id="collapseOne"
          class="accordion-collapse collapse"
          aria-labelledby="headingOne"
          data-bs-parent="#accordionExample"
        >
          <div class="accordion-body">
            <label for="extraWords" class="form-label"
              >Parole Extra (separate da spazi):</label
            >
            <textarea
              v-model="extraWords"
              id="extraWords"
              class="form-control"
              rows="3"
              @input="saveToLocalStorage('extraWords', extraWords)"
            ></textarea>
            <label for="excludeWords" class="form-label mt-2"
              >Parole da Escludere (separate da spazi):</label
            >
            <textarea
              v-model="excludeWords"
              id="excludeWords"
              class="form-control"
              rows="3"
              @input="saveToLocalStorage('excludeWords', excludeWords)"
            ></textarea>
          </div>
        </div>
      </div>
    </div>

    <label for="wordCount" class="form-label mt-3">Numero di Parole:</label>
    <div class="input-group mb-3">
      <input
        type="number"
        id="wordCount"
        v-model.number="wordCount"
        class="form-control"
        min="1"
        placeholder="Inserisci il numero di parole"
      />
      <button
        class="btn btn-outline-success"
        type="button"
        id="button-addon2"
        @click="generateText"
      >
        Genera Testo
      </button>
    </div>
    <div class="position-relative mt-4">
      <button
        :class="
          copyStatus === 'done' ? 'btn-outline-light' : 'btn-outline-secondary'
        "
        class="btn border-0 p-1 btn-sm position-absolute top-0 end-0"
        @click="copyText"
      >
        <span
          :class="{ 'text-success': copyStatus === 'done' }"
          class="material-symbols-outlined"
        >
          {{ copyStatus }}
        </span>
      </button>
      <textarea
        v-model="generatedText"
        class="form-control mt-3 pt-3"
        rows="8"
        readonly
      ></textarea>
    </div>
  </div>
</template>

<script>
import wordsArray from "../../tech_terms.js"; // Assicurati che questo file contenga l'array di parole

export default {
  data() {
    return {
      wordCount: 50,
      generatedText: "",
      extraWords: localStorage.getItem("extraWords") || "",
      excludeWords: localStorage.getItem("excludeWords") || "",
      copyStatus: "content_copy",
    };
  },
  methods: {
    saveToLocalStorage(key, value) {
      localStorage.setItem(key, value);
    },
    async generateText() {
      let words = [...wordsArray, ...this.extraWords.split(" ")].filter(
        (word) => !this.excludeWords.split(" ").includes(word)
      );
      let selectedWords = [];
      for (let i = 0; i < this.wordCount; i++) {
        selectedWords.push(words[Math.floor(Math.random() * words.length)]);
        if ((i + 1) % Math.floor(Math.random() * (20 - 10 + 1) + 10) === 0) {
          selectedWords.push(";");
        }
      }
      this.generatedText = "";
      for (let word of selectedWords) {
        await new Promise((resolve) => setTimeout(resolve, 100));
        this.generatedText += word + " ";
      }
    },
    copyText() {
      navigator.clipboard.writeText(this.generatedText).then(
        () => {
          this.copyStatus = "done"; // Cambia l'icona in "done"
          setTimeout(() => {
            this.copyStatus = "content_copy"; // Torna all'icona originale dopo 1 secondo
          }, 1000);
        },
        () => alert("Errore durante la copia del testo.")
      );
    },
  },
};
</script>

<style>
/* Stile per una visualizzazione migliore */
.container {
  max-width: 600px;
}
</style>
