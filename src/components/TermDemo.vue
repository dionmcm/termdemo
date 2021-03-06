<template>
  <div class="termdemo">
    <p>
      1. First we will find a body-part code:
      <autocomplete
      :search='findBodyPart'
      placeholder="Search For Body Parts"
      aria-label="Search For Body Parts"
      :get-result-value='getResultValue'
      @submit="handleSubmitBodyPart" />
    </p>
    <p>Selected body part: {{bodyPartCode}}</p>
    <hr/>
    <p>2. Next we will do a lookup to retrieve the details of the code:
    <button v-on:click="lookup" :disabled="!bodyPartCode">Lookup</button>
    </p>
    <p>Lookup result: {{detailedBodyPart}}</p>
    <hr/>
    <p>3. Next we will do a translate of the code to map it to SNOMED-CT:
    <button v-on:click="translate" :disabled="!bodyPartCode">Translate</button>
    </p>
    <p>Result: {{sctBodyPartCode}}</p>
    <hr/>
    <p>4. Now we have a standard code, we can do a search of SNOMED condition codes whose finding site is the mapped SNOMED-CT body part:
    <autocomplete
      :search='findCondition'
      placeholder="Search For Conditions"
      aria-label="Search For Conditions"
      :get-result-value="getResultValue"
      @submit="handleSubmitCondition"
      :disabled="!sctBodyPartCode"/>
    </p>
    <p>Result: {{sctConditionCode}}</p>
    <hr/>
    <p>5. Then we can try to search a FHIR repository for Condition resources with this code, or any of its descendents:
    Conditions:
    <button v-on:click="findConditions" :disabled="!sctConditionCode">Find conditions</button>
    <p>
    Results: {{conditions}}
    </p>
  </div>
</template>

<script>
import Autocomplete from '@trevoreyre/autocomplete-vue'
import '@trevoreyre/autocomplete-vue/dist/style.css'

const fhir = 'https://r4.ontoserver.csiro.au/fhir'

export default {
  name: 'TermDemo',
  components: {
    Autocomplete
  },
  props: {
    bodyPartCode: String,
    detailedBodyPart: Object,
    sctBodyPartCode: String,
    sctConditionCode: String,
    conditions: Object
  },
  methods: {
    findBodyPart: function(input) {
      var url = fhir + '' //FIXME    
      return new Promise(resolve => {
        if (input.length < 1) {
          return resolve([])
        }
        fetch(url)
          .then(response => response.json())
          .then(data => {
            resolve(data) //FIXME
          })
      })
    },
    
    getResultValue(result) {
      return result.display
    },

    handleSubmitBodyPart(result) {
      this.bodyPartCode = result.code;
    },

    lookup: function() {
      var url = fhir + '' //FIXME
      new Promise(resolve => {
        fetch(url)
          .then(response => response.json())
          .then(data => {
            this.detailedBodyPart = data //FIXME
            resolve(data) //FIXME
          })
      })
    },
    
    translate: function() {
      var url = fhir + '' //FIXME
      new Promise(resolve => {
        fetch(url)
          .then(response => response.json())
          .then(data => {
            this.sctBodyPartCode = data //FIXME
            resolve(data) //FIXME
          })
      })
    },
    
    findCondition: function(input) {
      var findConditionUrl = fhir + '' //FIXME    
      return new Promise(resolve => {
        if (input.length < 1) {
          return resolve([])
        }
        fetch(findConditionUrl)
          .then(response => response.json())
          .then(data => {
            resolve(data) //FIXME
          })
      })
    },
    
    handleSubmitCondition(result) {
      this.sctConditionCode = result; //FIXME
    },

    findConditions: function() {
          var url = fhir + '' //FIXME
      new Promise(resolve => {
        fetch(url)
          .then(response => response.json())
          .then(data => {
            this.conditions = data
            resolve(data)
          })
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
