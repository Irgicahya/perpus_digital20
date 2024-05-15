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
      <div class="my-3 text-muted fs-6">menampilkan {{ books.length }} dari {{ jumlah }}</div>
      <div class="row"> 
        <div v-for="(book,i) in books" :key="i" class="col-lg-2 pb-5"> 
          <div class="card mb-3 dcdc "> 
            <div class="card-body">
            <NuxtLink :to ="`/buku/${book.id}`">
                <img :src="book.cover" alt="" width="186" height="205">
                <h6> {{  book.judul  }}</h6>
              </NuxtLink>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
        <nuxt-link to="/">
          <button type="submit" class="btn btn-warning btn-lg rounded-3 px-3">Kembali</button>
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