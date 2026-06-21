<template>
  <v-card>
    <v-card-title>Options</v-card-title>
    <v-card-text>
      <v-switch
        v-model="configurationState.showScore"
        @click="handleChangeConfiguration"
        label="Show score at bottom of screen"
      />

      <v-switch
        v-model="configurationState.showBlankLines"
        @click="handleChangeConfiguration"
        label="Show blank lines between background rows"
      />

      <v-select
        v-model="configurationState.romSize"
        :items="romSizeOptions"
        label="ROM Size"
        @change="handleChangeConfiguration"
      />
    </v-card-text>
  </v-card>
</template>

<script>
import {computed, defineComponent} from '@vue/composition-api';

import {useConfigurationStorage} from '../hooks/project';

export default defineComponent({
  setup() {
    const configurationStorage = useConfigurationStorage();

    const romSizeOptions = [
      '4k',
      '8k',
      '16k',
      '32k',
      '64k',
    ];

    const configurationState = computed({
      get() {
        const DEFAULT_CONFIGURATION = {
          showScore: true,
          showBlankLines: true,
          romSize: '8k',
        };

        try {
          const configuration =
            configurationStorage.value ||
            structuredClone(DEFAULT_CONFIGURATION);

          return Object.fromEntries(
            Object.entries(DEFAULT_CONFIGURATION)
                .map(([k, v]) => [k, configuration[k] ?? v]),
          );
        } catch (e) {
          console.error('Error loading configuration from local storage', e);
          return structuredClone(DEFAULT_CONFIGURATION);
        }
      },

      set(newState) {
        configurationStorage.value = newState;
      },
    });

    const handleChangeConfiguration = () => {
      configurationState.value = configurationState.value;
    };

    return {
      configurationState,
      handleChangeConfiguration,
      romSizeOptions,
    };
  },
});
</script>
