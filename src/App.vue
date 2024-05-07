<template>
  <div class="flex justify-center items-start px-4 py-8">
    <!-- Selección de usuarios -->
    <div class="w-1/3">
      <h2 class="text-2xl font-bold mb-4">Selecciona 4 usuarios:</h2>
      <button
        @click="selectRandomUsers"
        class="bg-gray-400 hover:bg-gray-600 text-white font-bold py-2 px-4 rounded mt-4 w-full"
      >
        Seleccionar aleatoriamente
      </button>
      
      
    </div>

    <!-- Pregunta y opciones -->
    <div class="w-2/3">
      <div class="p-8 bg-white shadow-lg rounded-lg">
        <div v-if="question" class="text-center">
          <h2 class="text-3xl font-bold mb-8">Pregunta {{ currentQuestionIndex + 1 }}:</h2>
          <p class="text-xl mb-4">{{ question.text }}</p>
          <div class="options mt-6">
            <button
              v-for="option in question.options"
              :key="option"
              @click="checkAnswer(option)"
              :class="[optionClasses(option)]"
            >
              {{ option }}
            </button>
          </div>
          <button
            @click="nextQuestion"
            class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded mt-8"
          >
            Siguiente pregunta
          </button>
        </div>
        <div v-else class="text-center">
          <p class="text-2xl font-bold">Haz clic en "Empezar ronda" para comenzar...</p>
        </div>
      </div>
    </div>

    <!-- Modal de usuarios seleccionados y pregunta -->
    <div class="fixed top-0 left-0 w-full h-full bg-black bg-opacity-50 flex justify-center items-center" v-if="modalOpen">
      <div class="modal-content bg-white p-8 rounded-lg text-center">
        <h2 class="text-2xl font-bold mb-4">Usuarios Seleccionados</h2>
        <ul class="mb-4">
          <li v-for="user in selectedUsers" :key="user.id">{{ user.name }}</li>
        </ul>
        <h2 class="text-2xl font-bold mb-4">Pregunta {{ currentQuestionIndex + 1 }}</h2>
        <p class="mb-4">{{ question.text }}</p>
        <button @click="closeModal" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
          Cerrar
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      users: [
      { id: 1, name: 'Luis Eduardo Aguilar Esponda', selected: false },
      { id: 2, name: 'Alex Omar Alvarez Lopez', selected: false },
      { id: 4, name: 'Diego Beltran Lopez', selected: false },
      { id: 5, name: 'Oscar Abraham Castellon Gomez', selected: false },
      { id: 6, name: 'Jorge Arturo Copado Luna', selected: false },
      { id: 7, name: 'Luis Humberto Covarrubias Rosales', selected: false },
      { id: 8, name: 'José Nathaniel Cruz Cardenas', selected: false },
      { id: 9, name: 'Jorge Antonio Espinoza Garcia', selected: false },
      { id: 10, name: 'Francisco Manuel Figueroa Parra', selected: false },
      { id: 12, name: 'Omar Garcia Torres', selected: false },
      { id: 13, name: 'Sebastian Gonzalez Elogio', selected: false },
      { id: 14, name: 'Yael Guillermo Gonzalez Hernandez', selected: false },
      { id: 15, name: 'Anthony Guillen Martinez', selected: false },
      { id: 16, name: 'Luis Enrique Gutierrez Aldama', selected: false },
      { id: 17, name: 'Edmar Maximiliano Hernandez Gonzalez', selected: false },
      { id: 18, name: 'Carlos Alberto Iñiguez Gallego', selected: false },
      { id: 19, name: 'Mildred Abigail Islas Baez', selected: false },
      { id: 20, name: 'Jesus Alberto Lizarraga Rodriguez', selected: false },
      { id: 21, name: 'Rocelyn Guadalupe Lopez Palomarez', selected: false },
      { id: 22, name: 'Genaro Damian Martinez Perez', selected: false },
      { id: 23, name: 'Eduardo Emir Medrano Vazquez', selected: false },
      { id: 24, name: 'Raul Giovanni Molina Mendez', selected: false },
      { id: 25, name: 'Cesar Andree Morales Castillo', selected: false },
      { id: 26, name: 'Amanda Vanessa Munguia Flores', selected: false },
      { id: 27, name: 'Miguel Angel Murillo Mora', selected: false },
      { id: 28, name: 'Brian Ulises Nava Villagrana', selected: false },
      { id: 29, name: 'Abner Ramirez Castaneda', selected: false },
      { id: 30, name: 'Juan Marcos Rivas Mendoza', selected: false },
      { id: 31, name: 'Braulio Damian Rivera Cisneros', selected: false },
      { id: 32, name: 'Carlos Alexis Rodriguez Perez', selected: false },
      { id: 33, name: 'Christopher Alexander Roman Castro', selected: false },
      { id: 34, name: 'Dennis Victor Sanchez Rompelotto', selected: false },
      { id: 35, name: 'Daniel Alejandro Silva Ramirez', selected: false },
      { id: 36, name: 'Alejandro Soto Garcia', selected: false },
      { id: 37, name: 'Manuel Enrique Toledo Ramirez', selected: false },
      { id: 38, name: 'Omar Antonio Valdez Fuentes', selected: false },
      
      { id: 40, name: 'Oscar Bernardino Vera Cortez', selected: false },
      { id: 41, name: 'Ana Lizeth Villalpando Prieto', selected: false },
      { id: 42, name: 'Silverio Adrian Zamora Anaya', selected: false }
    ],
      selectedUsers: [],
      question: null,
      questions: [
        {
          text: '¿Cuál de las siguientes opciones es un elemento del diagrama de clases del patrón bridge?',
          options: ['Concrete Implementation', 'Concrete Abstraction', 'Abstract Implementation', 'Abstract Bridge'],
          answer: 'Concrete Abstraction'
        },
        {
          text: '¿Cuál es una ventaja del patrón bridge?',
          options: ['Reutilización de código', 'Facilidad de implementación', 'Menor costo de mantenimiento', 'Escalabilidad'],
          answer: 'Reutilización de código'
        },
        {
          text: '¿Cuál es una desventaja del patrón bridge?',
          options: ['Mayor flexibilidad', 'Mejor rendimiento', 'Fácil comprensión','Costo de mantenimiento'],
          answer: 'Costo de mantenimiento'
        },
        {
          text: '¿Cuál es la definición del patrón bridge?',
          options: ['Es una técnica para implementar algoritmos',
            'Es una técnica que separa una abstracción de su implementación',
            'Es un diseño basado en herencia múltiple',
            'Es un patrón que no se utiliza en desarrollo de software',
            
          ],
          answer: 'Es una técnica que separa una abstracción de su implementación'
        }
        // Agregar más preguntas aquí...
      ],
      currentQuestionIndex: -1,
      modalOpen: false
    };
  },
  methods: {
    toggleUserSelection(user) {
      if (this.selectedUsers.includes(user)) {
        this.selectedUsers = this.selectedUsers.filter(u => u !== user);
        user.selected = false;
      } else if (this.selectedUsers.length < 4) {
        this.selectedUsers.push(user);
        user.selected = true;
      }
    },
    selectRandomUsers() {
      this.selectedUsers.forEach(user => {
        user.selected = false;
      });
      this.selectedUsers = [];

      const randomIndexes = [];
      while (randomIndexes.length < 4) {
        const randomIndex = Math.floor(Math.random() * this.users.length);
        if (!randomIndexes.includes(randomIndex)) {
          randomIndexes.push(randomIndex);
          this.users[randomIndex].selected = true;
          this.selectedUsers.push(this.users[randomIndex]);
        }
      }

      this.modalOpen = true;
      this.currentQuestionIndex = -1;
      this.nextQuestion();
    },
    nextQuestion() {
      this.currentQuestionIndex++;
      if (this.currentQuestionIndex < this.questions.length) {
        this.question = this.questions[this.currentQuestionIndex];
      } else {
        this.question = null;
        this.currentQuestionIndex = -1;
        this.selectedUsers.forEach(user => {
          user.selected = false;
        });
        this.selectedUsers = [];
      }
    },
    checkAnswer(selectedOption) {
      if (selectedOption === this.question.answer) {
        alert('¡Respuesta correcta!');
      } else {
        alert('Respuesta incorrecta. Inténtalo de nuevo.');
      }
    },
    closeModal() {
      this.modalOpen = false;
    },
    optionClasses(option) {
      return {
        'bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded mr-4 mb-2': !this.question.answered,
        'bg-gray-300 hover:bg-gray-400 text-gray-700 font-bold py-2 px-4 rounded mr-4 mb-2': this.question.answered && option !== this.question.answer,
        'bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded mr-4 mb-2': this.question.answered && option === this.question.answer
      };
    }
  }
};
</script>

<style scoped>
/* Estilos globales del componente */
.user-button.selected {
  background-color: #4caf50 !important;
}
</style>
