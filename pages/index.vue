<template> 
<div class="container-fluid"> 
  <div class="row my-5">
    <div class="col-lg-6">
      <nuxt-link to="/pengunjung/tambah">
        <div class="card bg-pengunjung rounded-5 m-3">
          <div class="card-body">
            <h2> Pengunjung </h2>
          </div>
        </div>
      </nuxt-link>
    </div>
    <div class="col-lg-6"> 
      <nuxt-link to="/buku"> 
        <div class="card bg-buku rounded-5 m-3"> 
          <div class="card-body"> 
            <h2> Cari Buku </h2>
          </div>
        </div>
      </nuxt-link>
    </div>
  </div>
</div>
<h1 style="margin: 25px;"> <strong> STATISTIK</strong> </h1>
<div class="container-fluid">
  <div class="row justify-content-evenly rounded-5">
    <div class="col-5">
      <div class="jeep">
        <nuxt-link to="/pengunjung">
        <h2> {{  visitors.length }} pengunjung </h2>
      </nuxt-link>
      </div>
    </div>
    <div class="col-5">
      <div class="raccing2">
        <h2> {{  books.length }} Buku </h2>
      </div>
    </div>
  </div>
</div>
</template>

<style scoped> 
.card {
  height: 250px;
  box-shadow: 1px 1px 10px #424242;
}
.card.bg-pengunjung {
  background-image: url('../assets/img/bg-home-kunjungan.webp');
  background-repeat: no-repeat;
  background-position: center center;
  background-size: cover;
}
.card.bg-buku {
  background: url('../assets/img/bg-home-cari-buku.webp')no-repeat center center;
  background-size: cover;
}

.jeep{
  height: 200px;
  box-shadow: 1px 1px 10px;
  border-radius: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgb(242, 255, 167);
}
  .raccing2{
    height: 200px;
    box-shadow: 1px 1px 10px;
    border-radius: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: rgb(101, 250, 200);


  }

  

</style> 

<script setup> 

const supabase = useSupabaseClient()
const visitors = ref ([])
const books = ref([]) 

const getPengunjung = async () => {
  const { data, error } = await supabase.from('pengunjung').select(`*, keanggotaan(*), keperluan(*)`)
  if(data) visitors.value = data
}

const getBooks = async () => {
  const {data, error } = await supabase.from('buku').select(`*, kategori(*)`)
  if(data) books.value = data
}

onMounted (() => {
  getPengunjung()
  getBooks()
} 
)

</script>