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
import axios from "axios/dist/axios";
import AppItemList from "./AppItemList";

export default {
  name: "app",
  components: { AppItemList },
  data() {
    return {
      prefixes: [],
      sufixes: []
    };
  },
  methods: {
    addPrefix(prefix) {
      axios({
        url: "http://localhost:4000",
        method: "post",
        data: {
          query: `
                mutation ($item: ItemInput) {
                    newPrefix: saveItem(item: $item){
                        id
                        type
                        description
                    }
                }
            `,
          variables: {
            item: {
              type: "prefix",
              description: prefix
            }
          }
        }
      }).then(response => {
        const query = responde.data;
        const newPrefix = query.data.newPrefix;
        this.prefixes.push(newPrefix.description);
      });
    },
    addSufix(sufix) {
      this.sufixes.push(sufix);
    },
    deletePrefix(prefix) {
	axios({
		url: "http://localhost:4000",
		method: "post",
		data: {
			query: `
				mutation{
					deleteItem
				}
			`
		}
		}).then();
    },
    deleteSufix(sufix) {
      this.sufixes.splice(this.sufixes.indexOf(sufix), 1);
    },
    getPrefixes() {
      axios({
        url: "http://localhost:4000",
        method: "post",
        data: {
          query: `
					{
						prefixes: items (type: "prefix") {
						id
						type
						description
						}
					}
				`
        }
      }).then(response => {
        const query = response.data;
        this.prefixes = query.data.prefixes;
      });
    },
    getSuffixes() {
      axios({
        url: "http://localhost:4000",
        method: "post",
        data: {
          query: `
					{
						sufixes: items (type: "sufix") {
							description
						}
					}
				`
        }
      }).then(response => {
        const query = response.data;
        this.sufixes = query.data.sufixes;
      });
    }
  },
  computed: {
    domains() {
      const domains = [];
      for (const prefix of this.prefixes) {
        for (const sufix of this.sufixes) {
          domains.push(prefix.description + sufix.description);
        }
      }
      return domains;
    }
  },
  created() {
    this.getPrefixes();
    this.getSuffixes();
  }
};
</script>

<style>
</style>
