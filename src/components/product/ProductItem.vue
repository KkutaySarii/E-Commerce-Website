<template>
  <div
    class="mb-3 col-sm-6 col-md-6 item"
    :class="{ 'list-group-item': displayList }"
  >
    <div class="thumbnail card">
      <div class="img-event intrinsic">
        <img
          :src="item.thumbnail_url"
          alt=""
          class="grow thumbnail-image card-img-top intrinsic-item p-3"
        />
      </div>
      <div class="card-body">
        <router-link :to="'/product/' + item.id" tag="h5" class="card-title"
          ><a id="item-title">{{ item.title }}</a></router-link
        >
        <h6
          v-if="item.quantity < 1000 && item.quantity > 0"
          class="card-subtitle mb-2 remain"
        >
          Son {{ item.quantity }} paket kaldı!
        </h6>

        <p class="card-text truncate">
          {{ item.description | shortDescription }}
        </p>

        <div class="row">
          <p class="col-6 lead">${{ item.price }}</p>
          <p class="col-6">
            <button
              class="btn btn-success pull-right"
              :disabled="item.quantity === 0"
              @click="addItem"
            >
              Sepete Ekle
            </button>
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapActions } from "vuex";
export default {
  props: ["item", "displayList"],
  methods: {
    ...mapActions(["updateCart", "addMessage"]),
    addItem() {
      if (localStorage.getItem("user_id") == null) {
        this.$router.push("/login");
      } else {
        const order = {
          item: Object.assign({}, this.item),
          quantity: 1,
          isAdd: true,
        };

        // console.log("Berkay");
        // console.log(this.item.quantity);
        this.updateCart(order);
      }
    },
  },
  filters: {
    shortDescription(value) {
      if (value && value.length > 100) {
        return value.substring(0, 100) + "...";
      } else {
        return value;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
#item-title{
  font-family: "Open Sans", sans-serif!important;
}
#item-title:hover{
  font-family: "Open Sans", sans-serif!important;
  color:#EFB631;
  text-decoration: none!important;
}
div.card {
  height: 100%;
}

.card-text {
  font-size: 0.875rem;
}

.remain {
  color: #b30118;
}

.grow {
  transition: all 0.2s ease-in-out;
}

.grow:hover {
  transform: scale(1.1);
}

.list-group-item {
  float: none;
  width: 100%;
  background-color: #fff;
  margin-bottom: 30px;
  -ms-flex: 0 0 100%;
  flex: 0 0 100%;
  max-width: 100%;
  padding: 0 1rem;
  border: 0;
  .thumbnail {
    display: inline-block;
    width: 100%;
  }
  .img-event {
    width: 20%;
    float: left;
    padding: 0 !important;
    margin: 0;
    height: auto;
  }

  .thumbnail-image {
    position: static;
  }

  .card-body {
    float: left;
    width: 80%;
    margin: 0;
  }

  @media (max-width: 767.98px) {
    .img-event {
      width: 30%;
      float: left;
      padding: 0 !important;
      margin: 0;
    }

    .card-body {
      float: left;
      width: 70%;
      margin: 0;
    }
  }
}

.item.list-group-item:before,
.item.list-group-item:after {
  display: table;
  content: " ";
}

.item.list-group-item:after {
  clear: both;
}
.btn-success {
  color: #fff;
  background-color: #091239;
  border-color: #091239;
}
.btn-success:hover{
  background-color: #EFB631;
  border-color: #EFB631;
}
</style>

