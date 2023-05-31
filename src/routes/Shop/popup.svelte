<script>
    import { getContext } from 'svelte'

const variable = getContext('my-var')

</script>

<h1>Checkout:</h1>
<p>The amount is {$variable}</p>
<p>Here is the amount you have to pay, thank you for playing and shopping on my webbshop!</p>
<p>Hope you enjoy and keep playing for more amazing things to buy!</p>
<button>Pay</button>

<style>
    button {
        color: rgb(233, 235, 196);
        background-color: rgb(98, 41, 41);
    }

</style>