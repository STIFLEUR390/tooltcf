<script>
var diplome = [
  {
    diplome: 'Etudes secondaires non complétées',
    point: 0
  },
  {
    diplome: "Diplôme d'études secondaires",
    point: 5
  },
  {
    diplome:
      "Degré, diplôme ou certificat de un (01) an obtenu dans le cadre d'un programme à une université, un collège, une école technique ou de métiers ou un autre institut",
    point: 15
  },
  {
    diplome:
      "Degré, diplôme ou certificat de deux (02) ans obtenu dans le cadre d'un programme à une université, un collège, une école technique ou de métiers ou un autre institut",
    point: 19
  },
  {
    diplome:
      "Baccalauréat OU degré, diplôme ou certificat de trois (03) ans obtenu dans le cadre d'un programme à une université, un collège, une école technique ou de métiers ou un autre institut",
    point: 21
  },
  {
    diplome:
      "Au moins deux degrés, diplômes ou certificats. Un doit avoir été obtenu dans le cadre d'un programme d'une durée de trois (03) ans ou plus ",
    point: 22
  },
  {
    diplome: "Diplôme d'études universitaires de deuxième cycle (Master)",
    point: 23
  },
  {
    diplome: 'Diplôme universitaire de troisième cycle (Ph.D)',
    point: 25
  }
]

var experience = [
  {
    title: '1 an',
    point: 9
  },
  {
    title: '2 à 3 ans',
    point: 11
  },
  {
    title: '4 à 5 ans',
    point: 13
  },
  {
    title: '6 ans et plus',
    point: 15
  }
]
export default {
  mounted() {
    this.pointAdaptabilite.a1 = 0
    this.pointAdaptabilite.a2 = 0
    this.pointAdaptabilite.a3 = 0
    this.pointAdaptabilite.a4 = 0
    this.pointAdaptabilite.a5 = 0
    this.pointAdaptabilite.a6 = 0
    this.pointAdaptabilite.a7 = 0
    this.affLangP.ee = 0
    this.affLangP.eo = 0
    this.affLangP.ce = 0
    this.affLangP.co = 0
    this.affLangS.ee = 0
    this.affLangS.eo = 0
    this.affLangS.ce = 0
    this.affLangS.co = 0
    this.affLangP.total = 0
    this.affLangS.total = 0
    this.pointAdaptabilite.total = 0
  },
  data() {
    return {
      diplomes: diplome,
      experiences: experience,
      age: {},
      pointDiplome: 0,
      pointAdaptabilite: {},
      pointEmploi: 0,
      pointExp: 0,
      languePrinc: {},
      affLangP: {},
      langueSec: {},
      affLangS: {},
      pointTotal: 0,
      decision: 'NON ADMISSIBLE',
      estAdmissible: false
    }
  },
  computed: {
    ageComputed() {
      if (this.age.val >= 18 && this.age.val <= 35) return 12
      if (this.age.val > 35) return 47 - this.age.val
      else return 0
    },
    languePrincComputed() {
      if (
        this.languePrinc.ee &&
        this.languePrinc.ce &&
        this.languePrinc.co &&
        this.languePrinc.eo
      ) {
        this.affLangP.ee = this.languePrinc.ee - 3
        this.affLangP.eo = this.languePrinc.eo - 3
        this.affLangP.ce = this.languePrinc.ce - 3
        this.affLangP.co = this.languePrinc.co - 3

        if (this.affLangP.ee == 7) this.affLangP.ee = 6
        if (this.affLangP.eo == 7) this.affLangP.eo = 6
        if (this.affLangP.co == 7) this.affLangP.co = 6
        if (this.affLangP.ce == 7) this.affLangP.ce = 6

        this.affLangP.total =
          this.affLangP.ce + this.affLangP.co + this.affLangP.ee + this.affLangP.eo
      }
    },
    langueSecComputed() {
      // console.info('yes')
      if (this.langueSec.ee && this.langueSec.ce && this.langueSec.co && this.langueSec.eo) {
        if (
          this.langueSec.ee > 4 &&
          this.langueSec.ce > 4 &&
          this.langueSec.co > 4 &&
          this.langueSec.eo > 4
        ) {
          this.affLangS.ee = 1
          this.affLangS.eo = 1
          this.affLangS.co = 1
          this.affLangS.ce = 1
        } else {
          this.affLangS.ee = 0
          this.affLangS.eo = 0
          this.affLangS.co = 0
          this.affLangS.ce = 0
        }

        this.affLangS.total =
          this.affLangS.ce + this.affLangS.co + this.affLangS.ee + this.affLangS.eo
      }
    },
    pointAdaptabiliteComputed() {
      return this.pointAdaptabilite.a1 +
        this.pointAdaptabilite.a2 +
        this.pointAdaptabilite.a3 +
        this.pointAdaptabilite.a4 +
        this.pointAdaptabilite.a5 +
        this.pointAdaptabilite.a6 +
        this.pointAdaptabilite.a7 >=
        10
        ? 10
        : 0
    },

    pointTotalComputed() {
      this.pointTotal =
        this.pointAdaptabiliteComputed +
        this.ageComputed +
        this.affLangP.total +
        this.affLangS.total +
        this.pointDiplome +
        this.pointEmploi +
        this.pointExp
      // console.log('valeur ', this.pointTotal)
      if (this.pointTotal >= 67) {
        this.decision = 'ADMISSIBLE'
        this.estAdmissible = true
      } else {
        this.decision = 'NON ADMISSIBLE'
      }

      return this.pointTotal
    }
  },
  methods: {},
  watch: {
    languePrincComputed() {},
    langueSecComputed() {},
    pointTotalComputed() {}
  }
}
</script>

