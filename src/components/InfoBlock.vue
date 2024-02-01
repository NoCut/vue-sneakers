<script setup>
 defineProps({
    title: String,
    imageUrl: String,
    description: String
})
</script>
 
 <template>
    <div class="flex flex-col items-center flex-1 justify-center w-72 mx-auto">
        <img height="120" width="120"  :src="imageUrl" :alt="title"/>
        <h2 class="mt-4 text-2xl font-medium">{{ title }}</h2>
        <p class="text-center mt-2">{{ description }}</p>
    </div>
 </template>
 
 <style scoped>

 </style>