<script lang="ts">
import CreateEditView from '@/mixins/create-edit-view';
import CruResource from '@/components/CruResource.vue';
import Loading from '@/components/Loading.vue';
import LabeledInput from '@/components/form/LabeledInput.vue';
import LabeledSelect from '@/components/form/LabeledSelect.vue';

export default {
  components: {
    CruResource,
    Loading,
    LabeledInput,
    LabeledSelect
  },
  props: {
    mode: {
      type:    String,
      default: 'edit'
    },

  },
  mixins:     [
    CreateEditView
  ],

  data() {
    return { 'org-options': [1, 2, 3] };
  },

  computed: {
    valid() {
      return false;
    },
  },

  methods: {
    save() {
      // stage
      // create app
      // redirect apps list
      // show vm manager w/ staging logs
    }
  }
};
</script>

<template>
  <div>
    <Loading v-if="!value" />
    <div v-else>
      Mode: {{ mode }}<br>
      Value: {{ JSON.stringify(value) }}<br>
      originalValue: {{ JSON.stringify(originalValue) }}<br>
    </div>

    <CruResource
      :validation-passed="true"
      :resource="value"
      :mode="mode"
      :errors="errors"
      :done-route="doneRoute"
      @finish="save"
      @error="e=>errors = e"
    >
      <LabeledInput
        label="Application Name"
        :mode="mode"
        :required="true"
      />
      <LabeledSelect
        v-model="org-options[1]"
        :options="org-options"
        label="Org Name"
        :mode="mode"
      />
    </CruResource>
  </div>
</template>
