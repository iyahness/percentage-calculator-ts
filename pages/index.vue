<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <div class="statement-container">
          <h2 class="statement">
            I want to know
          </h2>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col">
        <div class="choices">
          <b-form-select class="dropdown" v-model="selected" :options="options" />
        </div>
      </div>
    </div>
    <div class="inputs">
      <div class="input-container savings" v-if="selected == 'savings'">
        <div class="row">
          <div class="col-xl-6 col-lg-6 col-md-6">
              <b-input-group>
                <b-form-input class="input-field"
                  v-model="originalPrice"
                  placeholder="0.00">
                </b-form-input>
              </b-input-group>
              <div class="field-label">
                Original Price
              </div>
          </div>
          <div class="col-xl-6 col-lg-6 col-md-6">
            <b-input-group>
              <b-form-input class="input-field"
                v-model="discountedPrice"
                placeholder="0.00">
              </b-form-input>
            </b-input-group>
            <div class="field-label">
              Discounted Price
            </div>
          </div>
        </div>
      </div>
      <div class="input-container discount" v-if="selected == 'discount'">
        <div class="row">
          <div class="col-xl-6 col-lg-6 col-md-6">
            <b-input-group>
              <b-form-input class="input-field"
                v-model="originalPrice"
                placeholder="0.00">
              </b-form-input>
            </b-input-group>
            <div class="field-label">
              Original Price
            </div>
          </div>
          <div class="col-xl-6 col-lg-6 col-md-6">
            <b-input-group>
              <b-form-input class="input-field"
                v-model="percentageDiscount"
                placeholder="00">
              </b-form-input>
            </b-input-group>
            <div class="field-label">
              Discount (%)
            </div>
          </div>
        </div>
      </div>
      <div class="input-container original" v-if="selected == 'original'">
        <div class="row">
          <div class="col-xl-6 col-lg-6 col-md-6">
            <b-input-group>
              <b-form-input class="input-field"
                v-model="discountedPrice"
                placeholder="0.00">
              </b-form-input>
            </b-input-group>
            <div class="field-label">
              Discounted Price
            </div>
          </div>
          <div class="col-xl-6 col-lg-6 col-md-6">
            <b-input-group>
              <b-form-input class="input-field"
                v-model="percentageDiscount"
                placeholder="0.00">
              </b-form-input>
            </b-input-group>
            <div class="field-label">
              Discount (%)
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="output-container">
      <div class="row">
        <div class="col">
          <h3 class="tagline">
            {{ outputTagline }}
          </h3>
        </div>
      </div>
      <div class="row">
        <div class="col">
          <div v-if="selected == 'savings'" class="main-output">
            {{ percentageDiscount }}
          </div>
          <div v-if="selected == 'discount'" class="main-output">
            {{ discountedPrice }}
          </div>
          <div v-if="selected == 'original'" class="main-output">
            {{ originalPrice }}
          </div>
        </div>
      </div>
      <div class="row">
        <div class="col">
          <h3 class="tagline">
            or
          </h3>
        </div>
      </div>
      <div class="row">
        <div class="col">
          <div class="main-output">
            {{ savings }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      discountedPrice: 0,
      originalPrice: 0,
      percentageDiscount: 0,
      selected: 'savings',
      savings: 0,
      outputTagline: 'My discount is',
      options: [
        { value: 'savings', text: 'how much discount will I get' },
        { value: 'discount', text: 'how much the discounted price is' },
        { value: 'original', text: 'how much the original price is' },
      ],
    };
  },
  mounted() {},
  methods: {
    computeDiscountedPrice() {
      this.discountedPrice = this.originalPrice
        - (this.originalPrice * (this.percentageDiscount / 100));
    },
    computeOriginalPrice() {
      this.originalPrice = this.discountedPrice / (1 - this.percentageDiscount / 100);
    },
    computeDiscount() {
      this.discountedPrice = Math.round((1 - this.discountedPrice / this.originalPrice) * 100);
    },
    computeSavings() {
      this.savings = this.originalPrice - this.discountedPrice;
    },
  },
  watch: {
    // eslint-disable-next-line object-shorthand
    discountedPrice: function () {
      this.computeOriginalPrice();
      this.computeDiscount();
      this.computeSavings();
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  margin: auto;
  min-height: 100vh;
  justify-content: center;
  align-items: center;
  padding: 24px 15px;
  text-align: center;
  .choices {
    display: inline-block;
    .dropdown {
      background-color: transparent;
      border: 0;
      color: #A83361;
      font-family: 'Sarala', sans-serif;
      text-align: center;
      width: fit-content;
      &:focus, &:active {
        box-shadow: none;
        outline: none;
      }
    }
  }
  .inputs {
    padding: 48px 0 12px 0;
    .input-container {
      border-bottom: 2px solid white;
      display: inline-block;
      .input-field {
        background: transparent;
        border: 2px solid white;
        border-radius: 0;
        color: #A83361;
        font-family: 'Sarala', sans-serif;
        font-size: 36px;
        height: 100px;
        text-align: center;
        width: 200px;
      }
      .field-label {
        font-family: 'Titillium Web', sans-serif;
        font-size: 18px;
        padding: 12px 0 24px 0;
      }
    }
  }
  .output-container {
    padding: 24px 0 0 0;
    .main-output {
      font-family: 'Sarala', sans-serif;
      font-size: 36px;
    }
  }
}
</style>
