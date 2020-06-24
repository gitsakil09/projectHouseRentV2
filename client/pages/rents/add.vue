<template>
  <client-only>
    <v-layout column justify-center align-center>
      <v-flex xs12 sm12 md12>
        <v-card>
          <v-card-title primary-title class="text-center"
            >Add New Rent</v-card-title
          >
          <v-card-text>
            <v-layout row wrap>
              <v-flex xs12>
                <v-form ref="addRentForm">
                  <v-container>
                    <v-row>
                      <v-col cols="12" md="6">
                        <v-text-field
                          name="House Name"
                          label="House Name *"
                          v-model="rentInfo.house_name"
                          :rules="[rules.required]"
                          :error-messages="
                            errors &&
                            errors.find(
                              (error) => error.field === 'rentInfo.house_name'
                            )
                              ? errors.find(
                                  (error) =>
                                    error.field === 'rentInfo.house_name'
                                ).message
                              : ''
                          "
                        ></v-text-field>
                      </v-col>
                      <v-col cols="12" md="2">
                        <v-autocomplete
                          label="Total floors *"
                          :items="floors"
                          v-model="rentInfo.total_floor_no"
                          item-text="text"
                          item-value="id"
                          :rules="[rules.required]"
                        ></v-autocomplete>
                      </v-col>
                      <v-col cols="12" md="2">
                        <v-autocomplete
                          label="Rented floor *"
                          :items="floors"
                          v-model="rentInfo.rented_floor_no"
                          item-text="text"
                          item-value="id"
                          :rules="[rules.required]"
                        ></v-autocomplete>
                      </v-col>
                      <v-col cols="12" md="2">
                        <v-text-field
                          label="Rented flat *"
                          v-model="rentInfo.rented_flat_no"
                          :rules="[rules.required]"
                        ></v-text-field>
                      </v-col>
                    </v-row>
                    <v-row>
                      <v-col cols="12" md="4">
                        <v-autocomplete
                          label="Gas *"
                          :items="[
                            { text: 'Yes', value: 'yes' },
                            { text: 'No', value: 'no' },
                          ]"
                          v-model="rentInfo.gas_available"
                          item-text="text"
                          item-value="value"
                          :rules="[rules.required]"
                        ></v-autocomplete>
                      </v-col>
                      <v-col cols="12" md="4">
                        <v-autocomplete
                          label="Electricity *"
                          :items="[
                            { text: 'Yes', value: 'yes' },
                            { text: 'No', value: 'no' },
                          ]"
                          v-model="rentInfo.electricity_available"
                          item-text="text"
                          item-value="value"
                          :rules="[rules.required]"
                        ></v-autocomplete>
                      </v-col>
                      <v-col cols="12" md="4">
                        <v-autocomplete
                          label="Water *"
                          :items="[
                            { text: 'Yes', value: 'yes' },
                            { text: 'No', value: 'no' },
                          ]"
                          v-model="rentInfo.water_available"
                          item-text="text"
                          item-value="value"
                          :rules="[rules.required]"
                        ></v-autocomplete>
                      </v-col>
                    </v-row>
                    <v-row>
                      <v-col cols="12" md="12">
                        <v-textarea
                          name="Address"
                          label="Address *"
                          v-model="rentInfo.address"
                          :rules="[rules.required]"
                          rows="3"
                          :error-messages="
                            errors &&
                            errors.find(
                              (error) => error.field === 'rentInfo.house_name'
                            )
                              ? errors.find(
                                  (error) =>
                                    error.field === 'rentInfo.house_name'
                                ).message
                              : ''
                          "
                        ></v-textarea>
                      </v-col>
                    </v-row>
                    <v-row>
                      <v-col cols="12" md="12">
                        <v-file-input
                          v-model="rentInfo.pictures"
                          counter
                          label="Pictures *"
                          multiple
                          placeholder="Select Pictures"
                          prepend-icon="mdi-paperclip"
                          :show-size="1000"
                        >
                          <template v-slot:selection="{ index, text }">
                            <v-chip
                              v-if="index < 2"
                              color="deep-purple accent-4"
                              dark
                              label
                              small
                            >
                              {{ text }}
                            </v-chip>
                            <span
                              v-else-if="index === 2"
                              class="overline grey--text text--darken-3 mx-2"
                            >
                              +{{ rentInfo.pictures.length - 2 }} File(s)
                            </span>
                          </template>
                        </v-file-input>
                      </v-col>
                    </v-row>
                  </v-container>
                </v-form>
              </v-flex>
            </v-layout>
          </v-card-text>
          <v-card-actions>
            <v-btn
              small
              color="primary"
              :loading="isAddRentLoading"
              :disabled="!isValidToAdd || isAddRentLoading"
              @click="addRent"
            >
              <v-icon class="mr-1" small>mdi-plus</v-icon>Add
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-flex>
    </v-layout>
  </client-only>
