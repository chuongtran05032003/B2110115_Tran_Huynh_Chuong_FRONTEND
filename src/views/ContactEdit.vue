<template>
    <div v-if="contact">
        <h4 class="flex justify-center font-bold text-[20px] mt-[20px] text-green-700">Hiệu chỉnh liên hệ</h4>
        <ContactForm :contactLocal="contact" @submit:contact="updateContact"
        @delete:contact="deleteContact" />
        <p>{{ message }}</p>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRouter, useRoute } from 'vue-router';
import ContactForm from "@/components/ContactForm.vue";
import ContactService from "@/services/contact.service";

const props = defineProps({
  id: { type: String, required: true },
});

const contact = ref(null);
const message = ref("");
const router = useRouter();
const route = useRoute();

const getContact = async (id) => {
  try {
    contact.value = await ContactService.get(id);
  } catch (error) {
    console.log(error);
    // Chuyển sang trang NotFound đồng thời giữ cho URL không đổi
    router.push({
      name: "notfound",
      params: {
        pathMatch: route.path.split("/").slice(1),
      },
      query: route.query,
      hash: route.hash,
    });
  }
};

const updateContact = async (data) => {
  try {
    await ContactService.update(contact.value._id, data);
    alert('Liên hệ được cập nhật thành công.');
    router.push({ name: "contactbook" });
  } catch (error) {
    console.log(error);
  }
};

const deleteContact = async () => {
  if (confirm("Bạn muốn xóa Liên hệ này?")) {
    try {
      await ContactService.delete(contact.value._id);
      router.push({ name: "contactbook" });
    } catch (error) {
      console.log(error);
    }
  }
};

onMounted(() => {
  getContact(props.id);
  message.value = "";
});
</script>