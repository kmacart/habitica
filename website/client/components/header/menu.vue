<template lang="pug">
div
  inbox-modal
  creator-intro
  profileModal
  report-flag-modal
  send-gems-modal
  b-navbar.topbar.navbar-inverse.static-top(toggleable="lg", type="dark", :class="navbarZIndexClass")
    b-navbar-brand.brand(aria-label="Habitica")
      .logo.svg-icon.d-none.d-xl-block(v-html="icons.logo")
      .svg-icon.gryphon.d-xs-block.d-xl-none
    b-navbar-toggle(target='menu_collapse').menu-toggle
    .quick-menu.mobile-only.form-inline
      a.item-with-icon(@click="sync", v-b-tooltip.hover.bottom="$t('sync')", :aria-label="$t('sync')")
        .top-menu-icon.svg-icon(v-html="icons.sync")
      notification-menu.item-with-icon
      user-dropdown.item-with-icon
    b-collapse#menu_collapse(v-model="menuIsOpen").collapse.navbar-collapse
      b-navbar-nav.menu-list
        b-nav-item.topbar-item(:class="{'active': $route.path === '/'}" tag="li", :to="{name: 'tasks'}", exact) {{ $t('tasks') }}
        li.topbar-item(:class="{'active': $route.path.startsWith('/inventory'), 'down': $route.path.startsWith('/inventory') && this.isDesktop()}").droppable
          .chevron.rotate(@click='dropdownMobile($event)')
            .chevron-icon-down(v-html="icons.chevronDown", v-once)
          router-link.nav-link(:to="{name: 'items'}") {{ $t('inventory') }}
          .topbar-dropdown
            router-link.topbar-dropdown-item.dropdown-item(:to="{name: 'items'}", exact) {{ $t('items') }}
            router-link.topbar-dropdown-item.dropdown-item(:to="{name: 'equipment'}") {{ $t('equipment') }}
            router-link.topbar-dropdown-item.dropdown-item(:to="{name: 'stable'}") {{ $t('stable') }}
        li.topbar-item(:class="{'active': $route.path.startsWith('/shop'), 'down': $route.path.startsWith('/shop') && this.isDesktop()}").droppable
          .chevron.rotate(@click='dropdownMobile($event)')
            .chevron-icon-down(v-html="icons.chevronDown", v-once)
          router-link.nav-link(:to="{name: 'market'}") {{ $t('shops') }}
          .topbar-dropdown
            router-link.topbar-dropdown-item.dropdown-item(:to="{name: 'market'}", exact) {{ $t('market') }}
            router-link.topbar-dropdown-item.dropdown-item(:to="{name: 'quests'}") {{ $t('quests') }}
            router-link.topbar-dropdown-item.dropdown-item(:to="{name: 'seasonal'}") {{ $t('titleSeasonalShop') }}
            router-link.topbar-dropdown-item.dropdown-item(:to="{name: 'time'}") {{ $t('titleTimeTravelers') }}
        b-nav-item.topbar-item(:class="{'active': $route.path.startsWith('/party')}" tag="li", :to="{name: 'party'}", v-if='this.user.party._id') {{ $t('party') }}
        b-nav-item.topbar-item(:class="{'active': $route.path.startsWith('/party')}" @click='openPartyModal()', v-if='!this.user.party._id') {{ $t('party') }}
        li.topbar-item(:class="{'active': $route.path.startsWith('/groups'), 'down': $route.path.startsWith('/groups') && this.isDesktop()}").droppable
          .chevron.rotate(@click='dropdownMobile($event)')
            .chevron-icon-down(v-html="icons.chevronDown", v-once)
          router-link.nav-link(:to="{name: 'tavern'}") {{ $t('guilds') }}
          .topbar-dropdown
            router-link.topbar-dropdown-item.dropdown-item(:to="{name: 'tavern'}") {{ $t('tavern') }}
            router-link.topbar-dropdown-item.dropdown-item(:to="{name: 'myGuilds'}") {{ $t('myGuilds') }}
            router-link.topbar-dropdown-item.dropdown-item(:to="{name: 'guildsDiscovery'}") {{ $t('guildsDiscovery') }}
        li.topbar-item(:class="{'active': $route.path.startsWith('/group-plans'), 'down': $route.path.startsWith('/group-plans') && this.isDesktop()}").droppable
          .chevron.rotate(v-if="groupPlans.length > 0", @click='dropdownMobile($event)')
            .chevron-icon-down(v-html="icons.chevronDown", v-once)
          router-link.nav-link(:to="{name: 'groupPlan'}") {{ $t('group') }}
          .topbar-dropdown
            router-link.topbar-dropdown-item.dropdown-item(v-for='group in groupPlans', :key='group._id', :to="{name: 'groupPlanDetailTaskInformation', params: {groupId: group._id}}") {{ group.name }}
        li.topbar-item(:class="{'active': $route.path.startsWith('/challenges'), 'down': $route.path.startsWith('/challenges') && this.isDesktop()}").droppable
          .chevron.rotate(@click='dropdownMobile($event)')
            .chevron-icon-down(v-html="icons.chevronDown", v-once)
          router-link.nav-link(:to="{name: 'myChallenges'}") {{ $t('challenges') }}
          .topbar-dropdown
            router-link.topbar-dropdown-item.dropdown-item(:to="{name: 'myChallenges'}") {{ $t('myChallenges') }}
            router-link.topbar-dropdown-item.dropdown-item(:to="{name: 'findChallenges'}") {{ $t('findChallenges') }}
        li.topbar-item(:class="{'active': $route.path.startsWith('/help'), 'down': $route.path.startsWith('/help') && this.isDesktop()}").droppable
          .chevron.rotate(@click='dropdownMobile($event)')
            .chevron-icon-down(v-html="icons.chevronDown", v-once)
          router-link.nav-link(:to="{name: 'faq'}") {{ $t('help') }}
          .topbar-dropdown
            router-link.topbar-dropdown-item.dropdown-item(:to="{name: 'faq'}") {{ $t('faq') }}
            router-link.topbar-dropdown-item.dropdown-item(:to="{name: 'overview'}") {{ $t('overview') }}
            router-link.topbar-dropdown-item.dropdown-item(to="/groups/guild/a29da26b-37de-4a71-b0c6-48e72a900dac") {{ $t('reportBug') }}
            router-link.topbar-dropdown-item.dropdown-item(to="/groups/guild/5481ccf3-5d2d-48a9-a871-70a7380cee5a") {{ $t('askAQuestion') }}
            a.topbar-dropdown-item.dropdown-item(href="https://trello.com/c/odmhIqyW/440-read-first-table-of-contents", target='_blank') {{ $t('requestAF') }}
            a.topbar-dropdown-item.dropdown-item(href="http://habitica.fandom.com/wiki/Contributing_to_Habitica", target='_blank') {{ $t('contributing') }}
            a.topbar-dropdown-item.dropdown-item(href="http://habitica.fandom.com/wiki/Habitica_Wiki", target='_blank') {{ $t('wiki') }}
            a.topbar-dropdown-item.dropdown-item(@click='modForm()') {{ $t('contactForm') }}
      .currency-tray.form-inline
        .item-with-icon(v-if="userHourglasses > 0")
          .top-menu-icon.svg-icon(v-html="icons.hourglasses", v-b-tooltip.hover.bottom="$t('mysticHourglassesTooltip')")
          span {{ userHourglasses }}
        .item-with-icon
          a.top-menu-icon.svg-icon.gem(:aria-label="$t('gems')", href="#buy-gems" v-html="icons.gem", @click.prevent='showBuyGemsModal("gems")', v-b-tooltip.hover.bottom="$t('gems')")
          span {{userGems}}
        .item-with-icon.gold
          .top-menu-icon.svg-icon(:aria-label="$t('gold')", v-html="icons.gold", v-b-tooltip.hover.bottom="$t('gold')")
          span {{Math.floor(user.stats.gp * 100) / 100}}
      .form-inline.desktop-only
        a.item-with-icon(@click="sync", @keyup.enter="sync", role="link", :aria-label="$t('sync')", tabindex="0", v-b-tooltip.hover.bottom="$t('sync')")
          .top-menu-icon.svg-icon(v-html="icons.sync")
        notification-menu.item-with-icon
        user-dropdown.item-with-icon
