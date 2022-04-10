<template>
  <v-card
  elevation="4"
  class="mx-auto ma-4"
  max-width="720">
    <v-card-title class="green">
      <span style="color: white">Input Data Penerima Bansos</span>
    </v-card-title>
    <v-form
    class="ma-4"
    v-model="valid"
    ref="input"
    @submit.prevent="submit">
      <v-container>
        <v-text-field
          ref="nama"
          v-model="nama"
          :rules="namaRules"
          @input="edited()"
          label="Nama"
          required
        ></v-text-field>

        <v-text-field
          counter
          type="number"
          ref="nik"
          v-model="nik"
          :rules="nikRules"
          @input="edited()"
          label="NIK"
          required
        ></v-text-field>

        <v-text-field
          counter
          type="number"
          ref="no_kk"
          v-model="no_kk"
          :rules="no_kkRules"
          @input="edited()"
          label="Nomor Kartu Keluarga"
          required
        ></v-text-field>

        <v-file-input
          ref="ktp"
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
          ref="kk"
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
          ref="umur"
          v-model="umur"
          :rules="umurRules"
          @input="edited()"
          label="Umur"
          suffix="Tahun"
          required
        ></v-text-field>

        <v-select
          ref="jenis_kelamin"
          v-model="jenis_kelamin"
          :items="jenis_kelaminList"
          :rules="jenis_kelaminRules"
          @input="edited()"
          label="Jenis Kelamin"
          required
        ></v-select>

        <v-textarea
          counter
          ref="alamat"
          v-model="alamat"
          :rules="alamatRules"
          @input="edited()"
          label="Alamat"
        ></v-textarea>

        <v-text-field
          type="number"
          ref="rt"
          v-model="rt"
          :rules="rtRules"
          @input="edited()"
          label="RT"
          required
        ></v-text-field>

        <v-text-field
          type="number"
          ref="rw"
          v-model="rw"
          :rules="rwRules"
          @input="edited()"
          label="RW"
          required
        ></v-text-field>

        <v-text-field
          type="number"
          ref="p_sebelum_pandemi"
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
          ref="p_setelah_pandemi"
          v-model="p_setelah_pandemi"
          :rules="p_setelah_pandemiRules"
          :messages="p_setelah"
          @input="convert('Setelah', p_setelah_pandemi)"
          label="Penghasilan Setelah Pandemi"
          prefix="Rp."
          required
        ></v-text-field>

        <v-combobox
          ref="alasan_bantuan"
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

        <div v-if="!btnSubmit">
          <v-btn dark color="#388E3C"
            v-on:click="submit()">

            Submit
            <v-icon
              right
              dark>
              mdi-upload
            </v-icon>

          </v-btn>
        </div>
        <div v-else>
          <v-btn color="#388E3C"
            loading
            disabled>

            Sedang Di Proses...
            <span right>
              <v-icon light>mdi-cached</v-icon>
            </span>

            <template v-slot:loader>
              Sedang Di Proses...
              <span class="custom-loader" right>
                <v-icon light>mdi-cached</v-icon>
              </span>
            </template>

          </v-btn>
        </div>

        <v-dialog
          v-model="notice"
          persistent
          max-width="600">
          <v-card>
            <v-card-title dark :class="[noticeMsg.title == 'Sukses' ? 'green' : 'red']">
              <span style="color: white">{{ noticeMsg.title }}</span>
            </v-card-title>
            <v-card-text class="pt-4">
              {{ noticeMsg.text }}
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn class="red" dark @click="notice = false">Tutup</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>

      </v-container>
    </v-form>
  </v-card>
</template>

<style>
  .custom-loader {
    animation: loader 1s infinite;
    display: flex;
  }
  @-moz-keyframes loader {
    from {
      transform: rotate(0);
    }
    to {
      transform: rotate(360deg);
    }
  }
  @-webkit-keyframes loader {
    from {
      transform: rotate(0);
    }
    to {
      transform: rotate(360deg);
    }
  }
  @-o-keyframes loader {
    from {
      transform: rotate(0);
    }
    to {
      transform: rotate(360deg);
    }
  }
  @keyframes loader {
    from {
      transform: rotate(0);
    }
    to {
      transform: rotate(360deg);
    }
  }
</style>

