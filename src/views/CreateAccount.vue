<template>
  <div id="create">
    <main id="main">
      <div class="row">
        <div
          class="container d-flex flex-column align-items-center"
          data-aos="zoom-down"
        >
          <a href="/" class="btn btn-about">Log in</a>
        </div>
      </div>
      <section id="about" class="about" data-aos="zoom-up" data-aos-delay="100">
        <div class="container" data-aos="fade-up">
          <div class="section-title">
            <h2>Create your account</h2>
          </div>
          <div class="row g-3">
            <!-- profile picture section -->
            <div class="col-lg-4" style="text-align: center">
              <div class="row">
                <div class="col-12 mb-3">
                  <form v-on:submit.prevent="scryfallSearch(scryfallNameField)">
                    <div class="section-title" style="padding: 0px">
                      <h3><strong>Profile Picture</strong></h3>
                    </div>
                    <div>
                      <a :href="cardInfo.scryfall_uri" target="_blank" alt="">
                        {{ cardInfo.name }}
                      </a>
                      <br v-if="!cardInfo.name" />

                      <img
                        :src="inputParams.profile_picture"
                        class="img-fluid centered-element rounded"
                        alt=""
                        id="profile-pic"
                      />
                      <div v-if="cardInfo.name">
                        <span style="font-style: italic"
                          >Artist: {{ cardInfo.artist }}</span
                        >
                      </div>
                      <br v-if="!cardInfo.name" />
                    </div>
                    <br />
                    <div>
                      <strong>Search for your favorite card...</strong>
                      <div class="input-group">
                        <input
                          type="text"
                          v-model="scryfallNameField"
                          class="form-control"
                        />
                        <div class="input-group-append">
                          <button
                            class="btn btn-outline-secondary"
                            data-bs-toggle="modal"
                            data-bs-target="#cardList"
                          >
                            Search
                          </button>
                        </div>
                      </div>
                    </div>
                  </form>
                </div>
              </div>
              <div class="row mb-3">
                <strong>Or choose your...</strong>
                <!-- Color choosing dropdown  -->
                <div class="col-6">
                  <button
                    class="
                      btn btn-outline-secondary
                      centered-element
                      dropdown-toggle
                      float-end
                    "
                    type="button"
                    data-bs-toggle="dropdown"
                    aria-expanded="false"
                  >
                    Favorite Color
                  </button>
                  <ul class="dropdown-menu">
                    <li>
                      <span
                        class="dropdown-item"
                        @click="selectIcon(color)"
                        v-for="color in colors"
                        :key="`color-id-${color.id}`"
                      >
                        {{ color.name }}
                      </span>
                    </li>
                  </ul>
                </div>

                <!-- end Color choosing dropdown  -->
                <!-- guild choosing dropdown  -->
                <div class="col-6">
                  <button
                    class="
                      btn btn-outline-secondary
                      dropdown-toggle
                      float-start
                    "
                    type="button"
                    data-bs-toggle="dropdown"
                    aria-expanded="false"
                  >
                    Ravnica Guild
                  </button>
                  <ul class="dropdown-menu">
                    <li>
                      <a
                        class="dropdown-item"
                        @click="selectIcon(guild)"
                        v-for="guild in guilds"
                        :key="`guild-id-${guild.id}`"
                      >
                        {{ guild.name }}
                      </a>
                    </li>
                  </ul>
                </div>
              </div>
              <div class="row">
                <div class="col-12">
                  <button
                    class="btn btn-secondary centered-element"
                    @click="randomScryfall()"
                  >
                    Or a random card!
                  </button>
                </div>
              </div>

              <!-- end guild choosing dropdown  -->
            </div>
            <!-- end profile picture section -->

            <!-- forms section -->
            <form
              class="col-lg-8 pt-4 pt-lg-0 content"
              id="signup-form "
              v-on:submit.prevent="createAccount()"
            >
              <div class="row">
                <div class="col">
                  <h3>
                    <label for="EmailForm" class="form-label">Email</label>
                  </h3>

                  <input
                    type="text"
                    class="form-control"
                    id="EmailForm"
                    v-model="inputParams.email"
                  />
                  <div class="invalid-feedback">{{ errors["Email"] }}.</div>
                  <div class="valid-feedback">Looks good!</div>
                </div>
                <div class="col">
                  <h3>
                    <label for="PasswordForm" class="form-label"
                      >Password</label
                    >
                  </h3>

                  <input
                    type="text"
                    class="form-control"
                    id="PasswordForm"
                    v-model="inputParams.password"
                  />
                  <div class="invalid-feedback">{{ errors["Password"] }}.</div>
                  <div class="valid-feedback">Looks good!</div>
                </div>
              </div>
              <br />
              <div class="row">
                <div class="col-lg-6">
                  <ul>
                    <li>
                      <strong
                        ><label for="FirstForm" class="form-label"
                          >First Name</label
                        ></strong
                      >
                      <input
                        type="text"
                        class="form-control"
                        id="FirstForm"
                        v-model="inputParams.first_name"
                      />
                      <div class="invalid-feedback">{{ errors["First"] }}.</div>
                      <div class="valid-feedback">Looks good!</div>
                    </li>
                    <li>
                      <strong
                        ><label for="ZIPForm" class="form-label"
                          >Zip Code</label
                        ></strong
                      >
                      <input
                        type="text"
                        class="form-control"
                        id="ZIPForm"
                        v-model="inputParams.zipcode"
                      />
                      <div class="invalid-feedback">{{ errors["ZIP"] }}</div>
                      <div class="valid-feedback">Looks good!</div>
                    </li>
                    <div class="mb-3">
                      <label for="AboutForm" class="form-label">
                        <strong>A little about yourself</strong>
                      </label>
                      <textarea
                        placeholder="Never tell anyone your full name or address. You should always meet strangers at public places like game stores, libraries, or bars."
                        class="form-control"
                        id="AboutForm"
                        rows="7"
                        v-model="inputParams.about_me"
                      ></textarea>
                      <div class="invalid-feedback">{{ errors["About"] }}</div>
                      <div class="valid-feedback">Looks good!</div>
                    </div>
                  </ul>
                </div>
                <div class="col-lg-6">
                  <ul>
                    <li>
                      <strong
                        ><label for="AgeForm" class="form-label"
                          >Age</label
                        ></strong
                      >
                      <input
                        type="number"
                        class="form-control"
                        id="AgeForm"
                        v-model="inputParams.age"
                        min="18"
                      />
                      <div class="invalid-feedback">
                        I'm sorry, you must be over 18 to use this site.
                      </div>
                      <div class="valid-feedback">Looks good!</div>
                    </li>
                    <li>
                      <div>
                        <strong>What are your favorite formats?</strong>
                      </div>
                      <div
                        v-for="format in favorite_formats"
                        :key="format.name"
                      >
                        <div class="form-check">
                          <input
                            class="form-check-input"
                            type="checkbox"
                            value=""
                            :id="format.name"
                            v-model="format.checked"
                          />
                          <label class="form-check-label" :for="format.name">{{
                            format.name
                          }}</label>
                        </div>
                      </div>
                    </li>
                  </ul>
                </div>
              </div>
              <button class="d-none">
                This is my hidden button to allow the 2nd form to work
              </button>
            </form>
          </div>
          <div class="row">
            <div class="col">
              <div data-aos="zoom-in" data-aos-delay="50">
                <button
                  @click="createAccount()"
                  class="btn btn-about centered-element"
                >
                  Sign up
                </button>
              </div>
            </div>
          </div>
        </div>
      </section>
    </main>

    <!-- Modal -->
    <div
      class="modal fade"
      id="cardList"
      tabindex="-1"
      aria-labelledby="cardListLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog modal-lg modal-dialog-centered">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="cardListLabel" v-if="!error">
              Which card did you mean?
            </h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body" style="height: 60vh">
            <div
              class="
                container-fluid
                d-flex
                flex-column flex-grow-1
                overflow-hidden
              "
            >
              <div class="row flex-grow-1 overflow-hidden" id="card-picker">
                <div class="col-5 overflow-auto py-2">
                  <div
                    class="list-group"
                    id="card-list"
                    role="tablist"
                    v-if="!showSaveButton"
                    style="max-height: 50vh; overflow: auto"
                  >
                    <div
                      class="list-group-item list-group-item-action"
                      data-bs-toggle="list"
                      :href="`#image-${card.id}`"
                      v-for="card in cards"
                      :key="`card-${card.id}`"
                      :id="`card-id-${card.id}`"
                      @click="selectCard(card)"
                      :aria-controls="`list-${card.id}`"
                    >
                      {{ card.name }}
                    </div>
                  </div>
                  <div
                    class="list-group"
                    id="list-tab"
                    role="tablist"
                    v-if="showSaveButton"
                    style="max-height: 50vh; overflow: auto"
                  >
                    <div
                      class="list-group-item list-group-item-action"
                      data-bs-toggle="list"
                      :href="`#image-${card.id}`"
                      v-for="card in cards"
                      :key="`format-${card.id}`"
                      :id="`format-id-${card.id}`"
                      @click="selectCard(card)"
                    >
                      {{ card.set_name }}
                    </div>
                  </div>
                </div>
                <div class="col-7 mh-100 overflow-auto py-2">
                  <div class="tab-content" id="nav-tabContent">
                    <div
                      class="tab-pane fade"
                      role="tabpanel"
                      v-for="card in cards"
                      :key="`img-${card.id}`"
                      :id="`image-${card.id}`"
                    >
                      <img
                        v-if="card.image_uris && card.image_uris.art_crop"
                        :src="card.image_uris.art_crop"
                        alt=""
                        id="picture-preview"
                        class="centered-element rounded"
                      />
                      <img
                        v-if="
                          card.card_faces &&
                          card.card_faces.length > 0 &&
                          card.card_faces[0] &&
                          card.card_faces[0]['image_uris']
                        "
                        :src="card.card_faces[0]['image_uris']['art_crop']"
                        alt=""
                        id="picture-preview"
                        class="centered-element rounded"
                      />
                      <div
                        style="font-style: italic"
                        class="float-end"
                        v-if="card.lang"
                      >
                        Artist: {{ card.artist }}
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="modal-footer" v-if="!error">
            <button
              type="button"
              class="btn btn-secondary"
              data-bs-dismiss="modal"
            >
              Cancel
            </button>
            <button
              v-if="!showSaveButton"
              class="btn btn-primary"
              @click="scryfallFormatSearch(selectedCard.name)"
            >
              Next! Choose which printing...
            </button>
            <button
              v-if="showSaveButton"
              class="btn btn-primary"
              @click="saveCard()"
              data-bs-dismiss="modal"
            >
              Save it!
            </button>
          </div>
        </div>
      </div>
    </div>
    <!-- END MODAL -->
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      inputParams: { profile_picture: "/assets/img/icons/B.png" },
      scryfallNameField: "",
      cards: [],
      error: false,
      selectedCard: {},
      showSaveButton: false,
      cardInfo: {},
      errors: {
        Password: "",
        Email: "",
        First: "",
        Age: "",
        About: "",
        ZIP: "",
      },
      favorite_formats: [
        {
          id: 1,
          name: "Commander / EDH",
          checked: false,
          user_id: localStorage.user_id,
        },
        {
          id: 2,
          name: "Standard",
          checked: false,
          user_id: localStorage.user_id,
        },
        {
          id: 3,
          name: "Cube / Draft",
          checked: false,
          user_id: localStorage.user_id,
        },
        {
          id: 4,
          name: "Modern",
          checked: false,
          user_id: localStorage.user_id,
        },
        {
          id: 5,
          name: "Pauper",
          checked: false,
          user_id: localStorage.user_id,
        },
        {
          id: 6,
          name: "Pioneer",
          checked: false,
          user_id: localStorage.user_id,
        },
        { id: 7, name: "Brawl", checked: false, user_id: localStorage.user_id },
        {
          id: 8,
          name: "Historic",
          checked: false,
          user_id: localStorage.user_id,
        },
        {
          id: 9,
          name: "Legacy",
          checked: false,
          user_id: localStorage.user_id,
        },
        {
          id: 10,
          name: "Vintage",
          checked: false,
          user_id: localStorage.user_id,
        },
      ],
      colors: [
        { id: 1, name: "White", img: "assets/img/icons/W.png" },
        { id: 2, name: "Blue", img: "/assets/img/icons/U.png" },
        { id: 3, name: "Black", img: "/assets/img/icons/B.png" },
        { id: 4, name: "Red", img: "/assets/img/icons/R.png" },
        { id: 5, name: "Green", img: "/assets/img/icons/G.png" },
        { id: 6, name: "Colorless", img: "/assets/img/icons/C.png" },
      ],
      guilds: [
        { id: 1, name: "Azorius", img: "assets/img/icons/Azorius_Logo.png" },
        { id: 2, name: "Dimir", img: "assets/img/icons/Dimir_Logo.png" },
        { id: 3, name: "Rakdos", img: "assets/img/icons/Rakdos_Logo.png" },
        { id: 4, name: "Gruul", img: "assets/img/icons/Gruul_Logo.png" },
        { id: 5, name: "Selesnya", img: "assets/img/icons/Selesnya_Logo.png" },
        { id: 6, name: "Orzhov", img: "assets/img/icons/Orzhov_Logo.png" },
        { id: 7, name: "Izzet", img: "assets/img/icons/Izzet_Logo.png" },
        { id: 8, name: "Golgari", img: "assets/img/icons/Golgari_Logo.png" },
        { id: 9, name: "Boros", img: "assets/img/icons/Boros_Logo.png" },
        { id: 10, name: "Simic", img: "assets/img/icons/Simic_Logo.png" },
      ],
    };
  },
  created: function () {
    let coinFlip = Math.round(Math.random() * 1);
    setTimeout(() => {
      if (coinFlip === 0) {
        // picks a random color
        let randColor = Math.round(Math.random() * 4);
        this.inputParams.profile_picture = this.colors[randColor].img;
      } else {
        // picks a random guild
        let randGuild = Math.round(Math.random() * 9);
        this.inputParams.profile_picture = this.guilds[randGuild].img;
      }
    }, 100);
  },

  methods: {
    showLogIn: function () {
      this.$router.push("/log-in");
    },
    scryfallSearch: function (cardName) {
      this.error = false;
      this.showSaveButton = false;

      // reset all selected items
      let actives = document.getElementsByClassName("active");
      actives.forEach((element) => {
        element.classList.remove("active");
      });

      // selects the loading image
      this.cards = [
        {
          id: 1,
          name: "Loading...",
          image_uris: { art_crop: "/assets/img/loading.gif" },
        },
      ];
      setTimeout(() => {
        let firstCard = document.getElementById(`card-id-${this.cards[0].id}`);
        let firstPicture = document.getElementById(`image-${this.cards[0].id}`);
        firstCard.classList.add("active");
        firstPicture.classList.add("show");
        firstPicture.classList.add("active");
      }, 50);

      fetch(`https://api.scryfall.com/cards/search?q=${cardName}`)
        .then((response) => response.json())
        .then((data) => {
          if (data.object === "list") {
            console.log("data", data);

            // reset all selected items
            let actives = document.getElementsByClassName("active");
            actives.forEach((element) => {
              element.classList.remove("active");
            });
            this.cards = data.data;
            this.selectedCard = this.cards[0];

            setTimeout(() => {
              let firstCard = document.getElementById(
                `card-id-${this.cards[0].id}`
              );
              let firstPicture = document.getElementById(
                `image-${this.cards[0].id}`
              );
              firstCard.classList.add("active");
              firstPicture.classList.add("show");
              firstPicture.classList.add("active");
            }, 50);
          } else {
            this.cards = [
              {
                id: 1,
                name: data.details,
                image_uris: { art_crop: "/assets/img/table-flip.gif" },
              },
            ];
            this.error = true;
          }
        });
    },
    scryfallFormatSearch: function (cardName) {
      this.showSaveButton = true;

      // reset all selected items
      let actives = document.getElementsByClassName("active");
      actives.forEach((element) => {
        element.classList.remove("active");
      });

      // selects the loading image
      this.cards = [
        {
          id: 1,
          name: "Loading...",
          set_name: "Loading...",
          image_uris: { art_crop: "/assets/img/loading.gif" },
        },
      ];

      setTimeout(() => {
        let firstCard = document.getElementById(
          `format-id-${this.cards[0].id}`
        );
        firstCard.classList.add("active");
        let firstPicture = document.getElementById(`image-${this.cards[0].id}`);
        firstPicture.classList.add("show");
        firstPicture.classList.add("active");
      }, 60);

      fetch(`https://api.scryfall.com/cards/named?fuzzy=${cardName}`)
        .then((response) => response.json())
        .then((data) => {
          let formatsearch = data.prints_search_uri;
          setTimeout(() => {
            fetch(formatsearch)
              .then((response) => response.json())
              .then((data) => {
                this.cards = data.data;
                this.selectedCard = this.cards[0];

                setTimeout(() => {
                  let firstCard = document.getElementById(
                    `format-id-${this.cards[0].id}`
                  );
                  firstCard.classList.add("active");
                  let firstPicture = document.getElementById(
                    `image-${this.cards[0].id}`
                  );
                  firstPicture.classList.add("show");
                  firstPicture.classList.add("active");
                }, 60);
              });
          }, 60);
        });
    },
    randomScryfall: function () {
      fetch("https://api.scryfall.com/cards/random")
        .then((response) => response.json())
        .then((data) => {
          if (data.card_faces) {
            this.inputParams.profile_picture =
              data.card_faces[0]["image_uris"]["art_crop"];
          } else {
            this.inputParams.profile_picture = data["image_uris"]["art_crop"];
          }
          this.cardInfo = data;
          console.log(this.cardInfo);
        });
    },
    selectCard: function (card) {
      this.selectedCard = card;
    },
    saveCard: function () {
      if (this.selectedCard.card_faces) {
        this.inputParams.profile_picture =
          this.selectedCard.card_faces[0]["image_uris"]["art_crop"];
      } else {
        this.inputParams.profile_picture =
          this.selectedCard["image_uris"]["art_crop"];
      }
      this.cardInfo = this.selectedCard;
    },
    selectIcon: function (card) {
      this.inputParams.profile_picture = card.img;
      this.cardInfo = {};
    },
    createAccount: function () {
      axios
        .post("/users", this.inputParams)
        .then((response) => {
          this.favorite_formats.forEach((format) => {
            if (format.checked === true) {
              format.user_id = response.data.id;
              axios.post("/favoriteformats", format).then(() => {
                this.$router.push("/");
              });
            }
          });
        })
        .catch((error) => {
          // reset errors
          this.errors = {
            Password: "",
            Email: "",
            First: "",
            ZIP: "",
            Age: "",
            About: "",
          };
          for (var key in this.errors) {
            // console.log(key);
            let form = document.getElementById(`${key}Form`);
            // console.log(form);
            form.classList.add("is-valid");
            form.classList.remove("is-invalid");
          }

          // now make any error an invalid state
          console.log("ERROR!", error.response.data.errors);
          error.response.data.errors.forEach((error) => {
            let errorName = error.split(" ")[0];
            this.errors[errorName] = error;
            let form = document.getElementById(`${errorName}Form`);
            // console.log(form)
            if (form) {
              form.classList.add("is-invalid");
              form.classList.remove("is-valid");
            }
          });
          // console.log(this.errors);
        });
    },
  },
};
</script>
