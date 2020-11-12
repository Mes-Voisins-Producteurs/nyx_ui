<template>
  <el-dialog
    width="600px"
    :title="title"
    :visible.sync="dialogFormVisible"
    :before-close="closeDialog"
    :close-on-click-modal="false"
    class="purchase-order-editor"
  >
    <div style="width:100%">
      <el-row>
        <el-card shadow="never">
          <div>
            <span style="font-size:30px;">{{ this.record._source.supplier | capitalizeFirstLetter }}</span>
          </div>
        </el-card>
      </el-row>

      <el-row>
        <el-card shadow="never">
          <div>
            <div>
              <el-row>
                <span>
                Actuellement assigné(s) :
                </span>
              </el-row>
              <!-- DEFAULT PICKER ROW -->
              <!-- <el-row>
                <el-card  shadow="hover" body-style="padding: 0px 1em 0px 1em;">
                  <span style="float: left; min-width: 200px;">Ramasseur par défaut</span>
                  <span>
                    <el-select
                      v-model="defaultPicker"
                      placeholder="Choisir le ramasseur"
                      style="min-width: 300px;"
                      :disabled="isDisabled"
                    >
                      <el-option
                        v-for="user in usersList"
                        :key="user._source.login"
                        :label="
                        user._source.firstname + ' ' + user._source.lastname
                      "
                        :value="user._source.login"
                      ></el-option>
                    </el-select>
                  </span>

                  <el-button-group style="float:right;">
                    <el-tooltip
                      class="item"
                      effect="dark"
                      content="Désaffecter cet utilisateur"
                      placement="top-start"
                    >
                      <el-button
                        v-if="defaultPicker"
                        type="danger"
                        icon="el-icon-delete"
                        size="small"
                        @click="defaultPicker = ''"
                      ></el-button>
                    </el-tooltip>
                  </el-button-group>
                </el-card>
              </el-row> -->

              <!-- CURRENT PICKER ROW -->
              <el-row >
                <el-card 
                shadow="hover" 
                body-style="padding: 10px 1em 5px 1em;">
                  <el-row v-if="!isDisabled">
                    <el-col :span="8" style="text-align:left; margin-top:4px;">
                      <div><b>Ramasseur par défaut</b></div>
                    </el-col>

                    <el-col :span="12">
                      <el-select
                        size="mini"
                        v-model="defaultPicker"
                        placeholder="Choisir le ramasseur"
                        style="width:80%"
                        :disabled="isDisabled"
                      >
                        <el-option
                          v-for="user in usersList"
                          :key="user._source.login"
                          :label="
                        user._source.firstname + ' ' + user._source.lastname
                      "
                          :value="user._source.login"
                        ></el-option>
                      </el-select>
                    </el-col>

                    <el-col :span="4" style="text-align:right">
                      <el-button-group>
                        <el-tooltip
                          class="item"
                          effect="dark"
                          content="Désaffecter cet utilisateur"
                          placement="top-start"
                        >
                          <el-button
                            type="danger"
                            icon="el-icon-delete"
                            size="mini"
                            v-if="defaultPicker"
                            @click="defaultPicker = ''"
                          ></el-button>
                        </el-tooltip>
                      </el-button-group>
                    </el-col>
                  </el-row>
                  <el-row v-else>
                    <el-alert
                      title="Impossible de modifier le ramasseur par défaut pour ce fournisseur"
                      type="error"
                      :closable="false"
                      effect="dark">
                    </el-alert>
                  </el-row>
                </el-card>
              </el-row>
              <el-row >
                <el-card shadow="hover" body-style="padding: 10px 1em 5px 1em;">
                  <el-row>
                    <el-col :span="8" style="text-align:left; margin-top:4px;">
                      <div><b>Assigné à ce bon</b></div>
                    </el-col>

                    <el-col :span="12">
                      <el-select
                        size="mini"
                        v-model="currentPicker"
                        placeholder="Choisir le ramasseur"
                        style="width:80%"
                      >
                        <el-option
                          v-for="user in usersList"
                          :key="user._source.login"
                          :label="
                        user._source.firstname + ' ' + user._source.lastname
                      "
                          :value="user._source.login"
                        ></el-option>
                      </el-select>
                    </el-col>

                    <el-col :span="4" style="text-align:right">
                      <el-button-group>
                        <el-tooltip
                          class="item"
                          effect="dark"
                          content="Désaffecter cet utilisateur"
                          placement="top-start"
                        >
                          <el-button
                            type="danger"
                            icon="el-icon-delete"
                            size="mini"
                            v-if="currentPicker"
                            @click="currentPicker = ''"
                          ></el-button>
                        </el-tooltip>
                      </el-button-group>
                    </el-col>
                  </el-row>
                </el-card>
              </el-row>
            </div>
          </div>
        </el-card>
      </el-row>
    </div>

    <div class="row"></div>
    <span slot="footer" class="dialog-footer">
      <el-button @click="closeDialog">Annuler</el-button>
      <el-button @click="closeAndSaveDialog">Valider</el-button>
    </span>
  </el-dialog>
</template>

<script>
import Vue from "vue";

import YAML from "js-yaml";
import axios from "axios";

