<script>
export default {
    data() {
        return {
            
        }
    },
    props: [
        "locationName",//縣市名稱
        "parameterName",//天氣描述
        "minT",//最低溫度
        "maxT",//最高溫度
    ]
}
</script>

<template>
    <div class="w-64 bg-white p-3 rounded-xl m-6 relative shadow-md shadow-gray-400">
        <div class="text-area flex items-center ml-2">
            <h2 class="text-2xl">{{ locationName }}</h2>
            <span class="ml-2 break-words w-1/4 text-sm">{{ parameterName }}</span>
        </div>
        <p class="text-2xl mt-2 ml-2">
            {{ `${minT}-${maxT}°C` }}
        </p>
        <slot class="absolute"></slot>
    </div>
</template>

<style lang='scss' scoped>

</style>