</template>

<style lang="scss" scoped>
  @import '~client/assets/scss/colors.scss';
  @import '~client/assets/scss/utils.scss';

  @media only screen and (max-width: 1200px) {
    .chevron {
      display: none
    }

    .gryphon {
      background-image: url('~assets/images/melior@3x.png');
      width: 30px;
      height: 30px;
      background-size: cover;
      color: $white;
      margin: 0 auto;
    }

    .topbar-item {
      font-size: 14px !important;
    }
  }

  @media only screen and (min-width: 992px) {
    .chevron {
      display: none
    }

    .mobile-only {
      display: none !important;
    }

    .topbar {
      max-height: 56px;

      .currency-tray {
        margin-left: auto;
      }

      .topbar-item {
        padding-top: 5px;
        height: 56px;

        &:hover {
          background: $purple-200;
        }

        &.active:not(:hover) {
          box-shadow: 0px -4px 0px $purple-300 inset;
        }
      }

      .topbar-dropdown {
        position: absolute;
      }
    }
  }

  @media only screen and (max-width: 992px) {
    .brand {
      margin: 0;
    }

    .gryphon {
      position: absolute;
      left: calc(50% - 30px);
      top: 10px;
    }

    #menu_collapse {
      margin: 0.6em -16px -8px;
      overflow: auto;
      flex-direction: column;
      background-color: $purple-100;

      .menu-list {
        width: 100%;
        order: 1;
        text-align: center;

        .topbar-dropdown  {
          transition: max-height 0.25s ease;
        }

        .topbar-dropdown-item {
          background: #432874;
          border-bottom: #6133b4 solid 1px;
        }

        .chevron {
          width: 20%;
          height: 42px;
          position: absolute;
          right: 0;
          top: 0;
          display: block;
        }

        .chevron-icon-down {
          width: 14px;
          top: 11px;
          right: 12px;
          position: absolute;
          display: block;
          transition: transform 0.25s ease;
        }

        .down .rotate .chevron-icon-down {
          transform: rotate(-180deg);
          }

        .topbar-item {
          position: relative;

          &.active {
            background: #6133b4;
          }

          background: #4f2a93;
          border-bottom: #6133b4 solid 1px;
        }
      }
    }

    .currency-tray {
      justify-content: center;
      min-height: 40px;
      background: #271b3d;
      width: 100%;
    }

    .desktop-only {
      display: none !important;
    }
  }

  .menu-toggle {
    border: none;
  }

  #menu_collapse {
    display: flex;
    justify-content: space-between;
  }

  .topbar {
    background: $purple-100 url(~assets/svg/for-css/bits.svg) right top no-repeat;
    min-height: 56px;
    box-shadow: 0 1px 2px 0 rgba($black, 0.24);

    a {
      color: white !important;
    }
  }

  .navbar-z-index {
    &-normal {
      z-index: 1080;
    }

    &-modal {
      z-index: 1035;
    z-index: 1042; // To stay above snakbar notifications and modals
    }
  }

  .logo {
    padding-left: 10px;
    width: 128px;
    height: 28px;
  }

  .quick-menu {
    display: flex;
    margin-left: auto;
  }

  .currency-tray {
    display: flex;
  }

  .topbar-item {
    font-size: 16px;
    color: $white !important;
    font-weight: bold;
    transition: none;

    .topbar-dropdown  {
        overflow: hidden;
        max-height: 0;

        .topbar-dropdown-item {
          line-height: 1.5;
          font-size: 16px;
        }
    }

    >a {
      padding: .8em 1em !important;
    }

    &.down {
      color: $white !important;
      background: $purple-200;

      .topbar-dropdown {
        margin-top: 0; // Remove gap between navbar and drop-down.
        background: $purple-200;
        border-radius: 0px;
        border: none;
        box-shadow: none;
        padding: 0px;

        border-bottom-right-radius: 5px;
        border-bottom-left-radius: 5px;

        .topbar-dropdown-item {
          font-size: 16px;
          box-shadow: none;
          color: $white;
          border: none;
          line-height: 1.5;
          display: list-item;

          &.active {
            background: $purple-300;
          }

          &:hover {
            background: $purple-300;

            &:last-child {
              border-bottom-right-radius: 5px;
              border-bottom-left-radius: 5px;
            }
          }
        }
      }
    }
  }

  .dropdown + .dropdown {
    margin-left: 0px;
  }

  .item-with-icon {
    color: $white;
    font-size: 16px;
    font-weight: normal;
    white-space: nowrap;

    span {
      font-weight: bold;
    }

    &.gold {
      margin-right: 24px;
    }

    &:focus /deep/ .top-menu-icon.svg-icon,
    &:hover /deep/ .top-menu-icon.svg-icon {
      color: $white;
    }

    & /deep/ .top-menu-icon.svg-icon {
      color: $header-color;
      vertical-align: bottom;
      display: inline-block;
      width: 24px;
      height: 24px;
      margin-right: 12px;
      margin-left: 12px;
    }
  }

  .menu-icon {
    margin-left: 24px;
  }

  .gem:hover {
    cursor: pointer;
  }

  .message-count {
    background-color: $blue-50;
    border-radius: 50%;
    height: 20px;
    width: 20px;
    float: right;
    color: $white;
    text-align: center;
    font-weight: bold;
    font-size: 12px;
  }

  .message-count.top-count {
    background-color: $red-50;
    position: absolute;
    right: 0;
    top: -0.5em;
    padding: .2em;
  }
