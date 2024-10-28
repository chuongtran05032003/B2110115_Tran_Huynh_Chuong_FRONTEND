<script setup>
import { RouterLink} from 'vue-router';
</script>
<template>
  <nav class="bg-green-600 border-b border-green-500">
    <div class="mx-auto max-w-7xl px-2 sm:px-6 lg:px-8">
        <div class="flex flex-1 h-20 items-center justify-center">
            <RouterLink class="flex flex-shrink-0 items-center mr-4" to="/">
                <span class=" text-white text-2xl font-bold ml-2">
                    ỨNG DỤNG QUẢN LÝ DANH BẠ
                </span>
            </RouterLink>
        </div>
    </div>
  </nav>
</template>