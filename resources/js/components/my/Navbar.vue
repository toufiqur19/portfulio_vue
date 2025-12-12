<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';
import { Link, usePage } from '@inertiajs/vue3';
import { GithubIcon, LinkedinIcon, MenuIcon, XIcon } from 'lucide-vue-next';

const mobileMenuOpen = ref(false);
const page = usePage();
const user = page.props.auth?.user;

const isScrolled = ref(false);
const activeSection = ref('home');

const sections = ['home', 'about', 'skills', 'projects', 'experience', 'faqs', 'contact'];

const onScroll = () => {
    isScrolled.value = window.scrollY > 50;

    for (const id of sections) {
        const el = document.getElementById(id);
        if (el) {
            const rect = el.getBoundingClientRect();
            if (rect.top <= 100 && rect.bottom >= 100) {
                activeSection.value = id;
                break;
            }
        }
    }
};

onMounted(() => {
    window.addEventListener('scroll', onScroll);
    onScroll();
});

onUnmounted(() => {
    window.removeEventListener('scroll', onScroll);
});

const scrollToSection = (id: string) => {
    mobileMenuOpen.value = false;
    const section = document.getElementById(id);
    if (section) {
        section.scrollIntoView({ behavior: 'smooth' });
    }
};
</script>

<template>
    <header
        :class="['fixed w-full z-50 transition-all duration-300', isScrolled ? 'bg-white shadow-md' : 'bg-transparent']">
        <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8">
            <nav class="flex justify-between items-center py-5 px-5 lg:px-0">
                <!-- Logo -->
                <div class="text-xl font-semibold text-[#4a4745]">
                    <Link class="flex items-center">
                        <!-- <img src="/assets/img/400_143.png" alt="Logo" class="h-12 w-30 mr-2" /> -->
                        Brand Name
                    </Link>
                </div>

                <!-- Desktop Links -->
                <div class="hidden lg:flex space-x-6">
                    <a v-for="id in sections" :key="id" :href="'#' + id" @click.prevent="scrollToSection(id)" :class="[
                        activeSection === id ? 'text-[#FF014F] font-semibold' : isScrolled ? 'text-[#4a4745]' : 'text-black',
                        'hover:text-[#FF014F] transition'
                    ]">
                        {{ id.charAt(0).toUpperCase() + id.slice(1).replace('-', ' ') }}
                    </a>
                </div>

                <!-- Auth Buttons -->
                <div class="hidden lg:flex items-center space-x-4">
                    <ul class="flex space-x-3 items-center">
                        <li
                            class="bg-white shadow border border-[#FF014F]/10 rounded-full p-1.5 cursor-pointer hover:bg-[#FF014F] duration-300 hover:text-white">
                            <GithubIcon class="w-5.5 h-5.5" />
                        </li>
                        <li
                            class="bg-white shadow border border-[#FF014F]/10 rounded-full p-1.5 cursor-pointer hover:bg-[#FF014F] duration-300 hover:text-white">
                            <LinkedinIcon class="w-5.5 h-5.5" />
                        </li>
                    </ul>
                </div>

                <!-- Mobile Toggle -->
                <button @click="mobileMenuOpen = !mobileMenuOpen"
                    class="lg:hidden focus:outline-none text-xl text-black">
                    <component :is="mobileMenuOpen ? XIcon : MenuIcon"></component>
                </button>
            </nav>
        </div>

        <!-- Mobile Menu -->
        <div v-if="mobileMenuOpen" class="lg:hidden space-y-3 px-10 lg:px-0 bg-white py-5 shadow-md">
            <a v-for="id in sections" :key="id" href="#" @click.prevent="scrollToSection(id)"
                class="block text-[#4a4745] hover:text-[#0f79bc] transition">
                {{ id.charAt(0).toUpperCase() + id.slice(1).replace('-', ' ') }}
            </a>
            <hr />
            <ul class="flex space-x-3 items-center mt-4">
                <li
                    class="bg-white shadow border border-[#FF014F]/10 rounded-full p-1.5 cursor-pointer hover:bg-[#FF014F] duration-300 hover:text-white">
                    <GithubIcon class="w-5 h-5" />
                </li>
                <li
                    class="bg-white shadow border border-[#FF014F]/10 rounded-full p-1.5 cursor-pointer hover:bg-[#FF014F] duration-300 hover:text-white">
                    <LinkedinIcon class="w-5 h-5" />
                </li>
            </ul>
        </div>
    </header>
</template>

<style></style>
