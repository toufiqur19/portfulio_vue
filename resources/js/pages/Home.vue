<script setup lang="ts">
import FlashMessage from '@/components/my/FlashMessage.vue';
import Navbar from '@/components/my/Navbar.vue';
import { Head, Link } from '@inertiajs/vue3';

const props = defineProps<{
    flash: {
        message?: string;
    };
}>();

</script>

<template>
    <Head title="Welcome" />
    <FlashMessage :message="props.flash.message" />
    <div class="bg-[#F1FAFF] h-screen">
        <Navbar />
    </div>
</template>
