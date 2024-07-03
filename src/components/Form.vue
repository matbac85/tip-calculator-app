<template>
  <form>
    <section aria-labelledby="data-entry-section" class="entries">
      <fieldset>
        <div class="bill-amount">
          <label for="bill">Bill</label>
          <input type="number" name="bill" id="bill" v-model="bill" />
          <p class="error-message">{{ billError }}</p>
        </div>
      </fieldset>
      <fieldset>
        <legend>Select Tip %</legend>
        <div class="preferences">
          <div class="preference" :class="{ selected: selectedTip === '5' }">
            <input type="radio" name="tip" id="tip-5" value="5" selected v-model="selectedTip" />
            <label for="tip-5" class="tip-button">5%</label>
          </div>
          <div class="preference" :class="{ selected: selectedTip === '10' }">
            <input type="radio" name="tip" id="tip-10" value="10" v-model="selectedTip" />
            <label for="tip-10" class="tip-button">10%</label>
          </div>
          <div class="preference" :class="{ selected: selectedTip === '15' }">
            <input type="radio" name="tip" id="tip-15" value="15" v-model="selectedTip" />
            <label for="tip-15" class="tip-button">15%</label>
          </div>
          <div class="preference" :class="{ selected: selectedTip === '25' }">
            <input type="radio" name="tip" id="tip-25" value="25" v-model="selectedTip" />
            <label for="tip-25" class="tip-button">25%</label>
          </div>
          <div class="preference" :class="{ selected: selectedTip === '50' }">
            <input type="radio" name="tip" id="tip-50" value="50" v-model="selectedTip" />
            <label for="tip-50" class="tip-button">50%</label>
          </div>
          <div class="custom">
            <label for="custom" class="sr-only">Custom</label>
            <input
              type="number"
              name="tip"
              id="custom"
              min="0"
              step="1"
              max="100"
              inputmode="numeric"
              placeholder="Custom"
              v-model="customTip"
            />
          </div>
        </div>
      </fieldset>
      <fieldset>
        <div class="people-number">
          <label for="people-number">Number of People</label>
          <input type="number" name="people-number" id="people-number" v-model="peopleNumber" />
          <p class="error-message">{{ peopleNumberError }}</p>
        </div>
      </fieldset>
    </section>
    <section aria-labelledby="results-section" class="results-section">
      <div class="results">
        <div class="result">
          <p class="result-type">Tip Amount <span>/ person</span></p>
          <output class="p" aria-live="polite" id="tip-amount-per-person"
            >${{ tipAmountPerPerson.toFixed(2) }}</output
          >
        </div>
        <div class="result">
          <p class="result-type">Total <span>/ person</span></p>
          <output class="p" aria-live="polite" id="total-amount-per-person"
            >${{ totalAmountPerPerson.toFixed(2) }}</output
          >
        </div>
      </div>
      <button type="button" id="reset-btn" :disabled="isFormIncomplete" @click="resetForm">
        Reset
      </button>
    </section>
  </form>
</template>

<script setup>
import { ref, watch, computed } from 'vue'

const bill = ref(null)
const customTip = ref(null)
const peopleNumber = ref(null)
const tipAmountPerPerson = ref(0)
const totalAmountPerPerson = ref(0)
const selectedTip = ref(null)
const peopleNumberError = ref(null)
const billError = ref(null)

const calculateTip = () => {
  let tipPercentage = selectedTip.value
    ? parseFloat(selectedTip.value)
    : parseFloat(customTip.value)
  if (isNaN(tipPercentage)) {
    tipPercentage = 0
  }
  const tipAmount = (bill.value * tipPercentage) / 100
  const totalAmount = bill.value + tipAmount
  const numberOfPeople = peopleNumber.value || 1

  tipAmountPerPerson.value = tipAmount / numberOfPeople
  totalAmountPerPerson.value = totalAmount / numberOfPeople
}

const validateBill = (newVal) => {
  if (newVal === '' || newVal === null) {
    billError.value = ''
  } else if (newVal === 0) {
    billError.value = 'Can’t be zero'
  } else if (newVal < 0) {
    billError.value = 'Can’t be negative'
  } else {
    billError.value = ''
  }
}

const validatePeopleNumber = (newVal) => {
  if (newVal === '' || newVal === null) {
    peopleNumberError.value = ''
  } else if (newVal === 0) {
    peopleNumberError.value = 'Can’t be zero'
  } else if (newVal < 0) {
    peopleNumberError.value = 'Can’t be negative'
  } else if (!Number.isInteger(newVal)) {
    peopleNumberError.value = 'Can’t be decimal'
  } else {
    peopleNumberError.value = ''
  }
}

const validateCustomTip = () => {
  if (customTip.value !== null && customTip.value < 0) {
    customTip.value = null // Réinitialiser la valeur si elle est négative
  }
}

const resetCustomTip = () => {
  customTip.value = null
}

const resetSelectedTip = () => {
  selectedTip.value = null
}

const resetForm = () => {
  bill.value = null
  selectedTip.value = null
  customTip.value = null
  peopleNumber.value = null
  peopleNumberError.value = null
  billError.value = null
  tipAmountPerPerson.value = 0
  totalAmountPerPerson.value = 0
}

