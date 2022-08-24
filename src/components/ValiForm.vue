<template>
  <div class="container">
    <div class="d-flex justify-content-center align-content-center flex-row">
      <div class="card p-4 mt-3 shadow">
        <form style="width: 390px" @submit.prevent="onSubmit">
          <div class="form-group">
            <label>Ad-Soyad<span class="text-danger">*</span></label>
            <input
              v-model="$v.name.$model"
              type="text"
              class="form-control"
              placeholder="Adınızı ve soyadınızı giriniz"
            />
            <small v-if="!$v.name.required" class="form-text text-danger"
              >Bu alan zorunludur...</small
            >
          </div>
          <div class="form-group">
            <label>Telefon <span class="text-danger">*</span></label>
            <input
              v-model="$v.phone_number.$model"
              type="tel"
              placeholder="90 (123) 456-7890"
              class="form-control"
              id="input-phone"
              maxlength="18"
              @input="enforcePhoneFormat()"
            />
            <small
              v-if="!$v.phone_number.required"
              class="form-text text-danger"
              >Bu alan zorunludur...</small
            >
          </div>
          <div class="form-group">
            <label>E-posta Adresiniz <span class="text-danger">*</span></label>
            <input
              @blur="$v.email.$touch()"
              :class="{ 'is-invalid': $v.email.$error }"
              v-model="email"
              type="email"
              class="form-control"
              placeholder="E-posta adresini giriniz"
            />
            <small v-if="!$v.email.required" class="form-text text-danger"
              >*Buranın doldurulması zorunludur</small
            >
            <small v-if="!$v.email.email" class="form-text text-danger"
              >*Eksik veya yanlış e-posta girdiniz..</small
            >
          </div>
          <div class="form-group">
            <label class="col-form-label col-form-label-lg"
              >City <span class="text-danger">*</span></label
            >
            <select
              v-model.trim="$v.city.$model"
              class="form-control form-control-lg"
            >
              <option value="">İstanbul</option>
              <option value="">İzmir</option>
              <option value="">Çanakkale</option>
            </select>
            <div v-if="!$v.city.required" class="invalid-feedback">
              The city field is required.
            </div>
          </div>
          <div class="form-group">
            <p>
              <label for="w3review"
                >Adres <span class="text-danger">*</span></label
              >
            </p>
            <textarea
              id="w3review"
              name="w3review"
              rows="4"
              cols="50"
              v-model="$v.address.$model"
            ></textarea>
            <br />
            <small v-if="!$v.address.required" class="form-text text-danger"
              >Bu alan zorunludur...</small
            >
          </div>

          <button class="btn-6">Gönder</button>
        </form>
      </div>
    </div>
  </div>
</template>
<script>
import { required, email } from "vuelidate/lib/validators";
export default {
  data() {
    return {
      email: null,
      address: null,
      name: null,
      phone_number: null,
      city: null,
    };
  },
  validations: {
    email: {
      required,
      email,
    },
    address: {
      required,
    },
    name: {
      required,
    },

    phone_number: {
      required,
    },
    city: {
      required,
    },
  },
  methods: {
    onSubmit() {
      let form = {
        email: this.email,
        address: this.address,
        name: this.name,
        phone_number: this.phone_number,
        city: this.city,
      };
      console.log(form);
    },
    validationStatus: function (validation) {
      return typeof validation != "undefined" ? validation.$error : false;
    },
    enforcePhoneFormat() {
      let x = this.phone_number
        .replace(/\D/g, "")
        .match(/(\d{0,2})(\d{0,3})(\d{0,3})(\d{0,4})/);
      x[1] = "90";
      this.phone_number = !x[2]
        ? x[1]
        : "(" +
          "+" +
          x[1] +
          ") " +
          x[2] +
          (x[3] ? "-" + x[3] : "") +
          ("-" + x[4]);
    },
  },
};
</script>
<style lang="scss" scoped>
@import "../assets/bootstrap.min.css";
.btn-6 {
  background: rgb(247, 150, 192);
  background: radial-gradient(
    circle,
    rgba(247, 150, 192, 1) 0%,
    rgba(118, 174, 241, 1) 100%
  );
  height: 35px;
  width: 392px;
  border: none;
}
.btn-6:hover {
  background: radial-gradient(
    circle,
    rgba(118, 174, 241, 1) 0%,
    rgba(247, 150, 192, 1) 100%
  );
}
</style>
