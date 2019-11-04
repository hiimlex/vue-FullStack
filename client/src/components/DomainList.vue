<template>
  <div>
    <div id="main">
      <div class="container">
        <div class="row">
          <div class="col-md">
            <AppItemList
              :items="prefixes"
              @addItem="addPrefix"
              @deleteItem="deletePrefix"
              title="Prefixos"
            ></AppItemList>
          </div>
          <div class="col-md">
            <AppItemList
              :items="sufixes"
              @addItem="addSufix"
              @deleteItem="deleteSufix"
              title="Sufixos"
            ></AppItemList>
          </div>
        </div>
        <br />
        <h5>
          Domains
          <span class="badge badge-info">{{domains.length}}</span>
        </h5>
        <div class="card">
          <div class="card-body">
            <ul class="list-group">
              <li class="list-group-item" v-for="domain in domains" :key="domain">
                <div class="row">
                  <div class="col-md">{{domain}}</div>
                  <div class="col-md text-right">
                    <button class="btn btn-info">
                      <span class="fa fa-shopping-cart"></span>
                    </button>
                  </div>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import "bootstrap/dist/css/bootstrap.css";
import "font-awesome/css/font-awesome.css";
import AppItemList from "./AppItemList";

export default {
  name: "app",
  components: { AppItemList },
  data() {
    return {
      prefixes: ["Air", "Jet", "Flight"],
      sufixes: ["Hub", "Station", "Mart"]
    };
  },
  methods: {
    addPrefix(prefix) {
      this.prefixes.push(prefix);
    },
    addSufix(sufix) {
      this.sufixes.push(sufix);
    },
    deletePrefix(prefix) {
      this.prefixes.splice(this.prefixes.indexOf(prefix), 1);
    },
    deleteSufix(sufix) {
      this.sufixes.splice(this.sufixes.indexOf(sufix), 1);
    }
  },
  computed: {
    domains() {
      const domains = [];
      for (const prefix of this.prefixes) {
        for (const sufix of this.sufixes) {
          domains.push(prefix + sufix);
        }
      }
      return domains;
    }
  }
};
</script>

<style>
</style>
