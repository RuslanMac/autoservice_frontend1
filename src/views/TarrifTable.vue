<template>
  <v-container>
    <v-row>
      <v-col md="1">
        <label>ID</label>
      </v-col>
      <v-col md="2">
        <label>Название тарифа</label>
      </v-col>
      <v-col md="6">
        <label>Описание</label>
      </v-col>
      <v-col md="1">
        <label>Цена (км)</label>
      </v-col>
    </v-row>
    <v-row>
      <v-col md="2">
        <v-text-field
          label="Название тарифа"
          v-model="newitem.name"
        ></v-text-field>
      </v-col>
      <v-col md="6">
        <v-text-field
          label="Описание"
          v-model="newitem.description"
        ></v-text-field>
      </v-col>
      <v-col md="1">
        <v-text-field label="Цена" v-model="newitem.price"></v-text-field>
      </v-col>

      <v-col>
        <v-btn @click="sendTarrif">Добавить тариф</v-btn>
      </v-col>
    </v-row>
    <v-row v-for="tarrif in rows" :key="tarrif">
      <v-col md="1" style="padding: 20px;">
        <label>{{ tarrif.idRate }}</label>
      </v-col>
      <v-col md="2">
        <CellTable
          :value="tarrif.name"
          :isActive="isActive"
          v-model="tarrif.name"
        >
        </CellTable>
      </v-col>
      <v-col md="6"
        ><CellTable
          :value="tarrif.description"
          :isActive="isActive"
          v-model="tarrif.description"
        ></CellTable>
      </v-col>
      <v-col md="1"
        ><CellTable
          :value="tarrif.price"
          :isActive="isActive"
          v-model="tarrif.price"
        ></CellTable>
      </v-col>
      <v-col md="1"
        ><v-btn
          :value="tarrif.idRate"
          color="blue"
          v-on:click="isActive = !isActive"
          >Изменить</v-btn
        ></v-col
      >
      <v-col md="1"
        ><v-btn
          v-on:click="deleteNPSLNT(tarrif.idRate)"
          :value="tarrif.idRate"
          color="error"
          >Удалить</v-btn
        >
      </v-col>
    </v-row>
    <v-col>
      <v-btn color="primary" @click="updateRows">Сохранить изменение </v-btn>
    </v-col>
  </v-container>
</template>
<script>
import CellTable from "@/components/CellTable.vue";
import OrderService from "@/services/OrderService.js";
export default {
  data() {
    return {
      title: "",
      rows: [
        {
          idRate: 1,
          name: "Simple order",
          description: "This simple order can make factor to help",
          price: "123",
        },
        {
          idRate: 2,
          name: "Express",
          description: "Express delivery",
          price: "4567",
        },
        {
          idRate: 3,
          name: "Super Power",
          description: "Super Power Autoservice Delivery",
          price: "12345",
        },
        {
          idRate: 4,
          name: "Big load service type",
          description: "Big load when cars are super power",
          price: "23895",
        },
      ],
      headers: [],
      newitem: {
        name: "",
        description: "",
        price: 0,
      },
      disalbed: true,
    };
  },
  props: {
    isActive: {
      type: Boolean,
      default: true,
    },
  },
  components: {
    CellTable,
  },
  methods: {
    updateRow() {},
    deleteNPSLNT(index) {
      var i = 0;
      while (index != this.rows[i].idRate) {
        i++;
      }
      this.rows.splice(i, 1);
    },
    sendTarrif() {
      this.newitem.price = Number(this.newitem.price);
      OrderService.sendTarrif(this.newitem);
    },
    updateTarrif() {
      OrderService.updateTarrif(this.rows);
    },
    setState() {
      this.isActive = !this.isActive;
    },
    updateRows() {
      OrderService.updateRows(this.rows)
        .then((response) => {
          this.rows = response.data;
        })
        .catch((error) => {
          console.log(
            "There was an error during sending the data, the error :  ",
            error.response
          );
        });
    },
  },

  created() {
    OrderService.getServices()
      .then((response) => (this.rows = response.data))
      .catch((error) => {
        console.log("Error: " + error.response);
      });
  },
};
</script>

<style scoped>
.table-label {
  padding: 10px;
}
</style>
