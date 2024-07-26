<script>
    export let data
    let desserts = data.desserts
    import Card from '../components/Card.svelte'
    import Cart from '../components/Cart.svelte'

    let cart = []

    let activated = desserts.map(item => {
        return {
            name: item.name,
            state: false
        }
    })

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

    
</script>


<main>
    <section>
        <h1>Desserts</h1>
        <section class="dessertList">
        {#each desserts as dessert}
            <Card imgUrl={dessert.image.mobile} name={dessert.name} category={dessert.category} price={dessert.price} handleAddToCart={handleAddToCart} changeQuantity={changeQuantity} removeFromCart={removeFromCart} activated={derivedActivated}/>
        {/each}
        </section>
    </section>

    <section class="cart">
        <Cart cart={derivedCart} removeFromCart={removeFromCart} />

    </section>
</main>



<style>
    * {
        margin: 0
    }
    h1 {
        font-family: "Red Hat Text", sans-serif;
        color: hsl(14, 65%, 9%);
        margin-bottom: 20px;
        align-self: flex-start;
    }
    main {
        background-color: hsl(13, 31%, 94%);
        margin: 0;
        padding: 25px;
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    .dessertList {
        display: flex;
        flex-direction: column;
        gap: 20px;
    }

    @media screen and (width >= 800px) {
        .dessertList {
            display: grid;
            grid-template-columns: 1fr 1fr;
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
    }
    @media screen and (width >= 1300px) {
        main {
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
            margin-bottom: 50px;
        }
    }
</style>