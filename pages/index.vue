<template>
  <v-row justify="center" align="center">
    <v-col cols="12">
      <v-data-table
        :headers="headers"
        :items="customers"
        :items-per-page="10"
        class="elevation-1"
        :loading="loading"
        loading-text="Loading... Please wait"
      >
        <template v-slot:item.profilePicture="{ item }">
          <v-badge
            color="purple"
            overlap
            content="VIP"
            offset-x="10"
            offset-y="40"
            bordered
            v-if="item.isVip == true"
          >
            <v-avatar size="60">
              <v-img
                lazy-src="https://picsum.photos/id/11/10/6"
                :src="item.profilePicture"
              ></v-img>
            </v-avatar>
          </v-badge>
          <div v-else>
            <v-avatar size="60">
              <v-img
                lazy-src="https://picsum.photos/id/11/10/6"
                :src="item.profilePicture"
              ></v-img>
            </v-avatar>
          </div>
        </template>
        <template v-slot:item.actions="{ item }">
          <v-btn color="purple" @click="getCustomerDetails(item)">View</v-btn>
        </template>
      </v-data-table>

      <div class="text-center">
        <v-dialog v-model="customerDialog" width="500">
          <v-card>
            <v-card-title class="text-h5 primary">
              <v-icon>mdi-information</v-icon>

              Customer Information
              <v-spacer></v-spacer>
              <v-btn text color="white"  @click="customerDialog = false"><v-icon>mdi-close</v-icon></v-btn>
              
            </v-card-title>
             <v-divider></v-divider>

            <v-spacer></v-spacer>

            <v-card-text class="mt-5">
              <v-row>

                <v-col cols="12" md="4">
                  <v-img
  max-height="250"
  max-width="250"
  src="https://picsum.photos/id/11/500/300"
></v-img>
                </v-col>

                <v-col cols="12" md="8">
                  <v-list-item two-line>
              <v-list-item-content>
        <v-list-item-title>Name</v-list-item-title>
        <v-list-item-subtitle>{{ customerName }}</v-list-item-subtitle>
      </v-list-item-content>
    </v-list-item>

    <v-list-item two-line>
      <v-list-item-content>
        <v-list-item-title>Email</v-list-item-title>
        <v-list-item-subtitle>{{customerEmail}}</v-list-item-subtitle>
      </v-list-item-content>
    </v-list-item>

    <v-list-item two-line>
      <v-list-item-content>
        <v-list-item-title>Phone #</v-list-item-title>
        <v-list-item-subtitle>{{customerPhone}}</v-list-item-subtitle>
      </v-list-item-content>
    </v-list-item>

    <v-list-item two-line>
      <v-list-item-content>
        <v-list-item-title>Age</v-list-item-title>
        <v-list-item-subtitle>{{customerAge}}</v-list-item-subtitle>
      </v-list-item-content>
    </v-list-item>
    
                </v-col>

              </v-row>
              
            </v-card-text>

            <v-divider></v-divider>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="green">
                Update
              </v-btn>
              <v-btn color="red">
                Delete
              </v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </div>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: "IndexPage",
  data() {
    return {
      headers: [
        // {
        //   text: 'ID',
        //   align: 'start',
        //   sortable: false,
        //   value: 'id',
        // },
        { text: "Photo", value: "profilePicture" },
        { text: "Name", value: "name" },
        { text: "Email", value: "email" },
        { text: "Phone", value: "phone" },
        { text: "Address", value: "address" },
        { text: "Age", value: "age" },
        { text: "Status", value: "isVip" },
        { text: "", value: "actions" },
      ],
      customers: [],
      customerName:"",
      customerEmail:"",
      customerPhone:"",
      customerAge:"",
      customerDialog: false,
      loading: true,
    };
  },
  methods: {
    getCustomers() {
      this.$axios
        .get("/customers")
        .then((response) => {
          console.log(response.data);
          this.customers = response.data;
          this.loading = false;
        })
        .catch((err) => {
          console.log(err);
        });
    },

    getCustomerDetails(item) {
      console.log(item);
      this.customerName = item.name;
      this.customerEmail = item.email;
      this.customerPhone = item.phone;
      this.customerAge = item.age;
      this.customerDialog = true;
    },
  },

  mounted() {
    this.getCustomers();
  },
};
</script>
