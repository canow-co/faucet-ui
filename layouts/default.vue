<template>
  <v-app class="vappbackground">
    <v-header id="header">
      <div class="flex_contheader">
        <div class="statusheaderleft">
          <img
            class="mainimglogo"
            src="../assets/img/canow_logo.png"
            alt="logo"
          />
        </div>
        <h1 class="testnetheader">Testnet Faucet</h1>
        <div class="statusheader">
          <div class="desktopstatusdiv">
            <v-select
              class="select_chain mr-3"
              v-model="selected"
              :items="items"
              outlined
              label="Chain ID"
            ></v-select>
            <div class="faucetcompstatus">
              <h4 style="font-weight: 300">
                Faucet Status:
                <span class="statustextcol">
                  <span style="margin-right: 3px">{{ faucet_status }}</span>
                  <v-tooltip top>
                    <template v-slot:activator="{ on, attrs }">
                      <v-icon v-bind="attrs" v-on="on" small :color="faucet_status_color">
                        mdi-circle
                      </v-icon>
                    </template>
                    <span>{{ faucet_status }}</span>
                  </v-tooltip>
                </span>
              </h4>
            </div>
          </div>
          <!--  -->
          <div class="mobilestatusdiv">
            <template>
              <div class="text-center">
                <v-dialog v-model="dialog" width="auto">
                  <template v-slot:activator="{ on, attrs }">
                    <v-btn icon color="black" v-bind="attrs" v-on="on">
                      <v-icon>mdi-information-outline</v-icon>
                    </v-btn>
                  </template>

                  <v-card>
                    <v-card-title class="text-h5 lighten-2"> </v-card-title>

                    <v-card-text class="mt-2">
                      <div class="faucetcompstatus">
                        <v-select
                          class="select_chain mr-3"
                          v-model="selected"
                          :items="items"
                          outlined
                          label="Chain ID"
                        ></v-select>
                        <h4 style="font-weight: 300">
                          Faucet Status:
                          <span class="statustextcol">
                            <span style="margin-right: 3px">{{
                              faucet_status
                            }}</span>
                            <v-tooltip top>
                              <template v-slot:activator="{ on, attrs }">
                                <v-icon
                                  v-bind="attrs"
                                  v-on="on"
                                  small
                                  :color="faucet_status_color"
                                >
                                  mdi-circle
                                </v-icon>
                              </template>
                              <span>{{ faucet_status }}</span>
                            </v-tooltip>
                          </span>
                        </h4>
                      </div>
                    </v-card-text>

                    <v-divider></v-divider>

                    <v-card-actions>
                      <v-spacer></v-spacer>
                      <v-btn color="primary" text @click="dialog = false">
                        OK
                      </v-btn>
                    </v-card-actions>
                  </v-card>
                </v-dialog>
              </div>
            </template>
          </div>
          <!--  -->
        </div>
      </div>
    </v-header>
    <v-main>
      <v-container class="maincontainer">
        <Nuxt />
      </v-container>
    </v-main>
    <v-footer class="pagefooter" relative>
      <p>&copy; {{ new Date().getFullYear() }} Canow</p>
    </v-footer>
  </v-app>
</template>

<script>
import { CHEQD_FAUCET_SERVER } from "../constants/constants";

export default {
  name: "DefaultLayout",
  data() {
    return {
      fixed: false,
      title: "cheqd Testnet faucet",
      faucet_status: "",
      faucet_status_color: "green",
      items: ["cheqd-testnet-6"],
      selected: "cheqd-testnet-6",
      dialog: false,
    };
  },

  async mounted() {
    await this.handle_interval(this.handle_faucet_status(), 30000);
  },
  methods: {
    async handle_faucet_status() {
      try {
        const status = await this.$axios.get(`${CHEQD_FAUCET_SERVER}/status`);
        if (status.data.status === "ok") {
          this.faucet_status_color = "green";
          this.faucet_status = "Operational";
          return;
        }
      } catch (error) {
        this.faucet_status_color = "red";
        this.faucet_status = "Down";
        return;
      }

      this.faucet_status_color = "red";
      this.faucet_status = "Down";
      return;
    },

    async handle_interval(promise, interval) {
      while (true) {
        await new Promise((resolve) => setTimeout(resolve, interval));
        await promise;
      }
    },
  },
};
</script>
<style scoped>
.vappbackground {
  font-family: "Inter", sans-serif;
  background: #fff;
}
.maincontainer {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100%;
  margin: auto;
}

.maincontainer > div {
  width: 600px;
}
.statustextcol {
  display: inline-flex;
  align-items: center;
}
.statustextcol i {
  margin-top: -1px;
}
/*  */
#header {
  color: #101828;
  width: 100%;
  display: block;
  background: #fff;
  padding: 10px;
  z-index: 100;
  border-bottom: 1px solid #ccc;
}
.flex_contheader {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
}
.statusheader {
  width: 250px;
}
.mainimglogo {
  width: 200px;
  display: block;
}
/*  */
.pagefooter {
  width: 100%;
  display: block;
  background: #fff;
  color: #101828;
  text-align: center;
  padding: 10px;
}

.pagefooter p {
  margin: auto;
}
.social-links {
  width: 100%;
  align-items: center;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
.social-links__icon {
  background-color: transparent;
  border-radius: 4px;
  display: inline-block;
  margin: 0 0.7rem 0.35rem 0;
  padding: 0.7rem 0.7rem 0.2rem 0.7rem;
  cursor: pointer;
}
.social-links svg {
  fill: rgba(255, 255, 255, 1);
  width: 40px;
  height: 40px;
  transition: fill 0.2s ease-in-out;
}
.social-links svg:hover {
  fill: #0689ff;
}
.sociallinks {
  font-weight: 300;
  text-decoration: underline;
  font-family: Nunito, sans-serif;
  font-style: normal;
  color: #00c6ab;
}
.select_chain {
  max-width: 225px;
}
.faucetcompstatus {
  margin-top: -15px;
}
.mobilestatusdiv {
  display: none;
}

.testnetheader {
  font-size: 24px;
  font-weight: 500;
}

@media only screen and (max-width: 992px) {
  .maincontainer > div {
    width: 80%;
  }

  .mainimglogo {
    width: 120px;
    margin: 0 auto;
  }
  .statusheader {
    width: auto;
  }
  .statusheaderleft {
    width: auto;
  }
  .desktopstatusdiv {
    display: none;
  }
  .mobilestatusdiv {
    display: block;
    width: 100px;
  }
}
@media only screen and (max-width: 756px) {
  .mainimglogo {
    width: 100px;
    margin: 0 auto;
  }
  .testnetheader {
    font-size: 30px;
  }
  .mobilestatusdiv {
    display: block;
  }
}
@media only screen and (max-width: 576px) {
  .mobilestatusdiv {
    display: block;
    width: 60px;
  }
  .mainimglogo {
    width: 80px;
    margin: 0;
  }
  .testnetheader {
    font-size: 24px;
  }
  .bgdarkopacity {
    width: 100%;
    min-width: auto;
  }
  .social-links__icon {
    margin: 0;
  }
}
@media only screen and (max-width: 480px) {
  .social-links__icon {
    padding: 0.7rem 0.4rem 0.2rem 0.4rem;
  }
  .mobilestatusdiv {
    display: block;
  }
}
</style>