const isFormIncomplete = computed(() => {
  return !bill.value || (!selectedTip.value && !customTip.value) || !peopleNumber.value
})

watch(customTip, (newVal) => {
  if (newVal) {
    resetSelectedTip()
  }
})

watch(selectedTip, (newVal) => {
  if (newVal) {
    resetCustomTip()
  }
})

watch([bill, selectedTip, customTip, peopleNumber], calculateTip)
watch(bill, validateBill)
watch(peopleNumber, validatePeopleNumber)
watch(customTip, validateCustomTip)
</script>

<style scoped>
fieldset {
  all: unset;
  display: block;
}

form {
  background-color: var(--neutral-100);
  border-radius: 1.5625rem 1.5625rem 0 0;
  padding: clamp(1.5rem, 0.751vw + 1.324rem, 2rem);
  display: flex;
  flex-direction: column;
  gap: 2rem;
  width: 100%;
  max-width: 57.5rem;
  box-shadow: 0px 32px 43px 0px rgba(79, 166, 175, 0.2007);
}

.people-number,
.bill-amount {
  position: relative;
}

.error-message {
  position: absolute;
  color: var(--accent);
  right: 0;
}

.bill-amount,
.people-number {
  display: flex;
  flex-direction: column;
  gap: 0.375rem;
}

.bill-amount label,
.people-number label,
legend {
  color: var(--neutral-500);
  font-size: 1rem;
}

.bill-amount input,
.people-number input {
  all: unset;
  background-color: var(--neutral-200);
  border-radius: 5px;
  padding: 0.375rem 1rem 0.375rem 1.25rem;
  text-align: end;
  background-repeat: no-repeat;
  background-position-y: 50%;
  background-position-x: 1.25rem;
  color: var(--secondary);
  font-size: 1.5rem;
  cursor: pointer;
  border: 2px solid transparent;
}

.bill-amount input {
  background-image: url(../assets/images/icon-dollar.svg);
}

.people-number input {
  background-image: url(../assets/images/icon-person.svg);
}

input[type='number'] {
  appearance: textfield;
}

input[type='number']::-webkit-outer-spin-button,
input[type='number']::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

.entries {
  padding-inline: 0.5rem;
  display: flex;
  flex-direction: column;
  gap: clamp(2rem, 0.751vw + 1.824rem, 2.5rem);
}

button {
  all: unset;
  background-color: var(--primary);
  color: var(--secondary);
  min-width: 100%;
  padding: 0.75rem 0;
  display: flex;
  justify-content: center;
  border-radius: 5px;
  font-size: 1.25rem;
  text-transform: uppercase;
}

button:hover {
  background-color: var(--hover);
}

button {
  cursor: pointer;
}

.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

.preference input[type='radio'] {
  display: none;
}

.preferences {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: repeat(3, 1fr);
  column-gap: 1rem;
  row-gap: 1rem;
}

.preference {
  background-color: var(--secondary);
  color: var(--neutral-100);
  font-size: 1.5rem;
  display: flex;
  justify-content: center;
  padding: 0.375rem 0;
  border-radius: 5px;
}

.preference,
.preference label {
  cursor: pointer;
}

.preference:hover,
.preference label:hover {
  background-color: var(--primary);
  color: var(--secondary);
}

.custom input {
  all: unset;
  min-width: 100%;
  min-height: 100%;
  background-color: var(--neutral-200);
  border-radius: 5px;
  padding-right: 0.75rem;
  font-size: 1.5rem;
  text-align: end;
  box-sizing: border-box;
  color: var(--secondary);
  cursor: pointer;
  border: 2px solid transparent;
}

.custom input:focus,
.bill-amount input:focus,
.people-number input:focus {
  outline: none;
  border: 2px solid var(--primary);
}

legend {
  margin-bottom: 1rem;
}

.results-section {
  background-color: var(--secondary);
  border-radius: 15px;
  padding: 2.25rem 1.5rem 1.5rem 1.5rem;
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.results {
  display: grid;
  row-gap: 1.25rem;
}

.result {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.result-type {
  display: flex;
  flex-direction: column;
  color: var(--neutral-100);
}

.result-type span {
  color: var(--neutral-400);
  font-size: 0.8125rem;
}

.preference.selected,
.preference.selected label {
  background-color: var(--primary);
  color: var(--secondary);
}

output {
  color: var(--primary);
  font-size: clamp(2rem, 1.502vw + 1.648rem, 3rem);
  text-overflow: ellipsis; /* Ajoute des points de suspension pour le texte débordant */
  white-space: nowrap; /* Empêche le retour à la ligne */
  max-width: 100%;
}

#reset-btn:disabled {
  background-color: var(--disabled-bg);
  color: var(--disabled-color);
  cursor: not-allowed;
}

@media (min-width: 37.5rem) {
  form {
    border-radius: 1.5625rem;
  }
}

@media (min-width: 70rem) {
  form {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    column-gap: 3rem;
  }

  .entries {
    padding: 1rem 0 1rem 1rem;
  }

  .results-section {
    justify-content: space-between;
    padding: 2.5rem;
  }

  .preferences {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(2, 1fr);
    column-gap: 0.875rem;
  }
}
</style>
