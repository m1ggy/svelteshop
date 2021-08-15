<script>
  import { cartItems, toggle } from '../stores';
  import { clickOutside } from './onclickoutside';

  let localToggle = false;
  let localItems = [];
  let cartQuantity = 0;

  ///subscribe to writables
  toggle.subscribe((value) => (localToggle = value));
  cartItems.subscribe((value) => (localItems = value));

  ///cart quantity
  $: {
    cartQuantity = 0;
    localItems.forEach((item) => (cartQuantity += item.quantity));
  }
  ///local functions

  function handleToggle() {
    toggle.update((prev) => !prev);
  }
  function decrement(item) {
    localItems.forEach((cartitem) => {
      if (cartitem.id === item.id) {
        cartitem.quantity -= 1;
        return cartItems.set(localItems);
      }
    });
    return;
  }
  function increment(item) {
    localItems.forEach((cartitem) => {
      if (cartitem.id === item.id) {
        cartitem.quantity += 1;
        return cartItems.set(localItems);
      }
    });
    return;
  }
  function removeItem(item) {
    localItems.forEach(() => {
      localItems = localItems.filter((curr) => curr.id !== item.id);
      console.log(localItems);
      return cartItems.set(localItems);
    });
    return;
  }

  function checkout() {
    cartItems.set([]);
  }
  function clickedOutside(event) {
    console.log(event);
    toggle.update((e) => (e = false));
  }
</script>

<nav>
  <h1>SvelteShop</h1>
  <ul>
    <li>
      <h4 on:click={handleToggle} class="cart">
        Cart <span class="cartNumber">{cartQuantity}</span>
      </h4>

      <div class={localToggle ? 'openCart' : 'closeCart'}>
        <div
          class="cartItemsContainer"
          use:clickOutside
          on:click_outside={clickedOutside}
        >
          {#if localItems.length}
            <div>
              <h4>Items in Cart</h4>
              {#each localItems as item}
                <p>{item.name}</p>
                <p>Quantity: {item.quantity}</p>
                {#if item.quantity >= 2}
                  <button on:click={() => decrement(item)}>-</button>
                {/if}
                <button on:click={() => increment(item)}>+</button>
                <button on:click={() => removeItem(item)}>remove</button>
              {/each}
              <div class="checkoutContainer">
                <button on:click={checkout}>Checkout</button>
              </div>
            </div>
          {:else}
            <div>
              <h4>Items in Cart</h4>

              <p>Cart is Empty</p>
            </div>
          {/if}
        </div>
      </div>
    </li>
  </ul>
</nav>

<style>
  nav {
    width: 100%;
    display: flex;
    flex-direction: row;
    background-color: aliceblue;
    justify-content: space-evenly;
    overflow-x: hidden;
    margin: 0;
    padding: 0;
    position: relative;
    z-index: 1;
    height: 15vh;
    overflow: visible;
  }
  ul {
    list-style: none;
  }
  .checkoutContainer {
    padding: 50px;
  }
  .cartNumber {
    background-color: red;
    border-radius: 25%;
    color: white;
  }
  .cart {
    cursor: pointer;
    user-select: none;
  }
  .cartItemsContainer {
    position: absolute;
    background-color: #1e3f20;
    color: white;
    min-width: 500px;
    text-align: center;
    height: max-content;
    max-height: 50vh;
    overflow-y: auto;
  }
  .closeCart {
    animation: forwards fadeout 0.2s;
  }
  .openCart {
    animation: forwards popup 0.2s;
  }

  @keyframes popup {
    from {
      transform: scale(0);
    }
    to {
      transform: scale(1);
    }
  }
  @keyframes fadeout {
    from {
      transform: scale(1);
      opacity: 1;
    }
    to {
      transform: scale(0);
      opacity: 0;
    }
  }
</style>
