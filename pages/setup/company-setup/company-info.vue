<template>
  <v-container
    id="user-profile"
    fluid
    tag="section"
  >
    <v-row justify="center">
      <v-col
        cols="12"
        md="12"
      >
        <MaterialCard
          color="success"
          title="Company Info"
          class="px-5 py-3"
        >
          <v-data-table
            :headers="headers"
            :items="allData"
            sort-by="en_name"
          >
            <template v-slot:top>
              <v-toolbar
                flat
              >
                <v-spacer></v-spacer>
                <v-dialog
                  v-model="dialog"
                  max-width="500px"
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-btn
                      color="primary"
                      dark
                      class="mb-2"
                      v-bind="attrs"
                      v-on="on"
                      @click="reset"
                      rounded
                    >
                      Add Company Info
                    </v-btn>
                  </template>
                  <v-card>
                    <v-card-title>
                      <span class="headline">{{ formTitle }}</span>
                    </v-card-title>
                    <v-card-text>
                      <v-container>
                        <v-form ref="form">
                          <v-container class="py-0">
                            <v-row>
                              <v-col
                                cols="12"
                                sm="6"
                                md="6"
                              >
                                <v-text-field
                                  label="Name in Arabic"
                                  class="direction"
                                  v-model="editedItem.ar_name"
                                  :rules="[ (value) => !!value || 'This  field is required',
                                (value) => (value && value.length <= 50) || 'maximum 50 characters',]"
                                ></v-text-field>
                              </v-col>
                              <v-col
                                cols="12"
                                sm="6"
                                md="6"
                              >
                                <v-text-field
                                  label="Name in English"
                                  v-model="editedItem.en_name"
                                  :rules="[ (value) => !!value || 'This  field is required',
                                (value) => (value && value.length <= 50) || 'maximum 50 characters',]"
                                ></v-text-field>
                              </v-col>
                              <v-col
                                cols="12"
                                sm="6"
                                md="6"
                              >
                                <v-text-field
                                  label="Register Name in English"
                                  v-model="editedItem.en_register_name"
                                  :rules="[ (value) => !!value || 'This  field is required',
                                (value) => (value && value.length <= 50) || 'maximum 50 characters',]"
                                ></v-text-field>
                              </v-col>
                              <v-col
                                cols="12"
                                sm="6"
                                md="6"
                              >
                                <v-text-field
                                  label="Register Name in Arabic"
                                  v-model="editedItem.er_register_name"
                                  :rules="[ (value) => !!value || 'This  field is required',
                                (value) => (value && value.length <= 50) || 'maximum 50 characters',]"
                                ></v-text-field>
                              </v-col>
                              <v-col
                                cols="12"
                                sm="6"
                                md="6"
                              >
                                <v-text-field
                                  label="Incorporation Date Hijri"
                                  type="number"
                                  v-model="editedItem.incorporation_date_hijri"
                                  :rules="[ (value) => !!value || 'This  field is required',
                                (value) => (value && value.length <= 20) || 'maximum 20 characters',]"
                                ></v-text-field>
                              </v-col>
                              <v-col
                                cols="12"
                                sm="6"
                                md="6"
                              >
                                <v-text-field
                                  label="Incorporation Date"
                                  type="date"
                                  v-model="editedItem.incorporation_date"
                                  :rules="[ (value) => !!value || 'This  field is required']"
                                ></v-text-field>
                              </v-col>
                              <v-col
                                cols="12"
                                sm="6"
                                md="6"
                              >
                                <v-text-field
                                  label="En type of business"
                                  v-model="editedItem.en_type_of_business"
                                  :rules="[ (value) => !!value || 'This  field is required',
                                (value) => (value && value.length <= 20) || 'maximum 20 characters',]"
                                ></v-text-field>
                              </v-col>
                              <v-col
                                cols="12"
                                sm="6"
                                md="6"
                              >
                                <v-text-field
                                  label="Ar type of business"
                                  v-model="editedItem.ar_type_of_business"
                                  :rules="[ (value) => !!value || 'This  field is required',
                                (value) => (value && value.length <= 20) || 'maximum 20 characters',]"
                                ></v-text-field>
                              </v-col>
                              <v-col
                                cols="12"
                                sm="6"
                                md="6"
                              >
                                <v-text-field
                                  label="no_br"
                                  v-model="editedItem.no_br"
                                  :rules="[ (value) => !!value || 'This  field is required',
                                (value) => (value && value.length <= 10) || 'maximum 6 characters',]"
                                ></v-text-field>
                              </v-col>
                            </v-row>
                          </v-container>
                        </v-form>

                      </v-container>
                    </v-card-text>

                    <v-card-actions>
                      <v-spacer></v-spacer>
                      <v-btn
                        color="blue darken-1"
                        text
                        @click="dialog = false"
                        rounded
                      >
                        Cancel
                      </v-btn>
                      <v-btn
                        color="blue darken-1"
                        text
                        @click="save"
                        rounded
                      >
                        Save
                      </v-btn>
                    </v-card-actions>
                  </v-card>
                </v-dialog>
                <v-dialog v-model="dialogDelete" max-width="390px" persistent>
                  <v-card>
                    <v-card-title class="headline">Are you sure you want to delete this record?</v-card-title>
                    <v-card-actions>
                      <v-spacer></v-spacer>
                      <v-btn color="blue darken-1" text @click="dialogDelete=false">Cancel</v-btn>
                      <v-btn color="blue darken-1" text @click="deleteItemConfirm">OK</v-btn>
                      <v-spacer></v-spacer>
                    </v-card-actions>
                  </v-card>
                </v-dialog>
              </v-toolbar>
            </template>

            <template v-slot:item.actions="{ item }">
              <v-icon
                small
                class="mr-2"
                @click="editItem(item)"
              >
                mdi-pencil
              </v-icon>
              <v-icon
                small
                @click="deleteItem(item.id)"
              >
                mdi-delete
              </v-icon>
            </template>
          </v-data-table>
        </MaterialCard>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import MaterialCard from "../../../components/base/MaterialCard";
