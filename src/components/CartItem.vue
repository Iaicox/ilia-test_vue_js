<template>
    <tr class="product-bio">
        <td class="product-desc product-title">{{ item.group_name }}. {{ item.name }}</td>
        <td class="product-quantity" v-bind:class="{deficiencies: item.P < 5}"><input type="number" v-model="item.quantity" @change="checkQuant(item); $root.$children[0].$refs.cart.countSum();"> шт.</td>
        <td class="product-single-price">{{ item.cur_price }} руб./шт.</td>
        <td class="product-delete-btn" @click="$root.$children[0].$refs.cart.delProduct(item)"><span>удалить</span></td>
    </tr>
</template>

<script>
export default {
    props: ['item'],
    methods: {
        checkQuant(el) {
            if (el.quantity >= el.P) {
                alert("На складе больше нет этого товара.")
                el.quantity--
            } else if (el.quantity <= 0) {
                this.$root.$children[0].$refs.cart.delProduct(el)
            }
        }
    }
}
</script>