<template>
    <div>
        <h4 class="flex justify-center font-bold text-[20px] mt-[20px] text-green-700">Thêm liên hệ mới</h4>
        <ContactForm @submit:contact="addContact"/>
    </div>
</template>

<script setup>
import ContactForm from '@/components/ContactForm.vue';
import ContactService from '@/services/contact.service';
import { useRouter} from 'vue-router';

const router = useRouter();
const addContact = async(data) =>{
    await ContactService.create(data);
    alert('Liên hệ được thêm thành công.');
    router.push({ name: "contactbook" });
}
</script>