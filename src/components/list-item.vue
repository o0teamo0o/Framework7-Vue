<script>
  export default {
    render: function (c) {
      var liChildren, linkEl, itemContentEl;
      var self = this;

      // Item Content
      itemContentEl = c('f7-list-item-content', {
        props: {
          'title': self.title,
          'text': self.text,
          'media': self.media,
          'subtitle': self.subtitle,
          'after': self.after,
          'badge': self.badge,
          'badge-color': self.badgeColor,
          'media-list': self.mediaListComputed,
          'accordion-item': self.accordionItem,

          'checkbox': self.checkbox,
          'checked': self.checked,
          'radio': self.radio,
          'name': self.name,
          'value': self.value,
          'readonly': self.readonly,
          'required': self.required,
          'disabled': self.disabled
        },
        on: (self.link || self.accordionItem || self.smartSelect) ? {} : {click: self.onClick, change: self.onChange}
      }, [self.$slots['content-start'], self.$slots.content, self.$slots['media-start'], self.$slots.media, self.$slots['inner-start'], self.$slots.inner, self.$slots['after-start'], self.$slots.after, (self.swipeout || self.accordionItem ? [] : self.$slots.default)]);

      // Link
      if (self.link || self.accordionItem || self.smartSelect) {
        linkEl = c('a', {
          attrs: {
            href: self.link === true || self.accordionItem || self.smartSelect ? '#' : self.link,
            'data-searchbar': self.smartSelectSearchbar,
            'data-searchbar-paceholder': self.smartSelectSearchbarPlaceholder,
            'data-searchbar-cancel': self.smartSelectSearchbarCancel,
            'data-page-title': self.smartSelectPageTitle,
            'data-back-text': self.smartSelectBackText,
            'data-back-on-select': self.smartSelectBackOnSelect,
            'data-virtual-list': self.smartSelectVirtualList,
            'data-virtual-list-height': self.smartSelectVirtualListHeight,
            'data-open-in': self.smartSelectOpenIn,
            'data-navbar-theme': self.smartSelectNavbarTheme,
            'data-form-theme': self.smartSelectFormTheme,

            'data-view': typeof self.linkView === 'string' ? self.linkView : false,
            'data-panel': typeof self.linkOpenPanel === 'string' ? self.linkOpenPanel : false,
            'data-popup': typeof self.linkOpenPopup === 'string' ? self.linkOpenPopup : false,
            'data-popover': typeof self.linkOpenPopover === 'string' ? self.linkOpenPopover : false,
            'data-picker': typeof self.linkOpenPicker === 'string' ? self.linkOpenPicker : false,
            'data-login-screen': typeof self.linkOpenLoginScreen === 'string' ? self.linkOpenLoginScreen : false,
            'data-sortable': typeof self.linkOpenSortable === 'string' ? self.linkOpenSortable : (typeof self.linkToggleSortable === 'string' ? self.linkToggleSortable : false),

            'data-force': self.linkForce,
            'data-reload': self.linkReload,
            'data-animate-pages': self.linkAnimatePages,
            'data-ignore-cache': self.linkIgnoreCache,
            'data-page-name': typeof self.linkPageName === 'string' ? self.linkPageName : false,
            'data-template': typeof self.linkTemplate === 'string' ? self.linkTemplate : false,
          },
          'class': {
            'item-link': true,
            'external': self.linkExternal,
            'back': self.linkBack,
            'no-fastclick': self.linkNoFastclick,
            'smart-select': self.smartSelect,
            'close-panel': self.linkClosePanel,
            'open-panel': self.linkOpenPanel,
            'close-popup': self.linkClosePopup,
            'open-popup': self.linkOpenPopup,
            'close-popover': self.linkClosePopover,
            'open-popover': self.linkOpenPopover,
            'close-picker': self.linkClosePicker,
            'open-picker': self.linkOpenPicker,
            'close-login-screen': self.linkCloseLoginScreen,
            'open-login-screen': self.linkOpenLoginScreen,
            'close-sortable': self.linkCloseSortable,
            'open-sortable': self.linkOpenSortable,
            'toggle-sortable': self.linkToggleSortable,
          },
          on: {
            click: self.onClick
          }
        }, [itemContentEl])
      }

      if (self.dividerOrGroupTitle) {
        liChildren = [c('span', self.$slots.default || self.title)]
      }
      else {
        var linkItemEl = (self.link || self.smartSelect || self.accordionItem) ? linkEl : itemContentEl;
        if (self.swipeout) {
          liChildren = [c('div', {'class':{'swipeout-content': true}}, [linkItemEl])]
        }
        else {
          liChildren = [linkItemEl];
        }
        if (self.sortableComputed) {
          liChildren.push(c('div', {'class': {'sortable-handler': true}}));
        }
        if (self.swipeout || self.accordionItem) {
          liChildren.push(self.$slots.default);
        }
        liChildren.unshift(self.$slots['root-start']);
        liChildren.push(self.$slots.root);
      }

      return c(
        'li',
        {
          'class': {
            'item-divider' : self.divider,
            'list-group-title': self.groupTitle,
            'swipeout': self.swipeout,
            'accordion-item': self.accordionItem
          },
          on: {
            'swipeout:open': self.onSwipeoutOpen,
            'swipeout:opened': self.onSwipeoutOpened,
            'swipeout:close': self.onSwipeoutClose,
            'swipeout:closed': self.onSwipeoutClosed,
            'swipeout:delete': self.onSwipeoutDelete,
            'swipeout:deleted': self.onSwipeoutDeleted,
            'swipeout': self.onSwipeout,
            'accordion:open': self.onAccOpen,
            'accordion:opened': self.onAccOpened,
            'accordion:close': self.onAccClose,
            'accordion:closed': self.onAccClosed,
          }
        },
        liChildren
      )
    },
    props: {
      'title': [String, Number],
      'text': [String, Number],
      'media': String,
      'subtitle': [String, Number],

      // Link Props
      'link': [String, Boolean],
      'link-external': Boolean,
      'link-back': Boolean,
      'link-no-fastclick': Boolean,

      'link-force': Boolean,
      'link-reload': Boolean,
      'link-animate-pages': Boolean,
      'link-ignore-cache': Boolean,
      'link-page-name': String,
      'link-template': String,

      'link-view': String,
      'link-open-panel': [String, Boolean],
      'link-close-panel': Boolean,
      'link-open-popup': [String, Boolean],
      'link-close-popup': Boolean,
      'link-open-popover': [String, Boolean],
      'link-close-popover': Boolean,
      'link-open-login-screen': [String, Boolean],
      'link-close-login-screen': Boolean,
      'link-open-picker': [String, Boolean],
      'link-close-picker': Boolean,

      'after': [String, Number],
      'badge': [String, Number],
      'badge-color': String,
      'media-item': Boolean,
      'media-list-item': Boolean,
      'media-list': Boolean,
      'media-list-computed': Boolean,
      'divider': Boolean,
      'group-title': Boolean,
      'divider-or-group-title': Boolean,
      'swipeout': Boolean,
      'sortable': Boolean,
      'sortable-computed': Boolean,
      'accordion-item': Boolean,

      // Smart Select
      'smart-select': Boolean,
      'smart-select-searchbar': Boolean,
      'smart-select-searchbar-paceholder': String,
      'smart-select-searchbar-cancel': String,
      'smart-select-page-title': String,
      'smart-select-back-text': String,
      'smart-select-back-on-select': Boolean,
      'smart-select-virtual-list': Boolean,
      'smart-select-virtual-list-height': Number,
      'smart-select-open-in': String, //popup or picker or page
      'smart-select-navbar-theme': String,
      'smart-select-form-theme': String,

      // Inputs
      'checkbox': Boolean,
      'checked': Boolean,
      'radio': Boolean,
      'name': String,
      'value': [String, Number],
      'readonly': Boolean,
      'required': Boolean,
      'disabled': Boolean
    },
    computed: {
      dividerOrGroupTitle: function () {
        return this.divider || this.groupTitle;
      },
      sortableComputed: function () {
        return this.sortable || this.$parent.sortable || this.$parent.sortableComputed;
      },
      mediaListComputed: function () {
        return this.mediaList || this.mediaItem || this.$parent.mediaList || this.$parent.mediaListComputed;
      }
    },
    methods: {
      onClick: function (event) {
        this.$emit('click', event)
      },
      onSwipeoutDeleted: function (event) {
        this.$emit('swipeout:deleted', event)
      },
      onSwipeoutDelete: function (event) {
        this.$emit('swipeout:delete', event)
      },
      onSwipeoutClose: function (event) {
        this.$emit('swipeout:close', event)
      },
      onSwipeoutClosed: function (event) {
        this.$emit('swipeout:closed', event)
      },
      onSwipeoutOpen: function (event) {
        this.$emit('swipeout:open', event)
      },
      onSwipeoutOpened: function (event) {
        this.$emit('swipeout:opened', event)
      },
      onSwipeout: function (event) {
        this.$emit('swipeout', event)
      },
      onAccClose: function (event) {
        this.$emit('accordion:close', event)
      },
      onAccClosed: function (event) {
        this.$emit('accordion:closed', event)
      },
      onAccOpen: function (event) {
        this.$emit('accordion:open', event)
      },
      onAccOpened: function (event) {
        this.$emit('accordion:opened', event)
      },
      onChange: function (event) {
        this.$emit('change', event)
      }
    }
  }
</script>