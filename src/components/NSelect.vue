<template>
  <n-select v-model="selectedStreet" placeholder="请选择街道" :options="streets" @update:value="handleStreetChange" />
  <n-select v-model="selectedVillage" placeholder="请选择村" :options="villages" v-if="selectedStreet" />
</template>

<script>
import { ref, watch } from 'vue';
import { NSelect } from 'naive-ui';

export default {
  components: {
    NSelect
  },
  setup() {
    const streets = ref([
      { label: '东荆街道', value: '东荆街道' },
      // ... other streets
    ]);

    const villagesByStreet = {
      '东荆街道': [
        { label: '东荆大队', value: '东荆大队' },
        // ... other villages
      ],
      // ... other streets and their villages
    };

    const selectedStreet = ref(null);
    const selectedVillage = ref(null);
    const villages = ref([]);

    watch(selectedStreet, (newStreet) => {
      if (newStreet) {
        villages.value = villagesByStreet[newStreet];
      } else {
        selectedVillage.value = null;
        villages.value = [];
      }
    });

    function handleStreetChange(value) {
      // Logic to handle change in street selection
      // This can involve setting the selected village to null and updating the list of villages
    }

    return { streets, villages, selectedStreet, selectedVillage, handleStreetChange };
  }
};
</script>
