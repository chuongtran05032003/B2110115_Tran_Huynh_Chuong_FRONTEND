<script setup>
import { ref, computed, onMounted, watch } from 'vue';
import { useRouter } from 'vue-router';
import ContactCard from '@/components/ContactCard.vue';
import InputSearch from '@/components/InputSearch.vue';
import ContactList from '@/components/ContactList.vue';
import ContactService from '@/services/contact.service';
const contacts = ref([]);
const activeIndex = ref(-1);
const searchText = ref("");
const router = useRouter();
watch(searchText, () => {
  activeIndex.value = -1;
});
const contactStrings = computed(() =>
  contacts.value.map((contact) => {
    const { name, email, address, phone } = contact;
    return [name, email, address, phone].join("");
  })
);
const filteredContacts = computed(() => {
  if (!searchText.value) return contacts.value;
  return contacts.value.filter((_contact, index) =>
    contactStrings.value[index].includes(searchText.value)
  );
});
const activeContact = computed(() => {
  if (activeIndex.value < 0) return null;
  return filteredContacts.value[activeIndex.value];
});
const filteredContactsCount = computed(() => filteredContacts.value.length);
const retrieveContacts = async () => {
  try {
    contacts.value = await ContactService.getAll();
  } catch (error) {
    console.log(error);
  }
};
const refreshList = () => {
  retrieveContacts();
  activeIndex.value = -1;
};
const removeAllContacts = async () => {
  if (confirm("Bạn muốn xóa tất cả Liên hệ?")) {
    try {
      await ContactService.deleteAll();
      refreshList();
    } catch (error) {
      console.log(error);
    }
  }
};
const goToAddContact = () => {
  router.push({ name: 'contact-add' });
};
onMounted(() => {
  refreshList();
});
</script>
<template>
  <div class="lg:flex-row">
    <div class="w-full">
      <InputSearch v-model="searchText" />
    </div>
    <div class="flex justify-center mx-[100px] flex-wrap">
        <div class="flex-1 max-w-[500px] min-w-[400px] mb-[20px] mx-[10px]">
            <div class="w-full">
              <h4 class="text-lg font-semibold text-center">
                      Danh bạ
              </h4>    
              <ContactList
                  v-if="filteredContactsCount > 0"
                  :contacts="filteredContacts"
                  v-model:activeIndex="activeIndex"
              />
              <p v-else class="text-center">Không có liên hệ nào.</p>
            </div>
            <div class="mt-6 flex justify-center items-center space-x-2">
                <button class="bg-blue-600 text-white  hover:bg-blue-700 focus:ring-2 
                              focus:outline-none focus:ring-blue-300 
                              font-medium rounded-lg text-sm w-full sm:w-auto px-5 py-2.5 text-center" 
                              @click="refreshList()">
                  <i class="fas fa-redo"></i> Làm mới
                </button>
                <button class="bg-green-600  text-white hover:bg-green-700 focus:ring-2 
                                focus:outline-none focus:ring-green-300 
                              font-medium rounded-lg text-sm w-full sm:w-auto px-5 py-2.5 text-center" 
                              @click="goToAddContact()">
                  <i class="fas fa-plus"></i> Thêm mới
                </button>
                <button class="bg-red-600  text-white hover:bg-red-700 
                              focus:ring-2 focus:outline-none focus:ring-red-300 
                              font-medium rounded-lg text-sm w-full sm:w-auto 
                              px-5 py-2.5 text-center" @click="removeAllContacts">
                  <i class="fas fa-trash"></i> Xóa tất cả
                </button>
            </div>
        </div>
        <div v-if="activeContact" class="bg-gray-100 rounded-[10px]  max-w-[500px] 
                                          min-w-[400px] pb-[20px] mx-[10px] h-full">
          <h4 class="text-lg font-semibold text-white 
                  bg-green-600 pl-[20px] rounded-t-[5px] py-[10px]">
              Chi tiết Liên hệ
            <i class="fas fa-address-card"></i>
          </h4>
          <ContactCard :contact="activeContact" />
          <router-link
            :to="{
            name: 'contact.edit',
            params: { id: activeContact._id },
            }"
          >
          <div class="bg-yellow-500  text-black  hover:bg-yellow-600 
                      focus:ring-2 focus:outline-none focus:ring-yellow-700 
                      rounded-lg text-sm  px-4 py-1.5 text-center mx-[30px] mt-[30px]">
              <i class="fas fa-edit"></i> Hiệu chỉnh
          </div>
          </router-link>
        </div>
    </div>
  </div>
</template>
