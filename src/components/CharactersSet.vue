<template>
    <div>
        <NSpace>
            <NCheckboxGroup v-model:value="quick_select">
                <NSpace vertical>
                    <NCheckbox v-for="[key, chars] in Object.entries(quick_select_table)" :value="chars" :label="key" />
                </NSpace>
            </NCheckboxGroup>
            <NInput placeholder="Additional Characters" type="textarea" v-model:value="additional_characters" rows="2"
                style="width: 30ch;" />
        </NSpace>
    </div>
</template>

<script setup lang="ts">
import { ref, Ref, defineExpose } from "vue";
import { NInput, NCheckbox, NCheckboxGroup, NSpace } from "naive-ui";

const additional_characters = ref("")
const quick_select_table: { [key: string]: string; } = {
    "a-z": "abcdefghijklmnopqrstuvwxyz",
    "A-Z": "ABCDEFGHIJKLMNOPQRSTUVWXYZ",
    "0-9": "0123456789"
}
const quick_select: Ref<string[]> = ref([
    // Default
    quick_select_table["a-z"],
    quick_select_table["A-Z"],
    quick_select_table["0-9"]
])

defineExpose({
    "getCharacters": () => {
        let res = ""
        quick_select.value.forEach((chars) => {
            res += chars
        })
        res += additional_characters.value
        return res
    }
})

</script>

<style></style>
