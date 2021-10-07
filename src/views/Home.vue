<template>
  <div class="container is-max-desktop wrapper">
    <div class="card place-card w-100" v-for="item in places" :key="item.id">
      <div class="card-image place-card__image-wrapper">
        <b-image
          :src="item.imageUrl"
          :alt="item.name"
          ratio="6by4"
          class="place-card__image-wrapper__image"
        ></b-image>
        <span class="place-card__image-wrapper__text">{{
          item.district.name
        }}</span>
      </div>
      <div class="card-content">
        <div class="content">
          <div class="place-card__brief">
            <span
              class="place-card__brief__items"
              v-for="briefs in item.listingDataBrief"
              :key="briefs.label"
            >
              {{ briefs.label }} {{ briefs.value }}
            </span>
          </div>
          <h3 class="mt-0 place-card__name">{{ item.name }}</h3>
          <div class="is-flex mb-3">
            <star-rating
              class="ml-0"
              :rating="item.score"
              :read-only="true"
              :increment="0.01"
              :show-rating="false"
              active-color="#00abaa"
              :star-size="15"
              rounded-corners
            ></star-rating>
            <span class="mb-1 ml-3 place-card__comment">{{
              item.commentCount
            }}</span>
          </div>
          <div class="columns is-flex">
            <div class="column is-6">
              <b-button type="is-danger" class="w-100" outlined
                >İncele</b-button
              >
            </div>
            <div class="column is-6">
              <b-button
                type="is-danger"
                class="w-100"
                @click="getOfferForm(item.id)"
                >Ücretsiz Teklif Al</b-button
              >
            </div>
          </div>
        </div>
      </div>
    </div>
    <b-modal v-model="offerModal" :width="500" scroll="keep">
      <div class="card">
        <div class="card-content">
          <h1 class="card-title mb-4">Ücretsiz Teklif Formu</h1>
          <div class="content">
            <div v-for="element in offer" :key="element.id">
              <Input
                :fieldLabel="element.fieldLabel"
                :inputType="element.fieldType"
                :inputName="element.fieldName"
                v-model="element.fieldValue"
                v-if="element.fieldType != 'select'"
                class="mb-3"
              />
              <Select
                :fieldLabel="element.fieldLabel"
                :options="element.infoRequestFormOptions"
                :selectName="element.fieldName"
                v-model="element.fieldValue"
                v-if="element.fieldType == 'select'"
                class="mb-3"
              />
            </div>
            <b-button type="is-danger" @click="getOffer()">Teklif Al</b-button>
          </div>
        </div>
      </div>
    </b-modal>
  </div>
</template>

<script>
import axios from "axios";
import StarRating from "vue-star-rating";
import Select from "../components/Select.vue";
import Input from "../components/Input.vue";
import { mapActions } from "vuex";

export default {
  name: "Home",
  data() {
    return {
      places: [],
      offerModal: false,
      offer: [],
    };
  },
  components: {
    StarRating,
    Select,
    Input,
  },
  methods: {
    ...mapActions(["offerSetter"]),
    getPlaces() {
      axios({
        method: "get",
        url: `https://private-1be47-duguncomapis.apiary-mock.com/companies`,
      })
        .then((response) => {
          this.places = response.data;
        })
        .catch(() => {
          this.$buefy.toast.open({
            duration: 3000,
            message: "Bir hata oluştu",
            position: "is-bottom",
            type: "is-danger",
          });
        });
    },
    getOfferForm(id) {
      axios({
        method: "get",
        url: `https://private-1be47-duguncomapis.apiary-mock.com/companies/${id}/forms`,
      })
        .then((response) => {
          this.offer = response.data;
          this.offerModal = true;
        })
        .catch(() => {
          this.$buefy.toast.open({
            duration: 3000,
            message: "Bir hata oluştu",
            position: "is-bottom",
            type: "is-danger",
          });
        });
    },
    getOffer() {
      const result = this.offer.map(({ fieldValue }) => fieldValue);
      this.offerSetter(result).then(() => {
        this.$router.push({ path: "/thank-you" });
      });
    },
  },
  created() {
    this.getPlaces();
  },
};
</script>
<style lang="scss" scoped>
.wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  max-width: 500px;
}

.place-card {
  margin-bottom: 15px;
  box-shadow: 0px 0px 2px #00000066;

  &__image-wrapper {
    position: relative;

    &__image {
      background: transparent
        linear-gradient(270deg, #34343400 0%, #343434 100%) 0% 0% no-repeat
        padding-box;
      opacity: 0.8;
    }

    &__text {
      position: absolute;
      bottom: 8px;
      left: 16px;
      font-size: 13px;
      letter-spacing: -0.2px;
      color: #ffffff;
    }
  }

  &__brief {
    margin-top: 10px;
    margin-bottom: 10px;
    display: flex;
    flex-direction: row;

    &__items {
      font-size: 13px;
      letter-spacing: -0.2px;
      color: #5f5f5f;
    }
    :first-child::after {
      content: "•";
      margin: 1em;
    }
  }

  &__name {
    font-size: 20px;
    margin-bottom: 12px;
    color: #1a1a1a;
  }

  &__comment {
    font-size: 13px;
    letter-spacing: -0.2px;
    color: #5f5f5f;
  }
}

.card-title {
  font-size: 20px;
}
</style>