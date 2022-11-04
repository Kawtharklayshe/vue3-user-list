<template>
  <div>

    <!-- Media -->
    <b-media class="mb-2">
      <template #aside>
        <b-avatar
          ref="previewEl"
          :src="userData.avatar"
          :text="avatarText(userData.first_name)"
          size="90px"
          rounded
        />
      </template>

      <h4 class="mb-1">
        {{ userData.first_name }}
      </h4>
      <div class="d-flex flex-wrap">
        <b-button
          variant="primary"
          @click="$refs.refInputEl.click()"
        >
          <input
            ref="refInputEl"
            type="file"
            class="d-none"
            @input="inputImageRenderer"
          >
          <span class="d-none d-sm-inline">Update</span>
          <feather-icon
            icon="EditIcon"
            class="d-inline d-sm-none"
          />
        </b-button>
        <b-button
          variant="outline-secondary"
          class="ml-1"
        >
          <span class="d-none d-sm-inline">Remove</span>
          <feather-icon
            icon="TrashIcon"
            class="d-inline d-sm-none"
          />
        </b-button>
      </div>
    </b-media>
    <validation-observer
        #default="{ handleSubmit }"
        ref="refFormObserver"
      >
    <!-- User Info: Input Fields -->
    <b-form   @submit.prevent="handleSubmit(onSubmit)"
         >
      <b-row>

        <!-- Field: first_name -->
        <b-col
          cols="12"
          md="4"
        >
          <b-form-group
            label="First Name"
            label-for="first_name"
          >
            <b-form-input
              id="first_name"
              v-model="userData.first_name"
            />
          </b-form-group>
        </b-col>

        <!-- Field: last_name -->
        <b-col
          cols="12"
          md="4"
        >
          <b-form-group
            label="Last Name"
            label-for="last_name"
          >
            <b-form-input
              id="last_name"
              v-model="userData.last_name"
            />
          </b-form-group>
        </b-col>

        <!-- Field: Email -->
        <b-col
          cols="12"
          md="4"
        >
          <b-form-group
            label="Email"
            label-for="email"
          >
            <b-form-input
              id="email"
              v-model="userData.email"
              type="email"
            />
          </b-form-group>
        </b-col>

   <!-- Action Buttons -->
   <b-button
      variant="primary"
      type="submit"
      class="mb-1 mb-sm-0 mr-0 mr-sm-1"
      :block="$store.getters['app/currentBreakPoint'] === 'xs'"
    >
      Save Changes
    </b-button>
    <b-button
      variant="outline-secondary"
      type="reset"
      :block="$store.getters['app/currentBreakPoint'] === 'xs'"
    >
      Reset
    </b-button>

      </b-row>
    </b-form>

   </validation-observer>
   
  </div>
</template>

<script>
import {
  BButton, BMedia, BAvatar, BRow, BCol, BFormGroup, BFormInput, BForm, BTable, BCard, BCardHeader, BCardTitle, BFormCheckbox,
} from 'bootstrap-vue'
import { avatarText } from '@core/utils/filter'
import vSelect from 'vue-select'
import { useInputImageRenderer } from '@core/comp-functions/forms/form-utils'
import { ref } from '@vue/composition-api'
import useUsersList from '../users-list/useUsersList'
import { ValidationProvider, ValidationObserver } from 'vee-validate'
import store from '@/store'

export default {
  components: {
    BButton,
    BMedia,
    BAvatar,
    BRow,
    BCol,
    BFormGroup,
    BFormInput,
    BForm,
    BTable,
    BCard,
    BCardHeader,
    BCardTitle,
    BFormCheckbox,
    vSelect,
        // Form Validation
        ValidationProvider,
    ValidationObserver,
  },
  props: {
    userData: {
      type: Object,
      required: true,
    },
  },
  setup(props) {
    const { resolveUserRoleVariant } = useUsersList()


    // ? Demo Purpose => Update image on click of update
    const refInputEl = ref(null)
    const previewEl = ref(null)

    const { inputImageRenderer } = useInputImageRenderer(refInputEl, base64 => {
      // eslint-disable-next-line no-param-reassign
      props.userData.avatar = base64
    })
    const onSubmit = () => {
      store.dispatch('app-user/UpdateUser', props.userData)
        .then(() => {
        
        })
    }

    return {
      resolveUserRoleVariant,
      avatarText,
      onSubmit,

      //  ? Demo - Update Image on click of update button
      refInputEl,
      previewEl,
      inputImageRenderer,
    }
  },
}
</script>

<style lang="scss">
@import '@core/scss/vue/libs/vue-select.scss';
</style>
