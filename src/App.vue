<template>
  <div id="layout-container" class="flex flex-col items-center justify-center min-h-screen bg-gray-100 p-5"
    ref="mainContent">
    <main class="flex flex-col items-center gap-10">
      <img src="./assets/LOGO VIEIRA.png" alt="Logo VieiraCred" class="w-64">
      <div class="flex flex-col lg:flex-row justify-center gap-10 p-2">
        <div>
          <h2 class="text-xl font-semibold mb-2 text-center">Adicional</h2>
          <div class="h-auto border-2 border-black rounded-xl p-4 flex flex-col items-center">
            <div class="flex gap-10">
              <div class="flex flex-col gap-4 justify-center mb-4">
                <div v-for="(member, index) in currentTeamRight" :key="'current-right-' + index"
                  class="w-20 h-20 border-2 border-gray-300 rounded-lg flex items-center justify-center cursor-pointer"
                  @click="openModal('currentTeamRight', index)">
                  {{ member }}
                </div>
              </div>
            </div>
          </div>
        </div>

        <div>
          <h2 class="text-xl font-semibold mb-2">Distribuição Atual</h2>
          <div class="h-auto w-[20em] border-2 border-blue-700 rounded-xl p-4 flex flex-col items-center">
            <div class="flex gap-10">
              <div class="flex flex-col gap-4 justify-center mb-4">
                <div v-for="(member, index) in currentTeamLeft" :key="'current-left-' + index"
                  class="w-20 h-20 border-2 border-gray-300 rounded-lg flex items-center justify-center cursor-pointer"
                  @click="openModal('currentTeamLeft', index)">
                  {{ member }}
                </div>
              </div>
              <div class="flex flex-col gap-4 justify-center mb-4">
                <div v-for="(member, index) in currentTeamRight" :key="'current-right-' + index"
                  class="w-20 h-20 border-2 border-gray-300 rounded-lg flex items-center justify-center cursor-pointer"
                  @click="openModal('currentTeamRight', index)">
                  {{ member }}
                </div>
              </div>
            </div>
            <div class="w-20 h-20 border-2 border-gray-300 rounded-lg flex items-center justify-center cursor-pointer"
              @click="openModal('supervisorCurrent')">
              {{ supervisorCurrent }}
            </div>
          </div>
        </div>
        <div>
          <h2 class="text-xl font-semibold mb-2">Nova Distribuição</h2>
          <div class="h-auto w-[20em] border-2 border-red-700 rounded-xl p-4 flex flex-col items-center">
            <div class="flex gap-10">
              <div class="flex flex-col gap-4 justify-center mb-4">
                <div v-for="(member, index) in newTeamLeft" :key="'new-left-' + index"
                  class="w-20 h-20 border-2 border-gray-300 rounded-lg flex items-center justify-center cursor-pointer"
                  @click="openModal('newTeamLeft', index)">
                  {{ member }}
                </div>
              </div>
              <div class="flex flex-col gap-4 justify-center mb-4">
                <div v-for="(member, index) in newTeamRight" :key="'new-right-' + index"
                  class="w-20 h-20 border-2 border-gray-300 rounded-lg flex items-center justify-center cursor-pointer"
                  @click="openModal('newTeamRight', index)">
                  {{ member }}
                </div>
              </div>
            </div>
            <div class="w-20 h-20 border-2 border-gray-300 rounded-lg flex items-center justify-center cursor-pointer"
              @click="openModal('supervisorNew')">
              {{ supervisorNew }}
            </div>
          </div>
        </div>

        <div>
          <h2 class="text-xl font-semibold mb-2 text-center">Adicional</h2>
          <div class="h-auto border-2 border-black rounded-xl p-4 flex flex-col items-center">
            <div class="flex gap-10">
              <div class="flex flex-col gap-4 justify-center mb-4">
                <div v-for="(member, index) in currentTeamRight" :key="'current-right-' + index"
                  class="w-20 h-20 border-2 border-gray-300 rounded-lg flex items-center justify-center cursor-pointer"
                  @click="openModal('currentTeamRight', index)">
                  {{ member }}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="flex justify-center mt-5">
        <button class="border-2 border-black p-3 rounded-xl bg-gray-100 hover:bg-gray-200 transition"
          @click="submitLayout">Enviar novo layout</button>
      </div>
    </main>

    <!-- Modal -->
    <div v-if="showModal" class="fixed inset-0 bg-gray-800 bg-opacity-50 flex items-center justify-center">
      <div class="bg-white p-5 rounded-lg">
        <h2 class="text-lg font-semibold mb-3">Digite o nome</h2>
        <input v-model="nameInput" type="text" class="border-2 border-gray-300 p-2 rounded w-full mb-3">
        <div class="flex justify-end">
          <button class="bg-red-500 text-white px-4 py-2 rounded mr-2" @click="closeModal">Cancelar</button>
          <button class="bg-blue-500 text-white px-4 py-2 rounded" @click="saveName">Confirmar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import html2canvas from 'html2canvas';

export default {
  data() {
    return {
      currentTeamAdictional: ['Vazio', 'Vazio', 'Vazio', 'Vazio', 'Vazio'],
      currentTeamLeft: ['Vazio', 'Vazio', 'Vazio', 'Vazio', 'Vazio'],
      currentTeamRight: ['Vazio', 'Vazio', 'Vazio', 'Vazio', 'Vazio'],
      supervisorCurrent: 'Supervisor',
      newTeamLeft: ['Vazio', 'Vazio', 'Vazio', 'Vazio', 'Vazio'],
      newTeamRight: ['Vazio', 'Vazio', 'Vazio', 'Vazio', 'Vazio'],
      supervisorNew: 'Supervisor',
      showModal: false,
      nameInput: '',
      currentSquare: null,
      currentType: '',
      currentIndex: null
    };
  },
  methods: {
    openModal(type, index = null) {
      this.currentType = type;
      this.currentIndex = index;
      if (index !== null) {
        this.nameInput = this[type][index];
      } else {
        this.nameInput = this[type];
      }
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
      this.nameInput = '';
    },
    saveName() {
      if (this.nameInput) {
        if (this.currentIndex !== null) {
          this[this.currentType].splice(this.currentIndex, 1, this.nameInput);
        } else {
          this[this.currentType] = this.nameInput;
        }
        this.closeModal();
      }
    },
    submitLayout() {
      // Capturar a área principal
      html2canvas(this.$refs.mainContent).then(canvas => {
        // Criar um link temporário para o download
        const link = document.createElement('a');
        link.href = canvas.toDataURL('image/png');
        link.download = 'layout-equipe.png';
        // Adicionar o link ao DOM e simular o clique
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);
      });
      alert('Novo layout enviado!');
    }
  }
};
</script>

<style scoped></style>
