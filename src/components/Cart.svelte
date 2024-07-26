<script>
    export let cart
    export let removeFromCart

    $: itemsInCart = numberOfItemsInCart(cart)

    function numberOfItemsInCart(cart) {
        let number = 0 
        cart.map(item => {
            number += item.quantity
        })
        return number
    }

    $: totalAmount = totalOrderAmount(cart)

    function totalOrderAmount(cart) {
        let total = 0
        cart.map(item => {
            total += item.total
        })
        return total
    }

    function removeItemFromCart(name) {
        removeFromCart(name)
    }
</script>

<section class="cart">
    <h2>Your Cart ({itemsInCart})</h2>

    <div class="holdItems">
        {#if itemsInCart < 1}
            <div class="emptyCart">
                <img src="assets/images/illustration-empty-cart.svg" alt="empty cart">
                <p>Your added items will appear here</p>
            </div>
        {:else}
            {#each cart as item}
                <div class="item">
                    <div class="nameAndDesc">
                        <div>
                            <h4 class="name">{item.name}</h4>
                        </div>
                        <div class="desc">
                            <h5 class="quantity">{item.quantity}X</h5>
                            <p class="price">@${item.price.toFixed(2)}</p>
                            <p class="total"><strong>${item.total.toFixed(2)}</strong></p>
                        </div>
                    </div>
                    <button class="removeBtn" on:click={() => removeItemFromCart(item.name)}>
                        <img src="assets/images/icon-remove-item.svg" alt="remove">
                    </button>
                </div>
            {/each}
            <div class="totalOrderAmount">
                <p class="price">Order Total</p>
                <h2 class="totalAmount">${totalAmount.toFixed(2)}</h2>
            </div>

            <div class="carbon">
                <img src="assets/images/icon-carbon-neutral.svg" alt="carbon">
                <p>This is a <strong>carbon-neutral</strong> delivery</p>
            </div>

            <button class="confirmBtn">Confirm Order</button>
        {/if}
    </div>
</section>

<style>
    .carbon {
        display: flex;
        align-items: center;
        justify-content: center;
        background-color: hsl(13, 31%, 94%);
        padding: 20px 10px;
        border-radius: 10px;
        gap: 5px;

        & p {
            font-size: 14px;
            color: hsl(14, 65%, 9%);
        }
    }
    h4, h5, p {
        margin: 0px;
    }

    .totalAmount {
        color: hsl(14, 65%, 9%);
    }

    .orderP {
        color: hsl(14, 65%, 9%);
    }

    .totalOrderAmount {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .holdItems {
        display: flex;
        flex-direction: column;
        gap: 20px;
    }

    .nameAndDesc {
        display: flex;
        flex-direction: column;
        gap: 10px;
    }

    .name {
        color: hsl(14, 65%, 9%);
    }

    .removeBtn {
        background-color: transparent;
        width: 20px;
        height: 20px;
        border-radius: 50%;
        border: 2px solid hsl(14, 25%, 72%);
        color: hsl(13, 31%, 94%);
        display: flex;
        align-items: center;
        justify-content: center;
        
    }

    .quantity {
        color: hsl(14, 86%, 42%);
    }

    .item {
        display: flex;
        width: 100%;
        justify-content: space-between;
        align-items: center;
        padding-bottom: 20px;
        border-bottom: 1px solid gray;
    }
    .desc {
        display: flex;
        align-items: center;
        gap: 15px
    }
    .cart {
        width: 90%;
        /* height: 400px; */
        margin-top: 30px;
        margin-bottom: 50px;
        background-color: white;
        padding: 20px;
        font-family: 'Red Hat Text', sans-serif;
        border-radius: 10px 20px;
        display: flex;
        flex-direction: column;
        /* align-items: center; */
        gap: 20px;
    }
    h2 {
        color: hsl(14, 86%, 42%);
        font-size: 30px;
        align-self: flex-start;
        margin: 0px;
    }
    p{
        color: hsl(12, 20%, 44%);
        font-size: 16px;
    }
    .emptyCart {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 20px;
        margin-top: 30px;
    }

    .confirmBtn {
        background-color: hsl(14, 86%, 42%);
        color: white;
        padding: 20px;
        border-radius: 30px;
        border: none;
        font-size: 16px;
        font-family: 'Red Hat Text', sans-serif;
    }
</style>