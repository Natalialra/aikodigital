<template>
  <div class="accordion mt-2" role="tablist">
    <b-card no-body class="mb-1" v-for="equip in equipment" :key="equip.id">
      <b-card-header header-tag="header" class="p-0" role="tab">
        <b-button style="width:100%;" block v-b-toggle="'accordion-1-'+equip.id" variant="success">
          <strong>Nome:</strong> {{equip.name}} | <span>{{getModel(equip.equipmentModelId)}}</span>
        </b-button>
      </b-card-header>
      <b-collapse :id="'accordion-1-'+equip.id" accordion="my-accordion" role="tabpanel">
        <b-card-body>
            <div >
                <table class="table">
                  <thead>
                    <tr>
                      <th scope="col">DATA</th>
                      <th scope="col">ESTADO DO EQUIPAMENTO</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="states in verificaStates(equip.id)" :key="states.id">
                      <td>{{formatDate(states.date)}} - {{formatTime(states.date)}}h</td>
                      <td>{{verificaEstado(states.equipmentStateId)}}</td>
                    </tr>
                  </tbody>
                </table>
            </div>
        </b-card-body>
      </b-collapse>
    </b-card>
  </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'equipmentlist',
    data() {
        return {
            equipment: JSON,
            state: JSON,
            history: JSON,
            model: JSON,
        }
    },
    methods:{
      ifHistory(history, equip){
        return history == equip ? true : false
      },
      verificaStates(id){
        var equipment = this.equipment.find(element => element.id == id)
        var history = this.history.find(element => element.equipmentId == equipment.id)
        return history.states
      },
      verificaEstado(param){
        var state = this.state.find(element => element.id == param)
        return state.name
      },
      getModel(id){
        var model = this.model.find(element => element.id == id)
        return model.name
      },
      formatDate(dataInput){
        var novaData = new Date(dataInput);
        var dataFormatada = novaData.toLocaleDateString('pt-BR', {timeZone: 'UTC'});
        return dataFormatada
      },
      formatTime(timeInput){
        var novaData = new Date(timeInput);
        var dataFormatada = novaData.toLocaleTimeString('pt-BR', {timeZone: 'UTC'});
        return dataFormatada
      }
    },
    mounted() {1
        axios
      .get('http://localhost:8000/api/equipment')
      .then(response => (this.equipment = response.data))
        axios
      .get('http://localhost:8000/api/equipmentState')
      .then(response => (this.state = response.data))
        axios
      .get('http://localhost:8000/api/equipmentStateHistory')
      .then(response => (this.history = response.data))
        axios
      .get('http://localhost:8000/api/equipmentModel')
      .then(response => (this.model = response.data))
    }
}
</script>
<style>
.accordion {
  text-align: center;
}
</style>
