<template>
    <div class="container my-5">
        <div class="row justify-content-center">
            <div class="col-8"> 
                <router-link 
                    :to="{ name: 'transaction.create' }" 
                    class="btn btn-primary btn-sm mb-3"
                    >Add
                </router-link>
                <div class="card">
                    <div class="card-header">
                        Transaction List
                    </div>
                    <div class="card-body">
                        <table class="table">
                            <thead>
                                <th>Title</th>
                                <th>Amount</th>
                                <th>Type</th>
                                <th>Action</th>
                            </thead>
                            <tbody>
                                <tr v-for="(transaction, index) in transactions.data" :key="index">
                                    <td>{{ transaction.title }}</td>
                                    <td>{{ transaction.amount }}</td>
                                    <td>{{ transaction.type }}</td>
                                    <td>
                                        <div class="btn-group">
                                            <router-link 
                                            :to="{ name: 'transaction.edit', params:{id: transaction.id}}"
                                            class="btn btn-sm btn-outline-info"
                                            >Edit
                                            </router-link>
                                            <button 
                                                class="btn btn-sm btn-outline-danger" 
                                                @click.prevent="destroy(transaction.id, index)"
                                                >Delete
                                            </button>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

import axios from 'axios'
import { onMounted, ref } from 'vue'

export default {
    setup() {
        // reactive state
        let transactions = ref([])

        onMounted(() => {
            // get data from api endpoint
            axios.get('http://akunt-api.test/api/transaction')
            .then((result) => {
                transactions.value = result.data
            }).catch((err) => {
                console.log(err.response)
            })
        })

        function destroy(id, index) {
            axios.delete(
                `http://akunt-api.test/api/transaction/${id}`
            )
            .then(() => {
                transactions.value.data.splice(index, 1)
            }).catch((err) => {
                console.log(err.response.data)
            })
        }

        return {
            transactions,
            destroy
        }
    }
}

</script>