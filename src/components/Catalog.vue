<template>
    <div class="products">
        <div class="products-groups" v-for="(group, index) of groups" :key="index">
            <h2>{{group}}</h2>
            <div v-for="product of products" :key="(product.G+'-'+product.T)">
                <product v-if="group == product.group_name" :item="product" />
            </div>
        </div>
    </div>
</template>

<script>
import product from './Product'

export default {
    data() {
        return {
            products: [],
            goods: [],
            groups: []
        }
    },
    methods: {
        getItems() {
            this.$parent.getJSON('../data/data.json')
            .then((data) => {
                if (data.Success) {
                    this.$goods = data.Value.Goods
                    this.$parent.getJSON('../data/names.json')
                        .then(res => {
                            let names = res
                            this.$goods.forEach(el => {
                                let item = {...el}
                                let randDol = 67
                                item.name = names[item.G].B[item.T].N
                                item.group_name = names[item.G].G
                                item.cur_price = item.C * randDol
                                item.prev_price = 0
                                if (!(this.groups.includes(item.group_name))) {
                                    this.groups.push(item.group_name)
                                }
                                this.products.push(item)
                            })
                        })
                }
            })
        },
        updateItems() {
            this.$parent.getJSON('../data/data.json')
            .then((data) => {
                if (data.Success) {
                    this.$goods = data.Value.Goods
                    this.$goods.forEach(el => {
                        let randDol = Math.round(20 - 0.5 + Math.random() * (80 - 20 + 1))
                        let find = this.products.find(item => item.T === el.T)
                        find.prev_price = find.cur_price
                        find.cur_price = el.C * randDol
                    })
                }
            })
        }
    },
    mounted() {
        this.getItems()
        setInterval(() => {
            this.goods.length = 0
            this.updateItems()
        }, 15000)
    },
    components: {
        product
    }
}
</script>

<style>
    .products {
        padding: 15px;
    }

    .products h2 {
        text-align: center;
        flex-basis: 100%;
        border: 1px solid black;
        border-right: 0;
        border-left: 0;
        margin: 10px 0;
        padding: 16px 0;
    }

    .products-groups {
        display: flex;
        flex-wrap: wrap;
    }

    .products-groups > div {
        flex-basis: calc(50% - 4px);
        padding: 6px 10px;
        box-sizing: border-box;
        margin: 2px;
    }

    .buy-btn {
        margin-top: 5px;
        background-color: #2f2a2d;
        padding: 5px 15px;
        border: 1px solid transparent;
        color: #fafafa;
        border-radius: 5px;
        transition: all ease-in-out .4s;
        cursor: pointer;
    }
    .buy-btn:hover {
        background-color: #fafafa;
        color: #2f2a2d;
        border: 1px solid #2f2a2d;
    }
    .less {
        background-color: rgba(0, 255, 0, 0.281);
    }

    .more {
        background-color: rgba(255, 0, 0, 0.247);
    }
</style>