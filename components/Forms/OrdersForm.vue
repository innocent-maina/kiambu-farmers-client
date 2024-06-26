<script setup lang="ts">
const userType = useCookie("user_type");

const userId = useCookie<string | undefined>("user_id");

const userFullName = useCookie<string | undefined>("full_name");

const { isEditingOrders, ordersFormState, updateSingleOrder, createOrder } =
  useOrders();

const saveOrder = async () => {
  if (isEditingOrders.value) {
    await updateSingleOrder(ordersFormState.value.id);
  } else {
    await createOrder();
  }
};

const paymentMethods = [
  {
    title: 'Cash deposit',
    value: 'deposit'
  },
  {
    title: 'Bank transfer',
    value: 'bank_transfer'
  }
  ,
  {
    title: 'Mobile money',
    value: 'mobile_money'
  }
]

const paymentStatus = [
  {
    title: "Paid",
    value: "paid",
  },
  {
    title: "Pending",
    value: "pending",
  },
];

const allUsers = ref<any>();

const usersResponse = await useApi<IGetAllUsers>("/user", {
  method: "GET",
});
allUsers.value = usersResponse?.data;

const formattedBuyers = ref<any>([]);
for (let i = 0; i < allUsers.value.length; i++) {
  const farmer = allUsers.value[i];
  formattedBuyers.value.push({
    title: farmer.full_name,
    value: farmer.id,
  });
}

const formattedSellers = ref<any>([]);
for (let i = 0; i < allUsers.value.length; i++) {
  const farmer = allUsers.value[i];
  formattedSellers.value.push({
    title: farmer.full_name,
    value: farmer.id,
  });
}

const allProducts = ref<any>();

const productsResponse = await useApi<IGetAllProducts>("/products", {
  method: "GET",
});
allProducts.value = productsResponse?.data;

const formattedProducts = ref<any>([]);
for (let i = 0; i < allProducts.value.length; i++) {
  const farmer = allProducts.value[i];
  formattedProducts.value.push({
    title: farmer.name,
    value: farmer.id,
  });
}

if (userType.value === 'farmer') {
  // Clear the array and add the single farmer object
  formattedSellers.value = [{
    title: userFullName.value,
    value: userId.value,
  }];
}
</script>

<template>
  <div>
    <v-row>
      <v-col cols="12" md="6" xs="12">
        <v-label class="font-weight-bold mb-1">Buyer</v-label>
        <v-autocomplete
          v-model="ordersFormState.buyer_id"
          outlined
          hide-details
          color="primary"
          :items="formattedBuyers"
        >
        </v-autocomplete>
      </v-col>
      <v-col cols="12" md="6" xs="12">
        <v-label class="font-weight-bold mb-1">Seller</v-label>
        <v-autocomplete
          v-model="ordersFormState.seller_id"
          outlined
          hide-details
          color="primary"
          :items="formattedSellers"
        >
        </v-autocomplete>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="6">
        <v-label class="font-weight-bold mb-1">Product name</v-label>
        <v-autocomplete
          v-model="ordersFormState.product_id"
          outlined
          hide-details
          color="primary"
          :items="formattedProducts"
        >
        </v-autocomplete>
      </v-col>
      <v-col cols="6">
        <v-label class="font-weight-bold mb-1">Quantity</v-label>
        <v-text-field
          v-model="ordersFormState.quantity"
          variant="outlined"
          hide-details
          color="primary"
        ></v-text-field>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="6">
        <v-label class="font-weight-bold mb-1">Total Price</v-label>
        <v-text-field
          v-model="ordersFormState.total_price"
          variant="outlined"
          hide-details
          color="primary"
        ></v-text-field>
      </v-col>
      <v-col cols="6">
        <v-label class="font-weight-bold mb-1">Status</v-label>
        <v-autocomplete
          v-model="ordersFormState.status"
          outlined
          hide-details
          color="primary"
          :items="paymentStatus"
        >
        </v-autocomplete>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="6">
        <v-label class="font-weight-bold mb-1">Payment Method</v-label>
        <v-autocomplete
          v-model="ordersFormState.payment_method"
          outlined
          hide-details
          color="primary"
          :items="paymentMethods"
        >
        </v-autocomplete>
      </v-col>
      <v-col cols="6">
        <v-label class="font-weight-bold mb-1">Transaction ID</v-label>
        <v-text-field
          v-model="ordersFormState.payment_transaction_id"
          variant="outlined"
          hide-details
          color="primary"
        ></v-text-field>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="6">
        <v-label class="font-weight-bold mb-1">Delivery address</v-label>
        <v-text-field
          v-model="ordersFormState.delivery_address"
          variant="outlined"
          hide-details
          color="primary"
        ></v-text-field>
      </v-col>
      <v-col cols="6">
        <v-label class="font-weight-bold mb-1">Tracking number</v-label>
        <v-text-field
          v-model="ordersFormState.tracking_number"
          variant="outlined"
          hide-details
          color="primary"
        ></v-text-field>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12">
        <v-label class="font-weight-bold mb-1">Comments</v-label>
        <v-textarea
          v-model="ordersFormState.comments"
          variant="outlined"
          hide-details
          color="primary"
        ></v-textarea>
      </v-col>
    </v-row>

    <v-row class="py-12 px-6" justify="end">
      <v-btn color="secondary" @click="saveOrder()">
        {{ isEditingOrders ? "Update" : "Create" }}
      </v-btn>
    </v-row>
  </div>
</template>
