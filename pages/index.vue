<template>
  <div class="container">
    <div>
      <no-ssr placeholder="loading...">
        <img class="qr-code" v-if="qrCodeData" :src="qrCodeData.src" alt="QR CODE" />
      </no-ssr>
      <div>
        <a :href="trinity_link" class="action button--green">Donate with Trinity</a>
      </div>
      <div class="box">
        <h1 class="title">#iotatrees</h1>
        <h2 class="subtitle">Plant a 🌳 with IOTA</h2>
        <p class="description">4 MIOTA = 1 TREE</p>
        <div class="logos">
          <img class="logo logo--akita" src="~/assets/AKITALogo.png" alt />
          <img class="logo logo--iota" src="~/assets/IOTALogo.png" alt />
          <img class="logo logo--einfach" src="~/assets/einfachLogo.png" alt />
        </div>
        <div class="links">
          <a href="https://teamtrees.org/" target="_blank" class="button button--green">Learn more</a>
          <a
            href="https://thetangle.org/address/OQYZHELDOEEA9ZYIKHZRHZQDORNAEPMYWJ9KSNDOUDNAKWLGYSEQUQVXR9UYWQRJLCZSKT99L9ZTHDQNW"
            target="_blank"
            class="button button--grey"
          >Balance on TheTangle.org</a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import * as IotaQR from "@tangle-frost/iota-qr-lib/pkg/iota-qr-lib.js";
//const {TrinityPaymentQR} = require("@tangle-frost/iota-qr-lib/pkg/iota-qr-lib.js");
console.log("TrinityPaymentQR");

export default {
  components: {},
  data() {
    return {
      qrCodeData: null,
      address:
        "OQYZHELDOEEA9ZYIKHZRHZQDORNAEPMYWJ9KSNDOUDNAKWLGYSEQUQVXR9UYWQRJLCZSKT99L9ZTHDQNWUUWJUK9BD",
      message: "Donated on https://trees.akitablock.io by <anonymous>",
      value: 5000000
    };
  },
  computed: {
    trinity_link() {
      return (
        "iota://" +
        this.address +
        "/?amount=" +
        this.value +
        "&message=" +
        this.message
      );
    }
  },
  created() {
    console.log("process.client client");
    console.log(process.client);
    if (process.client) {
      const paymentData = IotaQR.TrinityPaymentQR.generatePaymentData(
        this.address,
        this.value,
        "AKITA",
        this.message
      );
      IotaQR.TrinityPaymentQR.renderHtml(paymentData, "png", 11, 4, 0, {
        background: new IotaQR.Color(1, 0, 0, 0)
      }).then(qrCodeData => {
        this.qrCodeData = qrCodeData;
        console.log("qr_code_data", qrCodeData);
        console.log("qr_code_data", qrCodeData.src);
      });
    }
  }
};
</script>

<style lang="scss">
body {
  background-image: url("../assets/forrest.jpg");
  background-position: center center;
  background-repeat: no-repeat;
  background-attachment: fixed;
  background-size: cover;
  background-color: #999;

  font-family: "Open Sans", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
}
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
.action {
  color: white;
  background: #3b8070;
  margin: 20px;
  &:hover {
    background: #2ba085;
  }
}

.qr-code {
  margin-top: 30px;
}
.box {
  background: rgba(0, 0, 0, 0.4);
  color: white;
  border-radius: 10px;
  transition: background 1s;
  padding: 20px;
}

.logos {
  padding-bottom: 30px;
  .logo {
    &--akita {
      height: 80px;
    }
    &--iota {
      height: 60px;
      margin: 10px;
    }
    &--einfach {
      height: 100px;
      margin-bottom: -10px;
    }
  }
}

.title {
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #efefef;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #efefef;
  word-spacing: 5px;
  padding: 20px;
}

.description {
    font-size: 32px;
    margin: 20px 0;
}

.links {
  padding-top: 15px;
}
.button {
  color: #efefef;
}

@media only screen and (max-width: 1260px) {
  .title {
    font-size: 52px;
  }
  .subtitle {
    font-size: 32px;
  }

  .logos {
    padding-bottom: 30px;
    .logo {
      &--akita {
        height: 50px;
      }
      &--iota {
        height: 30px;
        margin: 10px;
      }
    &--einfach {
      height: 70px;
      margin-bottom: -10px;
    }
    }
  }
  .button {
    margin-top: 15px;
    color: #efefef;
  }

}
</style>