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
          title="Leave/Vacation Types"
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
                      @click="reset"  rounded
                    >
                      Add Leave Type
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
                                  label="Leave Type in Arabic"
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
                                  label="Leave Type in English"
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
                                  label="Duration"
                                  v-model="editedItem.duration"
                                  :rules="[ (value) => !!value || 'This  field is required']"
                                ></v-text-field>
                              </v-col>
                              <v-col
                                cols="12"
                                sm="6"
                                md="6"
                              >
                                <v-checkbox
                                  v-model="editedItem.is_salary"
                                  :false-value="0"
                                  :true-value="1"
                                  label="Is Salary"
                                  color="success"
                                  hide-details
                                ></v-checkbox>
                              </v-col>
                              <v-col
                                cols="12"
                                sm="6"
                                md="6"
                              >
                                <v-checkbox
                                  v-model="editedItem.requirevisa"
                                  :false-value="0"
                                  :true-value="1"
                                  label="Require Visa"
                                  color="success"
                                  hide-details
                                ></v-checkbox>
                              </v-col>
                              <v-col
                                cols="12"
                                sm="6"
                                md="6"
                              >
                                <v-checkbox
                                  v-model="editedItem.withpay"
                                  :false-value="0"
                                  :true-value="1"
                                  label="With Pay"
                                  color="success"
                                  hide-details
                                ></v-checkbox>
                              </v-col>
                              <v-col
                                cols="12"
                                sm="6"
                                md="6"
                              >
                                <v-checkbox
                                  v-model="editedItem.operator"
                                  :false-value="0"
                                  :true-value="1"
                                  label="Operator"
                                  color="success"
                                  hide-details
                                ></v-checkbox>
                              </v-col>
                              <v-col
                                cols="12"
                                sm="6"
                                md="6"
                              >
                                <v-checkbox
                                  v-model="editedItem.extra_leavecalc"
                                  :false-value="0"
                                  :true-value="1"
                                  label="Extra Leave calc"
                                  color="success"
                                  hide-details
                                ></v-checkbox>
                              </v-col>
                              <v-col
                                cols="12"
                                sm="6"
                                md="6"
                              >
                                <v-checkbox
                                  v-model="editedItem.is_active"
                                  :false-value="0"
                                  :true-value="1"
                                  label="Is Active"
                                  color="success"
                                  hide-details
                                ></v-checkbox>
                              </v-col>
                              <v-col
                                cols="12"
                                sm="6"
                                md="6"
                              >
                                <v-checkbox
                                  v-model="editedItem.is_settlement"
                                  :false-value="0"
                                  :true-value="1"
                                  label="Is Settlement"
                                  color="success"
                                  hide-details
                                ></v-checkbox>
                              </v-col>
                              <v-col
                                cols="12"
                                sm="6"
                                md="6"
                              >
                                <v-checkbox
                                  v-model="editedItem.request"
                                  :false-value="0"
                                  :true-value="1"
                                  label="Request"
                                  color="success"
                                  hide-details
                                ></v-checkbox>
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
                        @click="dialog=false"
                      >
                        Cancel
                      </v-btn>
                      <v-btn
                        color="blue darken-1"
                        text
                        @click="save"
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
  name: "LeaveTypes",
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
        { text: 'Duration', value: 'duration' },
        { text: 'Is Salary', value: 'is_salary' },
        { text: 'Require Visa', value: 'requirevisa' },
        { text: 'With Pay', value: 'withpay' },
        { text: 'Operator', value: 'operator' },
        { text: 'Extra Leave Calc', value: 'extra_leavecalc' },
        { text: 'Is Active', value: 'is_active' },
        { text: 'Is Settlement', value: 'is_settlement' },
        { text: 'request', value: 'request' },
        { text: 'Actions', value: 'actions', sortable: false },
      ],
      desserts: [],
      editedIndex: -1,
      editedItem: {
        en_name: '',
        ar_name: '',
        duration: '',
        is_salary: '0',
        requirevisa: '0',
        withpay: '0',
        operator: '0',
        extra_leavecalc: '0',
        is_active: '0',
        is_settlement: '0',
        request: '0',
      },
      countryId:[],
      allData: []
    }
  },
  computed: {
    formTitle () {
      return this.editedIndex === -1 ? 'New Leave/Vacation Type' : 'Edit Leave/Vacation Type'
    }
  },
  created () {
    this.getList()
  },
  methods: {
    getList(){
      let data = { path: "/leaves" }
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
            path:"/leave/"+this.editedItem.id,
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
            path:"/leaves",
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
        'path' : '/delete_leaves'
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
      this.editedItem.duration = ''
      this.editedItem.is_salary = '0'
      this.editedItem.requirevisa = '0'
      this.editedItem.withpay = '0'
      this.editedItem.operator = '0'
      this.editedItem.extra_leavecalc = '0'
      this.editedItem.is_active = '0'
      this.editedItem.is_settlement = '0'
      this.editedItem.request = '0'
      this.countryId = []
      this.editedIndex = -1
    }

  },
}
</script>

<style scoped>

</style>
