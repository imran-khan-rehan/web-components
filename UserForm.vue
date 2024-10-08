<template>
  <q-form @submit="submitForm" class="q-pa-md">
    <!-- Name Input -->
    <q-input
      v-model="formData.name"
      label="Full Name"
      outlined
      lazy-rules
      :rules="[val => !!val || 'Name is required']"
      class="q-mb-md"
    />

    <!-- Email Input -->
    <q-input
      v-model="formData.email"
      label="Email Address"
      type="email"
      outlined
      lazy-rules
      :rules="[val => !!val || 'Email is required']"
      class="q-mb-md"
    />

    <!-- Phone Input -->
    <q-input
      v-model="formData.phone"
      label="Phone Number"
      type="tel"
      outlined
      lazy-rules
      :rules="[val => !!val || 'Phone is required']"
      class="q-mb-md"
    />

    <!-- Location Input -->
    <q-input
      v-model="formData.location"
      label="Location"
      outlined
      class="q-mb-md"
    />

    <!-- Bio Input -->
    <q-input
      v-model="formData.bio"
      label="Bio"
      type="textarea"
      outlined
      class="q-mb-md"
    />

    <!-- Action Buttons -->
    <div class="row q-mt-md q-pt-md justify-end">
      <q-btn type="submit" label="Submit" color="primary" />
      <q-btn flat label="Reset" color="warning" @click="resetForm" class="q-ml-sm" />
    </div>
  </q-form>
</template>

<script setup>
import { ref, watch, onMounted } from 'vue';
import { useRouter } from 'vue-router';
import { useQuasar } from 'quasar';

// Props to handle form type (create/edit) and initial user data
const props = defineProps({
  userData: Object, // userData is optional, will be populated if editing
  isEditMode: Boolean // Determines if we are editing or creating
});

const router = useRouter();
const $q = useQuasar();

// Reactive form data
const formData = ref({
  name: '',
  email: '',
  phone: '',
  location: '',
  bio: ''
});

// Prepopulate form data when editing
onMounted(() => {
  if (props.isEditMode && props.userData) {
    formData.value = { ...props.userData };
  }
});

// Submit form handler
const submitForm = () => {
  if (props.isEditMode) {
    // Editing existing user
    console.log('User edited:', formData.value);
    $q.notify({ type: 'positive', message: 'User updated successfully!' });
  } else {
    // Creating a new user
    console.log('User created:', formData.value);
    $q.notify({ type: 'positive', message: 'User created successfully!' });
  }
  router.push('/user-list'); // Redirect after form submission
};

// Reset form
const resetForm = () => {
  if (props.isEditMode && props.userData) {
    formData.value = { ...props.userData };
  } else {
    formData.value = {
      name: '',
      email: '',
      phone: '',
      location: '',
      bio: ''
    };
  }
};
</script>

<style scoped>
/* Optional custom styles */
</style>
