<template>
    <div>
        <div class="uk-form-row color-input-row">
            <div class="color-box"></div>
            <div class="uk-inline">
                <a class="uk-form-icon" @click="openSelect=!openSelect"
                   :style="{backgroundColor:model.rgba}"></a>
                <input type="text" v-model="model.rgba" placeholder="Pick a color.." class="uk-input uk-width-1-1">
            </div>
        </div>
        <div class="select select--inline" :class="{'select--open':openSelect}">
            <div class="select__dropdown" style="text-align: center" :style="{'display':openSelect?'block':'none'}">
                <material-picker :value="colors" @input="updateValue"></material-picker>
            </div>
        </div>
    </div>
</template>

<script>
  import VueColor from 'vue-color'

  export default {
    components: {
      'MaterialPicker': VueColor.Sketch
    },
    mixins: [window.Storyblok.plugin],
    data () {
      return {
        colors: '#000000',
        openSelect: false
      }
    },
    mounted () {
      let s = this.model.rgba
      if (s) {
        const currentRgba = s.match(/\(([^)]+)\)/)[1]
        const arr = currentRgba.split(',')
        this.colors = {
          r: Number(arr[0]),
          g: Number(arr[1]),
          b: Number(arr[2]),
          a: Number(arr[3])
        }
      }
    },
    methods: {
      updateValue (v) {
        let rgba = v.rgba
        const str = `rgba(${rgba.r},${rgba.g},${rgba.b},${rgba.a})`
        this.$set(this.model, 'rgba', str)
      },
      initWith () {
        return {
          // needs to be equal to your storyblok plugin name
          plugin: 'vue-color-picker',
          rgba: ''
        }
      },
      pluginCreated () {
        // eslint-disable-next-line
        console.log('View source and customize: https://github.com/storyblok/storyblok-fieldtype')
      }
    },
    watch: {
      'model': {
        handler: function (value) {
          this.$emit('changed-model', value)
        },
        deep: true
      }
    }
  }
</script>

<style>
    .color-input-row .uk-form-icon {
        position: absolute;
        top: 6px;
        left: 16px;
        display: inline-block;
        width: 24px;
        height: 24px;
        border-radius: 50%;
        border: 2px solid black;
    }

    .color-input-row .uk-input {
        padding-left: 50px !important;
    }

    .select__dropdown > div {
        width: 90% !important;
        margin: 0 auto !important;
    }
</style>
