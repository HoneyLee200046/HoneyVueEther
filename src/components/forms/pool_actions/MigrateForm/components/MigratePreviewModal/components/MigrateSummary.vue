<script setup lang="ts">
import useNumbers, { FNumFormats } from '@/composables/useNumbers';
import { Pool } from '@/services/pool/types';

/**
 * TYPES
 */
type Props = {
  pool: Pool;
  fromTotal: string;
  toTotal: string;
  priceImpact: number;
  isLoadingBptOut?: boolean;
  highPriceImpact?: boolean;
  summaryTitle?: string | undefined;
};

/**
 * PROPS & EMITS
 */
withDefaults(defineProps<Props>(), {
  isLoadingPriceImpact: false,
  highPriceImpact: false,
  summaryTitle: undefined,
});

/**
 * COMPOSABLES
 */
const { fNum } = useNumbers();
</script>

<template>
  <div class="summary-table">
    <h6 class="p-2">{{ summaryTitle || $t('summary') }}</h6>
    <div class="flex flex-col py-2">
      <div class="summary-table-row">
        <div class="summary-table-label">
          {{ $t('totalToMigrate') }}
        </div>
        <div class="summary-table-number">
          ~{{ fNum(fromTotal, FNumFormats.fiat) }}
          <BalTooltip
            :text="$t('migratePool.tooltips.totalTo')"
            iconSize="sm"
            class="ml-2"
          />
        </div>
      </div>
      <div class="summary-table-row">
        <div class="summary-table-label">
          {{ $t('newTotal') }}
        </div>
        <div class="summary-table-number">
          ~{{ fNum(toTotal, FNumFormats.fiat) }}
          <BalTooltip
            :text="$t('migratePool.tooltips.newTotal')"
            iconSize="sm"
            class="ml-2"
          />
        </div>
      </div>
      <div
        :class="[
          'summary-table-row',
          {
            'bg-red-50 dark:bg-red-500 text-red-500 dark:text-white':
              highPriceImpact,
          },
        ]"
      >
        <div class="summary-table-label">
          {{ $t('priceImpact') }}
        </div>
        <div class="summary-table-number">
          <BalLoadingBlock v-if="isLoadingBptOut" class="w-10 h-6" />
          <template v-else>
            {{ fNum(priceImpact, FNumFormats.percent) }}
            <BalTooltip
              :text="$t('tooltips.addLiquidity.priceImpact')"
              iconSize="sm"
              :iconName="highPriceImpact ? 'alert-triangle' : 'info'"
              :iconClass="
                highPriceImpact
                  ? 'text-red-500 dark:text-white'
                  : 'text-gray-300'
              "
              width="72"
              class="ml-2"
            />
          </template>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.summary-table {
  @apply border dark:border-gray-700 divide-y dark:divide-gray-700 rounded-lg mt-4;
}

.summary-table-row {
  @apply grid grid-cols-2 px-2 py-1;
}

.summary-table-label {
  @apply flex items-center;
}

.summary-table-number {
  @apply flex items-center justify-end;
}
</style>
