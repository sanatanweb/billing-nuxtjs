<template>
  <v-card>
    <v-card-title class="headline" primary-title>
      {{ formTitle }}
    </v-card-title>
    <v-card-text>
      <v-container>
        <v-alert v-if="response.status" type="success">
          {{ response.message }}
        </v-alert>
        <v-row>
          <v-col cols="6" sm="6" md="6">
            <v-text-field 
              label="Name" 
              v-model="brand.name" 
            />
            <v-alert type="error" 
              v-if="submitted && !$v.brand.name.required"
              dense 
              text
            >
              Field is required!
            </v-alert>
            <v-alert type="error" 
              v-if="submitted && !$v.brand.name.minLength"
              dense 
              text
            >
              Minimum length is 3!
            </v-alert>
          </v-col>
          <v-col cols="6" sm="6" md="6">
            <v-text-field 
              label="Country" 
              v-model="brand.country" 
            />
            <v-alert type="error" 
              v-if="submitted && !$v.brand.country.required"
              dense 
              text
            >
              Field is required!
            </v-alert>
          </v-col>
          <v-col cols="12" sm="12" md="12">
            <v-textarea
              label="Description"
              v-model="brand.description"
            />
            <v-alert type="error" 
              v-if="submitted && !$v.brand.description.required"
              dense 
              text
            >
              Field is required!
            </v-alert>
            <v-alert type="error" 
              v-if="submitted && !$v.brand.description.minLength"
              dense 
              text
            >
              Minimum length is 10!
            </v-alert>
          </v-col>
          <v-col cols="12" sm="12" md="12">
            <v-textarea
              label="Meta Description"
              v-model="brand.meta_description"
            />
          </v-col>
          <v-col cols="6" sm="6" md="6">
            <v-file-input label="Logo" v-model="brand.logo" 
              v-on:change="handleFileUpload" 
              placeholder="Dimension 270x380 Pixels"
              accept="image/png, image/jpeg, image/bmp" 
            />
            <v-alert type="error" 
              v-if="submitted && !$v.brand.logo.required"
              dense 
              text
            >
              Field is required!
            </v-alert>
          </v-col>
          <v-col cols="3" sm="3" md="3">
            <v-select label="Top Brand"
              :items="topItems"
              v-model="brand.top_brand" 
            />
            <v-alert type="error" 
              v-if="submitted && !$v.brand.top_brand.required"
              dense 
              text
            >
              Field is required!
            </v-alert>
          </v-col>
          <v-col cols="3" sm="3" md="3">
            <v-select label="Status"
              :items="statusItems"
              v-model="brand.status" 
            />
            <v-alert type="error" 
              v-if="submitted && !$v.brand.status.required"
              dense 
              text
            >
              Field is required!
            </v-alert>
          </v-col>
          <v-col v-if="action == 'edit'" cols="6" sm="6" md="6">
            <v-img 
              :src="'/storage/images/brands/'+brandLogo" 
              height="200"
              width="200"
            />
          </v-col>
        </v-row>
      </v-container>
    </v-card-text>
    <v-card-actions>
      <v-spacer />
      <v-btn color="green darken-1" v-if="action == 'create'" @click="create">Save</v-btn>
      <v-btn color="green darken-1" v-else @click="update(id)">Update</v-btn>
      <v-btn color="red" @click="reset">Reset</v-btn>
    </v-card-actions>
  </v-card>
</template>
<script>
import { required, maxLength, minLength } from "vuelidate/lib/validators";
import axios from 'axios';

export default {
  mounted() {
    if (this.$route.name == 'edit.brand') {
      this.edit(this.id);
      this.action = 'edit';
    }
  },
  props: ['id'],
    data() {
      return {
        brand: {
          name: null,
          country: null,
          description: null,
          meta_description: null,
          logo: null,
          top_brand: null,
          status: null,
        },
        statusItems: [{ text: 'Active', value: 1 }, { text: 'Inactive', value: 0 }],
        topItems: [{ text: 'Yes', value: 1 }, { text: 'No', value: 0 }],
        submitted: false,
        brandLogo: null,
        action: 'create',
        response: [],
      }
    },
    // Methods
    methods: {
      create() {
        this.submitted = true;
        // stop here if form is invalid
        this.$v.$touch();
        if (this.$v.$invalid) {
          return;
        } else {
          let formData = new FormData();
          formData.append('name', this.brand.name);
          formData.append('country', this.brand.country);
          formData.append('description', this.brand.description);
          formData.append('meta_description', this.brand.meta_description);
          formData.append('logo', this.brand.logo);
          formData.append('top_brand', this.brand.top_brand);
          formData.append('status', this.brand.status);

          axios.post("/admin/brand", formData)
            .then(response => {
              this.reset();
              this.response = response.data;
              this.submitted = false;
            }).catch(error => {
              console.log(error);
          });
        }
      },

      edit(id) {
        axios.get("/admin/brand/"+id+"/edit")
          .then(response => {
            let dt = response.data;
            this.brand.name = dt.name;
            this.brand.country = dt.country;
            this.brand.description = dt.description;
            this.brand.meta_description = dt.meta_description;
            this.brand.top_brand = dt.top_brand;
            this.brand.status = dt.top_brand;
            this.brandLogo = dt.logo;
          })
          .catch(error => console.log(error));
      },

      update(id) {
        this.submitted = true;
        // stop here if form is invalid
        this.$v.$touch();
        if (this.$v.$invalid) {
          return;
        } else {
          let formData = new FormData();
          formData.append('name', this.brand.name);
          formData.append('country', this.brand.country);
          formData.append('description', this.brand.description);
          formData.append('meta_description', this.brand.meta_description);
          if (this.brand.logo) {
            formData.append('logo', this.brand.logo);
          }
          formData.append('top_brand', this.brand.top_brand);
          formData.append('status', this.brand.status);
          formData.append("_method", "put");

          axios.post("/admin/brand/"+id, formData)
            .then(response => {
              this.response = response.data;
              this.$router.push({ name: 'product' });
            }).catch(error => {
              console.log(error);
          });
        }
      },

      handleFileUpload() {
        
      },

      reset() {
        this.brand.name = null;
        this.brand.country = null;
        this.brand.description = null;
        this.brand.meta_description = null;
        this.brand.logo = null;
        this.brand.top_brand = null;
        this.brand.status = null;
      }
    },

    validations: {
      brand: {
        name: { required, minLength: minLength(3), maxLength: maxLength(60) },
        country: { required },
        description: { required, minLength: minLength(10), maxLength: maxLength(255) },
        meta_description: { required },
        top_brand: { required },
        status: { required }
      }
    },

    computed: {
      formTitle () {
        return this.action == 'create' ? 'New Brand' : 'Edit Brand';
      },
    },
}
</script>
<style scoped>

</style>