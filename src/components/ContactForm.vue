<template>
  <form class="max-w-sm mx-auto mt-[30px]" @submit.prevent="handleSubmit(onSubmit())" :validation-schema="contactFormSchema">
    <div class="mb-5">
        <label for="name" class="block mb-2 text-sm font-semibold text-gray-600">Họ và tên</label>
        <Field 
            name="name"
            type="text" 
            id="name" 
            v-model="contactLocal.name"
            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5" 
            required 
        />
        <ErrorMessage name="name" class="text-red-600 text-sm mt-1" />
    </div>
    <div class="mb-5">
        <label for="email" class="block mb-2 text-sm font-semibold text-gray-600">Email</label>
        <Field  
            name="email"
            type="email" 
            id="email" 
            v-model="contactLocal.email"
            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5" 
            placeholder="name@gmail.com" 
            required 
        />
        <ErrorMessage name="email" class="text-red-600 text-sm mt-1" />
    </div>
    <div class="mb-5">
        <label for="address" class="block mb-2 text-sm font-semibold text-gray-600">Địa chỉ</label>
        <Field  
            name="address"
            type="text" 
            id="address" 
            v-model="contactLocal.address"
            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5" 
            required 
        />
        <ErrorMessage name="address" class="text-red-600 text-sm mt-1" />
    </div>
    <div class="mb-5">
        <label for="phone" class="block mb-2 text-sm font-semibold text-gray-600">Số điện thoại</label>
        <Field  
            name="phone" 
            type="tel" 
            id="phone" 
            v-model="contactLocal.phone"
            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5" 
            required 
        />
        <ErrorMessage name="phone" class="text-red-600 text-sm mt-1" />
    </div>
    <div class="flex items-start mb-5">
        <div class="flex items-center h-5">
            <input 
                id="favorite" 
                type="checkbox" 
                class="w-4 h-4 border border-gray-300 rounded bg-gray-50 focus:ring-3 focus:ring-blue-300" 
                v-model="contactLocal.favorite"
            />
        </div>
        <label for="favorite" class="mt-0 ms-2 text-sm font-semibold text-gray-600">Liên hệ yêu thích</label>
    </div>
    <div class="mb-5 flex justify-end space-x-2">
        <button type="button" @click="cancel" class="bg-white border-solid border-[2px] text-gray-600 border-gray-600 hover:bg-gray-200 focus:ring-2 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm w-full sm:w-auto px-4 py-2 text-center">
            <i class="fas fa-backward"></i> Thoát
        </button>
        <button v-if="this.contactLocal.name !==''" type="button" @click="deleteContact" class="bg-white border-solid border-[2px] text-red-600 border-red-600 hover:bg-red-100 focus:ring-2 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm w-full sm:w-auto px-4 py-2 text-center">
            <i class="fas fa-trash"></i> Xóa
        </button>
        <button type="submit" class="flex-1 bg-green-600 text-white hover:bg-green-700 focus:ring-2 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm w-full sm:w-auto px-4 py-2 text-center">
            <i class="fas fa-plus"></i> Lưu
        </button>
    </div>
  </form>
</template>

<script setup>
import { useForm, Field, ErrorMessage } from 'vee-validate';
import * as yup from 'yup';
import { useRouter } from 'vue-router';

const router = useRouter();

const emit = defineEmits(['submit:contact','delete:contact']);

// Định nghĩa schema xác thực với yup
const contactFormSchema = yup.object().shape({
  name: yup
    .string()
    .required("Tên phải có giá trị.")
    .min(2, "Tên phải ít nhất 2 ký tự.")
    .max(50, "Tên có nhiều nhất 50 ký tự."),
  email: yup
    .string()
    .email("E-mail không đúng.")
    .max(50, "E-mail tối đa 50 ký tự."),
  address: yup.string().max(100, "Địa chỉ tối đa 100 ký tự."),
  phone: yup
    .string()
    .matches(/((09|03|07|08|05)+([0-9]{8})\b)/g, "Số điện thoại không hợp lệ."),
});

// Sử dụng useForm để quản lý form và xác thực
const { handleSubmit } = useForm({
  validationSchema: contactFormSchema
});

const props = defineProps({
    contactLocal: {
        type: Object,
        required: true,
        default: () => ({
        name: '',
        email: '',
        address: '',
        phone: '',
        favorite: false
    }),
    }
});

// Xử lý lưu form
function onSubmit() {
    emit('submit:contact', props.contactLocal);
}
function deleteContact() {
    emit('delete:contact', props.contactLocal.id);
}


// Xử lý thoát
function cancel(){
    const reply = window.confirm('You have unsaved changes! Do you want to leave?')

    if (!reply) {
    // stay on the page if
    // user clicks 'Cancel'
    return false
    }
    else router.push({ name: 'contactbook' });
};
</script>