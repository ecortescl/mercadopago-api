<template>
<section>
    <div class="container">
        <div class="row justify-content-md-center">
            <div class="col-lg-3"><img src="https://help.turitop.com/hc/article_attachments/4402778287890/logo-mercadopago29.png" width="100%" /></div>

            <div class="col-lg-12">
                <hr>
            </div>
            <div class="col-lg-12">
              PUBLIC KEY
                <input type="text" class="form-control w-100" v-model="key" >
                 <hr>
            </div>

            <div class="col-lg-8">
              <div class="card" >
              <div class="row">
               <div class="col-lg-6"><img class="w-100" src="https://assets.adidas.com/images/h_840,f_auto,q_auto,fl_lossy,c_fill,g_auto/06fe49d32c8f48f99429adf20157e704_9366/Camiseta_Local_Seleccion_Chilena_Rojo_GA8930_01_laydown.jpg" >
               <hr>
             <h5>{{title}}</h5> 
             <p>{{description}}</p>  
             Cantidad: {{quantity}} 
             Precio: ${{price}} 

                </div>
               <div class="col-lg-6">
                <div class="card-body">
                
                   <input type="text" v-model="title" class="form-control" value="10000">
                    <input type="number" v-model="price" class="form-control" value="10000">
                    <input type="number" v-model="quantity" class="form-control" placeholder="Cantidad">
                    <textarea class="form-control" v-model="description" id="" cols="30" rows="10"></textarea>
                  
              
                <button v-on:click="crearOrder" class="btn btn-danger btn-lg w-100 mt-4"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-credit-card" viewBox="0 0 16 16">
                        <path d="M0 4a2 2 0 0 1 2-2h12a2 2 0 0 1 2 2v8a2 2 0 0 1-2 2H2a2 2 0 0 1-2-2V4zm2-1a1 1 0 0 0-1 1v1h14V4a1 1 0 0 0-1-1H2zm13 4H1v5a1 1 0 0 0 1 1h12a1 1 0 0 0 1-1V7z" />
                        <path d="M2 10a1 1 0 0 1 1-1h1a1 1 0 0 1 1 1v1a1 1 0 0 1-1 1H3a1 1 0 0 1-1-1v-1z" />
                    </svg> Pagar</button>
                    </div>
                    </div>
                </div>
              </div>


            </div>
        </div>

    </div>
</section>
</template>

<script>
import axios from "axios";
export default {
  data(){
    return{
      key: 'Bearer TEST-7167826183432752-030315-c7dcbaade969a223cb2e6c0622e653ca-282365244',
      price: 10000,
      quantity: 1,
      description: 'Es muy bonita comprela', 
      title: 'Polera de la Seleccion Chilena',
    }

  },
    methods: {
        crearOrder() {
            var data = JSON.stringify({
                items: [{
                    title: this.title,
                    description: this.description,
                    picture_url: "https://assets.adidas.com/images/h_840,f_auto,q_auto,fl_lossy,c_fill,g_auto/06fe49d32c8f48f99429adf20157e704_9366/Camiseta_Local_Seleccion_Chilena_Rojo_GA8930_01_laydown.jpg",
                    quantity: this.quantity,
                    currency_id: "CLP",
                    unit_price: Number(this.price)
                }, ],
                payer: {
                    phone: {},
                    identification: {},
                    address: {},
                },
                payment_methods: {
                    excluded_payment_methods: [{}],
                    excluded_payment_types: [{}],
                },
                shipments: {
                    free_methods: [{}],
                    receiver_address: {},
                },
                back_urls: {
                  success:'http://localhost:58743/exito',
                  pending:'http://localhost:58743/pendiente',
                  failure: 'http://localhost:58743/fallo',
                },
                differential_pricing: {},
                metadata: {},
            });

            var config = {
                method: "post",
                url: "https://api.mercadopago.com/checkout/preferences",
                headers: {
                    Authorization: this.key,
                    "Content-Type": "application/json",
                },
                data: data,
            };

            axios(config)
                .then(function (response) {
                    console.log(JSON.stringify(response.data));
                    window.location.replace(response.data.sandbox_init_point);
                })
                .catch(function (error) {
                    console.log(error);
                });
        },
    },
};
</script>

<style>
.bg-mercado {
    background: #01B5F0;
}
</style>
