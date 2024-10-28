<script>
    export default {
        props: {
            contact: { type: Object, required: true },
        },
    };
</script>
<template>
  <div class="space-y-2 ml-[30px] mt-[10px]">
    <div class="p-2">
      <strong class="text-gray-700">Tên: </strong>
      <span>{{ contact.name }}</span>
    </div>
    <div class="p-2">
      <strong class="text-gray-700">E-mail: </strong>
      <span>{{ contact.email }}</span>
    </div>
    <div class="p-2">
      <strong class="text-gray-700">Địa chỉ: </strong>
      <span>{{ contact.address }}</span>
    </div>
    <div class="p-2">
      <strong class="text-gray-700">Điện thoại: </strong>
      <span>{{ contact.phone }}</span>
    </div>
    <div class="p-2">
      <strong class="text-gray-700">Liên hệ yêu thích: &nbsp;</strong>
      <i v-if="contact.favorite" class="fas fa-check text-green-500"></i>
      <i v-else class="fas fa-times text-red-500"></i>
    </div>
  </div>
</template>
