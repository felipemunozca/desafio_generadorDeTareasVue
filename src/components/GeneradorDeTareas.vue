<template>
    <div class="main-section">
        <h2>Lista de tareas</h2>
        
        <div class="to-do-section">
            <input v-model="inputTarea" type="text" placeholder="Escribe una tarea...">
            
            <button v-on:click="agregarTarea"><i class="fa-solid fa-circle-plus"></i></button>
            <!-- 
                v-on -> directiva que se utiliza para escuchar eventos en etiquetas, en este caso el evento click en un botón.
                v-on puede ser reemplazar por el símbolo arroba @ junto al evento.
                Como se ve en este ejemplo, ambos botones funcionaran de la misma manera.
                <button @click="agregarTarea"><i class="fas fa-plus-circle"></i></button> 
            -->
        </div>

        <div v-for="(tarea, index) in listaTareas" :key="index" class="response-section">
            <p>{{ tarea }}</p>
            <div class="response-section__buttons">
                <button v-on:click="editarTarea(index)" class="btn-edit"><i class="fa-solid fa-pen"></i></button>
                <button v-on:click="eliminarTarea(index)" class="btn-delete"><i class="fa-solid fa-trash"></i></button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'tareas-comp',
    // props: {},
    data: function(){
        return {
            /**
             * Se declaran los "datos locales".
             * Estos datos son los que se después se utilizaran dentro delos métodos y sus valores iniciales podrán ser alterados.
             */
            inputTarea: "",
            listaTareas: [],
        }
    },
    // computed: {},
    methods: {
        agregarTarea() {
            if (this.inputTarea == '') {
                alert('Por favor, escriba una tarea antes de presionar el botón agregar.')
                return
            }
            /**
             * this es una variable de contexto.
             * Si se llaman a las variables declaradas en "data" sin agregar "this", el compilador comenzara a buscar las variables
             * dentro del método, en este caso, agregarTarea() y como están declaradas e inicializadas en otra sección (data) la
             * respuesta que retornara sera "undefined".
             */
            this.listaTareas.push(this.inputTarea);
            this.inputTarea = "";
        },
        async editarTarea(index) {
            let tarea_editada = this.listaTareas[index];
            // console.log(tarea_editada)

            /**
             * Se utiliza sweet alert 2, para generar una ventana en donde el usuario podrá editar la tarea.
             */
            const { value: text } = await Swal.fire({
                title: 'Edita tu tarea',
                input: 'text',
                inputValue: tarea_editada,
                showCancelButton: true,
                confirmButtonText: 'Guardar',
                cancelButtonText: 'Cancelar',
                cancelButtonColor: '#d33',
            })

            if (text) {
                // console.log(text)
                // console.log(index)
                this.listaTareas.splice(index, 1, text)
                Swal.fire(
                    '',
                    'Tarea editada.',
                    'success'
                )
            }
        },
        eliminarTarea(index) {
            this.listaTareas.splice(index, 1);
        },
    }
    // watch: {},
    // components: {},
    // mixins: [],
    // filters: {},
    // -- Lifecycle Methods
    // -- End Lifecycle Methods
}
</script>

<style scoped>
    .main-section {
        max-width: 500px;
        margin: 10px auto;
        background: white;
        padding: 30px 10px;
        border-radius: 4px;
    }

    .to-do-section {
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 0 30px;
        margin-bottom: 30px;
    }
    .to-do-section input {
        padding: 10px;
        border-radius: 4px;
        width: 75%;
        border: #198754 solid 1px;
    }
    .to-do-section button {
        background: none;
        border: none;
    }
    .to-do-section i {
        font-size: 45px;
        color: #9a67ea;
    }
    .to-do-section i:hover {
        transform: scale(1.1);
        cursor: pointer;
    }

    .response-section {
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 0 30px;
        background-color: #FFFAD7;
        margin: 0 30px 15px;
        padding: 0 10px;
    }
    .response-section__buttons button {
        background: none;
        border: none;
        color: white;
        border-radius: 50%;
        height: 35px;
        width: 35px;
        cursor: pointer;
    }
    .response-section__buttons .btn-edit {
        background: #198754;
    }
    .response-section__buttons .btn-delete {
        background: #F45050;
        margin-left: 20px;
    }
    .response-section__buttons i {
        font-size: 18px;
    }

</style>