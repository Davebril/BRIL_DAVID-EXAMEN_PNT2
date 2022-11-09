<template>

  <section class="src-componentes-ingreso">
    <div class="jumbotron">
      <h2>Ingreso de Gastos</h2>
      <hr>

      <vue-form :state="formstate" @submit.prevent="enviar()">
        
        <!-- Campo nombre -->
        <validate tag="div">
          <label for="nombre">Nombre</label>
          <input type="text" id="nombre" v-model="formData.nombre" required 
            :minlength="nombreMinLength" name="nombre" autocomplete="off" no-espacios class="form-control" />
    
          <field-messages name="nombre" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
            <div slot="minlength" class="alert alert-danger mt-1">
              Este campo debe poseer al menos {{nombreMinLength}} caracteres y menos de
              {{nombreMaxLength}} caracteres.
            </div>
            <div slot="no-espacios" class="alert alert-danger mt-1">
              Este campo no admite espacios intermedios.
            </div>
          </field-messages>
        </validate>
        <br>

        <!-- Campo DNI -->
        <validate tag="div">
          <label for="dni">Descripción</label>
          <input type="text" id="dni" v-model="formData.dni" required name="dni" autocomplete="off" class="form-control" />
    
          <field-messages name="dni" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
          </field-messages>
        </validate>
        <br>
        
        <!-- Campo deuda -->
        <validate tag="div">
          <label for="deuda">Importe</label>
          <input type="number" id="deuda" v-model.number="formData.deuda" required name="deuda" autocomplete="off" class="form-control" />
    
          <field-messages name="deuda" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
          </field-messages>
        </validate>

        <br>
        
      
        <button class="btn btn-success my-3" :disabled="formstate.$invalid" type="submit">Enviar</button>
      </vue-form>
      <br>
      <hr>
      
     
      <label for="presupuesto">Presupuesto</label>
      <input type="number" id="presupuesto" v-model.number="formData.presupuesto" required name="presupuesto" autocomplete="off" class="form-control" />
      <br>
     
      <!-- Tabla para representar los datos ingresados -->
      <h2>Detalle de Gastos</h2>
      <br>

      <!-- <pre>{{ gastos }}</pre> -->

      <div v-if="gastos.length" class="table-responsive">
        <table class="table">
          <tr>
            <th>Nombre</th>
            <th>Descripción</th>
            <th>Importe</th>
            <th>Fecha</th>
           
          </tr>
          <tr v-for="(gasto,index) in gastos" :key="index" :style="{color: analizarSaldo(gasto).color }">
            <td>{{ gasto.nombre }}</td>
            <td>{{ gasto.dni }}</td>
            <td>{{"$"+ gasto.deuda }}</td>
           
            <td>{{ gasto.fecha }}</td>
          
          </tr>
        </table>
      </div>
      <h3 v-else class="alert alert-info">No hay gastos ingresados</h3>

    </div>
  </section>

</template>

<script>

  export default  {
    name: 'src-componentes-ingreso',
    props: [],
    mounted () {

    },
    data () {
      return {
        formstate : {},
        formData : this.getInitialData(),
        gastos : [],
        nombreMinLength: 3,
        nombreMaxLength: 15
      }
    },
    methods: {
      getInitialData() {
        return {
          nombre : null,
          dni: null,
          deuda: null,
          
        }
      },
      enviar() {
        let gasto = {...this.formData}
        gasto.fecha = new Date().toLocaleString()

        console.log(gasto)
        this.gastos.push(gasto)

        this.formData = this.getInitialData()
        this.formstate._reset()
      },
      analizarSaldo(gasto) {
        let dif = gasto.deuda
        let color = 'green'
        if(dif > 1000 && dif < 5000) color = 'magenta'
        if(dif > 5000) color = 'orange'
        return {
          valor : dif,
          color
        }
      }
    },
    computed: {
    }
}


</script>

<style scoped lang="css">
 
  .jumbotron {
    background-color: lightyellow;
    color: brown;
  }

  hr {
    background-color: #eee;
  }

  pre {
    color: white;
  }
</style>
