<script>
import CreateEditView from '@/mixins/create-edit-view';
import CruResource from '@/components/CruResource';
import Labels from '@/components/form/Labels';
import Loading from '@/components/Loading';
import NameNsDescription from '@/components/form/NameNsDescription';
import { MANAGEMENT } from '@/config/types';
import { _VIEW } from '@/config/query-params';
import { FLEET } from '@/config/labels-annotations';

export default {
  name: 'CruFleetCluster',

  components: {
    CruResource,
    Labels,
    Loading,
    NameNsDescription,
  },

  mixins: [CreateEditView],

  props: {
    mode: {
      type:     String,
      required: true,
    },
  },

  async fetch() {
    const clusterId = this.value?.metadata?.labels[FLEET.CLUSTER_NAME];

    this.rancherCluster = await this.$store.dispatch('management/find', {
      type: MANAGEMENT.CLUSTER,
      id:   clusterId
    });
  },

  data() {
    return { rancherCluster: null };
  },

  computed: {
    isView() {
      return this.mode === _VIEW;
    }
  },

  methods: {
    async save(buttonDone) {
      try {
        await this.value.save();
        await this.rancherCluster.save();
        this.done();
        buttonDone(true);
      } catch (e) {
        buttonDone(false);
      }
    },
  },
};
</script>

<template>
  <Loading v-if="$fetchState.pending" />
  <CruResource
    v-else
    :done-route="doneRoute"
    :mode="mode"
    :resource="value"
    :subtypes="[]"
    :validation-passed="true"
    :errors="errors"
    @error="e=>errors = e"
    @finish="save"
    @cancel="done"
  >
    <NameNsDescription v-model="value" :mode="mode" :namespaced="isNamespaced" />

    <hr class="mt-20 mb-20" />

    <Labels

      default-section-class="mt-20"
      :value="rancherCluster"
      :mode="mode"
      :display-side-by-side="false"
    />
  </CruResource>
</template>
