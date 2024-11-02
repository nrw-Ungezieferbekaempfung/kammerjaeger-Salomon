/**  FOXIZ MAN SCRIPT */
var FOXIZ_MAIN_SCRIPT = (function (Module, $) {
        'use strict';

        Module.initParams = function () {
            this.themeSettings = foxizParams || {};
            this.ajaxData = {};
            this.siteAccessFlag = false;
            this._document = $(document);
            this._body = $('body');
            this._window = $(window);
            this.html = $('html, body');
            this.outerHTML = $('html');
            this.iOS = /(iPad|iPhone|iPod)/g.test(navigator.userAgent);
            this.wPoint = {};
            this.sticky = {};
            this.eSticky = {};
            this.YTPlayers = {};
            this.articleData = [];
            this.readIndicatorPercent = 0;
            this.isProgress = false;
            this.readIndicator = $('#reading-progress');
            if (typeof foxizCoreParams !== 'undefined') {
                this.ajaxURL = foxizCoreParams.ajaxurl || '';
            } else {
                this.ajaxURL = '';
            }
            if (typeof FOXIZ_CORE_SCRIPT !== 'undefined') {
                this.personailizeUID = FOXIZ_CORE_SCRIPT.personailizeUID;
            }
        }

        Module.init = function () {
            this.initParams();
            this.syncLayoutLike();
            this.topSpacing();
            this.siteAccessDetector();
            this.headerDropdown();
            this.mobileCollapse();
            this.initSubMenuPos();
            this.privacyTrigger();
            this.popupNewsletter();
            this.documentClick();
            this.backTop();
            this.readIndicatorInit();
            this.fontResizer();
            this.breakingNews();
            this.sliders();
            this.carousels();
            this.liveSearch();
            this.personalizeBlocks();
            this.personalizeCategories();
            this.likeToggle();
            this.singleInfiniteLoadNext();
            this.loginPopup();
            this.loadYoutubeIframe();
            this.browserResize();
            this.footerSlideUp();
            this.cartNotification();
            this.newsNotification();
            this.tocToggle();
            this.delayLoad();
            this.showPostComment();
            this.replyReview();
            this.paginationNextPrev();
            this.paginationLoadMore();
            this.paginationInfinite();
            this.productQuantity();
            this.readingCollect();
            this.neededReloadFuncs();
        }

        Module.neededReloadFuncs = function () {
            this.hoverTipsy();
            this.scrollToComment();
            this.usersRating();
            this.singleGallery();
            this.floatingVideo();
            this.videoPreview();
            this.floatingVideoRemove();
            this.scrollTableContent();
            this.singleScrollRefresh();
            this.playerAutoPlay();
            this.hoverEffects();
            this.highlightShares();
            this.galleryLightbox();
            this.singleFeaturedLightbox();
        }

        Module.reInitAll = function () {

            this._window.trigger('load');
            this.syncLayoutLike();
            if (typeof FOXIZ_PERSONALIZE !== 'undefined') {
                FOXIZ_PERSONALIZE.syncPersonalizeLayout();
            }
            if (typeof RB_REACTION !== 'undefined') {
                RB_REACTION.syncReactLayout();
            }
            this.neededReloadFuncs();
            Waypoint.refreshAll();
        }

        /** sync layout & reload features */
        Module.reloadBlockFunc = function () {
            this._window.trigger('load');
            if (typeof FOXIZ_PERSONALIZE !== 'undefined') {
                FOXIZ_PERSONALIZE.syncLayoutBookmarks();
            }
            this.syncLayoutLike();
            this.videoPreview();
            this.hoverTipsy();
            Waypoint.refreshAll();
        }

        Module.initElementor = function () {
            if ('undefined' !== typeof initDarkMode && !FOXIZ_MAIN_SCRIPT.editorDarkModeInit) {
                FOXIZ_MAIN_SCRIPT.editorDarkModeInit = true;
                initDarkMode();
            }
            FOXIZ_MAIN_SCRIPT.breakingNews();
            FOXIZ_MAIN_SCRIPT.carousels();
            FOXIZ_MAIN_SCRIPT.sliders();
        }

        Module.isRTL = function () {
            return this._body.hasClass('rtl');
        }

        Module.animationFrame = function (callback) {
            var func = window.requestAnimationFrame || window.mozRequestAnimationFrame ||
                window.webkitRequestAnimationFrame || this.animationFrameFallback
            func.call(window, callback)
        }

        Module.animationFrameFallback = function (callback) {
            window.setTimeout(callback, 1000 / 60)
        }

        Module.SetTTLStorage = function (id, value, ttl) {
            var data = {value: value, ttl: Date.now() + ttl * 3600000}
            localStorage.setItem(id, JSON.stringify(data));
        }

        Module.getTTLStorage = function (id) {
            var data = localStorage.getItem(id);
            if (!data) {
                return null;
            }

            data = JSON.parse(data);
            if (Date.now() > data.ttl) {
                localStorage.removeItem(data);
                return null;
            }
            return data.value;
        }

        Module.setStorage = function (key, data) {
            localStorage.setItem(key, JSON.stringify(data));
        }

        Module.getStorage = function (key, defaultValue) {
            const data = localStorage.getItem(key);
            if (data === null) {
                return defaultValue;
            }
            return JSON.parse(data);
        }

        Module.deleteStorage = function (key) {
            localStorage.removeItem(key);
        }

        /** on load */
        Module.delayLoad = function () {
            var self = this;
            setTimeout(function () {
                self.stickyNavBar();
                self.stickyHeaderBuilder();
            }, 100)
        }

        /** resize */
        Module.browserResize = function () {
            var self = this;
            self._window.on('resize', function () {
                self.topSpacing();
                self.calcSubMenuPos();
            })
        }

        /* ================================ HEADERS ================================ */
        Module.hoverTipsy = function () {

            if (typeof $.fn.rbTipsy === 'undefined') {
                return false;
            }

            this._body.find('[data-copy]').rbTipsy({
                title: 'data-copy',
                fade: true,
                opacity: 1,
                trigger: 'hover',
                gravity: 's'
            });

            if (window.innerWidth > 1024) {
                this._body.find('#site-header [data-title]').rbTipsy({
                    title: 'data-title',
                    fade: true,
                    opacity: 1,
                    trigger: 'hover',
                    gravity: 'n'
                });

                this._body.find('.site-wrap [data-title]').rbTipsy({
                    title: 'data-title',
                    fade: true,
                    opacity: 1,
                    trigger: 'hover',
                    gravity: 's'
                });
            }
        }

        /** font resizer */
        Module.fontResizer = function () {
            var self = this;
            let size;
            if (navigator.cookieEnabled) {
                size = sessionStorage.getItem('rubyResizerStep');
            }
            if (!size) {
                size = 1;
            }

            self._body.on('click', '.font-resizer-trigger', function (e) {
                e.preventDefault();
                e.stopPropagation();
                size++;
                if (3 < size) {
                    size = 1;
                    self._body.removeClass('medium-entry-size big-entry-size')
                } else {
                    if (2 == size) {
                        self._body.addClass('medium-entry-size').removeClass('big-entry-size');
                    } else {
                        self._body.addClass('big-entry-size').removeClass('medium-entry-size')
                    }
                }
                if (navigator.cookieEnabled) {
                    sessionStorage.setItem('rubyResizerStep', size);
                }
            });
        }

        /** hover */
        Module.hoverEffects = function () {
            var selectors = $('.effect-fadeout');
            if (selectors.length > 0) {
                selectors.off('mouseenter mouseleave').on('mouseenter', function (e) {
                    e.stopPropagation();
                    var target = $(this);
                    if (!target.hasClass('activated')) {
                        target.addClass('activated');
                    }
                }).on('mouseleave', function () {
                    $(this).removeClass('activated');
                });
            }
        }

        Module.videoPreview = function () {
            var playPromise;
            $('.preview-trigger').on('mouseenter', function () {
                var target = $(this);
                var wrap = target.find('.preview-video');
                if (!wrap.hasClass('video-added')) {
                    var video = '<video preload="auto" muted loop><source src="' + wrap.data('source') + '" type="' + wrap.data('type') + '"></video>';
                    wrap.append(video).addClass('video-added');
                }
                target.addClass('show-preview');
                wrap.css('z-index', 3);
                var el = target.find('video')[0];
                if (el) {
                    playPromise = el.play();
                }
            }).on('mouseleave', function () {
                var target = $(this);
                target.find('.preview-video').css('z-index', 1);
                var el = target.find('video')[0];
                if (el && playPromise !== undefined) {
                    playPromise.then(_ => {
                        el.pause();
                    }).catch();
                }
            });
        }

        Module.playerAutoPlay = function () {
            var self = this;
            var items = $('.is-autoplay');
            var nonResIframe = $('.entry-content > iframe');

            if (items != null && items.length > 0) {
                items.each(function () {
                    var el = $(this);
                    if (!el.hasClass('is-loaded')) {
                        self.wPoint['iframe'] = new Waypoint({
                            element: el,
                            handler: function () {
                                var iframe = el.find('iframe');
                                self.initAutoPlay(iframe);
                                el.addClass('is-loaded');
                                this.destroy();
                            },
                            offset: '60%'
                        });
                    }
                })
            }

            if (nonResIframe != null && nonResIframe.length > 0) {
                nonResIframe.each(function () {
                    var el = $(this);
                    if (!el.hasClass('is-loaded')) {
                        var iURL = el.attr('src');
                        if (iURL.indexOf('youtube.com') > 0 || iURL.indexOf('youtu.be') > 0 || iURL.indexOf('vimeo.com') > 0) {
                            el.wrap('<div class="rb-ires is-loaded"></div>');
                        }
                    }
                })
            }
        }

        Module.initAutoPlay = function (item) {
            if (item.length > 0 && undefined !== item[0]) {
                var src = item[0].src;
                if (src.indexOf('?') > -1) {
                    item[0].src += "&autoplay=1";
                } else {
                    item[0].src += "?autoplay=1";
                }
            }
        }

        Module.tocToggle = function () {
            this._document.on('click', '.ruby-toc-toggle', function (e) {
                e.preventDefault();
                e.stopPropagation();

                var target = $(this);
                var content = target.parents('.ruby-table-contents').find('.inner');
                target.toggleClass('activate');
                content.toggle('250');
            })
        }

        /** Header JS functions */
        Module.headerDropdown = function () {

            var self = this;
            $('.more-trigger').on('click', function (e) {

                /** re calc menu  */
                self.calcSubMenuPos();

                e.preventDefault();
                e.stopPropagation();
                var target = $(this);
                var holder = target.parents('.header-wrap').find('.more-section-outer');

                if (!holder.hasClass('dropdown-activated')) {
                    self._body.find('.dropdown-activated').removeClass('dropdown-activated');
                    holder.addClass('dropdown-activated');
                } else {
                    holder.removeClass('dropdown-activated');
                }

                if (target.hasClass('search-btn')) {
                    setTimeout(function () {
                        holder.find('input[type="text"]').focus()
                    }, 50);
                }

                return false;
            });

            /** search trigger */
            $('.search-trigger').on('click', function (e) {
                e.preventDefault();
                e.stopPropagation();
                var holder = $(this).parent('.header-dropdown-outer');
                if (!holder.hasClass('dropdown-activated')) {
                    self._body.find('.dropdown-activated').removeClass('dropdown-activated');
                    holder.addClass('dropdown-activated');
                    setTimeout(function () {
                        holder.find('input[type="text"]').focus()
                    }, 50);

                } else {
                    holder.removeClass('dropdown-activated');
                }
                return false;
            });

            /** header dropdown */
            $('.dropdown-trigger').on('click', function (e) {
                e.preventDefault();
                e.stopPropagation();
                var holder = $(this).parent('.header-dropdown-outer');
                if (!holder.hasClass('dropdown-activated')) {
                    self._body.find('.dropdown-activated').removeClass('dropdown-activated');
                    holder.addClass('dropdown-activated');
                } else {
                    holder.removeClass('dropdown-activated');
                }
            });
        }

        Module.topSpacing = function () {
            var self = this;
            if (self._body.hasClass('top-spacing')) {
                var height = $('.top-site-ad').outerHeight();
                $('.site-outer').css('margin-top', height);
            }
        }

        /** outside click */
        Module.documentClick = function () {

            var self = this;
            var wrapper = $('.more-section-outer, .header-dropdown-outer, .mobile-collapse, .mfp-wrap');
            var inlineSearchForm = $('.is-form-layout');

            document.addEventListener('click', function (e) {

                if (!wrapper.is(e.target) && wrapper.has(e.target).length === 0) {
                    wrapper.removeClass('dropdown-activated');
                    self.outerHTML.removeClass('collapse-activated');
                }

                if (!inlineSearchForm.is(e.target) && inlineSearchForm.has(e.target).length === 0) {
                    inlineSearchForm.find('.live-search-response').fadeOut(500);
                }
            });
        }

        /** calc mega menu position */
        Module.initSubMenuPos = function () {
            var self = this;
            var trigger = false;

            /** add delay to ensure image loaded */
            setTimeout(function () {
                self.calcSubMenuPos();
            }, 1000);

            /** re calc when hovering */
            $('.menu-has-child-mega').on('mouseenter', function () {
                if (!trigger) {
                    self.calcSubMenuPos();
                }
                trigger = true;
            })
        }

        Module.calcSubMenuPos = function () {

            if (window.outerWidth < 1024) {
                return false;
            }

            var self = this;
            var megaParents = $('.menu-has-child-mega');
            var headerWrapper = $('#site-header');

            /** for mega wide */
            if (megaParents.length > 0) {
                megaParents.each(function () {
                    var item = $(this);
                    item.find('.mega-dropdown').css({
                        'width': self._body.width(),
                        'left': -item.offset().left,
                    });
                    item.addClass('mega-menu-loaded')
                })
            }

            /** sub-menu left right direction */
            if (headerWrapper.length > 0) {

                var headerLeftOffset = headerWrapper.offset().left;
                var headerWidth = headerWrapper.width();
                var headerRightOffset = headerLeftOffset + headerWidth;
                var flexDropdown = $('.flex-dropdown');

                /** sub menu direction */
                var subElements = $('ul.sub-menu');
                if (subElements.length > 0) {
                    subElements.each(function () {
                        var item = $(this);
                        var itemLeftOffset = item.offset().left;
                        var itemRightOffset = itemLeftOffset + item.width() + 100;
                        if (itemRightOffset > headerRightOffset) {
                            item.addClass('left-direction');
                        }
                    })
                }

                /** calc dropdown flex width */
                if (flexDropdown.length > 0) {
                    flexDropdown.each(function () {
                        var item = $(this);
                        var parentItem = item.parent();
                        if (parentItem.hasClass('is-child-wide') || item.hasClass('mega-has-left')) {
                            return;
                        }
                        var itemWidth = item.width();
                        var itemHalfWidth = itemWidth / 2;
                        var parentItemOffset = parentItem.offset().left;
                        var parentHalfWidth = parentItem.width() / 2;
                        var parentItemCenterOffset = parentItemOffset + parentHalfWidth;
                        var rightSpacing = headerRightOffset - parentItemCenterOffset;
                        var leftSpacing = parentItemCenterOffset - headerLeftOffset;

                        if (itemWidth >= headerWidth) {
                            item.css({
                                'width': headerWidth - 2,
                                'left': -parentItemOffset
                            });
                        } else if (itemHalfWidth > rightSpacing) {
                            item.css({
                                'right': -rightSpacing + parentHalfWidth + 1,
                                'left': 'auto',
                            });
                        } else if (itemHalfWidth > leftSpacing) {
                            item.css({
                                'left': -leftSpacing + parentHalfWidth + 1,
                                'right': 'auto',
                            });
                        } else {
                            item.css({
                                'right': 'auto',
                                'left': -itemHalfWidth + parentHalfWidth,
                            });
                        }
                    });
                }
            }
        }

        /**
         *
         * @returns {boolean}
         */
        Module.stickyNavBar = function () {

            var self = this;

            /** turn off sticky on editor mode */
            if (self._body.hasClass('elementor-editor-active')) {
                return false;
            }

            self.sticky.section = $('#sticky-holder');
            self.sticky.outer = $('#navbar-outer');

            if ((!self._body.hasClass('is-mstick') && !self._body.hasClass('yes-tstick')) || self.sticky.outer.length < 1 || self.sticky.section.length < 1) {
                return false;
            }

            self.sticky.smartSticky = !!self._body.hasClass('is-smart-sticky');
            self.sticky.isSticky = false;
            self.sticky.lastScroll = 0;


            if (self._body.hasClass('yes-tstick')) {
                self.sticky.isTitleSticky = true;
            } else {
                self.sticky.isTitleSticky = 0;
            }

            self.sticky.additionalOffset = 200;
            if (window.innerWidth > 1024) {
                if (self.sticky.isTitleSticky) {
                    self.sticky.additionalOffset = 450;
                } else {
                    self.sticky.additionalOffset = 0;
                }
            }

            if (self._body.hasClass('admin-bar')) {
                self.sticky.adminBarSpacing = 32;
            } else {
                self.sticky.adminBarSpacing = 0;
            }

            self.sticky.topOffset = self.sticky.section.offset().top;
            self.sticky.stickySectionHeight = self.sticky.section.outerHeight();

            self.sticky.outer.css('min-height', self.sticky.outer.outerHeight());
            self.sticky.activatePos = self.sticky.topOffset + 1 + self.sticky.stickySectionHeight + self.sticky.additionalOffset;
            self.sticky.deactivePos = self.sticky.topOffset - self.sticky.adminBarSpacing + self.sticky.additionalOffset;

            if (window.addEventListener) {
                if (self.sticky.smartSticky) {
                    window.addEventListener('scroll', function () {
                        self.animationFrame(self.initSmartStickyNavBar.bind(self));
                    }, false);
                } else {
                    window.addEventListener('scroll', function () {
                        self.animationFrame(self.initStickyNavBar.bind(self));
                    }, false);
                }
            }

            self._window.on('unstickMenu', function () {
                self.sticky.outer.css('min-height', self.sticky.outer.outerHeight());
                self.sticky.stickySectionHeight = self.sticky.section.outerHeight();
                self.sticky.topOffset = self.sticky.section.offset().top;
                self.sticky.activatePos = self.sticky.topOffset + 1 + self.sticky.stickySectionHeight + self.sticky.additionalOffset;
                self.sticky.deactivePos = self.sticky.topOffset - self.sticky.adminBarSpacing + self.sticky.additionalOffset;
            });
        }

        Module.initStickyNavBar = function () {
            var self = this;
            var scroll = self._window.scrollTop();
            if (!self.sticky.isSticky && scroll > self.sticky.activatePos) {
                self.sticky.isSticky = true;
                self._body.addClass('stick-animated sticky-on');
                self.sticky.stickAnimatedTimeout = setTimeout(function () {
                    self._body.removeClass('stick-animated');
                }, 200);
            } else if (self.sticky.isSticky && scroll <= self.sticky.deactivePos) {
                self.sticky.isSticky = false;
                self._body.removeClass('sticky-on stick-animated');
                self._window.trigger('unstickMenu');
            }
        }

        Module.initSmartStickyNavBar = function () {
            var self = this;
            var scroll = self._window.scrollTop();
            if (!self.sticky.isSticky && scroll > self.sticky.activatePos && scroll < self.sticky.lastScroll) {
                self.sticky.isSticky = true;
                self._body.addClass('stick-animated sticky-on');
                self.sticky.stickAnimatedTimeout = setTimeout(function () {
                    self._body.removeClass('stick-animated');
                }, 200);
            } else if (self.sticky.isSticky && (scroll <= self.sticky.deactivePos || scroll > self.sticky.lastScroll)) {
                self.sticky.isSticky = false;
                self._body.removeClass('sticky-on stick-animated');
                if (scroll <= self.sticky.deactivePos) {
                    self._window.trigger('unstickESection');
                }
            }
            self.sticky.lastScroll = scroll;
        }

        /** header sticky template */
        Module.stickyHeaderBuilder = function () {

            var self = this;

            /** turn off sticky on editor mode */
            if (self._body.hasClass('elementor-editor-active')) {
                return false;
            }

            var stickySection = $('.header-template .e-section-sticky').first();
            var hasTitleSticky = $('body.single-post #s-title-sticky').first();

            if (stickySection.length < 1) {
                return false;
            }

            if (hasTitleSticky.length > 0) {
                self._body.addClass('yes-tstick');
                self.eSticky.isTitleSticky = true;
            } else {
                self.eSticky.isTitleSticky = 0;
            }

            self.eSticky.additionalOffset = 200;
            if (window.innerWidth > 1024) {
                if (self.eSticky.isTitleSticky) {
                    self.eSticky.additionalOffset = 450;
                } else {
                    self.eSticky.additionalOffset = 0;
                }
            }

            self.eSticky.smartSticky = !!stickySection.hasClass('is-smart-sticky');

            /** mobile sticky for header template */
            if (window.innerWidth <= 1024) {
                var mobileHeader = $('#header-template-holder');
                stickySection.removeClass('e-section-sticky');
                mobileHeader.addClass('e-section-sticky');
                stickySection = mobileHeader;
            }

            self.eSticky.section = stickySection;
            self.eSticky.outer = stickySection.parent();

            self.eSticky.adminBarSpacing = 0;
            self.eSticky.isSticky = false;
            self.eSticky.lastScroll = 0;
            self.eSticky.stickySectionHeight = stickySection.outerHeight();
            self.eSticky.topOffset = stickySection.offset().top;

            /** set min height */
            if (self._body.hasClass('admin-bar')) {
                self.eSticky.adminBarSpacing = 32;
            }
            self.eSticky.outer.css('min-height', self.eSticky.outer.outerHeight());
            self.eSticky.activatePos = self.eSticky.topOffset + 1 + self.eSticky.stickySectionHeight + self.eSticky.additionalOffset;
            self.eSticky.deactivePos = self.eSticky.topOffset - self.eSticky.adminBarSpacing + self.eSticky.additionalOffset;

            if (window.addEventListener) {
                if (self.eSticky.smartSticky) {
                    window.addEventListener('scroll', function () {
                        self.animationFrame(self.initSmartStickyESection.bind(self));
                    }, false);
                } else {
                    window.addEventListener('scroll', function () {
                        self.animationFrame(self.initStickyESection.bind(self));
                    }, false);
                }
            }

            /** re-calc height values */
            self._window.on('unstickESection', function () {
                self.eSticky.outer.css('min-height', self.eSticky.outer.outerHeight());
                self.eSticky.stickySectionHeight = self.eSticky.section.outerHeight();
                self.eSticky.topOffset = self.eSticky.section.offset().top;
                self.eSticky.activatePos = self.eSticky.topOffset + 1 + self.eSticky.stickySectionHeight + self.eSticky.additionalOffset;
                self.eSticky.deactivePos = self.eSticky.topOffset - self.eSticky.adminBarSpacing + self.eSticky.additionalOffset;
            });
        }

        Module.initStickyESection = function () {
            var self = this;
            var scroll = self._window.scrollTop();
            if (!self.eSticky.isSticky && scroll > self.eSticky.activatePos) {
                self.eSticky.isSticky = true;
                self._body.addClass('stick-animated sticky-on');
                self.eSticky.stickAnimatedTimeout = setTimeout(function () {
                    self._body.removeClass('stick-animated');
                }, 200);
            } else if (self.eSticky.isSticky && scroll <= self.eSticky.deactivePos) {
                self.eSticky.isSticky = false;
                self._body.removeClass('sticky-on stick-animated');
                self._window.trigger('unstickESection');
            }
        }

        Module.initSmartStickyESection = function () {
            var self = this;
            var scroll = self._window.scrollTop();

            if (!self.eSticky.isSticky && scroll > self.eSticky.activatePos && scroll < self.eSticky.lastScroll) {
                self.eSticky.isSticky = true;
                self._body.addClass('stick-animated sticky-on');
                self.eSticky.stickAnimatedTimeout = setTimeout(function () {
                    self._body.removeClass('stick-animated');
                }, 200);
            } else if (self.eSticky.isSticky && (scroll <= self.eSticky.deactivePos || scroll > self.eSticky.lastScroll)) {
                self.eSticky.isSticky = false;
                self._body.removeClass('sticky-on stick-animated');
                if (scroll <= self.eSticky.deactivePos) {
                    self._window.trigger('unstickESection');
                }
            }

            self.eSticky.lastScroll = scroll;
        }

        /** mobileCollapse */
        Module.mobileCollapse = function () {
            var self = this;
            $('.mobile-menu-trigger').on('click', function (e) {
                e.preventDefault();
                e.stopPropagation();
                var target = $(this);
                if (target.hasClass('mobile-search-icon')) {
                    setTimeout(function () {
                        self.outerHTML.find('.mobile-search-form input[type="text"]').focus()
                    }, 50);
                }
                if (!self.outerHTML.hasClass('collapse-activated')) {
                    self.outerHTML.addClass('collapse-activated');
                } else {
                    self.outerHTML.removeClass('collapse-activated');
                }
            });
        }

        /**
         * privacy trigger
         */
        Module.privacyTrigger = function () {
            $('#privacy-trigger').on('click', function (e) {
                e.preventDefault();
                e.stopPropagation();
                localStorage.setItem('RubyPrivacyAllowed', '1');
                $('#rb-privacy').slideUp(250, function () {
                    $(this).remove();
                });
                return false;
            });
        }

        /** back top */
        Module.backTop = function () {
            if (this._body.hasClass('is-backtop')) {
                $().UItoTop({
                    text: '<i class="rbi rbi-darrow-top"></i>',
                });
            }
        }

        /** login popup */
        Module.loginPopup = function () {
            const form = $('#rb-user-popup-form');
            if (form.length < 1) {
                return false;
            }

            this._document.on('click', '.login-toggle', (e) => {
                e.preventDefault();
                e.stopPropagation();
                $.magnificPopup.open({
                    type: 'inline',
                    preloader: false,
                    removalDelay: 400,
                    showCloseBtn: true,
                    closeBtnInside: true,
                    closeOnBgClick: true,
                    items: {
                        src: form,
                        type: 'inline'
                    },
                    mainClass: 'popup-animation',
                    closeMarkup: '<span class="close-popup-btn mfp-close"><span class="close-icon"></span></span>',
                    fixedBgPos: true,
                    fixedContentPos: true
                });
            });
        };

        /**
         * newsletter
         */
        Module.popupNewsletter = function () {

            var self = this;

            var target = $('#rb-popup-newsletter');
            if (target.length > 0) {
                var display = target.data('display');
                self.newsletterExpired = target.data('expired');
                self.newsletterDisplayOffset = target.data('offset');
                var delay = target.data('delay');
                var oldExpired = localStorage.getItem('RubyNewsletterExpired');
                if (!oldExpired || self.newsletterExpired != oldExpired) {
                    localStorage.setItem('RubyNewsletterExpired', self.newsletterExpired);
                    localStorage.removeItem('RubyNewsletter');
                }
                if (!self.getTTLStorage('RubyNewsletter')) {
                    if (!display || 'scroll' == display) {
                        if (window.addEventListener) {
                            window.addEventListener('scroll', function () {
                                self.animationFrame(self.scrollPopupNewsletter.bind(self));
                            }, false);
                        }
                    } else {
                        setTimeout(function () {
                            self.popupNewsletterInit();
                        }, delay);
                    }
                }
            }
        }

        Module.scrollPopupNewsletter = function () {
            var self = this;
            if (!self.newsletterPopupFlag && self._window.scrollTop() > self.newsletterDisplayOffset) {
                self.newsletterPopupFlag = true;
                self.popupNewsletterInit();
            }
        }

        Module.popupNewsletterInit = function () {
            var self = this;

            if (self.siteAccessFlag) {
                return;
            }

            $.magnificPopup.open({
                type: 'inline',
                preloader: false,
                closeBtnInside: true,
                removalDelay: 300,
                showCloseBtn: true,
                closeOnBgClick: false,
                disableOn: 1024,
                items: {
                    src: '#rb-popup-newsletter',
                    type: 'inline'
                },
                mainClass: 'popup-animation',
                fixedBgPos: true,
                fixedContentPos: true,
                closeMarkup: '<span class="close-popup-btn mfp-close"><span class="close-icon"></span></span>',
                callbacks: {
                    close: function () {
                        if (self.siteAccessFlag) {
                            return;
                        }
                        self.SetTTLStorage('RubyNewsletter', 1, self.newsletterExpired * 24);
                    }
                }
            });
        }

        /** footer slide up */
        Module.footerSlideUp = function () {
            var target = $('#footer-slideup');
            if (target.length > 0) {
                var self = this;
                self.footerSlideUpExpired = target.data('expired');
                var delay = target.data('delay');
                var oldExpired = localStorage.getItem('footerSlideUpExpired');
                if (!oldExpired || self.footerSlideUpExpired != oldExpired) {
                    localStorage.setItem('footerSlideUpExpired', self.footerSlideUpExpired);
                    localStorage.removeItem('footerSlideUp');
                }
                if (!self.getTTLStorage('footerSlideUp')) {
                    setTimeout(function () {
                        self.footerSlideUpInit();
                    }, delay);
                }
                /** show hide toggle */
                setTimeout(function () {
                    self.footerSlideUpToggle();
                }, delay);
            }
        }

        Module.footerSlideUpToggle = function () {
            var self = this;
            $('.slideup-toggle').off('click').on('click', function (e) {
                e.preventDefault();
                e.stopPropagation();
                self.footerSlideUpInit();
                return false;
            });
        }

        Module.footerSlideUpInit = function () {
            if (this._body.hasClass('yes-f-slideup')) {
                this._body.removeClass('yes-f-slideup');
                this.SetTTLStorage('footerSlideUp', 1, this.footerSlideUpExpired * 24);
            } else {
                this._body.addClass('yes-f-slideup');
                localStorage.removeItem('footerSlideUp');
            }
        }


        /** youtube iframe */
        Module.loadYoutubeIframe = function () {

            var self = this;
            var blockPlaylist = $('.yt-playlist');
            if (blockPlaylist.length > 0) {
                var tag = document.createElement('script');
                tag.src = "//www.youtube.com/iframe_api";
                var firstScriptTag = document.getElementsByTagName('script')[0];
                firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);
            }

            window.onYouTubeIframeAPIReady = function () {
                $('.yt-playlist').each(function () {
                    var target = $(this);
                    var iframe = target.find('.yt-player');
                    var videoID = target.data('id');
                    var blockID = target.data('block');
                    self.YTPlayers[blockID] = new YT.Player(iframe.get(0), {
                        height: '540',
                        width: '960',
                        videoId: videoID,
                        events: {
                            'onReady': self.videoPlayToggle,
                            'onStateChange': self.videoPlayToggle
                        }
                    });
                });
                $('.plist-item').on('click', function (e) {
                    e.preventDefault();
                    e.stopPropagation();

                    var wrapper = $(this).parents('.yt-playlist');
                    var currentBlockID = wrapper.data('block');
                    var videoID = $(this).data('id');
                    var title = $(this).text();
                    var meta = $(this).data('index');
                    Object.keys(self.YTPlayers).forEach(function (id) {
                        self.YTPlayers[id].pauseVideo();
                    });
                    self.YTPlayers[currentBlockID].loadVideoById({
                        'videoId': videoID
                    });

                    wrapper.find('.yt-trigger').addClass('is-playing');
                    wrapper.find('.play-title').hide().text(title).fadeIn(250);
                    wrapper.find('.video-index').text(meta);
                });
            }
        }

        Module.videoPlayToggle = function () {
            var players = FOXIZ_MAIN_SCRIPT.YTPlayers;
            $('.yt-trigger').on('click', function (e) {
                e.preventDefault();
                e.stopPropagation();

                var target = $(this);
                var currentBlockID = target.parents('.yt-playlist').data('block');
                var currentState = players[currentBlockID].getPlayerState();
                if (-1 == currentState || 0 == currentState || 2 == currentState || 5 == currentState) {
                    players[currentBlockID].playVideo();
                    target.addClass('is-playing');
                } else {
                    players[currentBlockID].pauseVideo();
                    target.removeClass('is-playing');

                }
            })
        }


        /** Comment scripts */
        Module.showPostComment = function () {
            var self = this;

            this._document.on('click', '.smeta-sec .meta-comment', function (e) {
                e.stopPropagation();
                let commentBtn = $('.show-post-comment');
                if (commentBtn.length > 0) {
                    self.html.animate({scrollTop: commentBtn.offset().top}, 300);
                    commentBtn.trigger('click');
                }
            });

            this._document.on('click', '.show-post-comment', function (e) {
                e.preventDefault();
                e.stopPropagation();
                var wrap = $(this).parent();
                $(this).fadeOut(250, function () {
                    $(this).remove();
                    wrap.find('.is-invisible').removeClass('is-invisible');
                    wrap.next('.comment-holder').removeClass('is-hidden');
                })
            });
        }

        /** table scroll */
        Module.scrollTableContent = function () {
            var self = this;
            $('.anchor-link').on('click', function (e) {
                e.stopPropagation();

                /** support special language */
                var target = $(this).data('index');
                self.html.animate({
                    scrollTop: $('.' + target).offset().top - 75
                }, 400);
            });
        }

        /** scroll to comment  */
        Module.scrollToComment = function () {
            var commentBtn = $('.show-post-comment');
            var self = this;
            if (commentBtn.length > 0) {
                var hash = window.location.hash;
                if ('#respond' == hash || '#comment' == hash.substring(0, 8)) {
                    commentBtn.trigger('click');
                    self.html.animate({scrollTop: commentBtn.offset().top}, 300);
                }
            }
        }

        Module.replyReview = function () {
            this._document.on('click', '.comment-reply-link', function () {
                const target = $(this);
                const wrapper = target.parents('.rb-reviews-area');
                const cancelLink = $('#cancel-comment-reply-link');
                if (wrapper.length > 0) {
                    wrapper.find('.rb-form-rating').addClass('is-hidden');
                    cancelLink.on('click', function () {
                        wrapper.find('.rb-form-rating').removeClass('is-hidden');
                    });
                }
            });
        }

        /** user rating */
        Module.usersRating = function () {
            var self = this;
            var reviewsForm = self._body.find('.rb-reviews-form');
            if (reviewsForm.length > 0) {
                reviewsForm.each(function () {
                    var reviewForm = $(this);
                    if (!reviewForm.hasClass('is-loaded')) {
                        reviewForm.addClass('is-loaded');
                        var ratingForm = reviewForm.find('.rb-form-rating');
                        var selection = reviewForm.find('.rb-rating-selection');
                        var text = reviewForm.find('.rating-alert').html();
                        var ratingValue = null;
                        selection.val('');
                        selection.hide();
                        selection.before(
                            '<div class="rb-review-stars">\
                                <span>\
                                    <a class="star" data-rating="1" href="#"><i class="rbi rbi-star-o"></i></a>\
                                    <a class="star" data-rating="2" href="#"><i class="rbi rbi-star-o"></i></a>\
                                    <a class="star" data-rating="3" href="#"><i class="rbi rbi-star-o"></i></a>\
                                    <a class="star" data-rating="4" href="#"><i class="rbi rbi-star-o"></i></a>\
                                    <a class="star" data-rating="5" href="#"><i class="rbi rbi-star-o"></i></a>\
                                </span>\
                            </div>'
                        );

                        ratingForm.on('click', 'a.star', function (e) {
                            e.preventDefault();
                            e.stopPropagation();
                            var star = $(this);
                            ratingValue = star.data('rating');
                            star.siblings('a').removeClass('active');
                            star.addClass('active');
                            ratingForm.addClass('selected');
                        });

                        reviewForm.on('click', '#respond #submit', function () {
                            selection.val(ratingValue);
                            if (!selection.val()) {
                                window.alert(text);
                                return false;
                            }
                        });
                    }
                });
            }
        }

        /**
         *
         * @returns {boolean}
         */
        Module.readIndicatorInit = function () {

            var self = this;

            if (!self._body.hasClass('single-post') || self.readIndicator.length < 1) {
                return false;
            }

            var content = $('.entry-content');
            if (content.length < 1) {
                return false;
            }
            content = content.eq(0);
            self.indicatorTop = content.offset().top;
            self.indicatorHeight = content.outerHeight(true) - self._window.height();
            if (window.addEventListener) {
                window.addEventListener('scroll', function () {
                    self.animationFrame(self.readIndicatorCalc.bind(self));
                }, false);
            }
        }

        Module.readIndicatorCalc = function () {
            var self = this;
            var scroll = self._window.scrollTop();
            self.readIndicatorPercent = ((scroll - self.indicatorTop) / self.indicatorHeight) * 100;
            if (self.readIndicatorPercent <= 130) {
                self.readIndicator.css('width', self.readIndicatorPercent + '%');
            }
        }

        /** breaking news */
        Module.breakingNews = function () {
            var self = this;
            var breakingNews = $('.breaking-news-slider');
            if (breakingNews.length < 1) {
                return false;
            }
            breakingNews.each(function () {
                var el = $(this);
                var params = {
                    slidesPerView: 1,
                    loop: true,
                }
                if (el.data('play')) {
                    params.autoplay = {
                        delay: self.themeSettings.sliderSpeed,
                        pauseOnMouseEnter: true,
                        stopOnLastSlide: true,
                        disableOnInteraction: true,
                    }
                    if (el.data('speed')) {
                        params.autoplay.delay = el.data('speed');
                    }
                }
                if ('undefined' !== typeof self.isElementorEditor) {
                    delete params.autoplay;
                }
                params.navigation = {
                    nextEl: el.find('.breaking-news-next')[0],
                    prevEl: el.find('.breaking-news-prev')[0]
                }
                new RBSwiper(this, params);
            });
        }

        /** overlay slider */
        Module.sliders = function () {

            var self = this;
            var sliders = $('.post-slider');
            if (sliders.length < 1) {
                return false;
            }

            sliders.each(function () {
                var slider = $(this);
                var params = {
                    grabCursor: true,
                    allowTouchMove: true,
                    effect: self.themeSettings.sliderEffect,
                    loop: true,
                }
                if (slider.data('play')) {
                    params.autoplay = {
                        delay: self.themeSettings.sliderSpeed,
                        pauseOnMouseEnter: true,
                        stopOnLastSlide: true,
                        disableOnInteraction: true,
                    }
                    if (slider.data('speed')) {
                        params.autoplay.delay = slider.data('speed');
                    }
                }
                if ('undefined' !== typeof self.isElementorEditor) {
                    delete params.autoplay;
                }
                params.pagination = {
                    el: slider.find('.slider-pagination')[0],
                    clickable: true,
                };

                params.navigation = {
                    nextEl: slider.find('.slider-next')[0],
                    prevEl: slider.find('.slider-prev')[0]
                }
                new RBSwiper(this, params);
            });
        }


        /** carousel blocks */
        Module.carousels = function () {
            var self = this;
            var carousels = $('.post-carousel');
            if (carousels.length < 1) {
                return false;
            }
            carousels.each(function () {
                var carousel = $(this);
                var params = {
                    grabCursor: true,
                    allowTouchMove: true,
                    freeMode: false,
                    loop: true
                }
                params.slidesPerView = carousel.data('mcol');
                params.spaceBetween = carousel.data('mgap');
                params.centeredSlides = carousel.data('centered');

                params.navigation = {
                    nextEl: carousel.find('.slider-next')[0],
                    prevEl: carousel.find('.slider-prev')[0]
                }
                if (carousel.find('.slider-pagination')[0]) {
                    params.pagination = {
                        el: carousel.find('.slider-pagination')[0],
                        type: 'bullets',
                        clickable: true,
                    };
                }
                if (carousel.data('play')) {
                    params.autoplay = {
                        delay: self.themeSettings.sliderSpeed,
                        pauseOnMouseEnter: true,
                        stopOnLastSlide: true,
                        disableOnInteraction: true,
                    }

                    if (carousel.data('speed')) {
                        params.autoplay.delay = carousel.data('speed');
                    }
                }
                if ('undefined' !== typeof self.isElementorEditor) {
                    delete params.autoplay;
                }
                if (carousel.data('fmode')) {
                    params.freeMode = true;
                }
                params.breakpoints = {
                    768: {
                        slidesPerView: carousel.data('tcol'),
                        spaceBetween: carousel.data('tgap')
                    },
                    1025: {
                        slidesPerView: carousel.data('col'),
                        spaceBetween: carousel.data('gap')
                    },
                    1500: {
                        slidesPerView: carousel.data('wcol'),
                        spaceBetween: carousel.data('gap')
                    }
                };
                params.on = {
                    afterInit: function (swiper) {
                        var wrap = $(swiper.$wrapperEl);
                        $(swiper.$wrapperEl).find('.p-box').css('height', wrap.height());
                    },
                    resize: function (swiper) {
                        var wrap = $(swiper.$wrapperEl);
                        $(swiper.$wrapperEl).find('.p-box').css('height', wrap.height());
                    },
                };

                new RBSwiper(this, params);
            });
        }


        /* ================================ SINGLE GALLERY ================================ */
        Module.singleGallery = function () {
            var self = this;
            var gallerySections = self._body.find('.featured-gallery-wrap');
            if (!gallerySections.length) {
                return;
            }

            gallerySections.each(function () {
                var section = $(this);
                if (!section.hasClass('is-loaded')) {
                    var index = section.data('gallery');
                    var sliderEl = section.find('.gallery-slider').attr('id');
                    var sliderNavEl = section.find('.gallery-slider-nav').attr('id');
                    var carouselEl = section.find('.gallery-carousel').attr('id');
                    var coverflowEL = section.find('.gallery-coverflow').attr('id');

                    if ('undefined' !== typeof sliderEl && 'undefined' !== typeof sliderNavEl) {
                        var galleryNav = new RBSwiper('#' + sliderNavEl, {
                            spaceBetween: 15,
                            slidesPerView: 6,
                            freeMode: self.themeSettings.sliderFMode,
                            grabCursor: true,
                            loop: true,
                            watchSlidesVisibility: true,
                            watchSlidesProgress: true,
                            on: {
                                init: function () {
                                    $(this.$wrapperEl).removeClass('pre-load');
                                },
                            },
                        });

                        var gallerySlider = new RBSwiper('#' + sliderEl, {
                            spaceBetween: 0,
                            grabCursor: true,
                            loop: true,
                            pagination: {
                                el: '.swiper-pagination-' + index,
                                type: 'progressbar',
                                clickable: true,
                            },
                            on: {
                                init: function () {
                                    section.addClass('is-loaded');
                                },
                            },
                            thumbs: {
                                swiper: galleryNav
                            }
                        });

                        gallerySlider.on('slideChange', function () {
                            if (this.activeIndex) {
                                $(this.$el).next().find('.current-slider-count').fadeOut(0).html(this.activeIndex).fadeIn(250);
                            }
                        });
                    }

                    if ('undefined' !== typeof carouselEl) {
                        new RBSwiper('#' + carouselEl, {
                            spaceBetween: 20,
                            slidesPerView: 'auto',
                            freeMode: self.themeSettings.sliderFMode,
                            loop: false,
                            grabCursor: true,
                            scrollbar: {
                                el: '.swiper-scrollbar-' + index,
                                hide: true,
                            },
                            on: {
                                init: function () {
                                    $(this.$wrapperEl).removeClass('pre-load');
                                    section.addClass('is-loaded');
                                },
                            },
                        });
                    }

                    if ('undefined' !== typeof coverflowEL) {
                        new RBSwiper('#' + coverflowEL, {
                            effect: "coverflow",
                            grabCursor: true,
                            centeredSlides: true,
                            slidesPerView: 1.2,
                            coverflowEffect: {
                                rotate: 50,
                                stretch: 0,
                                depth: 250,
                                modifier: 1,
                                slideShadows: true,
                            },
                            breakpoints: {
                                768: {
                                    slidesPerView: 3
                                }
                            },
                            on: {
                                init: function () {
                                    $(this.$wrapperEl).removeClass('pre-load');
                                    section.addClass('is-loaded');
                                },
                            },
                            pagination: {
                                el: '.swiper-pagination-' + index,
                                clickable: true,
                            },
                        });
                    }
                }
            });
        }

        Module.highlightShares = function () {
            var self = this;
            if (!self.themeSettings.highlightShares || (window.matchMedia && window.matchMedia('(pointer: coarse)').matches)) {
                return;
            }
            var extra = [];
            var config = {
                selectableElements: ['.is-highlight-shares'],
                twitterUsername: self.themeSettings.twitterName,
            };
            if (self.themeSettings.highlightShareFacebook) {
                config.facebook = true;
            }
            if (self.themeSettings.highlightShareTwitter) {
                config.twitter = true;
            }
            if (self.themeSettings.highlightShareReddit) {
                extra.push({
                    icon: '<i class="rbi rbi-reddit"></i>',
                    url: 'https://reddit.com/submit?url=PAGE_URL&title=TEXT_SELECTION'
                });
            }
            Sharect.config(config).appendCustomShareButtons(extra).init();
        }

        /**
         *
         * @returns {boolean}
         */
        Module.floatingVideo = function () {

            var self = this;
            var floating = $('.floating-video').not('.is-loaded');
            if (floating.length < 1 || window.outerWidth < 1025) {
                return false;
            }
            self.wPoint['Floating'] = new Waypoint({
                element: floating,
                offset: -floating.height(),
                handler: function (direction) {
                    self._body.find('.floating-video').addClass('is-loaded').removeClass('floating-activated');
                    self._body.find('.floating-close').remove();
                    if ('down' == direction) {
                        this.element.addClass(' floating-activated');
                        this.element.find('.float-holder').prepend('<a class="floating-close close-popup-btn" href="#"><span class="close-icon"></span></a>');
                    }
                }
            });
        }

        Module.floatingVideoRemove = function () {
            var self = this;
            self._body.on('click', '.floating-close', function (e) {
                e.preventDefault();
                e.stopPropagation();
                self._body.find('.floating-video').removeClass('floating-activated');
                self.wPoint['Floating'].destroy();
            })
        }

        Module.siteAccessDetector = function () {
            var self = this;
            var checktag = $('#rb-checktag');
            var placebo = $('.rb-adbanner');

            if (checktag.length === 0) {
                return false;
            }

            setTimeout(function () {
                if (placebo.height() < 1) {
                    self.siteAccessFlag = true;
                    $.magnificPopup.close();
                    setTimeout(function () {
                        $.magnificPopup.open({
                            type: 'inline',
                            preloader: false,
                            showCloseBtn: false,
                            closeBtnInside: false,
                            enableEscapeKey: false,
                            removalDelay: 999999,
                            closeOnBgClick: false,
                            items: {
                                src: '#rb-site-access',
                                type: 'inline'
                            },
                            mainClass: 'popup-animation site-access-popup',
                            fixedBgPos: true,
                            fixedContentPos: true,
                        });
                    }, 310);
                    window.addEventListener('contextmenu', event => event.preventDefault());
                }
            }, 1500);
        }

        Module.galleryLightbox = function () {

            $('.gallery-popup-link').on('click', function (e) {
                e.preventDefault();
                e.stopPropagation();

                var target = $(this);
                var data = [];
                var galleryID = target.data('gallery');
                var slideIndex = target.data('index');
                var galleries = $(galleryID).find('.gallery-el');
                galleries.each(function () {
                    data.push({
                        title: $(this).find('.image-title').html(),
                        rbgallery: $(this).html()
                    });
                });

                $.magnificPopup.open({
                    type: 'inline',
                    mainClass: 'rb-gallery-popup popup-animation',
                    closeMarkup: '<button title="%title%" class="mfp-close mfp-button"><i class="rbi rbi-close"></i></button>',
                    closeOnBgClick: false,
                    removalDelay: 500,
                    showCloseBtn: true,
                    fixedBgPos: true,
                    fixedContentPos: true,
                    preloader: false,
                    gallery: {
                        enabled: true,
                        preload: [0, 2],
                        arrowMarkup: '<button title="%title%" type="button" class="mfp-arrow mfp-button mfp-arrow-%dir%"><i class="rbi rbi-%dir%"></i></button>',
                        tCounter: '<span>%curr% / %total%</span>'
                    },
                    callbacks: {
                        open: function () {
                            $.magnificPopup.instance.goTo(slideIndex);
                        },
                        close: function () {
                            $.magnificPopup.instance.popupsCache = {};
                        },
                        buildControls: function () {
                            if (this.arrowLeft && this.arrowRight) {
                                this.contentContainer.find('.gallery-popup-nav').append(this.arrowLeft.add(this.arrowRight));
                            }
                        }
                    },
                    inline: {
                        markup: '<div class="gallery-popup-header light-scheme">' +
                            '<span class="mfp-title h5"></span>' +
                            '<div class="popup-header-right"><div class="mfp-counter"></div><div class="mfp-close"></div></div>' +
                            '</div><div class="gallery-popup-nav"></div>' +
                            '<div class="gallery-popup-content"><div class="mfp-rbgallery"></div></div>'
                    },
                    items: data
                });

                return false;
            });
        };

        Module.singleFeaturedLightbox = function () {
            $('.featured-lightbox-trigger').on('click', function (e) {

                e.preventDefault();
                e.stopPropagation();

                var source = '<img src="' + $(this).data('source') + '" alt="' + $(this).find('img').attr('alt') + '">';
                var caption = $(this).data('caption');
                var attribution = $(this).data('attribution');

                $.magnificPopup.open({
                    mainClass: 'rb-gallery-popup single-featured-popup popup-animation',
                    closeOnBgClick: true,
                    removalDelay: 500,
                    showCloseBtn: true,
                    fixedBgPos: true,
                    fixedContentPos: true,
                    preloader: false,
                    closeMarkup: '<button title="%title%" class="mfp-close mfp-button"><i class="rbi rbi-close"></i></button>',
                    callbacks: {
                        close: function () {
                            $.magnificPopup.instance.popupsCache = {};
                        },
                    },
                    inline: {
                        markup: '<div class="gallery-popup-header light-scheme">' +
                            '<div class="mfp-close"></div>' +
                            '</div><div class="gallery-popup-nav"></div>' +
                            '<div class="gallery-popup-content"><div class="mfp-source gallery-popup-image"></div>' +
                            '<div class="gallery-popup-entry light-scheme">' +
                            '<span class="mfp-title h5"></span><span class="mfp-attribution description-text"></span>' +
                            '</div>' +
                            '</div>'
                    },
                    items: {
                        title: caption,
                        source: source,
                        attribution: attribution
                    },
                });
            });
        }

        /* Ajax pagination */
        Module.paginationNextPrev = function () {
            var self = this;
            self._body.on('click', '.pagination-trigger', function (e) {

                e.preventDefault();
                e.stopPropagation();
                const paginationTrigger = $(this);
                if (paginationTrigger.hasClass('is-disable')) {
                    return;
                }

                var block = paginationTrigger.parents('.block-wrap');
                var uuid = block.attr('id');

                if (!self.ajaxData[uuid]) {
                    self.ajaxData[uuid] = self.getBlockSettings(uuid);
                }
                if (self.ajaxData[uuid] && self.ajaxData[uuid].processing) {
                    return;
                }
                self.ajaxData[uuid].processing = true;
                var type = paginationTrigger.data('type');
                self.ajaxStartAnimation(block, 'replace');
                self.ajaxReplaceLoad(block, uuid, type);
            });
        }

        Module.ajaxReplaceLoad = function (block, uuid, type) {

            var self = this;

            if (!self.ajaxData[uuid].paged) {
                self.ajaxData[uuid].paged = 1;
            }
            if ('prev' === type) {
                self.ajaxData[uuid].page_next = parseInt(self.ajaxData[uuid].paged) - 1;
            } else {
                self.ajaxData[uuid].page_next = parseInt(self.ajaxData[uuid].paged) + 1;
            }

            var cacheID = self.cacheData.getCacheID(uuid, self.ajaxData[uuid].page_next);

            /** use cache */
            if (self.cacheData.exist(cacheID)) {
                var cache = self.cacheData.get(cacheID);
                if ('undefined' !== typeof cache.paged) {
                    self.ajaxData[uuid].paged = cache.paged;
                }
                setTimeout(function () {
                    self.ajaxRenderHTML(block, uuid, cache, 'replace');
                }, 250)

            } else {
                /** POST AJAX */
                $.ajax({
                    type: 'GET',
                    url: self.ajaxURL,
                    data: {
                        action: 'rblivep',
                        data: self.ajaxData[uuid]
                    },
                    success: function (response) {
                        response = JSON.parse(JSON.stringify(response));
                        if ('undefined' !== typeof response.paged) {
                            self.ajaxData[uuid].paged = response.paged;
                        }
                        self.cacheData.set(cacheID, response);
                        self.ajaxRenderHTML(block, uuid, response, 'replace');
                    }
                });
            }
        }

        Module.paginationLoadMore = function () {
            var self = this;
            self._body.on('click', '.loadmore-trigger', function (e) {

                e.preventDefault();
                e.stopPropagation();

                const paginationTrigger = $(this);
                if (paginationTrigger.hasClass('is-disable')) {
                    return;
                }

                var block = paginationTrigger.parents('.block-wrap');
                var uuid = block.attr('id');

                if (!self.ajaxData[uuid]) {
                    self.ajaxData[uuid] = self.getBlockSettings(uuid);
                }
                if (self.ajaxData[uuid] && self.ajaxData[uuid].processing) {
                    return;
                }
                self.ajaxData[uuid].processing = true;
                self.ajaxStartAnimation(block, 'append');
                self.ajaxAppendLoad(block, uuid);
            })
        }

        Module.paginationInfinite = function () {
            var self = this;
            var infiniteElements = $('.pagination-infinite');
            if (infiniteElements.length > 0) {
                infiniteElements.each(function () {
                    var paginationTrigger = $(this);
                    if (!paginationTrigger.hasClass('is-disable')) {
                        var block = paginationTrigger.parents('.block-wrap');
                        if ((block.hasClass('is-hoz-scroll') || block.hasClass('is-mhoz-scroll') || block.hasClass('is-thoz-scroll')) && window.outerWidth < 1024) {
                            paginationTrigger.addClass('is-disable');
                            return;
                        }
                        var uuid = block.attr('id');
                        var wPointID = 'infinite' + uuid;
                        if (!self.ajaxData[uuid]) {
                            self.ajaxData[uuid] = self.getBlockSettings(uuid);
                        }
                        var params = {
                            element: paginationTrigger,
                            offset: 'bottom-in-view',
                            handler: function (direction) {
                                if (self.ajaxData[uuid] && self.ajaxData[uuid].processing) {
                                    return;
                                }
                                if ('down' == direction) {
                                    self.ajaxData[uuid].processing = true;
                                    self.ajaxStartAnimation(block, 'append');
                                    self.ajaxAppendLoad(block, uuid);
                                }
                            }
                        }
                        self.wPoint[wPointID] = new Waypoint(params);
                    }
                });
            }
        }

        Module.ajaxAppendLoad = function (block, uuid) {
            var self = this;
            if (!self.ajaxData[uuid].paged) {
                self.ajaxData[uuid].paged = 1;
            }
            if (self.ajaxData[uuid].paged >= self.ajaxData[uuid].page_max) {
                return;
            }
            self.ajaxData[uuid].page_next = parseInt(self.ajaxData[uuid].paged) + 1;
            $.ajax({
                type: 'GET',
                url: self.ajaxURL,
                data: {
                    action: 'rblivep',
                    data: self.ajaxData[uuid]
                },
                success: function (response) {
                    response = JSON.parse(JSON.stringify(response));
                    if ('undefined' !== typeof response.paged) {
                        self.ajaxData[uuid].paged = response.paged;
                    }
                    if ('undefined' !== typeof response.notice) {
                        response.content = response.content + response.notice;
                    }
                    self.ajaxRenderHTML(block, uuid, response, 'append');
                }
            });
        }

        /** live search */
        Module.liveSearch = function () {

            var liveSearch = $('.live-search-form');
            if (liveSearch.length == 0) {
                return;
            }
            var self = this;

            liveSearch.each(function () {
                const liveSearchEl = $(this);
                const input = liveSearchEl.find('input[type="text"]');
                const responseWrap = liveSearchEl.find('.live-search-response');
                const animation = liveSearchEl.find('.live-search-animation');
                const limit = liveSearchEl.data('limit');
                const search = liveSearchEl.data('search');
                const follow = liveSearchEl.data('follow')

                input.attr('autocomplete', 'off');

                input.on('focus', function () {
                    var param = $(this).val();
                    if (param && !responseWrap.is(':empty')) {
                        responseWrap.css('height', 'auto').fadeIn(250);
                    }
                });

                let delay = (function () {
                    let timer = 0;
                    return function (callback, ms) {
                        clearTimeout(timer);
                        timer = setTimeout(callback, ms);
                    };
                })();

                input.keyup(function () {
                    var param = $(this).val();
                    delay(function () {
                        if (param) {
                            liveSearchEl.addClass('search-loading');
                            setTimeout(function () {
                                animation.fadeIn(200);
                            }, 250);

                            $.ajax({
                                type: 'GET',
                                url: self.ajaxURL,
                                data: {
                                    action: 'rbsearch',
                                    s: param,
                                    limit: limit,
                                    search: search,
                                    follow: follow,
                                },
                                success: function (data) {
                                    data = $.parseJSON(JSON.stringify(data));
                                    animation.fadeOut(200);
                                    setTimeout(function () {
                                        liveSearchEl.removeClass('search-loading');
                                    }, 250);
                                    responseWrap.hide().empty().css('height', responseWrap.height());
                                    responseWrap.html(data);
                                    if ('category' === search && follow && typeof FOXIZ_PERSONALIZE !== 'undefined') {
                                        FOXIZ_PERSONALIZE.syncLayoutCategories();
                                    }
                                    responseWrap.css('height', 'auto').fadeIn(250);
                                }
                            });
                        } else {
                            responseWrap.fadeOut(250, function () {
                                responseWrap.empty().css('height', 'auto');
                            });
                        }
                    }, 400);
                })
            });
        }

        /** personalized block */
        Module.personalizeBlocks = function () {
            var self = this;
            const elements = $('.is-ajax-block');
            if (elements.length > 0) {
                const blockRequests = elements.map(function () {
                    const block = $(this);
                    const uuid = block.attr('id');
                    if (!self.ajaxData[uuid]) {
                        self.ajaxData[uuid] = self.getBlockSettings(uuid);
                    }
                    return $.ajax({
                        type: 'GET',
                        url: self.ajaxURL,
                        data: {
                            action: 'rbpersonalizeb',
                            data: self.ajaxData[uuid]
                        }
                    });
                });

                Promise.all(blockRequests).then(responses => {
                    responses.forEach((response, index) => {
                        const block = $(elements[index]);
                        block.html(response).fadeIn(250);
                        block.dequeue();
                    });
                    self.reloadBlockFunc();
                });
            }
        };

        /** personalized categories */
        Module.personalizeCategories = function () {
            var self = this;
            const elements = $('.is-ajax-categories');
            if (elements.length > 0) {
                const categoryRequests = elements.map(function () {
                    const block = $(this);
                    const uuid = block.attr('id');
                    if (!self.ajaxData[uuid]) {
                        self.ajaxData[uuid] = self.getBlockSettings(uuid);
                    }
                    return $.ajax({
                        type: 'GET',
                        url: self.ajaxURL,
                        data: {
                            action: 'rbpersonalizecat',
                            data: self.ajaxData[uuid]
                        }
                    });
                });

                Promise.all(categoryRequests).then(responses => {
                    responses.forEach((response, index) => {
                        const block = $(elements[index]);
                        block.html(response).fadeIn(250);
                        block.dequeue();
                    });

                    self.reloadBlockFunc();

                    if (typeof FOXIZ_PERSONALIZE !== 'undefined') {
                        FOXIZ_PERSONALIZE.syncLayoutCategories();
                    }
                });
            }
        };

        Module.readingCollect = function () {
            var self = this;
            if (self.themeSettings.yesReadingHis === undefined) {
                return;
            }
            $.ajax({
                type: 'GET',
                url: self.ajaxURL,
                data: {
                    action: 'rbcollect',
                    id: self.themeSettings.yesReadingHis
                }
            });
        };

        /** header notification */
        Module.newsNotification = function () {

            const notificationWrapper = $('.rb-notification');
            if (notificationWrapper.length < 1) {
                return false;
            }
            const notificationID = this.personailizeUID + '-notification';
            const notificationCount = this.personailizeUID + '-notification-count';

            var self = this;
            const dot = $('.notification-info');
            const content = self.getTTLStorage(notificationID);
            const count = self.getTTLStorage(notificationCount);
            let reload = notificationWrapper.data('interval');
            if (!reload || isNaN(reload) || reload <= 0) {
                reload = 6;
            }
            if (!content) {
                $.ajax({
                    type: 'GET',
                    url: self.ajaxURL,
                    data: {
                        action: 'rbnotification',
                    },
                    success: function (response) {
                        response = $.parseJSON(JSON.stringify(response));
                        self.SetTTLStorage(notificationID, response.content, reload);
                        self.SetTTLStorage(notificationCount, response.count, reload);
                        notificationWrapper.append(response.content);
                        if (response.count) {
                            dot.append(response.count);
                            dot.css('opacity', 1);
                        }
                    }
                });
            } else {
                notificationWrapper.append(content);
                dot.append(count);
            }
        };

        /** register cache object */
        Module.cacheData = {

            data: {},
            get: function (id) {
                return this.data[id];
            },

            set: function (id, data) {
                this.remove(id);
                this.data[id] = data;
            },

            remove: function (id) {
                delete this.data[id];
            },

            getCacheID: function (blockID, currentPage) {
                return JSON.stringify('RB_' + blockID + '_' + currentPage);
            },

            exist: function (id) {
                return this.data.hasOwnProperty(id) && this.data[id] !== null;
            }
        }

        /**
         * ajax start animation
         * @param block
         * @param action
         */
        Module.ajaxStartAnimation = function (block, action) {

            var inner = block.find('.block-inner');
            block.find('.pagination-trigger').addClass('is-disable');
            inner.stop();

            if ('replace' === action) {
                inner.css('min-height', inner.outerHeight());
                inner.fadeTo('250', 0.05);
                inner.after('<i class="rb-loader loader-absolute"></i>');
            } else {
                block.find('.loadmore-trigger').addClass('loading');
                block.find('.rb-loader').css({'display': 'block'}).delay(250).animate({opacity: 1}, 250);
            }
        }

        /**
         * render ajax
         * @param block
         * @param uuid
         * @param response
         * @param action
         */
        Module.ajaxRenderHTML = function (block, uuid, response, action) {

            var self = this;
            block.delay(50).queue(function () {
                var uuid = block.attr('id');
                var inner = block.find('.block-inner');
                block.find('.pagination-trigger').removeClass('is-disable');
                inner.stop();

                if ('replace' === action) {
                    inner.html(response.content);
                    block.find('.rb-loader').animate({opacity: 0}, 250, function () {
                        $(this).remove();
                    })
                    inner.css('min-height', '');
                    inner.fadeTo(250, 1);

                } else {
                    var content = $(response.content);
                    inner.append(content);
                    content.addClass('is-invisible');
                    content.addClass('opacity-animate');

                    block.find('.rb-loader').animate({opacity: 0}, 250, function () {
                        $(this).css({'display': 'none'});
                    });
                    setTimeout(function () {
                        content.removeClass('is-invisible');
                    }, 250);
                    block.find('.loadmore-trigger').removeClass('loading');
                }

                /** reload */
                self.ajaxTriggerState(block, uuid);
                self.ajaxData[uuid].processing = false;
                block.dequeue();
                self.reloadBlockFunc();
            });
        }

        /**
         * set
         * @param block
         * @param uuid
         */
        Module.ajaxTriggerState = function (block, uuid) {
            var self = this;
            block.find('.pagination-trigger').removeClass('is-disable');
            if (self.ajaxData[uuid].paged < 2) {
                block.find('[data-type="prev"]').addClass('is-disable');
            } else if (self.ajaxData[uuid].paged >= self.ajaxData[uuid].page_max) {
                block.find('[data-type="next"]').addClass('is-disable');
                block.find('.loadmore-trigger').addClass('is-disable').hide();
                block.find('.pagination-infinite').addClass('is-disable').hide();
            }
        }

        /**
         * get block settings
         * @param uuid
         * @returns {string|*}
         */
        Module.getBlockSettings = function (uuid) {
            var settings;
            if ('undefined' !== typeof window[uuid]) {
                settings = window[uuid];
            }
            return this.cleanNull(settings);
        }

        /**
         * remove empty values
         * @param data
         * @returns {string|*}
         */
        Module.cleanNull = function (data) {
            if ('string' == typeof data) {
                return data;
            }
            $.each(data, function (key, value) {
                if (value === '' || value === null) {
                    delete data[key];
                }
            });

            return data;
        }

        /* ================================ SINGLE INFINITE ================================ */
        Module.singleInfiniteLoadNext = function () {

            const infiniteWrapper = $('#single-post-infinite');

            if (!infiniteWrapper.length) {
                return;
            }
            var self = this;
            const infiniteLoadPoint = $('#single-infinite-point');
            const animationIcon = infiniteLoadPoint.find('.rb-loader');
            const loadNexParam = {
                element: infiniteLoadPoint,
                offset: 'bottom-in-view',
                handler: function (direction) {
                    if (true === self.ajaxData['singleProcessing'] || 'up' == direction) {
                        return;
                    }
                    var nextPostURL = infiniteWrapper.data('nextposturl');
                    if (nextPostURL.indexOf('?') != -1 || nextPostURL.indexOf('#') != -1) {
                        nextPostURL = nextPostURL + '&rbsnp=1';
                    } else {
                        nextPostURL = nextPostURL + '?rbsnp=1';
                    }
                    self.ajaxData['singleProcessing'] = true;
                    animationIcon.css({'display': 'block'}).animate({opacity: 1}, 250);
                    $.ajax({
                        type: 'GET',
                        url: nextPostURL,
                        dataType: 'html',
                        success: function (response) {
                            response = $.parseJSON(JSON.stringify(response));
                            response = $('<div id="temp-dom"></div>').append($.parseHTML(response));
                            response = response.find('.single-post-outer');
                            var nextPostURL = response.data('nextposturl');
                            if ('undefined' !== typeof (nextPostURL) && nextPostURL.length > 0) {
                                infiniteWrapper.data('nextposturl', nextPostURL);
                            } else {
                                infiniteWrapper.removeAttr('id');
                                infiniteLoadPoint.remove();
                            }
                            animationIcon.animate({opacity: 0}, 250).delay(250).css({'display': 'none'});
                            infiniteWrapper.append(response);
                            self.ajaxData['singleProcessing'] = false;
                            setTimeout(function () {
                                self.reInitAll();
                                if (typeof FOXIZ_CORE_SCRIPT !== 'undefined') {
                                    FOXIZ_CORE_SCRIPT.loadGoogleAds(response);
                                    FOXIZ_CORE_SCRIPT.loadInstagram(response);
                                }
                            }, 1);
                        }
                    });
                }
            }

            self.wPoint['ajaxSingleNextPosts'] = new Waypoint(loadNexParam);
        };

        Module.singleScrollRefresh = function () {
            var infiniteWrapper = $('#single-post-infinite');
            if (!infiniteWrapper.length) {
                return;
            }
            var self = this;
            self.articleData = [];
            var articleOuter = infiniteWrapper.find('.single-post-outer');
            if (articleOuter.length > 0) {
                self.inviewPostID = articleOuter.eq(0).data('postid');
                articleOuter.each(function () {
                    var article = $(this);
                    var itemData = {
                        postID: article.data('postid'),
                        postURL: article.data('postlink'),
                        postTitle: article.find('h1.s-title').text(),
                        shareList: article.find('.sticky-share-list-buffer').html(),
                        top: article.offset().top,
                        bottom: article.offset().top + article.outerHeight(true)
                    };
                    if (self.readIndicator.length > 0) {
                        var content = article.find('div[itemprop="articleBody"]');
                        content = content.eq(0);
                        itemData.indicatorTop = content.offset().top;
                        itemData.indicatorHeight = content.outerHeight(true) - self._window.height();
                    }
                    self.articleData.push(itemData);
                });
                if (window.addEventListener) {
                    window.addEventListener('scroll', function () {
                        self.animationFrame(self.scrollToUpdateArticle.bind(self));
                    }, false);
                }
            }
        }

        /** scrollToUpdateArticle */
        Module.scrollToUpdateArticle = function () {
            var self = this;
            var scroll = self._window.scrollTop();
            self.articleData.every(function (article) {
                if (scroll > (article.top + 5) && scroll < (article.bottom - 5)) {
                    if (article.indicatorTop) {
                        self.readIndicatorPercent = ((scroll - article.indicatorTop) / article.indicatorHeight) * 100;
                        if (self.readIndicatorPercent <= 130) {
                            self.readIndicator.css('width', self.readIndicatorPercent + '%');
                        }
                    }
                    if (article.postID != self.inviewPostID) {
                        self.inviewPostID = article.postID;
                        if (article.postURL !== '') {
                            history.replaceState(null, null, article.postURL);
                        }
                        document.title = article.postTitle;
                        $('.single-post-outer').removeClass('activated');
                        $('[data-postid="' + article.postID + '"]').addClass('activated');
                        $('#s-title-sticky .sticky-title').hide().html(article.postTitle).fadeIn(300);
                        $('#s-title-sticky .sticky-share-list').html(article.shareList);
                        self._body.find('.floating-video').removeClass('floating-activated');

                        if (typeof FOXIZ_CORE_SCRIPT !== 'undefined') {
                            FOXIZ_CORE_SCRIPT.updateGA(article);
                        }
                    }
                    return false;
                }
                return true;
            });
        }

        /** productQuantity */
        Module.productQuantity = function () {
            this._document.on('click', '.quantity .quantity-btn', function (e) {
                e.preventDefault();
                var button = $(this);
                var step = 1;
                var input = button.parent().find('input');
                var min = 1;
                var max = 9999;
                var value_old = parseInt(input.val());
                var value_new = parseInt(input.val());

                if (input.attr('step')) {
                    step = parseInt(input.attr('step'));
                }

                if (input.attr('min')) {
                    min = parseInt(input.attr('min'));
                }

                if (input.attr('max')) {
                    max = parseInt(input.attr('max'));
                }

                if (button.hasClass('up')) {
                    if (value_old < max) {
                        value_new = value_old + step;
                    } else {
                        value_new = 1;
                    }
                } else if (button.hasClass('down')) {
                    if (value_old > min) {
                        value_new = value_old - step;
                    } else {
                        value_new = 0;
                    }
                }

                if (!input.attr('disabled')) {
                    input.val(value_new).change();
                }
            });
        }

        Module.cartNotification = function () {
            var self = this;
            var element = $('#add-cart-popup');
            self._body.on('added_to_cart', function () {
                element.fadeIn(200);
                self.cartNotificationTimeOut = setTimeout(function () {
                    element.fadeOut(200);
                }, 5000);
                self.hoveringNotification(element);

            });
        }

        Module.hoveringNotification = function (element) {
            var self = this;

            element.on('mouseenter', function () {
                clearTimeout(self.cartNotificationTimeOut);
            });

            element.on('mouseleave', function () {
                setTimeout(function () {
                    element.fadeOut(200);
                }, 1500);
            });
        }

        /** like/dislike */
        Module.likeToggle = function () {

            var self = this;
            let reaction = '';
            self._body.on('click', '.like-trigger, .dislike-trigger', function (e) {

                e.preventDefault();
                e.stopPropagation();

                const target = $(this);
                const wrap = target.parent('[data-like]');
                const pid = wrap.data('like');
                if (!pid || self.isProgress) {
                    return false;
                }

                self.isProgress = true;
                const storageKey = self.getLikeKey(pid);
                let isLike = target.hasClass('like-trigger');
                let isDislike = target.hasClass('dislike-trigger');
                let sibEl = target.siblings();
                let likeCountEl = wrap.find('.like-count');
                let dislikeCountEl = wrap.find('.dislike-count');
                let likeCountText = likeCountEl.text();
                let dislikeCountText = dislikeCountEl.text();

                if (isLike) {
                    if (!target.hasClass('triggered')) {
                        reaction = 'like';
                        if (!/[kKmM]/.test(likeCountText)) {
                            likeCountText = parseInt(likeCountText);
                            if (isNaN(likeCountText)) {
                                likeCountText = 0;
                            }
                            likeCountEl.text(likeCountText + 1);
                        }
                        target.addClass('triggered');

                        if (sibEl.hasClass('triggered')) {
                            sibEl.removeClass('triggered');
                            if (!/[kKmM]/.test(dislikeCountText)) {
                                dislikeCountText = parseInt(dislikeCountText) - 1;
                                if (isNaN(dislikeCountText) || dislikeCountText < 1) {
                                    dislikeCountText = '';
                                }
                                dislikeCountEl.text(dislikeCountText);
                            }
                        }
                        self.setStorage(storageKey, 'like');
                    } else {
                        reaction = 'rmlike';
                        if (!/[kKmM]/.test(likeCountText)) {
                            likeCountText = parseInt(likeCountText) - 1;
                            if (isNaN(likeCountText) || likeCountText < 1) {
                                likeCountText = '';
                            }
                            likeCountEl.text(likeCountText);
                        }
                        target.removeClass('triggered');
                        self.deleteStorage(storageKey);
                    }
                } else if (isDislike) {
                    if (!target.hasClass('triggered')) {
                        reaction = 'dislike';
                        if (!/[kKmM]/.test(dislikeCountText)) {
                            dislikeCountText = parseInt(dislikeCountText);
                            if (isNaN(dislikeCountText)) {
                                dislikeCountText = 0;
                            }
                            dislikeCountEl.text(dislikeCountText + 1);
                        }
                        target.addClass('triggered');
                        if (sibEl.hasClass('triggered')) {
                            sibEl.removeClass('triggered');
                            if (!/[kKmM]/.test(likeCountText)) {
                                likeCountText = parseInt(likeCountText) - 1;
                                if (isNaN(likeCountText) || likeCountText < 1) {
                                    likeCountText = '';
                                }
                                likeCountEl.text(likeCountText);
                            }
                        }
                        self.setStorage(storageKey, 'dislike');
                    } else {
                        reaction = 'rmdislike';
                        if (!/[kKmM]/.test(dislikeCountText)) {
                            likeCountText = parseInt(likeCountText) - 1;
                            if (isNaN(likeCountText) || likeCountText < 1) {
                                likeCountText = '';
                            }
                            dislikeCountEl.text(likeCountText);
                        }
                        target.removeClass('triggered');
                        self.deleteStorage(storageKey);
                    }
                }

                $.ajax({
                    type: 'GET',
                    url: self.ajaxURL,
                    data: {
                        uuid: self.personailizeUID,
                        action: 'rbvoting',
                        value: reaction,
                        pid: pid,
                    },
                    complete: (response, textStatus) => {
                        self.isProgress = false;
                    }
                })
            })
        }

        /** sync layout */
        Module.syncLayoutLike = function () {
            this.isProgress = true;
            const likeElements = document.querySelectorAll('[data-like]:not(.loaded)');
            const jsCount = this._body.hasClass('is-jscount');
            let count, countEl;

            for (const el of likeElements) {
                el.classList.add('loaded');
                const key = this.getLikeKey(el.getAttribute('data-like'));
                const triggered = this.getStorage(key);

                if (!triggered) continue;

                const likeEl = el.querySelector('.el-like');
                const dislikeEl = el.querySelector('.el-dislike');

                if (!likeEl || !dislikeEl) continue;

                if (triggered === 'like') {
                    likeEl.classList.add('triggered');
                    countEl = jsCount ? likeEl.querySelector('.like-count') : null;
                    if (countEl) {
                        count = parseInt(countEl.textContent.trim()) || 0;
                        countEl.textContent = count + 1;
                    }
                } else if (triggered === 'dislike') {
                    dislikeEl.classList.add('triggered');
                    countEl = jsCount ? dislikeEl.querySelector('.dislike-count') : null;
                    if (countEl) {
                        count = parseInt(countEl.textContent.trim()) || 0;
                        countEl.textContent = count + 1;
                    }
                }
            }

            this.isProgress = false;
        };


        /** like key */
        Module.getLikeKey = function (id) {
            return this.personailizeUID + '-like-' + id;
        }

        return Module;

    }(FOXIZ_MAIN_SCRIPT || {}, jQuery)
)

/** init */
jQuery(document).ready(function ($) {
    FOXIZ_MAIN_SCRIPT.init();
});

/** Elementor editor */
jQuery(window).on('elementor/frontend/init', function () {
    FOXIZ_MAIN_SCRIPT.isElementorEditor = true;
    FOXIZ_MAIN_SCRIPT.editorDarkModeInit = false;

    if (window.elementor && window.elementorFrontend) {
        elementorFrontend.hooks.addAction('frontend/element_ready/widget', FOXIZ_MAIN_SCRIPT.initElementor);
    }
});