<template> 
<div class="container-fluid">
  <div class="row"> 
    <div class="col-lg-13"> 
      <h2 class="text-center my-4">PILIHAN BUKU</h2>
      <div class="my-3">
        <form @submit.prevent="getBooks">
          <input v-model="keyword" type="search" class="form-control rounded-3" placeholder="Mau baca apa hari ini?"> 
      </form>
      </div>
      <div class="my-3 text-muted fs-6"> menampilkan {{ books.length }} dari {{ jumlah }}</div>
      <div class="row"> 
        <div v-for="(book,i) in books" :key="i" class="col-lg-3 pb-5">
          <NuxtLink :to ="`/buku/${book.id}`">
          <div class="card mb-3 dcdc " style="width: 200px;"> 
            <div class="card-header d-flex justify-content-center" style="height: 250px;">
                <img :src="book.cover" :alt="book.judul" style="width: 100%; height: 100%;">
            </div>
            <div class="card-body d-flex justify-content-center align-items-center" style="height: 50px;" >
              <h6> {{  book.judul  }} </h6>
            </div>
          </div>
        </NuxtLink>
        </div>
      </div>
    </div>
  </div>
        <nuxt-link to="/">
          <button type="submit" class="btn btn-secondary btn-lg rounded-3 px-3">Kembali</button>
        </nuxt-link>
          
      </div>
</template>


<style scoped> 
.btn-warning{
  background-color: #2DEF34;
}
.card-body {
  width: 90%;
  height: 16em;
  padding: 0;
}

.dcdc{
  width: 220px ;
  height: 100%;
}




</style>

<script setup>

const supabase = useSupabaseClient()

const books = ref([])
const jumlah = ref(0)
const keyword = ref('')

const getBooks = async () => {
  const {data, error } = await supabase.from('buku').select(`*, kategori(*)`)
  .ilike('judul', `%${keyword.value}%`)
  if(data) books.value = data
}

const totalBuku = async () => {
  const { data, count } = await supabase.from('buku').select("*", { count: 'exact'})
  if (data) jumlah.value = count
}

onMounted(() => {
  getBooks()
  totalBuku()
})

</script>
