<template>
  <div class="tempo-toolbar">
    <div class="row">
      <div class="form-group">
        <label class="right-space">{{ $t('toolbar:common.bpm') }}</label>
        <InputBpm
          :modelValue="element.bpm"
          @update:modelValue="$emit('update:bpm', $event)"
        />
      </div>

      <span class="space" />

      <div class="form-group">
        <label class="right-space">{{ $t('toolbar:common.spaceAfter') }}</label>
        <InputUnit
          unit="pt"
          :min="-spaceAfterMax"
          :max="spaceAfterMax"
          :step="0.5"
          :precision="2"
          :modelValue="element.spaceAfter"
          @update:modelValue="$emit('update:spaceAfter', $event)"
        />
      </div>
      <span class="space"></span>
      <div class="form-group">
        <label class="right-space">{{
          $t('toolbar:common.sectionName')
        }}</label>
        <input
          type="text"
          :value="element.sectionName"
          @change="
            $emit(
              'update:sectionName',
              ($event.target as HTMLInputElement).value,
            )
          "
        />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-facing-decorator';

import { TempoElement } from '@/models/Element';
import { PageSetup } from '@/models/PageSetup';
import { Unit } from '@/utils/Unit';

import InputBpm from './InputBpm.vue';
import InputUnit from './InputUnit.vue';

@Component({
  components: { InputUnit, InputBpm },
  emits: ['update:bpm', 'update:sectionName', 'update:spaceAfter'],
})
export default class ToolbarTempo extends Vue {
  @Prop() element!: TempoElement;
  @Prop() pageSetup!: PageSetup;

  get spaceAfterMax() {
    return Math.round(Unit.toPt(this.pageSetup.pageWidth));
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.tempo-toolbar {
  background-color: lightgray;

  padding: 0.25rem;

  --btn-size: 32px;
}

.row {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
}

label.right-space {
  margin-right: 0.5rem;
}

.space {
  width: 16px;
}
</style>
