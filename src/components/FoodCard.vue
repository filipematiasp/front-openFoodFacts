<template>
    <div class="row">
        <div v-for="product in products" :key="product._id" class="card" style="width: 12rem;">
            <img :src="product.image_small_url" class="card-img-top" alt="black dog">
            <div class="card-body">
                <h5 class="card-title">{{ product.product_name }}</h5>
                <button class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#exampleModal" @click="createModal(product)">Go somewhere</button>
            </div>
        </div>
    </div>
    <nav>
        <ul class="pagination justify-content-center">
            <li class="page-item disabled"><a class="page-link">Previous</a></li>
            <li v-for="index in sizeProducts" :key="index" class="page-item"><a class="page-link" href="#">{{ index  }}</a></li>
            <li class="page-item"><a class="page-link" href="#">Next</a></li>
        </ul>
    </nav>

    <div class="modal fade" id="exampleModal" tabindex="-1">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h1 class="modal-title fs-5" id="exampleModalLabel">{{ modalProductDescription.product_name }}</h1>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="row mb-3">
                        <label for="code" class="col-4 col-form-label">Code</label>
                        <div class="col-6">
                            <input type="text" class="form-control" :value="modalProductDescription.code" id="code" :readonly="readonly">
                        </div>
                    </div>
                    <div class="row mb-3">
                        <label for="status" class="col-form-label col-4">Status</label>
                        <div class="col-6">
                            <input type="text" class="form-control" :value="modalProductDescription.status" id="status" :readonly="readonly">
                        </div>
                    </div>
                    <div class="row mb-3">
                        <label for="imported_t" class="col-form-label col-4">Imported</label>
                        <div class="col-6">
                            <input type="text" class="form-control" :value="modalProductDescription.imported_t" id="imported_t" :readonly="readonly">
                        </div>
                    </div>
                    <div class="row mb-3">
                        <label for="url" class="col-form-label col-4">URL</label>
                        <div class="col-6">
                            <input type="text" class="form-control" :value="modalProductDescription.url" id="url" :readonly="readonly">
                        </div>
                    </div>
                    <div class="row mb-3">
                        <label for="quantity" class="col-form-label col-4">Quantidade</label>
                        <div class="col-6">
                            <input type="text" class="form-control" :value="modalProductDescription.quantity" id="quantity" :readonly="readonly">
                        </div>
                    </div>
                    <div class="row mb-3">
                        <label for="categories" class="col-form-label col-4">Categoria</label>
                        <div class="col-6">
                            <input type="text" class="form-control" :value="modalProductDescription.categories" id="categories" :readonly="readonly">
                        </div>
                    </div>
                    <div class="row mb-3">
                        <label for="packaging" class="col-form-label col-4">Packaging</label>
                        <div class="col-6">
                            <input type="text" class="form-control" :value="modalProductDescription.packaging" id="packaging" :readonly="readonly">
                        </div>
                    </div>
                    <div class="row mb-3">
                        <label for="brands" class="col-form-label col-4">Marca</label>
                        <div class="col-6">
                            <input type="text" class="form-control" :value="modalProductDescription.brands" id="brands" :readonly="readonly">
                        </div>
                    </div>

                </div>
                <div class="modal-footer">
                    <button type="button" class="btn" id="btnEdit" @click="toggleReadonly">Editar</button>
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                    <button type="button" class="btn btn-primary">Save changes</button>
                </div>
            </div>
        </div>
    </div>

</template>

<script setup>
import api from "@/services/api.js";
import { ref, onMounted } from 'vue';

const sizeProducts = ref()
const products = ref([])
let modalProductDescription = ref({})
let readonly = ref(true)

function createModal (product) {
    console.log(product);
    modalProductDescription.value = product
}

function toggleReadonly () {
    if(document.getElementById('btnEdit').innerHTML === "Reset"){
        readonly.value = true
        document.getElementById('btnEdit').innerHTML = "Editar"
    } else {
        readonly.value = false
        document.getElementById('btnEdit').innerHTML = "Reset"
    }
}

onMounted(() => {
    api.get("/products").then((response) => {
        products.value = response.data.products
        sizeProducts.value = response.data.sizeProducts / 10
    })
})
</script>

<style scoped>
</style>