import Vue from "vue";
export default {
  name: "CompanyInfo",
  middleware: ["auth"],
  components: {MaterialCard },
  data(){
    return{
      dialog: false,
      dialogDelete: false,
      headers: [
        {
          text: 'ID',
          align: 'start',
          value: 'id',
        },
        { text: 'En Name', value: 'en_name' },
        { text: 'Ar Name', value: 'ar_name' },
        { text: 'En Register Name', value: 'en_register_name' },
        { text: 'Ar Register Name', value: 'er_register_name' },
        { text: 'Incorporation Date', value: 'incorporation_date' },
        { text: 'Incorporation Date Hijri', value: 'incorporation_date_hijri' },
        { text: 'En Type of Business', value: 'en_type_of_business' },
        { text: 'Ar Type of Business', value: 'ar_type_of_business' },
        { text: 'No Br', value: 'no_br' },
        { text: 'Actions', value: 'actions', sortable: false },
      ],
      desserts: [],
      editedIndex: -1,
      editedItem: {
        en_name: '',
        ar_name: '',
        en_register_name: '',
        er_register_name: '',
        incorporation_date: '',
        incorporation_date_hijri: '',
        en_type_of_business: '',
        ar_type_of_business: '',
        no_br: '',
      },
      countryId:[],
      allData: []
    }
  },
  computed: {
    formTitle () {
      return this.editedIndex === -1 ? 'New Company Info' : 'Edit Company Info'
    }
  },
  created () {
    this.getList()
  },
  methods: {
    getList(){
      let data = { path: "/companies" }
      this.$store.dispatch('list',data).then(response => {
        this.allData = response.data.data
        this.$store.commit("SHOW_LOADER", false);
        this.$store.commit("SHOW_SNACKBAR", {
          snackbar: true,
          color: "green",
          message: response.data.message
        });
      });
    },
    async save () {
      if(this.$refs.form.validate()) {
        if (this.editedIndex > -1) {
          let data={
            path:"/company/"+this.editedItem.id,
            data:this.editedItem
          }
          this.dialog = false
          this.$store.commit("SHOW_LOADER", true);
          await this.$store.dispatch("update", data).then(response => {
            this.$store.commit("SHOW_LOADER", false);
            this.$store.commit("SHOW_SNACKBAR", {
              snackbar: true,
              color: "green",
              message: response.data.message
            });
            this.getList()
          }).catch(error => {
            this.$store.commit("SHOW_LOADER", false);
            this.$store.commit("SHOW_SNACKBAR", {
              snackbar: true,
              color: "error",
              message: error.response.data.message
            });
          })
        }
        else {
          let data={
            path:"/companies",
            data:this.editedItem
          }
          this.dialog = false
          this.$store.commit("SHOW_LOADER", true);
          await this.$store.dispatch("create", data).then(response => {
            this.$store.commit("SHOW_LOADER", false);
            this.$store.commit("SHOW_SNACKBAR", {
              snackbar: true,
              color: "green",
              message: response.data.message
            });
            this.getList()
          }).catch(error => {
            this.$store.commit("SHOW_LOADER", false);
            this.$store.commit("SHOW_SNACKBAR", {
              snackbar: true,
              color: "error",
              message: error.response.data.message
            });
          })
        }
      }

    },
    editItem (item) {
      this.editedIndex = 2
      // this.editedIndex =this.desserts.indexOf(item)
      // console.log('index',this.desserts.indexOf(item))
      this.editedItem = Vue.util.extend({}, item);
      this.dialog = true
    },
    deleteItem (id) {
      this.countryId[0]=id
      // this.editedIndex = this.desserts.indexOf(item)
      // this.editedItem = Object.assign({}, item)
      this.dialogDelete = true
    },
    async deleteItemConfirm() {
      this.dialogDelete = false
      this.$store.commit("SHOW_LOADER", true);
      let data = {
        'ids': this.countryId,
        'path' : '/delete_companies'
      }
      await this.$store.dispatch("delete", data).then(response => {
        this.$store.commit("SHOW_LOADER", false);
        this.$store.commit("SHOW_SNACKBAR", {
          snackbar: true,
          color: "green",
          message: response.data.message
        });
        this.getList()
      }).catch(error => {
        this.$store.commit("SHOW_LOADER", false);
        this.$store.commit("SHOW_SNACKBAR", {
          snackbar: true,
          color: "error",
          message: error.response.data.message
        });
      })
    },
    reset() {
      this.editedItem.en_name = ''
      this.editedItem.ar_name = ''
      this.editedItem.en_register_name = ''
      this.editedItem.er_register_name = ''
      this.editedItem.incorporation_date = ''
      this.editedItem.incorporation_date_hijri = ''
      this.editedItem.en_type_of_business = ''
      this.editedItem.ar_type_of_business = ''
      this.editedItem.no_br = ''
      this.countryId = []
      this.editedIndex = -1
    }
  },

}
</script>

<style scoped>
.direction {
  direction: rtl;
}
</style>
