<template>
    <div>
        <NSpace justify="space-between">
            <NSpace vertical>
                <NInput show-count v-model:value="format" placeholder="Format" />
                <NInputNumber v-model:value="count" :min="1" placeholder="Count" />
            </NSpace>
            <CharactersSet ref="characters_set"></CharactersSet>
        </NSpace>
    </div>
</template>

<script setup lang="ts">
import CharactersSet from "./CharactersSet.vue";
import { NInput, NInputNumber, NSpace } from "naive-ui";
import { ref } from "vue";

const characters_set = ref<InstanceType<typeof CharactersSet> | null>(null)
const format = ref("************")
const count = ref(1)

function randint(a: number, b: number) {
    // [a, b)
    return Math.floor(a + (b - a) * Math.random())
}

function generate_string(format: string, chars_set: string): string {
    let res = format
    while (res.includes("*")) {
        res = res.replace("*", chars_set[randint(0, chars_set.length)])
    }
    return res
}

function generate(): string[] {
    let cset = characters_set.value?.getCharacters()
    if (cset === undefined || cset.length === 0) {
        return []
    }

    let res: string[] = []
    for (let _ = 0; _ < count.value; _++) {
        res.push(generate_string(format.value, cset))
    }
    return res
}

defineExpose({
    generate
})

</script>

<style></style>
