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
    LabeledSelect,

  },
  props: {
    value: {
      type:    Object,
      default: () => {
        return {};
      }
    },
    mode: {
      type:    String,
      default: 'create'
    },
  },
  mixins:     [
    CreateEditView
  ],
  data() {
    return {

      name:            '',
      org:             ['workspace', 'workspace-1', 'workspace-2'],
    };
  },
  computed: {
    valid() {
      return false;
    },
  },
  methods: {
    async save() {
      try {
        await this.$store.dispatch('epinio/request', {
          url:    `v1/orgs/workspace/applications/sample-verbose-1/stage`,
          method: 'post',
          data:   {
            name: this.name,
            org:  this.org
          },
        });

        return true;
      } catch (e) {
        return false;
      }
    }
  }
};
</script>

<template>
  <div>
    <Loading v-if="!value" />
    <div v-else>
      <p>Mode: {{ mode }} </p>
      <p>Value: {{ JSON.stringify(value) }} </p>
      <p>originalValue: {{ JSON.stringify(originalValue) }} </p>
      <br><br>
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
      <div class="mb-20">
        <div class="row">
          <div class="col span-6 mb-10">
            <LabeledInput
              v-model="name"
              label="Application Name"
              :mode="mode"
              :required="true"
            />
          </div>
          <div class="col span-6 mb-10">
            <LabeledSelect
              v-model="org"
              :options="org[0]"
              label="Org Name"
              :mode="mode"
            />
          </div>
        </div>
      </div>
    </CruResource>
  </div>
</template>
