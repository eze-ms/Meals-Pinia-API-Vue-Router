<script setup>
    import { Dialog, DialogPanel, DialogTitle, TransitionChild, TransitionRoot } from '@headlessui/vue'

    import { useModalStore } from '../stores/modal'
    import { useComidasStore } from '../stores/comidas'
    import { useFavoritosStore } from '../stores/favoritos'

    const modal = useModalStore()
    const comidas = useComidasStore()
    const favoritos = useFavoritosStore()

    const formatearIngredientes = () => {
        const ingredientesDiv = document.createElement('DIV')

        for(let i = 1; i <= 15; i++) {
            if(comidas.receta[`strIngredient${i}`]) {
                const ingrediente = comidas.receta[`strIngredient${i}`]
                const cantidad = comidas.receta[`strMeasure${i}`]

                const ingredienteCantidad = document.createElement('P')
                ingredienteCantidad.classList.add('text-lg', 'text-gray-500')
                ingredienteCantidad.textContent = `${ingrediente} - ${cantidad}`

                ingredientesDiv.appendChild((ingredienteCantidad))
            }
        }

        return ingredientesDiv
    }

</script>

<template>
  <TransitionRoot as="template" :show="modal.modal">
    <Dialog as="div" class="relative z-10" @close="modal.handleClickModal()">
      <TransitionChild as="template" enter="ease-out duration-300" enter-from="opacity-0" enter-to="opacity-100" leave="ease-in duration-200" leave-from="opacity-100" leave-to="opacity-0">
        <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity" />
      </TransitionChild>
      <div class="fixed inset-0 z-10 overflow-y-auto">
        <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
          <TransitionChild as="template" enter="ease-out duration-300" enter-from="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95" enter-to="opacity-100 translate-y-0 sm:scale-100" leave="ease-in duration-200" leave-from="opacity-100 translate-y-0 sm:scale-100" leave-to="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95">
            <DialogPanel class="relative transform overflow-hidden rounded-lg bg-white px-4 pt-5 pb-4 text-left shadow-xl transition-all sm:my-8 sm:w-full sm:max-w-2xl sm:p-6 max-h-696 overflow-y-auto">
              <div>
                <div class="mt-3">
                  <DialogTitle as="h3" class="text-gray-900 text-3xl font-bold my-5">
                      {{ comidas.receta.strMeal }}
                  </DialogTitle>

                  <img
                      :src="comidas.receta.strMealThumb"
                      :alt="'Imagen de ' + comidas.receta.strMeal"
                      class="mx-auto w-96"
                  />

                  <DialogTitle as="h3" class="text-gray-900 text-2xl font-bold my-5">
                      Ingredientes y Cantidades
                  </DialogTitle>

                  <div v-html="formatearIngredientes().outerHTML"></div>

                  <DialogTitle as="h3" class="text-gray-900 text-2xl font-bold my-5">
                      Instrucciones
                  </DialogTitle>

                  <p class="text-lg text-gray-500">{{ comidas.receta.strInstructions }}</p>

                </div>
              </div>
              <div class="mt-5 sm:mt-6 flex justify-between gap-4">
                  <button
                      type="button"
                      class="w-full rounded bg-gray-600 p-3 font-bold uppercase text-white shadow hover:bg-gray-500"
                      @click="modal.handleClickModal()"
                  >
                      Cerrar
                  </button>

                  <button
                    type="button"
                    class="w-full rounded bg-red-700 p-3 font-bold uppercase text-white shadow hover:bg-red-600"
                    @click="favoritos.handleClickFavorito()"
                  >
                    {{ modal.textBoton }}
                  </button>
              </div> 
            </DialogPanel>
          </TransitionChild>
        </div>
      </div>
    </Dialog>
  </TransitionRoot>
</template>

  