<script>
    export let imgObj
    export let name
    export let category
    export let price
    export let handleAddToCart
    export let changeQuantity
    export let removeFromCart
    export let activated

    let innerWidth = 0

    let imgUrl
    if(innerWidth <= 500) {
        imgUrl = imgObj.mobile
    } else if (innerWidth > 500 && innerWidth < 1000) {
        imgUrl = imgObj.tablet
    } else {
        imgUrl = imgObj.desktop
    }
    
    
    
    
    $: state = getCardState(activated)
    $: selected = state

    function getCardState(activated) {
        let state
        activated.map(item => {
            if(item.name == name) {
                state = item.state
            }
        })
        return state
    }

    let quantity = 0
    $: if(!state) {
        quantity = 0
    }

    function handleAddToCartWithin() {
        if(quantity == 0) {
            handleAddToCart(name)
            quantity = 1
        }
        selected = true
    }


    

    function incrementQuantity() {
        changeQuantity(name, "increment")
        quantity += 1
    }
    function decrementQuantity() {
        changeQuantity(name, "decrement")
        quantity -= 1
        if(quantity == 0) {
            selected = false
            removeFromCart(name)
        }
    }
</script>

<svelte:window bind:innerWidth />

<div>
    <div class="upper">
        <img src={imgUrl} alt={name} class={selected ? "cardActive" : "cardInactive"}/>
        {#if selected}
            <button class="cartBtnBaseStyle cartBtnActive">
                <button class="qtyBtn" on:click={decrementQuantity}><svg xmlns="http://www.w3.org/2000/svg" width="10" height="2" viewBox="0 0 10 2"><path fill="#fff" d="M0 .375h10v1.25H0V.375Z"/></svg></button>
                {quantity}
                <button class="qtyBtn" on:click={incrementQuantity}><svg xmlns="http://www.w3.org/2000/svg" width="10" height="10" fill="none" viewBox="0 0 10 10"><path fill="#fff" d="M10 4.375H5.625V0h-1.25v4.375H0v1.25h4.375V10h1.25V5.625H10v-1.25Z"/></svg></button>
            </button>
        {:else}
            <button on:click={handleAddToCartWithin} class="cartBtnBaseStyle cartBtn">
                <img src="assets/images/icon-add-to-cart.svg" alt="icon" class="cartBtnImg" width="30px">Add to Cart
            </button>
        {/if}
        
    </div>
    <div class="lower">
        <p class="category">{category}</p>
        <p class="name">{name}</p>
        <p class="price">${price.toPrecision(3)}</p>
    </div>
</div>


<style>
    div {
        font-family: "Red Hat Text", sans-serif;
    }
    .upper {
        display: flex;
        flex-direction: column;
        align-items: center;
       
    }
    .upper > img {
        width: 100%;
        border-radius: 10px;
    }

    .cardActive {
        border: 4px solid hsl(14, 86%, 42%);
        transition: border 300ms ease-out;
    }
    .cardInactive {
        border: 0px solid transparent;
        transition: border 300ms ease-in;
        
    }

    .cartBtnBaseStyle {
        margin-top: -30px;
        display: flex;
        align-items: center;
        width: 60%;
        font-family: "Red Hat Text", sans-serif;
        font-size: 16px;
        font-weight: 600;
        padding: 15px 20px;
        border-radius: 30px;
        cursor: pointer;
    }
    .cartBtn {
        gap: 7px;
        justify-content: center;
        background-color: white;
        border: 0.2px solid hsl(7, 20%, 60%);
        cursor: pointer;

        &:hover {
            border-color: hsl(14, 86%, 42%);
            color: hsl(14, 86%, 42%);
            transition: all 200ms ease-in;
        }
    }

    .cartBtnActive{
        color: white;
        background-color: hsl(14, 86%, 42%);
        justify-content: space-between;
        border: none;
        cursor: pointer;
    }

    * {
        margin: 0;
    }

    .lower {
        margin-top: 15px;
        display: flex;
        flex-direction: column;
        gap: 4px;
    }

    .category {
        color: hsl(14, 25%, 72%);
        font-size: 16px;
    }
    .name {
        font-weight: 600;
        color: hsl(14, 65%, 9%);
        font-size: 18px;
    }

    .price {
        color: hsl(14, 86%, 42%);
        font-weight: 600;
    }
    .qtyBtn {
        background-color: inherit;
        border: 2px solid white;
        width: fit-content;
        height: 25px;
        cursor: pointer;
        display: grid;
        place-items: center;
        /* align-items: center; */
        /* justify-content: center; */
        border-radius: 50%;
    }

    .qtyBtn svg {
        width: 10px;
        height: 10px;
        /* border: 3px solid red; */
        margin: auto;
        
    }
    .qtyBtn:hover {
        background-color: white;
        
        & svg path {
        fill: hsl(14, 86%, 42%);
    }
    }
    
</style>