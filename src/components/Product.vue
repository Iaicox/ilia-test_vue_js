<template>
    <div class="product-item" v-bind:class="{less: (item.prev_price !=0 && item.prev_price > item.cur_price), more: (item.prev_price !=0 && item.prev_price < item.cur_price)}">
        <h3>{{item.name}}</h3>
        <p>{{item.cur_price}} руб.</p>
        <p>Осталось {{item.P}} шт.</p>
        <button class="buy-btn" @click="$root.$children[0].$refs.cart.addProduct(item)">Купить</button>
    </div>
</template>

<script>
export default {
    props: ['item']
}
</script>