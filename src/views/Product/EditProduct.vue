<template>
    <div class="container">
        <div class="row">
            <div class="col-12 text-center">
                <h4 class="pt-3"> Редактировать продукт</h4>
            </div>
        </div>
        <div class="row">
            <div class="col-3"></div>
            <div class="col-6">
                <form v-if="product">
                    <div class="form-group">
                        <label> Категория</label>
                        <select class="form-control" v-model="product.categoryId" required>
                            <option v-for="category of categories"
                                    :key="category.id"
                                    :value="category.id"> {{category.categoryName}}</option>

                        </select>
                    </div>
                    <div class="form-group">
                        <label>Название</label>
                        <input type="text" class="form-control" v-model="product.name" required/>
                    </div>
                    <div class="form-group">
                        <label>Описание</label>
                        <input type="text" class="form-control" v-model="product.description" required/>
                    </div>
                    <div class="form-group">
                        <label>Изображение URL</label>
                        <input type="text" class="form-control" v-model="product.imageURL" required/>
                    </div>
                    <div class="form-group">
                        <label>Цена</label>
                        <input type="number" class="form-control" v-model="product.price" required/>
                    </div>
                    <button type="button" class="btn btn-primary"
                            @click="editProduct" >Редактировать</button>
                </form>
            </div>
            <div class="col-3"></div>
        </div>
    </div>
</template>
<script>
    import axios from 'axios'
    import swal from 'sweetalert'
    export default {
        data() {
            return {
                product: null,
                id: null
            }
        },
        props: ["baseURL", "categories", "products"],
        methods: {
            async editProduct() {
                console.log('product', this.product)
                await axios.post(`${this.baseURL}product/update/${this.id}`,
                    this.product)
                    .then(() => {
                        this.$emit("fetchData");
                        this.$router.push({name: 'AdminProduct'})
                        swal({
                            text: "Продукт был успешно изменён!",
                            icon: "success"
                        })
                    }).catch(err => console.log('err', err));
            }
        },
        mounted() {
            this.id = this.$route.params.id;
            this.product = this.products.find(product => product.id == this.id)
        }
    }
</script>