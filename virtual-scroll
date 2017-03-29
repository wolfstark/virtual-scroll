/**
 *  依赖jq的自定义样式滚动条
 * @param  {contextObject} global 命名空间
 * @param  {DomObject} doc    root对象
 * @param  {function} $      jq函数
 */
! function (global, doc, $) {
    function CusScrillBar(config) {
        this._init(config);
    }

    $.extend(true, CusScrillBar.prototype, {
        _init: function (config) {
            var self = this;
            self.config = {
                scrollDir: "y",
                contSelector: "",
                barSelector: "",
                sliderSelector: "",
                wheelStep: 10
            };
            $.extend(true, self.config, config || {});
            self._initDomEvent();
            return self;
        },
        _initDomEvent: function () {
            var conf = this.config;
            this.$cont = $(conf.contSelector);
            this.$slider = $(conf.sliderSelector);
            this.$bar = conf.barSelector ? $(conf.barSelector) :
                this.$slider.parent();
            this.$doc = $(doc);
            /**/
            this._initSliderDragEvent()
                ._bindContSCroll()
                ._bindMousewheel();
        },
        _initSliderDragEvent: function () {
            var slider = this.$slider,
                sliderEl = slider.get(0);

            function mousemoveHandler(e) {
                e.preventDefault();
                if (dargStartPagePosition === null) {
                    return;
                }
                self.scrollTo(dargStartScrollPositon + (e.pageY -
                    dargStartPagePosition) * dargContBarRate);
                self._bindContSCroll();
            }

            if (sliderEl) {
                var doc = this.$doc,
                    dargStartPagePosition,
                    dargStartScrollPositon,
                    dargContBarRate,
                    self = this;

                slider.on('mousedown', function (event) {
                    event.preventDefault();
                    dargStartPagePosition = event.pageY;
                    dargStartScrollPositon = self.$cont.get(0)
                        .scrollTop;
                    dargContBarRate = self.getMaxScrollPosition() /
                        self.getMaxSliderPosition();


                    doc.on('mousemove.scroll', mousemoveHandler)
                        .on('mouseup.scroll', function (event) {
                            event.preventDefault();
                            doc.off("mousemove.scroll mouseup.scroll");
                            // doc.off("mouseup.scroll");
                        });
                });

            }
            return self;
        },
        //监听内容滚动，同步滑块位置
        _bindContSCroll: function () {
            var self = this;
            self.$cont.on("scroll", function () {
                var sliderEl = self.$slider && self.$slider.get(0);
                if (sliderEl) {
                    sliderEl.style.top = self
                        .getSliderPosition() + "px";
                }
            });
            return self;
        },
        _bindMousewheel: function () {
            var self = this;
            self.$cont.on("mousewheel DOMMouseScroll", function (e) {
                e.preventDefault();
                var oEv = e.originalEvent,
                    wheelRange = oEv.wheelDelta ?
                    -oEv.wheelDelta / 120 : (oEv.detail || 0) / 3;
                self.scrollTo(self.$cont[0].scrollTop +
                    wheelRange * self.config.wheelStep);
            });
            return self;
        },
        //计算滑块的当前位置
        getSliderPosition: function () {
            var self = this,
                maxSliderPosition = self.getMaxSliderPosition();
            return Math.min(maxSliderPosition,
                maxSliderPosition * self.$cont.get(0).scrollTop /
                self.getMaxScrollPosition());
        },
        //侧边栏可滚动的高度
        getMaxScrollPosition: function () {
            var self = this;
            return Math.max(self.$cont.innerHeight(), self.$cont.get(0)
                .scrollHeight) - self.$cont.innerHeight();
        },
        //内容可滚动的高度
        getMaxSliderPosition: function () {
            var self = this;
            return self.$bar.innerHeight() - self.$slider.outerHeight();
        },
        scrollTo: function (positionVar) {
            var self = this;
            self.$cont.scrollTop(positionVar);
        }
    });
    global.CustomScroll = CusScrillBar;
}(window, window.document, window.jQuery);
