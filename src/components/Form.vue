<template>
  <v-card
  elevation="4"
  class="mx-auto ma-4 pa-4"
  max-width="720">
    <v-card-title>Data Penerima Bansos</v-card-title>
    <v-form
    v-model="valid"
    @submit.prevent="submit">
      <v-container>
        <v-text-field
          v-model="nama"
          :rules="namaRules"
          label="Nama"
          required
        ></v-text-field>

        <v-text-field
          counter
          type="number"
          v-model="nik"
          :rules="nikRules"
          label="NIK"
          required
        ></v-text-field>

        <v-text-field
          type="number"
          v-model="no_kk"
          :rules="no_kkRules"
          label="Nomor Kartu Keluarga"
          required
        ></v-text-field>

        <v-file-input
          v-model="ktp"
          accept="image/jpg, image/jpeg, image/png, image/bmp"
          show-size
          :rules="ktpRules"
          label="Foto KTP"
          hint="File Harus Berbentuk Foto Dan Size Dibawah 2 MB"
          prepend-icon="mdi-camera"
          required
        ></v-file-input>

        <v-file-input
          v-model="kk"
          accept="image/jpg, image/jpeg, image/png, image/bmp"
          show-size
          :rules="kkRules"
          label="Foto Kartu Keluarga"
          hint="File Harus Berbentuk Foto Dan Size Dibawah 2 MB"
          prepend-icon="mdi-camera"
          required
        ></v-file-input>

        <v-text-field
          type="number"
          v-model="umur"
          :rules="umurRules"
          label="Umur"
          suffix="Tahun"
          required
        ></v-text-field>

        <v-select
          v-model="jenis_kelamin"
          :items="gender"
          :rules="jenis_kelaminRules"
          label="Jenis Kelamin"
          required
        ></v-select>

        <v-textarea
          counter
          v-model="alamat"
          :rules="alamatRules"
          label="Alamat"
        ></v-textarea>

        <v-btn v-on:click="kontol">asd</v-btn>
      </v-container>
    </v-form>
  </v-card>
</template>

<script>
  export default {
    data: () => ({
      valid : false,
      nama : '',
      namaRules: [
        v => !!v || 'Nama Tidak Boleh Kosong'
      ],
      nik : '',
      nikRules: [
        v => !!v || 'NIK Tidak Boleh Kosong',
        v => v.length <= 16 || 'NIK Tidak Boleh Lebih Dari 16 Digit'
      ],
      no_kk : '',
      no_kkRules: [
        v => !!v || 'Nomor Kartu Keluarga Tidak Boleh Kosong'
      ],
      ktp : [],
      kk : [],
      umur : '',
      umurRules: [
        v => !!v || 'Umur Tidak Boleh Kosong',
        v => v <= 25 || 'Umur Tidak Boleh Lebih Dari 25 Tahun',
        v => v >= 1 || 'Umur Tidak Boleh Kurang Dari 1 Tahun'
      ],
      gender : ['Laki-laki', 'Perempuan'],
      jenis_kelamin : '',
      jenis_kelaminRules: [
        v => !!v || 'Jenis Kelamin Tidak Boleh Kosong'
      ],
      alamat : '',
      alamatRules: [
        v => !!v || 'Alamat Tidak Boleh Kosong',
        v => v.length <= 255 || 'Alamat Tidak Boleh Lebih Dari 255 Digit'
      ],
    }),
    computed : {
      ktpRules() {
        let ruled = []
        ruled = ['Foto KTP Tidak Boleh Kosong']
        if (this.ktp) {
          ruled = [v => v.size <= 2000000 || 'Foto KTP Tidak Boleh Lebih Dari 2 MB']
        }
        return ruled
      },
      kkRules() {
        let ruled = []
        ruled = ['Foto Kartu Keluarga Tidak Boleh Kosong']
        if (this.kk) {
          ruled = [v => v.size <= 2000000 || 'Foto Kartu Keluarga Tidak Boleh Lebih Dari 2 MB']
        }
        return ruled
      }
    },
    methods : {
      kontol() {
        console.log(this.ktp)
        console.log(this.jenis_kelamin)
      }
    }
  }
</script>
