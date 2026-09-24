<template>
   <div>
      <Transition name="fade" mode="out-in">
         <!-- Loading State -->
         <div
            v-if="showLoading"
            key="loading"
            class="min-h-dvh flex items-center justify-center"
            role="status"
            aria-live="polite"
            data-test="pageloader-loading"
         >
            <div class="vstack-sm p-12">
               <slot name="preloader">
                  <!-- Fallback preloader if no slot provided -->
                  <ProgressSpinner aria-label="Loading" />

                  <!-- loading text (visible or accessible) -->
                  <div v-if="loadingText" class="text-center mt-4">
                     {{ loadingText }}
                  </div>
                  <!-- Invisible fallback for accessibility only if no text is provided -->
                  <span v-else class="sr-only">Loading...</span>
               </slot>
            </div>
         </div>

         <!-- Error State -->
         <div v-else-if="showError" key="error" class="flex items-center justify-center">
            <slot name="error">
               <!-- Fallback error if no slot provided -->
               <AppInfoError :errorText="errorText" class="max-w-xl" />
            </slot>
         </div>

         <!-- Success State, load component -->
         <div v-else key="loaded" class="contents">
            <slot name="default" />
         </div>
      </Transition>
   </div>
</template>

<script setup lang="ts">
import ProgressSpinner from "primevue/progressspinner"
import AppInfoError from "../feedback/AppErrorInfo.vue"

export interface PageLoaderProps {
   showLoading?: boolean
   loadingText?: string
   showError?: boolean
   errorText?: string // if you dont use the error slot, you can use this prop to show custom error text
}

defineSlots<{
   preloader?: () => unknown
   error?: () => unknown
   default?: () => unknown
}>()
withDefaults(defineProps<PageLoaderProps>(), {
   showLoading: false,
   showError: false,
})
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
   transition: opacity 0.3s ease;
}
.fade-enter-from,
.fade-leave-to {
   opacity: 0;
}
</style>
