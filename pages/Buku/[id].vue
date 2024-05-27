<script setup>
import { ref, onMounted } from 'vue'
const supabase = useSupabaseClient()

const route = useRoute()
const buku = ref ([null])

const getBookById = async () => {
    const { data, error } = await supabase.from('buku').select(`*, kategori(nama)`)
        .eq('id', route.params.id)
    if(data) buku.value = data[0]
    }


onMounted(() => {
    getBookById()
})
</script>

<template>
<div class="container"> 
<h2 class="text-start my-4"> {{ buku.judul }}</h2>
<div class="row"> 
    <div class="col-md-3"> 
        <img :src="buku.cover" class="cover" alt="cover buku">
    </div>
    <div class="col-md-6">
        <div class="badge bg-primary p-2" alt="cover buku">    
        </div>
        <ul class="list-group list-group-flush"> 
            <li class="list-group-item"><strong> Penulis : </strong> {{ buku.penulis }}</li>
            <li class="list-group-item"><strong> Penerbit :</strong> {{ buku.penerbit }}</li>
            <li class="list-group-item"><strong> Deskripsi : </strong> {{ buku.Deskripsi }}</li>
            <li class="list-group-item"><strong> Tahun_Terbit : </strong>{{ buku.tahun_terbit }}</li>          
        </ul>
    </div>
</div>
<div class="row">
    <div class="col-lg-12 d-flex justify-content-center mt-5">
        <nuxt-link to="/Buku/">
          <button type="submit" class="btn btn-warning btn-lg rounded-3 px-3">Kembali</button>
    </nuxt-link>
    </div>
</div>
</div>

</template>
<style scoped>
img{ 
    width: 60%;
}
</style>