export default {
  name: "poEditor",
  data: () => ({
    orgRec: null,
    newRec: null,
    formLabelWidth: "120px",
    changed: false,
    dialogFormVisible: false,
    title: "Assigner un ramasseur",
    currentPicker: "",
    defaultPicker: "",
    supplierPicker: "",
    usersList: []
  }),
  computed: {
    recordin: function() {
      return this.record;
    },
    recordstr: function() {
      return JSON.stringify(this.record);
    },
    recchanged: function() {
      return JSON.stringify(this.recordin) != JSON.stringify(this.newRec);
    },
    isDisabled: function() {
      if (this.record == null) return true;
      if (this.record._source.supplier_id == null) return true;
      return false;
    }
  },
  props: {
    record: {
      type: Object
    },
    config: {
      type: Object
    },
    editMode: {
      type: String
    }
  },
  watch: {
    recordin: {
      handler: function() {},
      deep: true
    }
  },
  created: function() {
    this.dialogFormVisible = true;
  },
  mounted: function() {
    this.init();
  },
  methods: {
    closeDialog: function() {
      this.$emit("dialogclose");
    },
    closeAndSaveDialog: function() {
      /*========== SAVING THE CURRENT PURCHASE ORDER ==========*/
      if (this.record._source.picker != this.currentPicker) {
        this.record._source.picker = this.currentPicker;

        var updatedRecord = {
          _index: this.record._index,
          _source: this.record._source,
          _id: this.record._id
        };

        this.$store.commit({
          type: "updateRecord",
          data: updatedRecord
        });

        this.notifyUser(
          "success",
          "Bon de commande",
          "Enregistrement réussi !"
        );

        this.$emit("dialogcloseupdated");
      }

      /*========== SAVING THE DEFAULT PICKER ==========*/
      if (this.supplierPicker != this.defaultPicker) {
        /*  GET SUPPLIER DATA */
        var urlSupplier =
          this.$store.getters.apiurl +
          "generic/supplier_veeqo/" +
          this.record._source.supplier_id +
          "?token=" +
          this.$store.getters.creds.token;

        axios
          .get(urlSupplier)
          .then(response => {
            var updatedSource = response.data.data._source;
            updatedSource.picker = this.defaultPicker;

            var updatedSupplier = {
              _index: "supplier_veeqo",
              _source: updatedSource,
              _id: this.record._source.supplier_id
            };

            this.$store.commit({
              type: "updateRecord",
              data: updatedSupplier
            });

            this.notifyUser(
              "success",
              "Ramasseur par défaut",
              "Enregistrement réussi !"
            );
          })
          .catch(error => {
            console.log(
              "=============== ERROR #1 : Error while saving default picker"
            );
            this.notifyUser(
              "error",
              "Error #1",
              "un problème est survenu pendant l'enregistrement."
            );
          });
      }
      this.$emit("dialogclose");
    },
    getUsersList: function() {
      var url =
        this.$store.getters.apiurl +
        "generic_search/nyx_user?token=" +
        this.$store.getters.creds.token;

      var usersQuery = {
        size: 500,
        sort: [
          {
            updated_at: {
              order: "desc",
              unmapped_type: "boolean"
            }
          }
        ],
        query: {
          bool: {
            must: [],
            filter: [
              {
                bool: {
                  should: [
                    {
                      match: {
                        privileges: "MVP_Ramasse"
                      }
                    }
                  ],
                  minimum_should_match: 1
                }
              }
            ]
          }
        }
      };

      axios
        .post(url, usersQuery)
        .then(response => {
          this.usersList = response.data.records;
        })
        .catch(error => {
          console.log("=============== ERROR #2 : unable to get users list");
          this.notifyUser(
            "error",
            "Error #2",
            "Echec lors de la récupération la liste des ramasseurs."
          );
        });
    },
    isThereAlreadyADefaultPicker() {
      if (this.record._source.supplier_id == null) {
        this.notifyUser(
          "error",
          "Error #4",
          "Le champ supplier_id n'est défini. Synchronisation impossible. Il sera impossible de sauvegarder le ramasseur par défaut."
        );
        return;
      }
      /*  GET SUPPLIER DATA */
      var urlSupplier =
        this.$store.getters.apiurl +
        "generic/supplier_veeqo/" +
        this.record._source.supplier_id +
        "?token=" +
        this.$store.getters.creds.token;

      axios
        .get(urlSupplier)
        .then(response => {
          if (
            response.data.data._source.picker != null &&
            response.data.data._source.picker != ""
          ) {
            this.defaultPicker = response.data.data._source.picker;
            this.supplierPicker = response.data.data._source.picker;
          } else {
            this.defaultPicker = "";
            this.supplierPicker = "";
          }
        })
        .catch(error => {
          console.log(
            "=============== ERROR #3 : unable to retrieve the default picker"
          );
          this.notifyUser(
            "error",
            "Error #3",
            "Lecture du ramasseur par défaut impossible."
          );
        });
    },
    getPoPicker() {
      if (
        this.record._source.picker != null &&
        this.record._source.picker != ""
      )
        this.currentPicker = this.record._source.picker;
      else {
        this.record._source.picker = "";
      }
    },
    init() {
      this.getUsersList();
      this.getPoPicker();
      this.isThereAlreadyADefaultPicker();
    },
    notifyUser(m_type, m_title, m_message) {
      this.$notify({
        title: m_title,
        message: m_message,
        type: m_type
      });
    }
  }
};
</script>

<style>
/* .box-card {
  min-width: 100%;
  text-align: center;
  margin-bottom: 20px;
}

.box-title {
  font-size: 30px;
}
.picker-card {
  padding: 10px 1em 10px 1em;
  margin-top: 5px;
  font-size: 20px;
  background-color: #d9ecff !important;
} */
</style>
