<template> 
<div class="container-fluid"> 
  <div class="row"> 
    <div class="col-lg-12"> 
      <h2 class="text-center my-4"> RIWAYAT KUNJUNGAN </h2>
      <div class="my-3"> 
        <form @submit.prevent="getPengunjung">
        <input v-model="keyword" type="search" class="form-control form-control-lg rounded-3" placeholder="Filter...">
      </form>
      </div>
      <div class="my-3 text-muted"> menampilkan {{ visitors.length }} dari {{ jumlah }}</div>
      <table class="table"> 
        <thead> 
          <tr> 
            <td>No</td>
            <td>NAMA</td>
            <td>KEANGGOTAAN</td>
            <td>WAKTU</td>
            <td>KEPERLUAN</td>
          </tr>
        </thead>
        <tbody> 
          <tr v-for="(visitor, i) in visitors" :key="i"> 
            <td> {{ i+1 }}.</td>
            <td> {{ visitor.nama }}</td>
            <td> {{ visitor.keanggotaan.nama }}</td>
            <td> {{ visitor.tanggal }}, {{ visitor.waktu }}</td>
            <td> {{ visitor.keperluan.nama }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
  <div class="my-3 text-muted">klik untuk kembali ke home</div>
  <nuxt-link to="/">
        <button type="submit" class="btn btn-succes btn-lg rounded-3 px-3">Kembali</button>
</nuxt-link>
</div>
</template>

<style scoped>
.btn-success{
  background-color: #2DEF34;
}
input{
  background: #D9D9D9;
} 
</style>

<script setup>
const supabase = useSupabaseClient()

const visitors = ref([])
const jumlah = ref(0)
const keyword = ref('')

const getPengunjung = async () => {
  const { data, error } = await supabase.from('pengunjung').select(`*, keanggotaan(*), keperluan(*)`)
  .ilike('nama', `%${keyword.value}%`)
  if(data) visitors.value = data

}

const totalPengunjung = async () => {
  const { data, count } = await supabase.from('pengunjung').select("*", { count: 'exact'})
  if (data) jumlah.value = count
}

  onMounted(() => {
    getPengunjung()
    totalPengunjung()
  })
  
</script>