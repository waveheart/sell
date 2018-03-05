<!--
    **author:gyx
    **msg:Be happy! Guy!
 -->

<template>
  <div class="ratingselect">
    <div class="rating-type border-1px">
      <span @click="select(2, $event)" class="block positive" :class="{'active': selected_Type === 2}">{{desc.all}}<span class="count">{{ratings.length}}</span></span>
      <span @click="select(0, $event)" class="block positive" :class="{'active': selected_Type === 0}">{{desc.positive}}<span class="count">{{positives.length}}</span></span>
      <span @click="select(1, $event)" class="block negative" :class="{'active': selected_Type === 1}">{{desc.negative}}<span class="count">{{negatives.length}}</span></span>
    </div>
    <div @click="toggleContent" class="switch" :class="{'on': only_content}">
      <i class="icon-check_circle"></i>
      <span class="text">只看有内容的评价</span>
    </div>
  </div>
</template>

<script>
const POSITIVE = 0
const NEGATIVE = 1
const ALL = 2

export default {
  data () {
    return {
      selected_Type: this.selectType,
      only_content: this.onlyContent
    }
  },
  props: {
    ratings: {
      type: Array,
      default: () => []
    },
    selectType: {
      type: Number,
      default: ALL
    },
    onlyContent: {
      type: Boolean,
      default: false
    },
    desc: {
      type: Object,
      default: () => ({
        all: '全部',
        positive: '满意',
        negative: '不满意'
      })
    }
  },
  watch: {
    onlyContent (newVal, oldVal) {
      this.only_content = newVal
    }
  },
  computed: {
    positives () {
      return this.ratings.filter((rating) => {
        return rating.rateType === POSITIVE
      })
    },
    negatives () {
      return this.ratings.filter((rating) => {
        return rating.rateType === NEGATIVE
      })
    }
  },
  methods: {
    select (type, event) {
      if (!event._constructed) {
        return
      }
      this.selected_Type = type
      this.$emit('ratingTypeSelect', type)
    },
    toggleContent (event) {
      if (!event._constructed) {
        return
      }
      this.only_content = !this.only_content
      this.$emit('contentToggle', this.only_content)
    }
  }
}
</script>

<style lang="stylus" scoped="" rel="stylesheet/stylus">
@import "../../common/stylus/mixin.styl"

.ratingselect
  .rating-type
    padding: 18px 0
    margin: 0 18px
    border-1px(rgba(7, 17, 27, .1))
    font-size: 0
    .block
      display: inline-block
      padding: 8px 12px
      border-radius: 2px
      margin-right: 8px
      line-height: 16px
      color: rgb(77, 75, 93)
      font-size: 12px
      &.active
        color: #fff
      .count
        margin-left: 2px
        font-size: 8px
      &.positive
        background-color: rgba(0, 160, 220, .2)
        &.active
          background-color: rgb(0, 160, 220)
      &.negative
        background-color: rgba(77, 85, 93, .2)
        &.active
          background-color: rgb(77, 85, 93)
  .switch
    padding: 12px 18px
    line-height: 24px
    border-bottom: 1px solid rgba(7, 17, 27, .1)
    color: rgb(147, 153, 159)
    font-size: 0
    &.on
      .icon-check_circle
        color: #00c850
    .icon-check_circle
      display: inline-block
      margin-right: 4px
      vertical-align: top
      font-size: 24px
    .text
      font-size: 12px
</style>
