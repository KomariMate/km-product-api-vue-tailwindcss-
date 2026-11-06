<script setup>
import { createProduct, getProducts } from '../api/Products';
</script>

<script>
export default {
    name: "ProductManager",
    data() {
        return {
            products: [],
            newProduct: {
                name: "",
                price: "",
                inStock: false,
            }
        }
    },
    methods: {
        async loadProducts() {
            this.products = await getProducts();
        },
        async addProduct(product) {
            await createProduct(product);
            await this.loadProducts();
        },
        async submitCreatedProduct() {
            await this.addProduct(this.newProduct)
        }
    },
    mounted() {
        this.loadProducts()
    },
}
</script>

<template>
    <main class="flex justify-center items-center flex-col min-h-screen">
        <h1 class="text-3xl">Termékek kezelése</h1>

        <table class="flex flex-col w-full p-4">
            <thead>
                <tr class="w-full flex justify-center">
                    <th class="w-full text-left">Azonosító</th>
                    <th class="w-full text-left">Név</th>
                    <th class="w-full text-left">Ár</th>
                    <th class="w-full text-left">Raktáron</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="product in products" :key="product.id" class="w-full flex justify-between my-2">
                    <td class="w-full text-left">{{ product.id }}</td>
                    <td class="w-full text-left">{{ product.name }}</td>
                    <td class="w-full text-left">{{ product.price }}</td>
                    <td class="w-full text-left">{{ product.inStock ? "igen" : "nem" }}</td>
                </tr>
            </tbody>
        </table>

        <form @submit.prevent="submitCreatedProduct" class="flex flex-col gap-3 my-5 border-2 border-stone-200 rounded-2xl p-5">
            <label for="name">Név</label>
            <input class="border-2 border-stone-200 rounded-xl px-2 py-1" type="text" name="name"
                v-model="newProduct.name" placeholder="Laptop..." />

            <label for="price">Ár</label>
            <input class="border-2 border-stone-200 rounded-xl px-2 py-1" type="number" name="price"
                v-model.number="newProduct.price" placeholder="1 500 Ft" />

            <div class="flex justify-between">
                <label for="inStock">Raktáron</label>
                <input type="checkbox" name="inStock" v-model="newProduct.inStock" />
            </div>

            <button type="submit" class="border-2 border-stone-200 rounded-x1 py-2 cursor-pointer">Létrehozás</button>
        </form>
    </main>
</template>