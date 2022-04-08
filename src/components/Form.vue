<template>
  <v-card
  elevation="4"
  class="mx-auto ma-4"
  max-width="720">
    <v-card-title class="green"><span style="color: white">Data Penerima Bansos</span></v-card-title>
    <v-form
    class="ma-4"
    v-model="valid"
    ref="input"
    @submit.prevent="submit">
      <v-container>
        <v-text-field
          v-model="nama"
          :rules="namaRules"
          @input="edited()"
          label="Nama"
          required
        ></v-text-field>

        <v-text-field
          counter
          type="number"
          v-model="nik"
          :rules="nikRules"
          @input="edited()"
          label="NIK"
          required
        ></v-text-field>

        <v-text-field
          counter
          type="number"
          v-model="no_kk"
          :rules="no_kkRules"
          @input="edited()"
          label="Nomor Kartu Keluarga"
          required
        ></v-text-field>

        <v-file-input
          v-model="ktp"
          accept="image/jpg, image/jpeg, image/png, image/bmp"
          show-size
          :rules="ktpRules"
          @change="edited()"
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
          @change="edited()"
          label="Foto Kartu Keluarga"
          hint="File Harus Berbentuk Foto Dan Size Dibawah 2 MB"
          prepend-icon="mdi-camera"
          required
        ></v-file-input>

        <v-text-field
          type="number"
          v-model="umur"
          :rules="umurRules"
          @input="edited()"
          label="Umur"
          suffix="Tahun"
          required
        ></v-text-field>

        <v-select
          v-model="jenis_kelamin"
          :items="jenis_kelaminList"
          :rules="jenis_kelaminRules"
          @input="edited()"
          label="Jenis Kelamin"
          required
        ></v-select>

        <v-textarea
          counter
          v-model="alamat"
          :rules="alamatRules"
          @input="edited()"
          label="Alamat"
        ></v-textarea>

        <v-text-field
          type="number"
          v-model="rt"
          :rules="rtRules"
          @input="edited()"
          label="RT"
          required
        ></v-text-field>

        <v-text-field
          type="number"
          v-model="rw"
          :rules="rwRules"
          @input="edited()"
          label="RW"
          required
        ></v-text-field>

        <v-text-field
          type="number"
          v-model="p_sebelum_pandemi"
          :rules="p_sebelum_pandemiRules"
          :messages="p_sebelum"
          @input="convert('Sebelum', p_sebelum_pandemi)"
          label="Penghasilan Sebelum Pandemi"
          prefix="Rp."
          required
        ></v-text-field>

        <v-text-field
          type="number"
          v-model="p_setelah_pandemi"
          :rules="p_setelah_pandemiRules"
          :messages="p_setelah"
          @input="convert('Setelah', p_setelah_pandemi)"
          label="Penghasilan Setelah Pandemi"
          prefix="Rp."
          required
        ></v-text-field>

        <v-combobox
          v-model="alasan_bantuan"
          :items="listAlasan_bantuan"
          :rules="alasan_bantuanRules"
          @input="edited()"
          hint="Pilih Salah Satu Atau Isi Sendiri"
          presistent-hint
          label="Alasan Membutuhkan Bantuan"
          required
        ></v-combobox>

        <v-checkbox
          v-model="setuju"
          :rules="setujuRules"
          label="Saya menyatakan bahwa data yang diisikan adalah benar dan siap mempertanggungjawabkan apabila ditemukan ketidaksesuaian dalam data tersebut."
          required
        ></v-checkbox>

        <v-btn dark color="#388E3C" v-on:click="submit()">Submit</v-btn>
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
        v => v.length >= 16 || 'NIK Harus Berjumlah 16 Digit',
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

      jenis_kelaminList : ['Laki-laki', 'Perempuan'],
      jenis_kelamin : '',
      jenis_kelaminRules: [
        v => !!v || 'Jenis Kelamin Tidak Boleh Kosong'
      ],

      alamat : '',
      alamatRules: [
        v => !!v || 'Alamat Tidak Boleh Kosong',
        v => v.length <= 255 || 'Alamat Tidak Boleh Lebih Dari 255 Digit'
      ],

      rt : '',
      rtRules: [
        v => !!v || 'RT Tidak Boleh Kosong'
      ],

      rw : '',
      rwRules: [
        v => !!v || 'RT Tidak Boleh Kosong'
      ],

      p_sebelum_pandemi : '',
      p_sebelum_pandemiRules: [
        v => !!v || 'Penghasilan Sebelum Pandemi Tidak Boleh Kosong',
        v => v >= 0 || 'Angkanya Tidak Boleh Bernilai Minus'
      ],
      p_sebelum : [''],

      p_setelah_pandemi : '',
      p_setelah_pandemiRules: [
        v => !!v || 'Penghasilan Setelah Pandemi Tidak Boleh Kosong',
        v => v >= 0 || 'Angkanya Tidak Boleh Bernilai Minus'
      ],
      p_setelah : [''],

      listAlasan_bantuan : [
        'Kehilangan pekerjaan',
        'Kepala keluarga terdampak atau korban Covid',
        'Tergolong fakir/miskin semenjak sebelum Covid'
      ],
      alasan_bantuan : '',
      alasan_bantuanRules: [
        v => !!v || 'Alasan Tidak Boleh Kosong. Bisa Pilih Salah Satu Atau Isi Sendiri'
      ],

      setuju : false,
      setujuRules : [
        v => !!v || 'Kamu Harus Setuju'
      ]
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
      submit() {
        console.log(this.$refs.input.validate())
      },

      edited() {
        this.setuju = false
      },

      convert(data, value) {
        let angka = Number(value)
        let pesan = []
        if (angka <= 0) {
          pesan = ['Penghasilan '+data+' Pandemi Tidak Boleh Kosong']
        } else {
          pesan = ['Rp. '+(angka).toFixed(2).replace(/\d(?=(\d{3})+\.)/g, '$&,')]
        }
        if (data == 'Sebelum') {
          this.p_sebelum = pesan
        } else if (data == 'Setelah') {
          this.p_setelah = pesan
        }
        this.edited()
      }
    }
  }
</script>