</template>
<script>
export default {
  name: 'AddRent',
  middleware: ['auth'],
  head() {
    return {
      titleTemplate: 'Add Rent | %s',
    }
  },
  data() {
    return {
      rules: {
        required: (v) => !!v || 'Value Required.',
      },
      isAddRentLoading: false,
      rentInfo: {
        house_name: 'sasasas',
        total_floor_no: 10,
        rented_floor_no: 5,
        rented_flat_no: '5A',
        gas_available: 'yes',
        electricity_available: 'yes',
        water_available: 'yes',
        address: 'address',
        pictures: [],
      },
      errors: [],
      users: [],
      floors: [
        { text: 1, id: 1 },
        { text: 2, id: 2 },
        { text: 3, id: 3 },
        { text: 4, id: 4 },
        { text: 5, id: 5 },
        { text: 6, id: 6 },
        { text: 7, id: 7 },
        { text: 8, id: 8 },
        { text: 9, id: 9 },
        { text: 10, id: 10 },
        { text: 11, id: 11 },
        { text: 12, id: 12 },
        { text: 13, id: 13 },
        { text: 14, id: 14 },
        { text: 15, id: 15 },
        { text: 16, id: 16 },
        { text: 17, id: 17 },
        { text: 18, id: 18 },
        { text: 19, id: 19 },
        { text: 20, id: 20 },
        { text: 21, id: 21 },
        { text: 22, id: 22 },
        { text: 23, id: 23 },
      ],
    }
  },
  async mounted() {},
  computed: {
    /**
     * Checking Validity to add
     */
    isValidToAdd() {
      if (
        this.rentInfo.house_name &&
        this.rentInfo.house_name &&
        this.rentInfo.total_floor_no &&
        this.rentInfo.rented_floor_no &&
        this.rentInfo.rented_flat_no &&
        this.rentInfo.gas_available &&
        this.rentInfo.electricity_available &&
        this.rentInfo.water_available &&
        this.rentInfo.address &&
        this.rentInfo.pictures &&
        this.rentInfo.pictures.length > 0
      ) {
        return true
      } else {
        return false
      }
    },
  },
  watch: {},
  methods: {
    /**
     * Getting All the Users
     * @param null
     */
    async getAllUsers() {
      let _this = this
      try {
        await _this.$axios
          .$get(_this.$APIRoutes.allUsers)
          .then((response) => {
            _this.users = response
          })
          .catch((error) => {})
      } catch (getUserError) {}
    },
    /**
     * Adding Rent
     * @param rentInfo
     */
    async addRent() {
      let _this = this
      try {
        _this.isAddRentLoading = true
        let formData = new FormData()
        if (_this.rentInfo.pictures && _this.rentInfo.pictures.length > 0) {
          for (const picture of _this.rentInfo.pictures) {
            formData.append('file', picture)
          }
        }
        // appending the other data
        formData.append('house_name', _this.rentInfo.house_name)
        formData.append('total_floor_no', _this.rentInfo.total_floor_no)
        formData.append('rented_floor_no', _this.rentInfo.rented_floor_no)
        formData.append('rented_flat_no', _this.rentInfo.rented_flat_no)
        formData.append('gas_available', _this.rentInfo.gas_available)
        formData.append(
          'electricity_available',
          _this.rentInfo.electricity_available
        )
        formData.append('water_available', _this.rentInfo.water_available)
        formData.append('address', _this.rentInfo.address)
        await _this.$axios
          .$post(_this.$APIRoutes.addRent, formData, {
            headers: {
              'content-type': 'multipart/form-data',
            },
          })
          .then((response) => {
            _this.isAddRentLoading = false
            if (response && response.TYPE === 'success') {
              _this.$toast.success(
                response.MESSAGE,
                'topRight',
                5000,
                'Success'
              )
              _this.$router.push('/rents/your/list')
            } else {
              _this.$toast.error(response.MESSAGE, 'topRight', 5000, 'error')
            }
          })
          .catch((error) => {
            _this.isAddRentLoading = false
            _this.$toast.error(
              'Something went wrong.Please contact with the administrator',
              'topRight',
              5000,
              'Error'
            )
          })
      } catch (addRentError) {
        _this.isAddRentLoading = false
        _this.$toast.error(
          'Something went wrong.Please contact with the administrator',
          'topRight',
          5000,
          'Error'
        )
      }
    },
  },
}
</script>
