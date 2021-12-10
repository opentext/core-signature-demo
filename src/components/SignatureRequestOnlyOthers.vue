<template>
  <div>
    <h3>Send a Signature Request to Only Others</h3>
    <div class="input-div-width">
      <label for="emails">Signer emails:</label>
      <input
        id="emails"
        v-model="signerEmails"
        class="input-width"
        type="text"
        placeholder="Enter comma-separated emails of signers in the desired signing order"
      />
      <label for="prepare">Prepare signature request:</label>
      <input id="prepare" v-model="isBeingPrepared" type="checkbox" />
    </div>
    <br />
    <SendSignatureRequest />
  </div>
</template>

<script>
import SendSignatureRequest from "./SendSignatureRequest";
import { mapMutations, mapGetters } from "vuex";

export default {
  name: "SignatureRequestOnlyOthers",
  components: {
    SendSignatureRequest
  },
  data() {
    return {
      isBeingPrepared: false,
      signerEmails: [],
      timeoutHandle: () => {}
    };
  },
  computed: {
    ...mapGetters("documents", ["documentURL"])
  },
  watch: {
    signerEmails() {
      this.updateRequestBody();
    },
    isBeingPrepared() {
      this.updateRequestBody();
    }
  },
  created() {
    this.setCanSendSignatureRequest(false);
  },
  methods: {
    ...mapMutations("signatureRequests", [
      "setRequestBody",
      "setCanSendSignatureRequest"
    ]),
    updateRequestBody() {
      const vueComponent = this;
      this.setCanSendSignatureRequest(false);
      clearTimeout(this.timeoutHandle);

      this.timeoutHandle = setTimeout(function updateBody() {
        const signersArray = [];
        let curOrder = 0;
        const emails = vueComponent.signerEmails.split(",");
        for (const signerEmail of emails) {
          signersArray.push({ email: signerEmail, order: curOrder });
          curOrder++;
        }
        const requestBody = {
          document: vueComponent.documentURL,
          signers: signersArray,
          who: "o",
          is_being_prepared: vueComponent.isBeingPrepared
        };
        vueComponent.setRequestBody(requestBody);
        vueComponent.setCanSendSignatureRequest(signersArray.length > 0);
      }, 1500);
    }
  }
};
</script>
