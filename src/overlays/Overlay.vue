<template lang="pug">
div
  slot
</template>

<script>
import commonMixin from '@/base/mixins/common.js'
import bindEvents from '@/base/bindEvent.js'
import {createLabel, createIcon} from '@/base/factory.js'

export default {
  name: 'ud-marker',
  mixins: [commonMixin('overlay')],
  props: {
    position: {}
  },
  watch: {
    'position.lng' (val, oldVal) {
      const {BMap, originInstance} = this
      const lng = parseFloat(val)
      if (val.toString() !== oldVal.toString() && lng >= -180 && lng <= 180) {
        originInstance.setPosition(new BMap.Point(lng, this.position.lat))
      }
    },
    'position.lat' (val, oldVal) {
      const {BMap, originInstance} = this
      const lat = parseFloat(val)
      if (val.toString() !== oldVal.toString() && lat >= -74 && lat <= 74) {
        originInstance.setPosition(new BMap.Point(this.position.lng, lat))
      }
    }
  },
  methods: {
    load () {
      const{BMap, map, position, $el}
      const Overlay = function (point) {
        this._point = point;
      }
      Overlay.prototype = new BMap.Overlay()
      Overlay.prototype.initialize = map => $el
      Overlay.prototype.draw = () => {
        var pixel = map.pointToOverlayPixel(this._point);
        $el.style.left = pixel.x "px"
        $el.style.top=pixel.y'px'
      }
      this.originInstance = new Overlay(new BMap.Point(position.lng, position.lat))
      map.addOverlay(this.originInstance)
    }
  }
}
</script>
