<template>
  <header>
    <nav :class="classHeader">
      <div id="brand">
        <div id="logo" />
        <div id="word-mark">
          <img src="@/assets/images/word.png" alt="work-mark" />
        </div>
      </div>
      <div id="menu">
        <div id="menu-toggle" @click="handleMenuToggle" :class="classMenuToggle">
          <div id="menu-icon">
            <div class="bar" />
            <div class="bar" />
            <div class="bar" />
          </div>
        </div>
        <ul class="ul-menu-toggle" :class="classShowMenu" id="ul-subMenu">
          <li>
            <router-link to="/">
              <h3>Home</h3>
            </router-link>
          </li>
          <li>
            <router-link to="/publish">
              <h3>Publish</h3>
            </router-link>
          </li>
          <li>
            <router-link to="/my-assets">
              <h3>MyAssets</h3>
            </router-link>
          </li>
          <li>
            <el-dropdown trigger="click" class="cursor-pointer">
              <h3 class="el-dropdown-link">
                My Account<i class="el-icon-arrow-down el-icon--right"></i>
              </h3>
              <el-dropdown-menu slot="dropdown">
                <el-dropdown-item>
                  <div>Account : {{ address }}</div>
                  <div>
                    Balance :
                    <p v-for="(token, index) in balance" :key="index" class="text-balance">
                      <span v-if="token.denom !== 'transfer/ruahosxkxc/uatom'">
                        {{ token.denom }} : {{ token.amount }}
                      </span>
                      <span v-if="token.denom === 'transfer/ruahosxkxc/uatom'">
                        Atom : {{ token.amount }}
                      </span>
                    </p>
                  </div>
                </el-dropdown-item>
              </el-dropdown-menu>
            </el-dropdown>
          </li>
        </ul>
      </div>
    </nav>
  </header>
</template>

<script>
import { mapState } from 'vuex';
export default {
  name: 'header-page',
  computed: {
    ...mapState('cosmos', ['address', 'balance'])
  },
  data() {
    return {
      classHeader: '',
      classMenuToggle: '',
      menuToggle: true,
      classShowMenu: ''
    };
  },
  created() {
    document.addEventListener('scroll', this.scrollPage);
  },
  destroyed() {
    document.removeEventListener('scroll', this.scrollPage);
  },
  methods: {
    handleMenuToggle() {
      this.menuToggle = !this.menuToggle;
      if (this.menuToggle) {
        this.classMenuToggle = 'closeMenu';
        if (this.classHeader) {
          this.classShowMenu = 'showMenu top-100';
        } else {
          this.classShowMenu = 'showMenu';
        }
      } else {
        this.classMenuToggle = '';
        this.classShowMenu = '';
      }
    },
    scrollPage() {
      if (window.scrollY > 200) {
        this.classHeader = 'navShadow';
        document.getElementById('ul-subMenu').classList.add('top-100');
      } else {
        this.classHeader = '';
        if (document.getElementById('ul-subMenu')) {
          document.getElementById('ul-subMenu').classList.remove('top-100');
        }
      }
    }
  }
};
</script>

<style lang="scss">
.float-right {
  float: right;
}

.float-left {
  float: left;
}

/*** Mixins & Default Styles ***/
@mixin object($width, $height, $bg) {
  width: $width;
  height: $height;
  background: $bg;
}
@mixin transPos($top, $right, $bottom, $left, $transX, $transY) {
  position: absolute;
  top: $top;
  left: $left;
  right: $right;
  bottom: $bottom;
  transform: translate($transX, $transY);
}
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  overflow-x: hidden;
  overflow-y: scroll;
}

/*** Color Variables ***/
$header-bg: #5661f2;
$nav-bg: #46b2f0;
$pink: #fa6c98;
$aqua: #79edfc;
$accent: #fff;

/*** Centering Hack ***/
@mixin center {
  display: flex;
  justify-content: center;
  align-items: center;
}

/*** Header Styles ***/
header {
  @include object(100vw, auto, $header-bg);
  display: flex;
}

/*** Navigation Styles ***/
nav {
  @include object(100vw, 120px, $nav-bg);
  display: grid;
  grid-template-columns: 1fr 1fr;
  position: fixed;
  z-index: 2001;
  transition: all 0.1s;
  &.navShadow {
    box-shadow: 0 4px 30px -5px rgba(#000, 0.2);
    height: 100px;
    #word-mark {
      opacity: 0;
    }
  }
}

#brand,
#menu,
header ul {
  display: flex;
  align-items: center;
}

#brand {
  padding-left: 40px;
}

