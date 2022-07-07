<template>
  <div>
    <b-table id="my-table" striped hover :fields="fields" :items="sales" :per-page="perPage" :current-page="currentPage">
    <template #cell(actions)="row">
        <b-button size="sm" @click="info(row.products, row.index, $event.target)" class="mr-1" id="show-button">
          Mostrar Productos
        </b-button>
    </template>
    </b-table>
    <b-pagination
      v-model="currentPage"
      :total-rows="rows"
      :per-page="perPage"
      aria-controls="my-table"
      id="pagination-pages"
    ></b-pagination>

    <b-modal :id="infoModal.id" :title="infoModal.title" ok-only @hide="resetInfoModal" class="product-modal">
        <div>
            <div v-for="product in products" :key="product.index" class="products">
                <div class="product">
                    {{ product.name }} - {{ product.price }} - {{ product.quantity }} 
                </div>
            </div>
        </div>
      <template #modal-footer>
        <div class="w-100">
          <p class="float-left"> Total venta {{ total }}</p>

        </div>
      </template>

    </b-modal>
  </div>

</template>

<script>
  export default {
    name: 'OrderTable',
    props:
    {
        perPage: {
            type: Number,
            default: 10
        },
        sales: Array
    },
    data() {
        return {
            fields: [
                {
                    key: 'date_closed',
                    label: 'Fecha',
                    sortable: true
                },
                {
                    key: 'total',
                    label: 'Total',
                    sortable: true
                },
                {
                    key: 'id',
                    label: 'ID'
                },
                { key: 'actions', 
                label: 'Mostrar' }
                
            ],
            currentPage: 1,
            infoModal: {
                id: 'info-modal',
                title: '',
                content: ''
            },
            products: [],
            total: 0
        }
        
    },   
    methods: {
      info(item, index, button) {
        this.infoModal.title = `Productos de la venta ${this.sales[index].id}`
        this.total = this.sales[index].total
        this.products= this.sales[index].products
        this.$root.$emit('bv::show::modal', this.infoModal.id, button)
      },
      resetInfoModal() {
        this.infoModal.title = ''
        this.infoModal.content = ''
        this.total = 0

      }
    },
    computed: {
      rows() {
        return this.sales.length
      }
    }
}
</script>

<style scoped>
#show-button {
  background: gray;
}
.product{
    margin:5%;
}

</style>