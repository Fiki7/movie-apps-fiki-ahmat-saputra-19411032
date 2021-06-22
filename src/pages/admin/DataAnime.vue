<template>
  <q-page padding>
    <div class="row q-mb-md col-gutter-md">
      <div class="col-md-12 col-xs-12 col-lg-12">
        <div class="row">
          <div class="col-auto">
            <div class="left blue"></div>
          </div>
          <div class="col">
            <q-banner inline-actions class="text-b;ue-grey-12">
              <div class="text-h6">Data Anime</div>
              <div>Data Katalog Anime Anda</div>
            </q-banner>
          </div>
        </div>
      </div>
    </div>
    <q-card flat>
      <q-card-section>
            <q-table
            :data="data"
            :columns="columns"
            row-key="name"
          >
            <template v-slot:body="props">
              <q-tr :props="props">
                <q-td key="judulAnime" :props="props">
                  <div>
                  {{ props.row.judulAnime }}
                  </div>
                </q-td>
                <q-td key="harga" :props="props">
                  Rp. {{ props.row.harga }},-
                </q-td>
                <q-td key="tahun" :props="props">
                  {{ props.row.tahun }}
                </q-td>
                <q-td key="genre" :props="props">
                  {{ props.row.genre }}
                </q-td>
                <q-td key="deskripsi" :props="props">
                  <div class="ellipsis" style="max-width: 250px;">
                  {{ props.row.deskripsi }}
                  </div>
                </q-td>
                <q-td key="gambar" :props="props">
                <q-img
                  :src="`http://localhost:5000/${props.row.image}`"
                  spinner-color="blue"
                  style="height: 120px; max-width: 85px"
                  />
                </q-td>
                <q-td key="aksi" :props="props">
                  <div class="row q-gutter-md">
                    <q-btn :to="{ name: 'formEditAnime', params: { id: props.row._id }}" label="Edit" icon="edit" color="orange"/>
                    <q-btn @click="deleteAnime(props.row._id)" label="Hapus" icon="delete" color="red"/>
                  </div>
                </q-td>
              </q-tr>
            </template>
            </q-table>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      columns: [
        { name: 'judulAnime', align: 'left', label: 'Judul Anime', field: 'judulAnime', sortable: true },
        { name: 'harga', align: 'left', label: 'Harga', field: 'harga', sortable: true },
        { name: 'tahun', align: 'left', label: 'Tahun', field: 'tahun', sortable: true },
        { name: 'genre', align: 'left', label: 'Genre', field: 'genre', sortable: true },
        { name: 'deskripsi', align: 'left', label: 'Deskripsi', field: 'deskripsi', sortable: true },
        { name: 'gambar', align: 'left', label: 'Gambar', field: 'gambar' },
        { name: 'aksi', align: 'left', label: 'Aksi', field: 'aksi' }
      ],
      data: []
    }
  },
  created () {
    this.getData()
  },
  methods: {
    getData () {
      this.$axios.get('anime/getall')
        .then((res) => {
          if (res.data.sukses) {
            this.data = res.data.data
          } else {
            this.showNotif(res.data.pesan, 'negative')
          }
        })
    },
    deleteAnime (id) {
      this.$q.dialog({
        title: 'Konfirmasi',
        message: 'Apakah Anda Yakin Menghapus Anime ini?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.$axios.delete(`anime/delete/${id}`)
          .then(res => {
            if (res.data.sukses) {
              this.$showNotif(res.data.pesan, 'positive')
              this.getData()
            } else {
              this.$showNotif(res.data.pesan, 'negative')
            }
          })
      })
    }
  }
}
</script>
<style scoped>
.left {
  width: 5px;
  height: 100%;
  background-color: aqua;
}
</style>