<template>
  <div class="p-8">
    <div id="app">
      <div class="container mx-auto">
        <h2 class="text-center mb-5 font-bold text-xl md:text-2xl">
          CALCULER VOTRE POINTAGE (SCORE ADMISSIBILITE ENTREE EXPRESS)
        </h2>
        <div class="grid grid-cols-1 md:grid-cols-4 gap-4 my-2 bg-gray-300 pt-3">
          <h2 class="text-center mb-4 font-bold text-xl md:text-2xl md:col-span-2">
            By Tool TCF CANADA
          </h2>
          <div>
            <a href="https://tcfcanada.dm-cm.net/quizzes" class="text-blue-600 text-center"
              >Site web préparation TCF Canada</a
            >
          </div>
        </div>
        <div class="grid grid-cols-1 md:grid-cols-4 gap-4 my-2">
          <h2
            class="text-center mb-4 font-bold text-xl md:text-2xl col-span-1 md:col-span-4 text-red-600"
          >
            NB : Vous devez avoir au minimum 67 points sur 100
          </h2>
        </div>
        <div class="grid grid-cols-1 md:grid-cols-4 gap-4 my-2 bg-gray-300">
          <h2 class="text-center mb-4 font-bold text-xl md:text-2xl col-span-3">
            Âge (12 Points Max)
          </h2>
          <div class="bg-gray-700 text-center text-white">
            {{ ageComputed }}
          </div>
        </div>
        <div class="grid grid-cols-1 md:grid-cols-4 gap-4 my-2">
          <div class="text-center bg-yellow-200">AGE (Obligatoire)</div>
          <div class="text-center bg-green-300">Votre âge</div>
          <div>
            <select
              id="adaptability_1"
              v-model.number="age.val"
              class="px-4 py-2 border rounded-md w-full"
            >
              <option disabled>moins 18 ans</option>
              <option v-for="n in 27">{{ 17 + n }}</option>
              <option>47 ans ou plus</option>
            </select>
          </div>
          <div class="bg-gray-400 text-center text-white">
            {{ ageComputed }}
          </div>
        </div>

        <hr class="my-4" />
        <div class="grid grid-cols-1 md:grid-cols-4 gap-4 my-2 bg-gray-300">
          <h2 class="text-center mb-4 font-bold text-xl md:text-2xl col-span-3">
            Niveau d'études (25 Points Max)
          </h2>
          <div class="bg-gray-700 text-center text-white">
            {{ pointDiplome }}
          </div>
        </div>
        <div class="grid grid-cols-1 md:grid-cols-4 gap-4 my-2">
          <div class="text-center bg-yellow-200">Diplôme (Obligatoire)</div>
          <div class="text-center bg-green-300">Votre équivalent de diplôme</div>
          <div>
            <select
              id="adaptability_1"
              v-model.number="pointDiplome"
              class="px-4 py-2 border rounded-md w-full"
            >
              <option :value="item.point" v-for="item in diplomes">{{ item.diplome }}</option>
            </select>
          </div>
          <div class="bg-gray-400 text-center text-white">
            {{ pointDiplome }}
          </div>
        </div>

        <hr class="my-4" />
        <div class="grid grid-cols-1 md:grid-cols-4 gap-4 my-2 bg-gray-300">
          <h2 class="text-center mb-4 font-bold text-xl md:text-2xl col-span-3">
            Langues (28 Points Max = 24 pour la première langue + 4 pour la deuxième)
          </h2>
          <div class="bg-gray-700 text-center text-white">
            {{ affLangP.total }}
          </div>
        </div>
        <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
          <!-- Colonne fusionnée -->
          <div class="row-span-4 bg-gray-200 p-4 text-center py-8">
            <h2 class="text-center">Vos résultats en première langue :</h2>
            <p>TEF Canada ou TCF Canada ou IELTS</p>
            <p>(Obligatoire)</p>
          </div>

          <!-- Ligne 1, colonne 2 à 4 -->
          <div class="bg-yellow-200 p-4">Compréhension écrite / Reading :</div>
          <div class="bg-yellow-300 p-4">
            <select
              id="adaptability_1"
              class="px-4 py-2 border rounded-md w-full"
              v-model.number="languePrinc.ce"
            >
              <option disabled>Mettez votre NCLC / CLB</option>
              <option v-for="n in 4">{{ 6 + n }}</option>
            </select>
          </div>
          <div class="bg-yellow-400 p-4 text-center">{{ affLangP.ce }}</div>

          <!-- Ligne 2, colonne 2 à 4 -->
          <div class="bg-green-200 p-4">Expression écrite / Writing :</div>
          <div class="bg-green-300 p-4">
            <select
              id="adaptability_1"
              class="px-4 py-2 border rounded-md w-full"
              v-model.number="languePrinc.ee"
            >
              <option disabled>Mettez votre NCLC / CLB</option>
              <option v-for="n in 4">{{ 6 + n }}</option>
            </select>
          </div>
          <div class="bg-green-400 p-4 text-center">{{ affLangP.ee }}</div>

          <!-- Ligne 3, colonne 2 à 4 -->
          <div class="bg-blue-200 p-4">Compréhension orale / Listening :</div>
          <div class="bg-blue-300 p-4">
            <select
              id="adaptability_1"
              class="px-4 py-2 border rounded-md w-full"
              v-model.number="languePrinc.co"
            >
              <option disabled>Mettez votre NCLC / CLB</option>
              <option v-for="n in 4">{{ 6 + n }}</option>
            </select>
          </div>
          <div class="bg-blue-400 p-4 text-center">{{ affLangP.co }}</div>

          <!-- Ligne 4, colonne 2 à 4 -->
          <div class="bg-red-200 p-4">Expression orale / Speaking :</div>
          <div class="bg-red-300 p-4">
            <select
              id="adaptability_1"
              class="px-4 py-2 border rounded-md w-full"
              v-model.number="languePrinc.eo"
            >
              <option disabled>Mettez votre NCLC / CLB</option>
              <option v-for="n in 4">{{ 6 + n }}</option>
            </select>
          </div>
          <div class="bg-red-400 p-4 text-center">{{ affLangP.eo }}</div>
        </div>
      </div>

      <hr class="my-4" />
      <div class="grid grid-cols-1 md:grid-cols-4 gap-4 my-2 bg-gray-300">
        <h2 class="text-center mb-4 font-bold text-xl md:text-2xl col-span-3">
          Deuxième langue (4 Points Max)
        </h2>
        <div class="bg-gray-700 text-center text-white">
          {{ affLangS.total }}
        </div>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
        <!-- Colonne fusionnée -->
        <div class="row-span-4 bg-gray-200 p-4 text-center py-8">
          <h2 class="text-center">Vos résultats en première langue :</h2>
          <p>TEF Canada ou TCF Canada ou IELTS</p>
          <p>(Facultatif)</p>
        </div>

        <!-- Ligne 1, colonne 2 à 4 -->
        <div class="bg-yellow-200 p-4">Compréhension écrite / Reading :</div>
        <div class="bg-yellow-300 p-4">
          <select
            id="adaptability_1"
            class="px-4 py-2 border rounded-md w-full"
            v-model.number="langueSec.ce"
          >
            <option disabled>Mettez votre NCLC / CLB</option>
            <option v-for="n in 7">{{ 3 + n }}</option>
          </select>
        </div>
        <div class="bg-yellow-400 p-4 text-center">{{ affLangS.ce }}</div>

        <!-- Ligne 2, colonne 2 à 4 -->
        <div class="bg-green-200 p-4">Expression écrite / Writing :</div>
        <div class="bg-green-300 p-4">
          <select
            id="adaptability_1"
            class="px-4 py-2 border rounded-md w-full"
            v-model.number="langueSec.ee"
          >
            <option disabled>Mettez votre NCLC / CLB</option>
            <option v-for="n in 7">{{ 3 + n }}</option>
          </select>
        </div>
        <div class="bg-green-400 p-4 text-center">{{ affLangS.ee }}</div>

        <!-- Ligne 3, colonne 2 à 4 -->
        <div class="bg-blue-200 p-4">Compréhension orale / Listening :</div>
        <div class="bg-blue-300 p-4">
          <select
            id="adaptability_1"
            class="px-4 py-2 border rounded-md w-full"
            v-model.number="langueSec.co"
          >
            <option disabled>Mettez votre NCLC / CLB</option>
            <option v-for="n in 7">{{ 3 + n }}</option>
          </select>
        </div>
        <div class="bg-blue-400 p-4 text-center">{{ affLangS.co }}</div>

        <!-- Ligne 4, colonne 2 à 4 -->
        <div class="bg-red-200 p-4">Expression orale / Speaking :</div>
        <div class="bg-red-300 p-4">
          <select
            id="adaptability_1"
            class="px-4 py-2 border rounded-md w-full"
            v-model.number="langueSec.eo"
          >
            <option disabled>Mettez votre NCLC / CLB</option>
            <option v-for="n in 7">{{ 3 + n }}</option>
          </select>
        </div>
        <div class="bg-red-400 p-4 text-center">{{ affLangS.eo }}</div>
      </div>

      <hr class="my-4" />
      <div class="grid grid-cols-1 md:grid-cols-4 gap-4 my-2 bg-gray-300">
        <h2 class="text-center mb-4 font-bold text-xl md:text-2xl col-span-3">
          Expérience professionnelle (15 Points Max)
        </h2>
        <div class="bg-gray-700 text-center text-white">
          {{ pointExp }}
        </div>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-4 gap-4 my-2">
        <div class="text-center bg-yellow-200">Expérience de travail (Obligatoire)</div>
        <div class="text-center bg-green-300">
          Nombre d'années d'expérience professionnelle cumulées
        </div>
        <div>
          <select
            id="adaptability_1"
            class="px-4 py-2 border rounded-md w-full"
            v-model.number="pointExp"
          >
            <option v-for="item in experiences" :value="item.point">{{ item.title }}</option>
          </select>
        </div>
        <div class="bg-gray-400 text-center text-white">
          {{ pointExp }}
        </div>
      </div>

      <hr class="my-4" />
      <div class="grid grid-cols-1 md:grid-cols-4 gap-4 my-2 bg-gray-300">
        <h2 class="text-center mb-4 font-bold text-xl md:text-2xl col-span-3">
          Emploi réservé au Canada (10 Points Max)
        </h2>
        <div class="bg-gray-700 text-center text-white">
          {{ pointEmploi }}
        </div>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-4 gap-4 my-2">
        <div class="text-center bg-yellow-200">Offre d'emploi valide (Facultatif)</div>
        <div class="text-center bg-green-300">
          Avez-vous réçu une offre d'emploi valide au Canada ?
        </div>
        <div>
          <select
            id="adaptability_1"
            class="px-4 py-2 border rounded-md w-full"
            v-model.number="pointEmploi"
          >
            <option value="10">Oui</option>
            <option value="0">Non</option>
          </select>
        </div>
        <div class="bg-gray-400 text-center text-white">
          {{ pointEmploi }}
        </div>
      </div>

      <hr class="my-4" />

      <div class="grid grid-cols-1 md:grid-cols-4 gap-4 my-2 bg-gray-300">
        <h2 class="text-center mb-4 font-bold text-xl md:text-2xl col-span-3">
          Adaptabilité (10 Points Max) Facultatif
        </h2>
        <div class="bg-gray-700 text-center text-white">
          {{ pointAdaptabiliteComputed }}
        </div>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-4 gap-4 my-2">
        <div class="text-center bg-yellow-200">
          Niveau linguistique du conjoint dans une seule langue (Facultatif)
        </div>
        <div class="text-center bg-green-300">
          votre conjoint a-t-il passé un test de langue et a obtenu NCLC4 au moins dans toutes les
          épreuves ?
        </div>
        <div>
          <select
            id="adaptability_1"
            v-model.number="pointAdaptabilite.a1"
            class="px-4 py-2 border rounded-md w-full"
          >
            <option value="5">Oui</option>
            <option value="0" selected>Non</option>
          </select>
        </div>
        <div class="bg-gray-400 text-center text-white">
          {{ pointAdaptabilite.a1 }}
        </div>
      </div>
      <div class="grid grid-cols-1 md:grid-cols-4 gap-4 my-2">
        <div class="text-center bg-yellow-200">
          Etudes effectuées antérieurement au Canada (Facultatif)
        </div>

        <div class="text-center bg-green-300">Avez-vous fait des études au Canada ?</div>
        <div>
          <select v-model.number="pointAdaptabilite.a2" class="px-4 py-2 border rounded-md w-full">
            <option value="5">Oui</option>
            <option value="0" selected>Non</option>
          </select>
        </div>
        <div class="bg-gray-400 text-center text-white">
          {{ pointAdaptabilite.a2 }}
        </div>
      </div>
      <div class="grid grid-cols-1 md:grid-cols-4 gap-4 my-2">
        <div class="text-center bg-yellow-200">
          Etudes du conjoint effectuées antérieurement au Canada (Facultatif)
        </div>
        <div class="text-center bg-green-300">
          votre conjoint a-t-il fait des étdudes antérieures au Canada ?
        </div>
        <div>
          <select
            id="adaptability_1"
            class="px-4 py-2 border rounded-md w-full"
            v-model.number="pointAdaptabilite.a3"
          >
            <option value="5">Oui</option>
            <option value="0" selected>Non</option>
          </select>
        </div>
        <div class="bg-gray-400 text-center text-white">
          {{ pointAdaptabilite.a3 }}
        </div>
      </div>
      <div class="grid grid-cols-1 md:grid-cols-4 gap-4 my-2">
        <div class="text-center bg-yellow-200">
          Expérience professionnelle antérieurement acquise au Canada (Facultatif)
        </div>
        <div class="text-center bg-green-300">
          Avez-vous de l'expérience professionnelle acquise au Canada ?
        </div>
        <div>
          <select
            id="adaptability_1"
            class="px-4 py-2 border rounded-md w-full"
            v-model.number="pointAdaptabilite.a4"
          >
            <option value="10">Oui</option>
            <option value="0" selected>Non</option>
          </select>
        </div>
        <div class="bg-gray-400 text-center text-white">
          {{ pointAdaptabilite.a4 }}
        </div>
      </div>
      <div class="grid grid-cols-1 md:grid-cols-4 gap-4 my-2">
        <div class="text-center bg-yellow-200">
          Expérience professionnelle du conjoint antérieurement acquise au Canada (Facultatif)
        </div>
        <div class="text-center bg-green-300">
          votre conjoint a-t-il de l'expérience professionnelle au Canada ?
        </div>
        <div>
          <select
            id="adaptability_1"
            class="px-4 py-2 border rounded-md w-full"
            v-model.number="pointAdaptabilite.a5"
          >
            <option value="5">Oui</option>
            <option value="0" selected>Non</option>
          </select>
        </div>
        <div class="bg-gray-400 text-center text-white">
          {{ pointAdaptabilite.a5 }}
        </div>
      </div>
      <div class="grid grid-cols-1 md:grid-cols-4 gap-4 my-2">
        <div class="text-center bg-yellow-200">Emploi reservé au Canada (Facultatif)</div>
        <div class="text-center bg-green-300">
          Avez-vous réçu une offre d'emploi valide au Canada ?
        </div>
        <div>
          <select
            id="adaptability_1"
            class="px-4 py-2 border rounded-md w-full"
            v-model.number="pointAdaptabilite.a6"
          >
            <option value="5">Oui</option>
            <option value="0" selected>Non</option>
          </select>
        </div>
        <div class="bg-gray-400 text-center text-white">
          {{ pointAdaptabilite.a6 }}
        </div>
      </div>
      <div class="grid grid-cols-1 md:grid-cols-4 gap-4 my-2">
        <div class="text-center bg-yellow-200">
          Votre famille ou celle du conjoint au Canada (Facultatif)
        </div>
        <div class="text-center bg-green-300">
          Avez-vous ou votre conjoint de la famille au Canada ?
        </div>
        <div>
          <select
            id="adaptability_1"
            class="px-4 py-2 border rounded-md w-full"
            v-model.number="pointAdaptabilite.a7"
          >
            <option value="5">Oui</option>
            <option value="0" selected>Non</option>
          </select>
        </div>
        <div class="bg-gray-400 text-center text-white">
          {{ pointAdaptabilite.a7 }}
        </div>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-4 gap-4 my-2 bg-yellow-400">
        <h2 class="text-center mb-4 font-bold text-2xl col-span-3">{{ decision }}</h2>
        <div class="text-center text-white text-3xl">
          {{ pointTotalComputed }}
        </div>
      </div>
    </div>
  </div>
</template>
