<template>
  <div
    class="flex gap-8px items-center"
    data-cy="debug-results-holder"
  >
    <ResultCounts
      :total-passed="results.totalPassed"
      :total-failed="results.totalFailed"
      :total-skipped="results.totalSkipped"
      :total-pending="results.totalPending"
      :order="['PASSED', 'FAILED', 'SKIPPED', 'PENDING']"
    />
    <div
      v-if="results?.totalFlakyTests"
      data-cy="debug-flaky-badge"
      class="border rounded flex flex-row gap-8px items-center h-6 bg-orange-50 border-orange-200 text-sm text-orange-600 px-2 gap-x-1 border-1"
    >
      <span
        data-cy="total-flaky-tests"
        class="font-medium pr-1 opacity-70"
      >
        {{ results.totalFlakyTests }}
      </span>
      <div class="w-px my-6px h-6 border-orange-200 border" />
      <span class="font-semibold pl-1">
        {{ t('specPage.flaky.badgeLabel') }}
      </span>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { computed } from 'vue'
import type { DebugResultsFragment } from '../generated/graphql'
import { gql } from '@urql/core'
import ResultCounts from '@packages/frontend-shared/src/components/ResultCounts.vue'
import { useI18n } from '@cy/i18n'

const { t } = useI18n()

gql`
fragment DebugResults on CloudRun {
  id
  totalPassed
  totalFailed
  totalPending
  totalSkipped
  totalFlakyTests
}
`

const props = defineProps<{
  gql: DebugResultsFragment
}>()

const results = computed(() => props.gql)
</script>
