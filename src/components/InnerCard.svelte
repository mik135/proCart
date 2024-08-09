<script>
    export let item

    
</script>


<div class="card">
    <div style="display: flex; align-items: center; gap: 10px;">
        <img src={item.image} alt={item.name}/>
        <div class="nameAndDesc">
            <div class="nameContainer">
                <h4 class="name">{item.name}</h4>
            </div>
            <div class="desc">
                <h5 class="quantity">{item.quantity}X</h5>
                <p class="price">@${item.price.toFixed(2)}</p>
            </div>
        </div>
    </div>
    <p class="total"><strong>${item.total.toFixed(2)}</strong></p>
</div>

<style>
    .card {
        background-color: hsl(20, 50%, 98%);
        font-family: 'Red Hat Text', sans-serif;
        display: flex;
        align-items: center;
        justify-content: space-between;
        gap: 10px;
        padding: 10px;
        border-bottom: 2px solid hsl(13, 31%, 94%);
    }
    .card img {
        width: 60px;
        border-radius: 10px;
    }
    h4, h5, p {
        margin: 0px;
    }
    .nameAndDesc {
        display: flex;
        flex-direction: column;
        gap: 10px;
        
    }
    .nameContainer {
        /* width: 80%; */
        text-overflow: ellipsis;
        /* white-space: nowrap; */
        overflow: hidden;
    }
    .name {
        color: hsl(14, 65%, 9%);
        
    }
    .desc {
        display: flex;
        align-items: center;
        gap: 15px
    }
    .quantity {
        color: hsl(14, 86%, 42%);
    }
</style>