#logo {
  @include object(55px, 55px, $accent);
  border-radius: 50%;
  cursor: pointer;
  background-image: url('../assets/images/logo.png');
  background-repeat: no-repeat;
  background-size: 100% 100%;
  box-shadow: 4px 8px 16px rgba(0, 0, 0, 0.3);
}

#word-mark {
  @include object(17%, auto, none);
  border-radius: 10px;
  margin-left: 5px;
  opacity: 1;
  transition: all 0.3s;
  padding: 0.35rem;
  img {
    width: 100%;
  }
}

/*** Menu Styles ***/
#menu {
  justify-content: flex-end;
  padding-right: 100px;
}
.ul-menu-toggle {
  text-align: center;
  li {
    margin-left: 25px;
    list-style: none;
    a {
      text-decoration: none;
    }
    a h3 {
      @include object(80px, auto, none);
      display: block;
      color: #324c84;
      font-weight: bold;
      border-radius: 90px;
    }
    a h3:hover {
      color: #000000;
    }

    .el-dropdown h3 {
      @include object(150px, auto, none);
      display: block;
      color: #324c84;
      font-weight: bold;
      border-radius: 90px;
    }
  }
}

#menu-toggle {
  @include object(55px, 55px, darken($nav-bg, 5%));
  @include center;
  border-radius: 50%;
  cursor: pointer;
  display: none;
  &:hover .bar {
    width: 25px;
  }
  &.closeMenu {
    .bar {
      width: 25px;
      &:first-child {
        transform: translateY(7px) rotate(45deg);
      }
      &:nth-child(2) {
        transform: scale(0);
      }
      &:last-child {
        transform: translateY(-7px) rotate(-45deg);
      }
    }
  }
}

.bar {
  @include object(25px, 2px, $accent);
  transition: 0.3s ease-in-out;
  &:nth-child(2) {
    width: 20px;
    margin: 5px 0;
  }
  &:last-child {
    width: 15px;
  }
}

/*** Hero Section Styles ***/
#hero-section {
  // background-color: $header-bg;
  // @include object(100vw, 70vh, null);
  padding: 0px 0px 300px 0px;
  @include center;
  background-image: url('../assets/images/wave.webp');
  background-repeat: no-repeat;
  background-size: 100% 100%;
  background-color: rgb(70, 178, 240);
  margin-top: 120px;
  height: 30rem;
}

#head-line {
  @include object(520px, 30px, $accent);
  border-radius: 90px;
  position: relative;
  &:before,
  &:after {
    content: '';
    height: 30px;
    border-radius: 90px;
  }
  &:before {
    @include object(360px, null, $accent);
    @include transPos(-60px, null, null, 50%, -50%, 0);
  }
  &:after {
    @include object(200px, null, $accent);
    @include transPos(null, null, -60px, 50%, -50%, 0);
  }
}

/*** Section Styles ***/
section {
  @include object(100vw, auto, null);
  display: flex;
  justify-content: center;
  // &:nth-child(odd) {
  //   background: $pink;
  // }
  // &:nth-child(even) {
  //   background: #c9f4f9;
  // }
}

#heading {
  @include object(120px, 20px, $accent);
  border-radius: 90px;
  margin-top: 40px;
}

/*** Responsive Menu For Smaller Device ***/

@media screen and (max-width: 767px) {
  #menu-toggle {
    display: flex;
  }
  .ul-menu-toggle {
    display: inline-block;
    @include object(100vw, 0, $aqua);
    @include transPos(120px, null, null, null, null, null);
    box-shadow: 0 5px 30px -4px rgba(#000, 0.2);
    transition: all 0.3s;
    background: #45afed;
    &.showMenu {
      height: 250px;
      left: 0;
      li {
        height: 80px;
        opacity: 1;
        visibility: visible;
        text-align: left;
      }
    }
  }
  .ul-menu-toggle li {
    @include object(50%, 80px, null);
    float: left;
    padding-left: 40px;
    opacity: 0;
    visibility: hidden;
    margin-left: 0;
    transition: all 0.3s 0.1s;
    &:first-child,
    &:nth-child(2) {
      margin-top: 80px;
    }
  }
  #head-line {
    transform: scale(0.8);
  }
  #word-mark {
    width: 70%;
  }

  #menu {
    padding-right: 25px;
  }
}

#youtube {
  position: fixed;
  right: 2vw;
  bottom: 2vh;
  font-size: 30px;
  color: #fff;
}

.top-100 {
  top: 100px;
}

#my-account {
  width: 120px !important;
  font-weight: bold;
  color: #324c84;
}

#my-account:hover {
  cursor: pointer;
}

.text-balance {
  margin-left: 60px;
}
</style>
