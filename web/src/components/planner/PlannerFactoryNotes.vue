<template>
  <v-card class="factory-card sub-card">
    <v-card-title>
      <i class="fas fa-sticky-note" />
      <span class="text-h5 ml-3">Notes</span>
    </v-card-title>
    <v-card-text>
      <v-textarea
        v-model="factory.notes"
        auto-grow
        :counter="charLimit"
        error-messages=""
        placeholder="Add some notes!"
        rows="1"
        :rules="[rules.length]"
      />
      <v-btn v-if="factory.notes.length > 0" class="mt-1" color="primary" @click="factory.notes = ''">Clear Notes</v-btn>
    </v-card-text>
  </v-card>
</template>

<script setup lang="ts">
  import { Factory } from '@/interfaces/planner/FactoryInterface'
  import eventBus from '@/utils/eventBus'

  const props = defineProps <{
    factory: Factory;
    helpText: boolean;
  }>()

  // Validation rule for the character limit
  const rules = {
    length: () => {
      // Check if the value length exceeds the character limit
      if (props.factory.notes.length >= charLimit) {
        props.factory.notes = props.factory.notes.slice(0, charLimit) // Trim the value
        return `Max character length (${charLimit}) reached, condense your notes, pioneer!`
      }
      return true // Validation passes
    },
  }

  const charLimit = 1000

  watch(() => props.factory.notes, () => {
    eventBus.emit('factoryUpdated') // Tell sync there's something changed
  })
</script>
