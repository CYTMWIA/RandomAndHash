<template>
  <NGrid x-gap="12" :cols="2">
    <NGridItem>
      <RandomStringGenerator ref="random_string_generator"></RandomStringGenerator>
    </NGridItem>
    <NGridItem>
      <Hasher ref="hasher"></Hasher>
    </NGridItem>
    <NGridItem>
      <NSpace vertical>
        <NButton @click="generate_random_string" type="primary">RandomString</NButton>
        <NInput v-model:value="be_hashed" type="textarea" :rows="buffer_rows" placeholder="Hello!" />
      </NSpace>
    </NGridItem>
    <NGridItem>
      <NSpace vertical>
        <NButton @click="hash_strings" type="primary">Hash</NButton>
        <NInput v-model:value="hash_result" type="textarea" :rows="buffer_rows" placeholder="Hi!" />
      </NSpace>
    </NGridItem>
  </NGrid>
</template>

<script setup lang="ts">
import RandomStringGenerator from "./components/RandomStringGenerator.vue"
import Hasher from "./components/Hasher.vue"
import { NInput, NButton, NGrid, NGridItem, NSpace } from "naive-ui"
import { ref } from "vue"

const random_string_generator = ref<InstanceType<typeof RandomStringGenerator> | null>(null)
const hasher = ref<InstanceType<typeof Hasher> | null>(null)
const buffer_rows = ref(20)
const be_hashed = ref("")
const hash_result = ref("")

function generate_random_string() {
  let rs = random_string_generator.value?.generate()
  if (rs === undefined) {
    console.log("Generate random string failed.")
    return
  }
  be_hashed.value = rs.join("\n")

  if (be_hashed.value.length) hash_strings()
}

function hash_strings() {
  let result = hasher.value?.hash(be_hashed.value.split("\n"))
  if (result === undefined) {
    console.log("Hash failed.")
    return
  }
  hash_result.value = result.join("\n")
}

</script>

<style scoped></style>
