<template>
  <div>
    <b-card
      class="flex-grow-1 border-bottom border-light rounded-0"
    >
      <b-card-header
        header-tag="header"
        class="p-0 mb-3"
      >
        <h5
          class="mb-0"
        >
          {{ $t('configurator:configuration') }}
        </h5>
      </b-card-header>
      <b-card-body
        class="p-0"
      >
        <b-form-group
          :label="$t('general:offset-expression')"
          label-class="text-primary"
          class="mb-0"
        >
          <expression-editor
            v-model="item.config.arguments[0].expr"
            font-size="18px"
            show-line-numbers
            @input="valueChanged"
          />
        </b-form-group>
      </b-card-body>
    </b-card>
  </div>
</template>

<script>
import base from './base'
import ExpressionEditor from '../ExpressionEditor'

export default {
  components: {
    ExpressionEditor,
  },

  extends: base,

  watch: {
    'item.config.stepID': {
      immediate: true,
      handler () {
        let args = [{
          target: 'offset',
          type: 'Duration',
          expr: '',
        }]

        if (this.item.config.arguments && this.item.config.arguments.length) {
          args = this.item.config.arguments.map(({ target, type, value, expr }) => {
            return {
              target,
              type,
              expr: expr || (value ? `"${value}"` : ''),
            }
          })
        }

        this.$set(this.item.config, 'arguments', args)
      },
    },
  },

  methods: {
    valueChanged (value) {
      this.$emit('update-default-value', {
        value: `Delay workflow execution for ${value}`,
        force: !this.item.node.value,
      })

      this.$root.$emit('change-detected')
    },
  },
}
</script>
