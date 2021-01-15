<template>
<div class="container">
  <div class="py-5 text-center">
    <h2>Welcome</h2>
    <p class="lead">{{ user.first_name }} {{ user.last_name }} has invited you to buy these item(s).</p>
  </div>

  <div class="row">
    <div class="col-md-4 order-md-2 mb-4">
      <h4 class="d-flex justify-content-between align-items-center mb-3">
        <span class="text-muted">Products</span>
      </h4>
      <ul class="list-group mb-3">
        <template v-for="(product, i) in products">
        <li class="list-group-item d-flex justify-content-between lh-condensed" :key="i">
          <div>
            <h6 class="my-0">{{ product.title }}</h6>
            <small class="text-muted">{{ product.description }}</small>
          </div>
          <span class="text-muted">${{ product.price }}</span>
        </li>
        <li class="list-group-item d-flex justify-content-between lh-condensed" :key="i">
          <div>
            <h6 class="my-0">Quantity</h6>
          </div>
          <input v-model="quantities[product.id]" class="text-muted form-control qty" type="number" min="0">
        </li>
        </template>

        <li class="list-group-item d-flex justify-content-between">
          <span>Total (USD)</span>
          <strong v-if="total">${{ total }}</strong>
          <strong v-else>$0</strong>
        </li>
      </ul>
    </div>
    <div class="col-md-8 order-md-1">
      <h4 class="mb-3">Personal Information</h4>
      <form class="needs-validation" novalidate>
        <div class="row">
          <div class="col-md-6 mb-3">
            <label for="firstName">First name</label>
            <input type="text" class="form-control" id="firstName" placeholder="First Name" required>
          </div>
          <div class="col-md-6 mb-3">
            <label for="lastName">Last name</label>
            <input type="text" class="form-control" id="lastName" placeholder="Last Name" required>
          </div>
        </div>

        <div class="mb-3">
          <label for="username">Username</label>
          <div class="input-group">
            <div class="input-group-prepend">
              <span class="input-group-text">@</span>
            </div>
            <input type="text" class="form-control" id="username" placeholder="Username" required>
            <div class="invalid-feedback" style="width: 100%;">
              Your username is required.
            </div>
          </div>
        </div>

        <div class="mb-3">
          <label for="email">Email</label>
          <input type="email" class="form-control" id="email" placeholder="you@example.com" required>
        </div>

        <div class="mb-3">
          <label for="address">Address</label>
          <input type="text" class="form-control" id="address" placeholder="1234 Main St" required>
        </div>

        <div class="mb-3">
          <label for="address2">Address 2 <span class="text-muted">(Optional)</span></label>
          <input type="text" class="form-control" id="address2" placeholder="Apartment or suite">
        </div>

        <div class="row">
          <div class="col-md-5 mb-3">
            <input type="text" class="form-control" id="country" placeholder="Country">
          </div>
          <div class="col-md-4 mb-3">
            <label for="state">City</label>
            <input type="text" class="form-control" id="city" placeholder="City">
          </div>
          <div class="col-md-3 mb-3">
            <label for="zip">Zip</label>
            <input type="text" class="form-control" id="zip" placeholder="Zip code" required>
          </div>
        </div>
        <button class="btn btn-primary btn-lg btn-block" type="submit">Continue to checkout</button>
      </form>
    </div>
  </div>
</div>
</template>

<script>
export default {
  async asyncData({params, app}) {
    const {data} = await app.$axios.$get(`links/${params.code}`)

    return {
        user: data.user,
        products: data.products
    }
  },
  data: () => ({
    user: null,
    products: [],
    quantities: []
  }),
  mounted() {
    console.log(this.$route.params)
  },
  computed: {
    total() {
        let total = 0;

        this.products.forEach((p) => {
            total += p.price * this.quantities[p.id]
        });

        return total
    }
  }
}
</script>

<style scoped>
.qty {
    width: 65px;
}
</style>
