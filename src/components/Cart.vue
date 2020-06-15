<template>
    <div class="cart-block" v-show="cartVisible">
        <table v-if="cartItems.length != 0">
            <thead>
                <tr>
                    <th>Наименование товара и описание</th>
                    <th>Количество</th>
                    <th colspan="2">Цена</th>
                </tr>
            </thead>
            <tbody>
                <cart-item v-for="cartItem of cartItems" :item="cartItem" :key="cartItem.T"></cart-item>
            </tbody>
            <tfoot>
                <tr>
                    <th class="product-sum" colspan="3">Общая стоимость: </th>
                    <td><input  v-model="sum" readonly> руб.</td>
                </tr>
            </tfoot>
        </table>
        <div v-else>
            <p>Не выбран ни один товар.</p>
        </div>
    </div>
</template>

<script>
import cart_item from './CartItem'

export default {
    data() {
        return {
            cartItems: [],
            sum: 0,
            cartVisible: false
        }
    },
    methods: {
        addProduct(el) {
            let find = this.cartItems.find(item => item.T === el.T)
            if (find) {
                if (find.quantity < find.P) {
                    find.quantity++
                    this.countSum()
                } else {
                    alert("На складе больше нет этого товара.")
                }
            } else {
                let cartItem = Object.assign({}, el, {quantity: 1})
                this.cartItems.push(cartItem)
                this.countSum()
            }
        },
        delProduct(el) {
            let find = this.cartItems.find(item => item.T === el.T)
            if (find.quantity > 1) {
                find.quantity--
                this.countSum()
            } else {
                this.cartItems.splice(this.cartItems.indexOf(find), 1)
                this.countSum()
            }
        },
        countSum() {
            this.sum = 0
            this.cartItems.forEach(item => {
                this.sum += item.quantity*item.cur_price
            })
        }
    },
    components: {
        'cart-item': cart_item
    }
}
</script>

<style>
    button:focus {
        outline: none;
    }
    .btn-cart {
        background-color: #fafafa;
        padding: 10px 20px;
        border: 1px solid transparent;
        color: #2f2a2d;
        border-radius: 5px;
        transition: all ease-in-out .4s;
        cursor: pointer;
    }
    .btn-cart:hover {
        background-color: transparent;
        border-color: #fafafa;
        color: #fafafa;
    }
    .btn-cart, .logo {
        align-self: center;
    }
    .desc {
        border: 1px solid #c0c0c040;
        padding: 15px
    }
    .cart{
        position: relative;
    }
    .cart-block{
        box-shadow: 0 0 5px rgba(0, 0, 0, 0.62);
        border-radius: 5px;
        box-sizing: border-box;
        right: 0;
        top: 130%;
        position: absolute;
        background-color: white;
        padding: 20px;
        color: black;
        width: 800px;
    }
    .cart-block table {
        width: 100%;
    }

    td:first-child,
    th:first-child {
        width: 50%;
    }

    td:nth-child(2),
    th:nth-child(2) {
        width: 10%;
    }
    td:nth-child(3),
    th:nth-child(3) {
        width: 20%;
    }
    th:nth-child(3) {
        text-align: left;
    }
    td:nth-child(4),
    th:nth-child(4) {
        width: 20%;
    }
    td, th {
        border: 1px solid black;
    }

    .product-delete-btn span {
        cursor: pointer;
        padding: 0 5px;
    }
    .product-delete-btn span:hover {
        font-weight: bold;
        border-bottom: 2px solid black;
    }

    .product-sum,
    .product-sum + td>input{
        text-align: end;
    }
    td>input:read-only {
        width: 70%;
    }

    .deficiencies {
        height: 52px;
        vertical-align: sub;
        position: relative;
    }

    .deficiencies::after {
        content: "Количество ограничено";
        display: block;
        position: absolute;
        bottom: 1.5px;
        left: 1.5px;
        right: 1.5px;
        color: red;
        border: 0.5px solid red;
        padding: 0 3px
    }

    .invisible{
        display: none;
    }
    .cart-block:before{
        content: '';
        width: 0;
        height: 0;
        position: absolute;
        top: -10px;
        right: 35px;
        border-left: 10px solid transparent;
        border-right: 10px solid transparent;
        border-bottom: 10px solid white;
    }

    .buy-btn, .del-btn{
        margin-top: 5px;
        background-color: #2f2a2d;
        padding: 5px 15px;
        border: 1px solid transparent;
        color: #fafafa;
        border-radius: 5px;
        transition: all ease-in-out .4s;
        cursor: pointer;
    }
    .buy-btn:hover, .del-btn:hover{
        background-color: #fafafa;
        color: #2f2a2d;
        border: 1px solid #2f2a2d;
    }
    .cart-item {
        display: flex;
        justify-content: space-between;
    }
    .cart-item:not(:last-child){
        margin-bottom: 20px;
    }
    .cart-item img{
        align-self: flex-start;
        margin-right: 15px;
    }
    .product-single-price{
        color: #474747;
        font-size: 0.9em;
    }
    .product-price{
        margin-left: 30px;
    }
    .product-desc{
        max-width: 150px;
    }
    .product-quantity {
        margin-top: 15px;
        font-size: 0.75em;
    }
    .product-quantity input {
        width: 70%;
        box-sizing: border-box;
    }
    .right-block{
        text-align: right;
    }
</style>