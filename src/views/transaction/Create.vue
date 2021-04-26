<template>
  <div class="container my-5">
    <div class="row justify-content-center">
      <div class="col-8">
        <router-link
          :to="{ name: 'transaction.index' }"
          class="btn btn-sm btn-primary rounded mb-3 shadow"
          >Back</router-link
        >

        <div class="card rounded shadow">
          <div class="card-header">Create Transaction</div>
          <div class="card-body">
            <form @submit.prevent="store()">
              <div class="mb-3">
                <label for="" class="form-lable">Title</label>
                <input
                  type="text"
                  class="form-control"
                  v-model="transaction.title"
                />
                <div class="text-danger" v-if="validation.title">
                  {{ validation.title[0] }}
                </div>
              </div>
              <div class="mb-3">
                <label for="" class="form-lable">Amount</label>
                <input
                  type="number"
                  class="form-control"
                  v-model="transaction.amount"
                />
                <div class="text-danger" v-if="validation.amount">
                  {{ validation.amount[0] }}
                </div>
              </div>
              <div class="mb-3">
                <label for="" class="form-lable">Time</label>
                <input
                  type="date"
                  class="form-control"
                  placeholder="yyyy-mm-dd hh:mm:ss"
                  v-model="transaction.time"
                />
                <div class="text-danger" v-if="validation.time">
                  {{ validation.time[0] }}
                </div>
              </div>
              <div class="mb-3">
                <label for="" class="form-lable">Type</label>
                <select
                  class="form-select"
                  name=""
                  id=""
                  v-model="transaction.type"
                >
                  <option value="" selected disabled>Pilih Type</option>
                  <option value="expense">Expense</option>
                  <option value="revenue">Revenue</option>
                </select>
                <div class="text-danger" v-if="validation.type">
                  {{ validation.type[0] }}
                </div>
              </div>
              <button class="btn btn-outline-primary">Save</button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive, ref } from "vue";
import { useRouter } from "vue-router";
import axios from "axios";
export default {
  setup() {
    //data binding
    const transaction = reactive({
      title: "",
      amount: "",
      time: "",
      type: "",
    });

    const validation = ref([]);

    const router = useRouter();

    function store() {
      axios
        .post("http://127.0.0.1:8001/api/transaction", transaction)
        .then(() => {
          router.push({
            name: "transaction.index",
          });
        })
        .catch((err) => {
          validation.value = err.response.data;
        });
    }

    return {
      transaction,
      validation,
      router,
      store,
    };
  },
};
</script>

<style></style>
