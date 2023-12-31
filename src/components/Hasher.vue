<template>
    <NSpace>
        <NSelect placeholder="Encoding" v-model:value="encoding" :options="encodings" :consistent-menu-width="false" />
        <NSelect placeholder="Hash Funciton" v-model:value="hash_function" :options="hash_functions"
            :consistent-menu-width="false" />
    </NSpace>
</template>

<script setup lang="ts">
import { ref, defineExpose } from "vue";
import { NSelect, NSpace } from "naive-ui";

import { sha1 } from "@noble/hashes/sha1"
import { sha3_256, sha3_512 } from "@noble/hashes/sha3"
import { bytesToHex as toHex } from "@noble/hashes/utils"

const simple_item = (name: string) => { return { "label": name, "value": name } }
const encoding = ref("UTF-8")
const encodings = ref([
    simple_item("UTF-8"),
])

let hidx = -1
const hash_item = (name: string, func: (arr: Uint8Array) => string) => {
    hidx++
    return { "label": name, "value": hidx.toString(), "func": func }
}
const hash_function = ref("1")
const hash_functions = ref([
    hash_item("SHA1", (arr) => toHex(sha1(arr))),
    hash_item("SHA3-256", (arr) => toHex(sha3_256(arr))),
    hash_item("SHA3-512", (arr) => toHex(sha3_512(arr))),
])

function encode(s: string): Uint8Array | undefined {
    switch (encoding.value) {
        case "UTF-8":
            return new TextEncoder().encode(s)
        default:
            return undefined
    }
}

function hash_array(arr: Uint8Array): string {
    let hi = Number(hash_function.value)
    return hash_functions.value[hi].func(arr)
}

function hash(string_list: string[]): string[] {
    let res: string[] = []
    string_list.forEach((s: string) => {
        let arr = encode(s)
        let r = ""
        if (arr !== undefined) {
            r = hash_array(arr)
        }
        res.push(r)
    })
    return res
}

defineExpose({
    hash
})

</script>

<style></style>