</style>

<script>
import { mapState, mapGetters } from 'client/libs/store';
import * as Analytics from 'client/libs/analytics';
import { goToModForm } from 'client/libs/modform';

import gemIcon from 'assets/svg/gem.svg';
import goldIcon from 'assets/svg/gold.svg';
import syncIcon from 'assets/svg/sync.svg';
import svgHourglasses from 'assets/svg/hourglass.svg';
import chevronDownIcon from 'assets/svg/chevron-down.svg';
import logo from 'assets/svg/logo.svg';

import creatorIntro from '../creatorIntro';
import InboxModal from '../userMenu/inbox.vue';
import notificationMenu from './notificationsDropdown';
import profileModal from '../userMenu/profileModal';
import reportFlagModal from '../chat/reportFlagModal';
import sendGemsModal from 'client/components/payments/sendGemsModal';
import sync from 'client/mixins/sync';
import userDropdown from './userDropdown';


export default {
  components: {
    creatorIntro,
    InboxModal,
    notificationMenu,
    profileModal,
    reportFlagModal,
    sendGemsModal,
    userDropdown,
  },
  mixins: [sync],
  data () {
    return {
      isUserDropdownOpen: false,
      menuIsOpen: false,
      icons: Object.freeze({
        gem: gemIcon,
        gold: goldIcon,
        hourglasses: svgHourglasses,
        sync: syncIcon,
        logo,
        chevronDown: chevronDownIcon,
      }),
    };
  },
  computed: {
    ...mapGetters({
      userGems: 'user:gems',
    }),
    ...mapState({
      user: 'user.data',
      userHourglasses: 'user.data.purchased.plan.consecutive.trinkets',
      groupPlans: 'groupPlans',
      modalStack: 'modalStack',
    }),
    navbarZIndexClass () {
      if (this.modalStack.length > 0) {
        return 'navbar-z-index-modal';
      }
      return 'navbar-z-index-normal';
    },
  },
  mounted () {
    this.getUserGroupPlans();
    Array.from(document.getElementById('menu_collapse').getElementsByTagName('a')).forEach(link => {
      link.addEventListener('click', this.closeMenu);
    });
    Array.from(document.getElementsByClassName('topbar-item')).forEach(link => {
      link.addEventListener('mouseenter', this.dropdownDesktop);
      link.addEventListener('mouseleave', this.dropdownDesktop);
    });
  },
  methods: {
    modForm () {
      goToModForm(this.user);
    },
    toggleUserDropdown () {
      this.isUserDropdownOpen = !this.isUserDropdownOpen;
    },
    async getUserGroupPlans () {
      this.$store.state.groupPlans = await this.$store.dispatch('guilds:getGroupPlans');
    },
    openPartyModal () {
      this.$root.$emit('bv::show::modal', 'create-party-modal');
    },
    showBuyGemsModal (startingPage) {
      this.$store.state.gemModalOptions.startingPage = startingPage;

      Analytics.track({
        hitType: 'event',
        eventCategory: 'button',
        eventAction: 'click',
        eventLabel: 'Gems > Toolbar',
      });

      this.$root.$emit('bv::show::modal', 'buy-gems', {alreadyTracked: true});
    },
    dropdownDesktop (hover) {
      if (this.isDesktop() && hover.target.classList.contains('droppable')) {
        this.dropdown(hover.target);
      }
    },
    dropdownMobile (click) {
      this.dropdown(click.currentTarget.parentElement);
    },
    dropdown (element) {
      let droppedElement = document.getElementsByClassName('down')[0];
      if (droppedElement && droppedElement !== element) {
        droppedElement.classList.remove('down');
        droppedElement.lastChild.style.maxHeight = 0;
      }

      element.classList.toggle('down');
      element.lastChild.style.maxHeight = element.classList.contains('down') ? `${element.lastChild.scrollHeight}px` : 0;
    },
    closeMenu () {
      if (this.isMobile()) {
        this.menuIsOpen = false;

        Array.from(document.getElementsByClassName('droppable')).forEach(droppableElement => {
          droppableElement.classList.remove('down');
          droppableElement.lastChild.style.maxHeight = 0;
        });
      }
    },
    isMobile () {
      return document.documentElement.clientWidth < 992;
    },
    isDesktop () {
      return !this.isMobile();
    },
  },
};
</script>
