<script>
  import { cartItems } from '../stores';
  let items = [...Array(20).keys()];
  items = [
    ...items.map((num) => {
      return { name: `Item ${num + 1}`, id: num + 1 };
    }),
  ];
  let cart_items = [];

  cartItems.subscribe((cart) => (cart_items = cart));

  function addToCart(item) {
    if (cart_items.filter((cartitem) => cartitem.id === item.id).length) {
      cart_items.forEach((cartitem) => {
        if (cartitem.id === item.id) {
          cartitem.quantity += 1;
          return cartItems.set(cart_items);
        }
      });
      return;
    }
    return cartItems.update((old) => [...old, { ...item, quantity: 1 }]);
  }
</script>

<div class="container">
  {#each items as item}
    <div class="item">
      <h5>{item.name}</h5>
      <button on:click={() => addToCart(item)}>add to cart</button>
    </div>
  {/each}
</div>

<style>
  .container {
    display: grid;
    width: 100%;
    grid-gap: 15px;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    grid-auto-rows: 200px;
    grid-auto-flow: dense;
    margin: auto;
    justify-items: center;
  }
</style>
