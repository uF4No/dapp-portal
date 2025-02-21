<template>
  <div>
    <BackButton :fallback="{ name: 'index' }" />
    <h1 class="h1">Where to send</h1>

    <ModalTransactionWithdrawExchangeWarning
      :opened="openedModal === 'withdraw-to-exchange'"
      :button-location="{ name: 'transaction-zksync-lite-withdraw' }"
      :withdraw-to-self-link-location="{ name: 'transaction-zksync-era-withdraw', query: { address: account.address } }"
      @close="closeModal"
    />

    <CommonCardWithLineButtons>
      <DestinationItem
        v-bind="destinations.zkSyncLite"
        as="RouterLink"
        :to="{ name: 'transaction-zksync-lite-send', query: $route.query }"
        description="Send inside zkSync Lite network"
      />
      <DestinationItem
        v-bind="destinations.ethereum"
        as="RouterLink"
        :to="{ name: 'transaction-zksync-lite-withdraw', query: $route.query }"
        description="Withdraw to Ethereum"
      />
      <DestinationItem
        v-bind="destinations.era"
        as="RouterLink"
        :to="{ name: 'transaction-zksync-lite-send-era', query: $route.query }"
        description="Send to zkSync Era∎ network"
      />
    </CommonCardWithLineButtons>

    <TypographyCategoryLabel>Send to exchange</TypographyCategoryLabel>
    <CommonCardWithLineButtons>
      <DestinationItem
        label="Official bridge"
        :icon-url="destinations.ethereum.iconUrl"
        description="Send to exchange using official bridge"
        @click="openedModal = 'withdraw-to-exchange'"
      />
    </CommonCardWithLineButtons>

    <TypographyCategoryLabel>Send to another network</TypographyCategoryLabel>
    <CommonCardWithLineButtons>
      <DestinationItem
        v-bind="destinations.orbiter"
        :icon="ArrowUpRightIcon"
        as="a"
        target="_blank"
        href="https://www.orbiter.finance/?source=zkSync%20Lite"
      />
    </CommonCardWithLineButtons>

    <TypographyCategoryLabel>Sell tokens for cash</TypographyCategoryLabel>
    <CommonCardWithLineButtons>
      <DestinationItem
        v-bind="destinations.banxa"
        :icon="ArrowUpRightIcon"
        as="a"
        target="_blank"
        href="https://zksync.banxa.com/"
      />
    </CommonCardWithLineButtons>
  </div>
</template>

<script lang="ts" setup>
import { ref } from "vue";

import { ArrowUpRightIcon } from "@heroicons/vue/24/outline";
import { storeToRefs } from "pinia";

import { useDestinationsStore } from "@/store/destinations";
import { useOnboardStore } from "@/store/onboard";

const { destinations } = storeToRefs(useDestinationsStore());
const { account } = storeToRefs(useOnboardStore());

const openedModal = ref<"withdraw-to-exchange" | undefined>();
const closeModal = () => {
  openedModal.value = undefined;
};
</script>

<style lang="scss" scoped></style>
