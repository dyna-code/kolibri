<template>

  <OnboardingStepBase
    :title="$tr('setUpFacilityTitle')"
    :description="$tr('setUpFacilityDescription')"
    @continue="handleContinue"
  >
    <KRadioButton
      v-model="selected"
      :label="$tr('createFacilityLabel')"
      :value="Options.NEW"
      class="radio-button"
    />
    <KRadioButton
      v-model="selected"
      :label="$tr('importFacilityLabel')"
      :value="Options.IMPORT"
      class="radio-button"
    />
    <SelectDeviceModalGroup
      v-if="showSelectAddressModal"
      @cancel="showSelectAddressModal = false"
      @submit="handleContinueImport"
    />
  </OnboardingStepBase>

</template>


<script>

  import { SelectDeviceModalGroup } from 'kolibri.coreVue.componentSets.sync';
  import OnboardingStepBase from '../OnboardingStepBase';
  import { FacilityTypePresets as Options } from '../../constants';

  export default {
    name: 'SetUpLearningFacilityForm',
    components: {
      OnboardingStepBase,
      SelectDeviceModalGroup,
    },
    inject: ['wizardService'],
    data() {
      return {
        Options,
        selected: Options.NEW,
        showSelectAddressModal: false,
      };
    },
    methods: {
      handleContinueImport(address) {
        this.wizardService.send({
          type: 'CONTINUE',
          value: { importOrNew: Options.IMPORT, importDeviceId: address.id },
        });
      },
      handleContinue() {
        if (this.selected === Options.IMPORT) {
          this.showSelectAddressModal = true;
        } else {
          this.wizardService.send({ type: 'CONTINUE', value: { importOrNew: Options.NEW } });
        }
      },
    },
    $trs: {
      setUpFacilityTitle: {
        message: 'Set up the learning facility for this full device',
        context: '',
      },
      setUpFacilityDescription: {
        message:
          'A learning facility is the location where you use Kolibri, such as a school, training center, or your home.',
        context: '',
      },
      createFacilityLabel: {
        message: 'Create a new learning facility',
        context: '',
      },
      importFacilityLabel: {
        message: 'Import all data from an existing learning facility',
        context: '',
      },
    },
  };

</script>


<style lang="scss" scoped>

  .radio-button {
    padding-bottom: 8px;
    font-size: 0.875em;
  }

</style>
