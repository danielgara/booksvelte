<script>
    let products = [
        {
            id: "1",
            name: "airpods"
        },
        {
            id: "2",
            name: "kindle"
        },
        {
            id: "3",
            name: "keyboard"
        }                
    ]
</script>

<main>
	<h1>Products</h1>
    {#each products as product (product.id)}	
    	<h3>{product.id}: {product.name}</h3>
    {/each}
</main>