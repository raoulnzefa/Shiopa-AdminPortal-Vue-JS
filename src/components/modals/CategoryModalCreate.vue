<template>
    <div class="overflow-hidden">
        <div class="absolute top-1/2 right-1/2 transform translate-x-1/2 -translate-y-1/2 w-1/2 z-40 p-10 bg-white rounded-lg shadow-lg">
            <button @click="$emit('close-modal')" class="float-right hover:text-red-600">
                <CancelIcon class="w-8 h-8" />
            </button>
            <h2 class="display-h2">Create a new category</h2>
            <div class="flex flex-col py-2">
                <label class="label" for="name">Name:</label>
                <input
                    class="text-input" 
                    type="text" 
                    id="name" 
                    v-model="name" 
                    placeholder="Winter Clothes"
                    />
            </div>
            <div class="flex flex-col py-2">
                <label class="label" for="name">Parent Category:</label>
                <Multiselect
                    v-model="categoryParent"
                    @select="handleCategorySelection"
                    :options="getCategoriesOptions"
                    :searchable="true"
                    placeholder="Select a parent category"
                    />
            </div>
            <div class="border-2 border-gray-200 rounded-md p-4">
                <div class="flex flex-row justify-between">
                    <p>App Logo</p>
                    <label class="flex flex-row items-center px-4 py-2 hover:text-blue-800 rounded-lg uppercase cursor-pointer">
                        <svg class="w-5 h-5" fill="currentColor" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
                            <path d="M16.88 9.1A4 4 0 0 1 16 17H5a5 5 0 0 1-1-9.9V7a3 3 0 0 1 4.52-2.59A4.98 4.98 0 0 1 17 8c0 .38-.04.74-.12 1.1zM11 11h3l-4-4-4 4h3v3h2v-3z" />
                        </svg>
                        <span class="ml-2">Select a file</span>
                        <input @change="handleCategoryFile($event)" type='file' accept="image/*" class="hidden" />
                    </label>
                </div>
                <!-- <img :src="configuration.image" class="w-48 mb-6" alt="App Logo" /> -->
                <div>
                    <p class="uppercase text-sm font-light">Preview</p>
                    <div class="flex flex-nowrap">
                        <img :src="newCategoryImagePreview" class="w-48 mb-6" />
                        <!-- <CancelIcon class="w-6 h-6 hover:text-red-600 cursor-pointer" @click="cancelAppLogoUpload" /> -->
                    </div>
                </div>
            </div>
            <div class="float-right">
                <button 
                    @click="$emit('close-modal')" 
                    class="base-btn-cancel-outline border-2 border-red-800 hover:border-red-700 mr-4">
                    Cancel
                </button>
                <button class="base-btn" @click="$emit('handleSave', selectedCategory)">Save</button>
            </div>
        </div>
        <div class="bg-gray-800 h-screen w-full opacity-90 z-30 fixed top-0 left-0"></div>
    </div>
</template>
<script lang="ts">
import { defineComponent } from 'vue';

// components
import Multiselect from '@vueform/multiselect'
import CancelIcon from '@/components/icons/CancelIcon.vue'

export default defineComponent({
    name: 'CategoryModalCreate',
    components: { CancelIcon, Multiselect },
    props: {
		categories: {
			required: true,
			type: Array
		}
	},
    data() {
        return {
            name: '' as string,
            categoryParent: null,
            newCategoryImagePreview: null as any,
            newCategoryImage: null as any,
        }
    },
    methods: {
        handleCategorySelection(e: any): any {
            console.log(e)
        },
        handleCategoryFile(e: any): void {
			this.newCategoryImagePreview = URL.createObjectURL(e.target.files[0])
			this.newCategoryImage = e.target.files[0]
            console.log(this.newCategoryImage)
		},
    },
    computed: {
        getCategoriesOptions(): Array<any> {
            let categories = this.categories.map((cat: any) => {
                return { value: cat.id, label: cat.name }
            })
            return categories
        },
        selectedCategory(): any {
            return {
                name: this.name,
                categoryParent: this.categoryParent,
                image: this.newCategoryImage
            }
        }
    }

})
</script>