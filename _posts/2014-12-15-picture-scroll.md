---
layout: post
title: Picture scrolling
logo: http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/1_zps2e587ec7.jpg
categories:
- Study
tags:
- Javascript
- jQuery
- picture
---

From last Friday till now, I almost finish the widget about scrolling pictures in about three days. Although it is not perfect, I will continuously enhance it and  provide more parameters and APIs in order to adapt more scenes.    

{% highlight javascript %}
/**
 * Scroll pictures.
 * @project: https://github.com/luolinjia/jquery-picScrolling
 * @auth: Karl Luo(360512239@qq.com)
 * @dependence jQuery
 * @usage:
 * <div id="testBox">
 *    <img src="img/1.jpg"/>
 *    <img src="img/2.jpg"/>
 *    <img src="img/3.jpg"/>
 *    <img src="img/4.jpg"/>
 * </div>
 *
 * $('#testBox').picScroll({
 *      size: {'height': '300px', width: '800px'}, // show the img size, this is required.
 *      color: '#999',  // the navigation color
 *      hovercolor: '#fff',   // the navigation hovercolor
 *      time: 2000,  // the time every scrolling
 *      hasNumber: false  // whether showing number, the default value is false
 * });
 **/
(function ($) {
    $.fn.picScroll = function (options) {
        var index = 1,
            time,
            o = $(this),
            qty = o.find('img').length,
            imgCSS = {
                'position': 'absolute',
                'z-index': 1
            },
            imgParentCSS = {
                'position': 'relative'
            },
            baseCSS = {
                'position': 'absolute',
                'width': '100%',
                'height': '30px',
                'bottom': 0,
                'background-color': 'rgba(221, 221, 221, 0.4)',
                'z-index': 2
            },
            naviCSS = {
                'float': 'left',
                'display': 'block',
                'width': '20px',
                'height': '20px',
                'margin-left': '10px',
                'margin-top': '5px',
                'cursor': 'pointer'
            },
            setting = {
                position: 'bl',  //tl, tr, bl, br
                style: 'square', // square, round
                time: 2000,
                hasNumber: false,
                color: '#068d72',
                hovercolor: '#6fcebb',
                size: {
                    'width': '800px',
                    'height': '300px'
                }
            },
            _ = {
                renderCSS: function () {
                    o.css(imgParentCSS).css(setting.size).find('img').css(imgCSS).css(setting.size);
                },
                renderNavi: function () {
                    var list = [];
                    for (var i = 0, size = qty; i < size; i++) {
                        list.push('<span data-no="' + (i+1) + '">' + (setting.hasNumber ? i+1 : '') + '</span>');
                    }
                    var dom = '<div id="navis">' + list.join('') + '</div>';
                    o.append(dom);
                    $('#navis').css(baseCSS).find('span').css(naviCSS).css(setting.color);
                },
                switchPic: function (num) {
                    index = num;
                    $('span', o).css(naviCSS).css('background-color', setting.color).eq(index - 1).css('background-color', '').css('background-color', setting.hovercolor);
                    $('img', o).hide().stop(true, true).eq(index - 1).fadeIn(500);
                    index = index + 1 > qty ? 1 : index + 1;
                    time = setTimeout(function () {
                        _.switchPic(index);
                    }, setting.time);
                },
                bindHover: function () {
                    $('span', $('#navis')).hover(function () {
                        clearTimeout(time);
                        var thiz = $(this), item = thiz.attr('data-no');
                        $('span', o).css(naviCSS).css('background-color', setting.color).eq(item - 1).css('background-color', '').css('background-color', setting.hovercolor);
                        $('img', o).hide().stop(true, true).eq(item - 1).fadeIn(500);
                    }, function () {
                        var thiz = $(this);
                        index = thiz.attr('data-no') > (qty - 1) ? 1 : parseInt(thiz.attr('data-no')) + 1;
                        time = setTimeout(function () {
                            _.switchPic(index);
                        }, setting.time);
                    });
                }
            };

        setting = $.extend(setting, options);
        _.renderCSS();
        _.renderNavi();
        _.switchPic(index);
        _.bindHover();
    };
})(jQuery);
{% endhighlight %}  

> If you have any question or suggestion, please contact with me.  