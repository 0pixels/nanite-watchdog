<template>
    <div class="sm-col-7 mx-auto">
        <p class="justify">
            Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod
            tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At
            vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren,
            no sea takimata sanctus est Lorem ipsum dolor sit amet.
        </p>
        <form @submit.prevent="getAddress">
            <span class="h4 block bold mb2">Wallet Address</span>
            <input type="text" required name="address" id="address" class="mx-auto" style="width: 100%; text-align: center;" />
            <button type="submit" class="block mt2 mx-auto btn blue">Send</button>
        </form>
    </div>
</template>



<script>
export default {
    name: 'Faucet',
    data: () => {
        return {
            address: '',
        };
    },
    methods: {
        getAddress(event) {
            this.address = event.target.elements.address.value;
            switch (this.address) {
                case String(this.address.match(/^xrb\_[A-Za-z0-9]{60}$/)):
                    event.target.elements.address.disabled = true;
                    console.log(`found address: ${this.address}`);
                    break;
                case '':
                    console.log('No address provided.');
                    break;
                default:
                    console.log(`'${this.address}' is not a valid address.`);
                    break;
            }
        },
    },
};
</script>
