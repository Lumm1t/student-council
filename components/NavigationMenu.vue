<template>
  <div :class="{ 'nav-active': isActive }">
    <header class="cd-header">
      <div class="nav-but-wrap">
        <div class="menu-icon hover-target" @click="openMenu">
          <span class="menu-icon__line menu-icon__line-left"></span>
          <span class="menu-icon__line"></span>
          <span
            class="menu-icon__line menu-icon__line-last menu-icon__line-right"
          >
          </span>
        </div>
      </div>

      <div class="nav">
        <div class="nav__content">
          <ul class="nav__list" @click="isActive = false">
            <li v-for="(link, i) in links" :key="i" class="nav__list-item">
              <nuxt-link :to="link.reference" class="hover-target">
                {{ link.name }}
              </nuxt-link>
            </li>
          </ul>
        </div>
      </div>
    </header>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isActive: false,
      links: [
        {
          name: 'Strona główna',
          reference: '/',
        },
        {
          name: 'Zespół',
          reference: '/team',
        },
        {
          name: 'Projekty',
          reference: '/projects',
        },
        {
          name: 'Kontakt',
          reference: '/contact',
        },
      ],
    };
  },
  mounted() {
    const mobileAndTabletCheck = () =>
      /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(
        navigator.userAgent
      );

    if (!mobileAndTabletCheck()) this.isActive = true;
  },
  methods: {
    openMenu() {
      this.isActive = !this.isActive;
    },
  },
};
</script>

<style lang="scss" scoped>
/* stylelint-disable */
@import './assets/sass/variables';

$radius: 30px;

.cd-header {
  position: fixed;
  height: 100%;
  width: 100%;
  max-width: 1500px;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  z-index: 9999;
  display: grid;
  grid-template-columns: 1fr 80px;
  grid-template-rows: 80px;
  grid-column-gap: 0;
  grid-row-gap: 0;
  pointer-events: none;
}

.nav-but-wrap {
  grid-area: 1 / 2 / 2 / 3;
  position: relative;
  pointer-events: all;
  display: inline-flex;
  float: right;
  justify-content: center;
  height: 80px;
  width: 80px;
  transition: border-radius linear 0.8s, background-color 0.3s ease;
  border-bottom-left-radius: $radius;
  background-color: var(--backgroundColor-primary);

  @media (min-width: 1500px) {
    border-bottom-right-radius: $radius;
  }

  &::before {
    content: '';
    width: 80px;
    height: 80px;
    border-bottom-left-radius: $radius;
    z-index: -10;
    position: absolute;
    box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.5);
    transition: border-radius linear 0.8s, box-shadow 0.3s ease;
    opacity: 1;

    @media (min-width: 1500px) {
      border-bottom-right-radius: $radius;
    }
  }
}

.menu-icon {
  width: 30px;
  position: relative;
  z-index: 2;
  cursor: pointer;
  align-self: center;
  display: block;

  &__line {
    height: 2px;
    width: 30px;
    display: block;
    background-color: var(--backgroundColor-secondary);
    margin-bottom: 7px;
    cursor: pointer;
    transition: transform 0.2s ease, background-color 0.5s ease;

    &-last {
      margin-bottom: 0;
    }

    &-left {
      width: 16.5px;
      transition: all 200ms linear;
    }

    &-right {
      width: 16.5px;
      float: right;
      transition: all 200ms linear;
    }
  }

  &:hover {
    .menu-icon__line-left,
    .menu-icon__line-right {
      width: 30px;
    }
  }
}

.nav {
  position: fixed;
  z-index: 1;
  pointer-events: all;

  &::before,
  &::after {
    content: '';
    position: fixed;
    top: 10px;
    right: 10px;
    width: 0;
    height: 0;
    background-color: rgba(20, 21, 26, 0.6);
    border-bottom-left-radius: 200%;
    z-index: -1;
    transition: border-radius ease 0.8s,
      width cubic-bezier(0.77, 0, 0.175, 1) 0.6s,
      height cubic-bezier(0.77, 0, 0.175, 1) 0.6s;
  }

  &::after {
    background-color: var(--backgroundColor-primary);
    background-position: bottom center;
    background-repeat: no-repeat;
    background-size: 300%;
    transition-delay: 0s;
    box-shadow: 6px 7px 28px 0 rgba(16, 16, 16, 0.3);
  }

  &::before {
    transition-delay: 0.2s;
  }

  &__content {
    position: fixed;
    visibility: hidden;
    top: 90px;
    right: 10px;
    width: 300px;
    text-align: left;
  }

  &__list {
    position: relative;
    z-index: 2;

    &-item {
      position: relative;
      display: block;
      opacity: 0;
      text-align: left;
      color: #ffffff;
      overflow: hidden;
      font-size: 22px;
      line-height: 1.2;
      letter-spacing: 2px;
      transform: translate(30px, 0%);
      transition: opacity 0.1s ease, transform 0.2s ease;
      transition-delay: 0.2s;
      margin-bottom: 7px;

      a {
        position: relative;
        text-decoration: none;
        color: var(--textColor);
        overflow: hidden;
        cursor: pointer;
        font-weight: 600;
        z-index: 2;
        height: 28px;
        padding-left: 40px;
        margin: 5px;
        display: inline-block;
        transition: all 200ms linear;

        &::after {
          position: absolute;
          content: '';
          top: 50%;
          left: 0;
          width: 5px;
          height: 0;
          opacity: 0;
          background: linear-gradient(288deg, $secondColor, $mainColor);
          z-index: 1;
          transition: all 200ms linear;
        }

        &:hover::after {
          height: 100%;
          opacity: 1;
          top: 0;
        }
      }

      &:not(.active-nav) a:hover {
        color: $mainColor;
        font-size: 23px;
        transition: color 100ms ease-in-out, font-size 100ms ease-in-out;
      }

      &.active-nav {
        a {
          background: linear-gradient(288deg, $secondColor, $mainColor);
          background-clip: text;
          font-size: 25px;

          &::after {
            height: 100%;
            opacity: 1;
            top: 0;
          }
        }
      }
    }
  }

  &-active {
    .nav__content {
      visibility: visible;
    }

    .menu-icon {
      &:hover {
        .menu-icon__line-left,
        .menu-icon__line-right {
          width: 15px;
        }
      }

      &__line {
        background-color: var(--backgroundColor-secondary);
        transform: translate(0, 0) rotate(-45deg);

        &-left {
          width: 15px;
          transform: translate(2px, 4px) rotate(45deg);
        }

        &-right {
          width: 15px;
          float: right;
          transform: translate(-3px, -3.5px) rotate(45deg);
        }
      }
    }

    .nav {
      visibility: visible;

      &::before,
      &::after {
        width: 300px;
        height: 300px;
        border-radius: 15px;
      }

      &::before {
        transition-delay: 0s;
      }

      &::after {
        transition-delay: 0.1s;
      }

      &__list-item {
        opacity: 1;
        transform: translateX(0%);
        transition: opacity 0.3s ease, transform 0.3s ease, color 0.3s ease;

        $delay: 0.4;
        @for $link from 0 through 4 {
          $delay: $delay + 0.1;

          &:nth-child(#{$link}) {
            transition-delay: #{$delay}s;
          }
        }
      }
    }
  }
}
</style>
