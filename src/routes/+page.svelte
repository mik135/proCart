<script>
    export let data
    let desserts = data.desserts
    import Card from '../components/Card.svelte'
    import Cart from '../components/Cart.svelte'
    import InnerCard from '../components/InnerCard.svelte'

    let cart = []

    let activated = desserts.map(item => {
        return {
            name: item.name,
            state: false
        }
    })

    let dialog

    function openDialog() {
        dialog.showModal()
    }

    function closeDialog() {
        dialog.close()
        cart = []
        activated = desserts.map(item => {
        return {
            name: item.name,
            state: false
        }
    })
    }

    $: derivedCart = cart

    $: derivedActivated = activated

    let handleAddToCart = (name) => {
        desserts.map(dessert => {
            if(dessert.name == name) {
                cart = [...cart, {
                    name: dessert.name,
                    quantity: 1,
                    price: dessert.price,
                    total: dessert.price,
                    image: dessert.image.thumbnail
                }]
            }
        })
        activated.map(item => {
            if(item.name == name) {
                item.state = true;
            }
        })
        activated = activated
    }

    let removeFromCart = (name) => {
        let currName = name
        cart = cart.filter(item => item.name !== name)

        activated.map(item => {
            if(item.name == name) {
                item.state = false
            }
        })
        activated = activated

    }

    let changeQuantity = (name, action) => {
        cart.map(item => {
            if(item.name == name && action == "increment") {
                item.quantity += 1
                item.total += item.price
            } else if (item.name == name && action == "decrement") {
                item.quantity -= 1
                item.total -= item.price
            }
        })
        cart = cart
    }

    $: totalAmount = totalOrderAmount(derivedCart)

    function totalOrderAmount(cart) {
        let total = 0
        cart.map(item => {
            total += item.total
        })
        return total
    }

    
</script>


<main>
    <section>
        <h1>Desserts</h1>
        <section class="dessertList">
        {#each desserts as dessert}
            <Card imgObj={dessert.image} name={dessert.name} category={dessert.category} price={dessert.price} handleAddToCart={handleAddToCart} changeQuantity={changeQuantity} removeFromCart={removeFromCart} activated={derivedActivated}/>
        {/each}
        </section>
    </section>

    <section class="cart">
        <Cart cart={derivedCart} removeFromCart={removeFromCart} {openDialog}/>

    </section>

    <dialog bind:this={dialog}>
        <div class="dialogDiv">
            <img src="assets/images/icon-order-confirmed.svg" alt="order confirmed" class="checkIcon">
            <h1>Order <br> Confirmed</h1>
            <p class="hope">We hope you enjoy your food!</p>
            <div class="cardContainer">
                    {#each derivedCart as item}
                        <InnerCard item={item}/>
                    {/each}
                <div class="totalOrderAmount">
                    <p class="price">Order Total</p>
                    <h2 class="totalAmount">${totalAmount.toFixed(2)}</h2>
                </div>
            </div>
            <button on:click={closeDialog} class="confirmBtn">Start New Order</button>
        </div>
    </dialog>
</main>



<style>
    .checkIcon {
        width: 15%;
    }
    dialog {
        width: 120%;
        margin-top: auto;
        border-top-left-radius: 15px;
        border-top-right-radius: 15px;
        border: none;
        font-family: 'Red Hat Text', sans-serif;
        height: 90vh;
        
    }
    .dialogDiv {
        display: flex;
        flex-direction: column;
        width: 100%;
        height: 100%;
    }
    * {
        margin: 0
    }
    h1 {
        font-family: "Red Hat Text", sans-serif;
        color: hsl(14, 65%, 9%);
        margin-bottom: 10px;
        align-self: flex-start;
        font-size: 2.5rem;
    }
    .hope {
        color: hsl(7, 20%, 60%)
    }
    main {
        background-color: hsl(13, 31%, 94%);
        margin: 0;
        padding: 25px;
        
    }

    .confirmBtn {
        cursor: pointer;
        background-color: hsl(14, 86%, 42%);
        color: white;
        padding: 20px;
        border-radius: 30px;
        border: none;
        font-size: 16px;
        font-family: 'Red Hat Text', sans-serif;
        width: 100%;
        font-weight: 600;
        margin-top: auto;
    }

    .cardContainer {
        border-radius: 10px;
        overflow: hidden;
        margin-top: 20px;
        padding: 20px 20px;
        background-color: hsl(20, 50%, 98%);
        margin-bottom: 20px;
        overflow-y: auto;
    }

    .dessertList {
        display: flex;
        flex-direction: column;
        gap: 20px;
    }

    .totalOrderAmount {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 20px 0px;
    }

    .totalAmount {
        color: hsl(14, 65%, 9%);
    }

    @media screen and (width >= 800px) {
        .dessertList {
            display: grid;
            grid-template-columns: 1fr 1fr;
        }
        dialog {
            max-width: 400px;
            max-height: 500px;
            margin: auto;
            border-radius: 15px;
            overflow: hidden;
        }
    }
    @media screen and (width >= 1000px) {
        .dessertList {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
        }
        h1 {
            font-size: 40px;
        }
        main {
            position: relative;
        }
        .cart {
            position: sticky;
            top: 10px;
        }
       
    }
    @media screen and (width >= 1300px) {
        main {
            display: flex;
            flex-direction: column;
            flex-direction: row;
            gap: 30px;
            padding: 50px;
        }
        .cart {
            align-self: flex-start;
            margin-top: 0px;
            width: 50%;
        }
        h1 {
            margin-bottom: 10px;
        }
    }
</style>