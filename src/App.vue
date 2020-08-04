<template>
  <div
    id="app"
    :style="cssVars"
  >
    <div class="app-grid">
      <div class="content">
        <div class="top" />
        <div class="menu" />
        <div class="main">
          <div class="calendar">
            <div>
              &lt;
            </div>
            <div class="month">
              {{ calendar.month }}
            </div>
            <div>
              &gt;
            </div>
            <div class="days-grid">
              <div
                v-for="day in calendar.days"
                :key="day"
                class="day"
                :class="{ active: day === calendar.day }"
              >
                {{ day }}
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="sidebar" />
    </div>
  </div>
</template>

<script>
import regression from 'regression'
import { getDate, getDaysInMonth, getMonth } from 'date-fns'
import { enUS } from 'date-fns/locale'
const horizontalSpacingRes = regression.polynomial([[360, 10], [768, 20], [1024, 25], [1280, 30]], { order: 3 })
const verticalSpacingRes = regression.polynomial([[360, 10], [768, 20]], { order: 3 })
const text1Res = regression.polynomial([[360, 15], [768, 18], [1024, 20], [1280, 22]], { order: 3 })
const headingText1Res = regression.polynomial([[360, 18], [768, 20], [1024, 22], [1280, 30]], { order: 3 })

export default {
  name: 'App',
  data () {
    return {
      width: window.innerWidth,
      today: new Date(),
      activeDate: new Date()
    }
  },
  computed: {
    cssVars () {
      const hSpacing = Math.min(Math.max(horizontalSpacingRes.predict(this.width)[1], 10), 30)
      const vSpacing = Math.min(Math.max(verticalSpacingRes.predict(this.width)[1], 10), 20)
      const text1 = Math.min(Math.max(text1Res.predict(this.width)[1], 18), 22)
      const headingText1 = Math.min(Math.max(headingText1Res.predict(this.width)[1], 20), 30)
      return {
        '--hspacing': `${hSpacing}px`,
        '--vspacing': `${vSpacing}px`,
        '--text1': `${text1}px`,
        '--heading1': `${headingText1}px`
      }
    },
    calendar () {
      return {
        month: enUS.localize.month(getMonth(this.activeDate), { width: 'abbreviated' }).toUpperCase(),
        days: getDaysInMonth(this.activeDate),
        day: getDate(this.activeDate)
      }
    }
  },
  mounted () {
    this.$nextTick(() => {
      window.addEventListener('resize', this.onResize)
    })
  },
  beforeDestroy () {
    window.removeEventListener('resize', this.onResize)
  },
  methods: {
    onResize () {
      this.width = window.innerWidth
    }
  }
}
</script>

<style lang="stylus">

@font-face {
  font-family 'Rubik'
  font-weight 300
  font-style 'italic'
  src url('assets/fonts/Rubik-LightItalic.ttf');
}

@font-face {
  font-family 'Rubik'
  font-weight 300
  font-style 'normal'
  src url('assets/fonts/Rubik-Light.ttf');
}

@font-face {
  font-family 'Rubik'
  font-weight 700
  font-style 'italic'
  src url('assets/fonts/Rubik-BoldItalic.ttf');
}

@font-face {
  font-family 'Rubik'
  font-weight 700
  font-style 'normal'
  src url('assets/fonts/Rubik-Bold.ttf');
}

@font-face {
  font-family 'Rubik'
  font-weight 500
  font-style 'italic'
  src url('assets/fonts/Rubik-MediumItalic.ttf');
}

@font-face {
  font-family 'Rubik'
  font-weight 500
  font-style 'normal'
  src url('assets/fonts/Rubik-Medium.ttf');
}

@font-face {
  font-family 'Rubik'
  font-weight 400
  font-style 'italic'
  src url('assets/fonts/Rubik-Italic.ttf');
}

@font-face {
  font-family 'Rubik'
  font-weight 400
  font-style 'normal'
  src url('assets/fonts/Rubik-Regular.ttf');
}

:root
  // --base #293241
  --base #fefdfa
  --primary-color #e29578
  --secondary-color #ffddd2
  --tertiary-color #FAEADE
  --quaternary-color #FDF7F2
  --quinary-color #ee6c4d

#app
  font-size var(--text1)
  font-family 'Rubik', sans-serif
  font-weight 300
  background-color var(--base)
  height 100vh
  transition width 2s

.text
  font-size var(--text1)

.heading
  font-size var(--heading1)

.app-grid
  display grid
  grid-template-columns auto 60px
  height 100vh

.content
  display grid
  grid-template-rows 100px 60px auto

.top
  background-color var(--quaternary-color)

.menu
  background-color var(--tertiary-color)

.main
  background-color var(--secondary-color)

.calendar
  display inline-grid
  text-align center

.month
  font-size 17px
  margin-bottom 4px

.days-grid
  display grid
  font-size 15px
  grid-template-columns repeat(7, 20px)

.sidebar
  background-color var(--primary-color)

html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, embed,
figure, figcaption, footer, header, hgroup,
menu, nav, output, ruby, section, summary,
time, mark, audio, video
  margin 0
  padding 0
  border 0
  font-size 100%
  font inherit
  vertical-align baseline

article, aside, details, figcaption, figure,
footer, header, hgroup, menu, nav, section
  display block

body
  line-height 1

ol, ul
  list-style none

blockquote, q
  quotes none

blockquote:before, blockquote:after,
q:before, q:after
  content ''
  content none

table
  border-collapse collapse
  border-spacing 0
</style>