<script>
  export default {
    data: () => ({
      //Saat Pertama Form Nya Di Set False Agar User Tidak Bisa Input...
      valid : false,

      //Form Untuk 'nama' Juga Rules-nya...
      nama : '',
      namaRules: [
        v => !!v || 'Nama Tidak Boleh Kosong'
      ],

      //Form Untuk 'nik' Juga Rules-nya...
      nik : '',
      nikRules: [
        v => !!v || 'NIK Tidak Boleh Kosong',
        v => v.length >= 16 || 'NIK Harus Berjumlah 16 Digit', 
        v => v.length <= 16 || 'NIK Tidak Boleh Lebih Dari 16 Digit'
      ],

      //Form Untuk 'no_kk' Juga Rules-nya...
      no_kk : '',
      no_kkRules: [
        v => !!v || 'Nomor Kartu Keluarga Tidak Boleh Kosong'
      ],

      //Form Untuk 'ktp'... Rules-nya Ada Di Computed...
      ktp : null,

      //Form Untuk 'kk'... Rules-nya Ada Di Computed...
      kk : null,

      //Form Untuk 'umur' Juga Rules-nya...
      umur : '',
      umurRules: [
        v => !!v || 'Umur Tidak Boleh Kosong',
        v => v >= 25 || 'Umur Tidak Boleh Kurang Dari 25 Tahun'
      ],

      //Form Untuk 'jenis_kelamin' Juga Rules-nya...
      jenis_kelamin : '',
      jenis_kelaminList : ['Laki-laki', 'Perempuan'],
      jenis_kelaminRules: [
        v => !!v || 'Jenis Kelamin Tidak Boleh Kosong'
      ],

      //Form Untuk 'alamat' Juga Rules-nya...
      alamat : '',
      alamatRules: [
        v => !!v || 'Alamat Tidak Boleh Kosong',
        v => v.length <= 255 || 'Alamat Tidak Boleh Lebih Dari 255 Digit'
      ],

      //Form Untuk 'rt' Juga Rules-nya...
      rt : '',
      rtRules: [
        v => !!v || 'RT Tidak Boleh Kosong'
      ],

      //Form Untuk 'rw' Juga Rules-nya...
      rw : '',
      rwRules: [
        v => !!v || 'RT Tidak Boleh Kosong'
      ],

      //Form Untuk 'p_sebelum_pandemi' Juga Rules-nya...
      p_sebelum_pandemi : '',
      p_sebelum_pandemiRules: [
        v => !!v || 'Penghasilan Sebelum Pandemi Tidak Boleh Kosong',
        v => v >= 0 || 'Angkanya Tidak Boleh Bernilai Minus'
      ],
      p_sebelum : [''],

      //Form Untuk 'p_setelah_pandemi' Juga Rules-nya...
      p_setelah_pandemi : '',
      p_setelah_pandemiRules: [
        v => !!v || 'Penghasilan Setelah Pandemi Tidak Boleh Kosong',
        v => v >= 0 || 'Angkanya Tidak Boleh Bernilai Minus'
      ],
      p_setelah : [''],

      //Form Untuk 'alasan_bantuan' Juga Rules-nya...
      alasan_bantuan : '',
      listAlasan_bantuan : [
        'Kehilangan pekerjaan',
        'Kepala keluarga terdampak atau korban Covid',
        'Tergolong fakir/miskin semenjak sebelum Covid'
      ],
      alasan_bantuanRules: [
        v => !!v || 'Alasan Tidak Boleh Kosong. Bisa Pilih Salah Satu Atau Isi Sendiri'
      ],

      //Form Untuk 'setuju' Juga Rules-nya...
      setuju : false,
      setujuRules : [
        v => !!v || 'Kamu Harus Setuju Ini Untuk Bisa Meng-Submit Data'
      ],

      //Status Button Nya... Sedang Submit Atau Tidak...
      btnSubmit : false,

      //Nitifikasi... Untuk Memberi Tahu User Apakah Proses Submit Nya Berhasil Atau Tidak...
      notice : false,
      noticeMsg : {
        'title' : 'Ini Title',
        'text' : 'Ini Deskripsi'
      }
    }),
    computed : {

      //Rules Untuk ktp... Filter Size Dan File Type...
      ktpRules() {
        let ruled = []
        ruled = ['Foto KTP Tidak Boleh Kosong']
        if (this.ktp !== null) {
          ruled = [
            v => v.size <= 2000000 || 'Foto KTP Tidak Boleh Lebih Dari 2 MB',
            v => v.type == "image/jpg" || v.type == "image/jpeg" || v.type == "image/png" || v.type == "image/bmp" || 'Terdeteksi Bahwa File Yang Di Input Bukan Gambar / Foto'
          ]
        }
        return ruled
      },

      //Rules Untuk kk... Filter Size Dan File Type...
      kkRules() {
        let ruled = []
        ruled = ['Foto Kartu Keluarga Tidak Boleh Kosong']
        if (this.kk !== null) {
          ruled = [
            v => v.size <= 2000000 || 'Foto Kartu Keluarga Tidak Boleh Lebih Dari 2 MB',
            v => v.type == "image/jpg" || v.type == "image/jpeg" || v.type == "image/png" || v.type == "image/bmp" || 'Terdeteksi Bahwa File Yang Di Input Bukan Gambar / Foto'
          ]
        }
        return ruled
      }
    },
    methods : {

      //Fungsi Untuk Submit Form... Agak Panjang Karena Vuetify Value Inputnya Tidak Bisa Berbentuk NUMBER...
      submit() {
        //List Untuk Validasi... Array Ke-0 = Form Yang Akan Di Validasi, Array Ke-1 = Format Data (s = string, n = number, o = other)...
        let allInputList = [
          ['nama', 's'],
          ['nik', 'n'],
          ['no_kk', 'n'],
          ['ktp', 'o'],
          ['kk', 'o'],
          ['umur', 'n'],
          ['jenis_kelamin', 's'],
          ['alamat', 's'],
          ['rt', 'n'],
          ['rw', 'n'],
          ['p_sebelum_pandemi', 'n'],
          ['p_setelah_pandemi', 'n'],
          ['alasan_bantuan', 's']
        ]

        //Fungsi Untuk Merubah String Ke Number Berdasarkan List Yang Telah Di Buat...
        function toNumber(data) {
          let returnValue
          if (data[1] == 'n') {
            returnValue = Number(data[0])
          } else {
            returnValue = data[0]
          }
          if (isNaN(returnValue)) {
            returnValue = data[0]
          }
          return returnValue
        }

        //Cek Apakah Form Nya Valid...
        if (this.$refs.input.validate()) {
          //Set Buttonnya Ke Mode Loading...
          this.btnSubmit = true;

          //Promise Untuk Simulasi Submit Data...
          const submitPromise = new Promise ((resolve, reject) => {
            setTimeout(() => {
              //Random... 1 = Berhasil, Selain Itu Dianggap Gagal... Kemungkinan Output Hanya 1 Dan 0...
              let success = Math.floor(Math.random() * 2);
              this.btnSubmit = false;
              if (success == 1) {
                //Jika Berhasil Notifikasi Nya Menjadi Sukses...
                this.noticeMsg.title = 'Sukses'
                this.noticeMsg.text = 'Form Berhasil Di Submit. Silahkan Input Form Kembali Bila Ada Yang Ingin Di Input Lagi.'
                this.notice = true
                this.clearForm()
                resolve('Berhasil Submit Data')
              } else {
                //Jika Gagal Notifikasi Nya Menjadi Gagal... Dan Tampilkan Pesan Errornya...
                this.noticeMsg.title = 'Gagal'
                this.noticeMsg.text = 'Form Gagal Di Submit. Submit Ulang Form Nya Beberapa Saat Lagi Dan Cek Koneksi Anda. Bila Masih Tetap Tidak Dimohon Untuk Hubungi Admin.'
                this.notice = true
                reject('Gagal : Internal Server Error')
              }
            }, 1500)
          })

          //Jalankan Promisenya... Dan Tampilkan Status Promise Nya... Berhasil Atau Tidak...
          submitPromise
            .then(function(resolve) {
              console.log('=============PROMISE=============')
              console.log(resolve)
              console.log('=================================')
            })
            .catch(function(reject) {
              console.log('=============PROMISE=============')
              console.log(reject)
              console.log('=================================')
            })

          //Selagi Proses Beritahu Kalau Submit Sedang Di Proses...
          this.noticeMsg.title = 'Proses...'
          this.noticeMsg.text = 'Proses...'
          console.log('Sedang Di Proses...')
        } else {
          //Kalau Form Tidak Valid Focus Input Ke Form Yang Tidak Valid...
          //Focus Form Nya Dari Bawah Ke Atas... Menggunakan List Form Yang Telah Di Buat...
          for (let i = 0;i < allInputList.length;i++) {
            if (!this.$refs[allInputList[allInputList.length - (i + 1)][0]].valid) {
              this.$refs[allInputList[allInputList.length - (i + 1)][0]].focus()
            }
          }
        }

        //Valid Ataupun Tidak Form Value Nya Di console.log...
        console.log('==============INPUT==============')
        for (let i = 0;i < allInputList.length;i++) {
          console.log(allInputList[i][0]+' : '+toNumber([this.$refs[allInputList[i][0]].value, allInputList[i][1]]))
        }
        console.log('=================================')
      },

      //Fungsi Ini Ada Di Setiap Input...
      //Jika Terdeteksi Ada Perubahan Input... Maka Checkbox Persetujuan Akan Di Set Ke False...
      edited() {
        this.setuju = false
      },

      //Fungsi Ini Digunakan Untuk Menampilkan Angka Berformat Rupiah...
      //Digunakan Saat Input Penghasilan...
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
      },

      clearForm() {
        let listForm = ['nama', 'nik', 'no_kk', 'umur', 'jenis_kelamin', 'alamat', 'rt', 'rw', 'p_sebelum_pandemi', 'p_sebelum', 'p_setelah_pandemi', 'p_setelah', 'alasan_bantuan']
        for (let i = 0;i < listForm.length;i++) {this[listForm[i]] = ''}
        this.ktp = null
        this.kk = null
        this.setuju = false
        this.$refs.input.resetValidation()
      }
    }
  }
</script>
