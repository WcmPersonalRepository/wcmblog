# 移动端兼容性问题

## ISO input 自动填充背景黄色

问题描述：iOS 部分 Safari 浏览器版本从外部复制粘贴内容到 input 输入框中，会出现黄色背景。原因是复制粘贴触发了浏览器的自动填充伪类：-webkit-autofill

解决方案 1 ：用阴影填充

```css
input:-webkit-autofill,
textarea:-webkit-autofill,
select:-webkit-autofill {
  -webkit-box-shadow: 0 0 0 1000px white inset;
}
```

解决方案 2 ：修改成透明色

```css
input[type='text'] {
  border: 1px solid #333333 !important;
  padding: 0px 12px 0px 32px;
  background: transparent !important;
}
input[type='password'] {
  border: 1px solid #333333 !important;
  padding: 0px 12px 0px 32px;
  background: transparent !important;
}

@-webkit-keyframes autofill {
  to {
    color: #666;
    background: transparent;
  }
}

input:-webkit-autofill {
  animation-name: autofill !important;
  animation-fill-mode: both !important;
}

input:-webkit-autofill,
input:-webkit-autofill:hover,
input:-webkit-autofill:focus {
  background-clip: content-box !important;
}
```

## ISO new Date()问题

问题描述：苹果的 Safari 浏览器中 new Date()创建实例是传入类似 yyyy-MM-dd hh:mm:ss 时间字符串无论传入什么时间，都会得到 1970 年 1 月 1 日或 Invalid Date 的情况。原因是在苹果的 Safari 浏览器中想用时间字符串创建时间日期分隔符必须为“/”，即类似 yyy/MM/dd hh:mm:ss 的时间字符串格式

解决方案：

```js
new Date('2019/02/02 08:08:08')
```

## 移动端输入框被遮挡问题

```js
setTimeout(function() {
  document.body.scrollTop = document.body.scrollHeight
}, 300)
```

## iOS 浏览器 video 自动全屏

问题描述：在 iOS 浏览器中的 video 元素，会出现自动全屏的情况

::: tip 官网描述
You must set this property to play inline video. Set this property to true to play videos inline. Set this property to false to use the native full-screen controller. When adding a video element to a HTML document on the iPhone, you must also include the playsinline attribute.
:::

::: tip 官网描述
Apps created before iOS 10.0 must use the webkit-playsinline attribute.
:::

解决方案：

```html
<video playsinline="true" webkit-playsinline="true"></video>
```

## ios 键盘弹起把页面弹起回落

问题描述：在 iOS 浏览器中当输入框键盘弹起输入完成后，页面无法自动回落

解决方案：

```js
document.body.scrollTop && (document.body.scrollTop = 0)
```

## 微信浏览器中保存长按保存解决方案

问题描述：微信浏览器中可能会出现长按图片不弹起保存图片菜单的情况

解决方案：给图片添加以下样式

```css
-webkit-touch-callout: none;
```

## 识别 360 浏览器的方法

```js
//方案1：www.360.com域名下360浏览器极速模式下
navigator.userAgent =
  'Mozilla/5.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/50.0.2661.102 Safari/537.36 QIHU 360EE'

//方案2：
window.onload = function() {
  //application/vnd.chromium.remoting-viewer 可能为360特有
  var is360 = _mime('type', 'application/vnd.chromium.remoting-viewer')
  if (isChrome() && is360) {
    alert('检测到是360浏览器')
  }
}
//检测是否是谷歌内核(可排除360及谷歌以外的浏览器)
function isChrome() {
  var ua = navigator.userAgent.toLowerCase()
  return ua.indexOf('chrome') > 1
}
//测试mime
function _mime(option, value) {
  var mimeTypes = navigator.mimeTypes
  for (var mt in mimeTypes) {
    if (mimeTypes[mt][option] == value) {
      return true
    }
  }
  return false
}

//方案3：
function is360() {
  if (window.navigator.userAgent.indexOf('Chrome') != -1) {
    return window.navigator.webkitPersistentStorage?false:true
  }esle{
    return false
  }
}

//方案4：
/**
 * 1. 获取ua字符串
 * $.ua().ua;
 *
 * 2. 设置ua字符串
 * $.ua("string");
 *
 * 3. 获取参数
 * $.ua().platform;
 * $.ua().browser;
 * $.ua().engine;
 *
 * 4. 内核判断
 * $.ua().isWebkit;
 * $.ua().isGecko;
 * $.ua().isTrident;
 *
 * 4. 外壳判断
 * $.ua().isChrome;
 * $.ua().isFirefox;
 * $.ua().is360se;
 * $.ua().is360ee;
 * $.ua().isLiebao;
 * $.ua().isSougou;
 * $.ua().ie;
 * $.ua().isIe;
 * $.ua().isIe6;
 * $.ua().isIe7;
 * $.ua().isIe8;
 * $.ua().isIe9;
 * $.ua().isIe10;
 * $.ua().isIe11;
 */
;
(function ($, win, undefined) {
    var UA = win.navigator.userAgent,
        doc = win.document,
        parseRule = {
            platforms: [
                // windows phone
                {
                    name: 'windows phone',
                    versionSearch: 'windows phone os ',
                    versionNames: [ // windows phone must be tested before win
                        {
                            number: '7.5',
                            name: 'mango'
                        }
                    ]
                },
                // windows
                {
                    name: 'win',
                    slugName: 'windows',
                    versionSearch: 'windows(?: nt)? ',
                    versionNames: [{
                        number: '6.2',
                        name: 'windows 8'
                    }, {
                        number: '6.1',
                        name: 'windows 7'
                    }, {
                        number: '6.0',
                        name: 'windows vista'
                    }, {
                        number: '5.2',
                        name: 'windows xp'
                    }, {
                        number: '5.1',
                        name: 'windows xp'
                    }, {
                        number: '5.0',
                        name: 'windows 2000'
                    }]
                },
                // ipad
                {
                    name: 'ipad',
                    versionSearch: 'cpu os ',
                    flags: ['ios']
                },
                // ipad and ipod must be tested before iphone
                {
                    name: 'ipod',
                    versionSearch: 'iphone os ',
                    flags: ['ios']
                },
                // iphone
                {
                    name: 'iphone',
                    versionSearch: 'iphone os ',
                    flags: ['ios']
                },
                // iphone must be tested before mac
                {
                    name: 'mac',
                    versionSearch: 'os x ',
                    versionNames: [{
                        number: '10.8',
                        name: 'mountainlion'
                    }, {
                        number: '10.7',
                        name: 'lion'
                    }, {
                        number: '10.6',
                        name: 'snowleopard'
                    }, {
                        number: '10.5',
                        name: 'leopard'
                    }, {
                        number: '10.4',
                        name: 'tiger'
                    }, {
                        number: '10.3',
                        name: 'panther'
                    }, {
                        number: '10.2',
                        name: 'jaguar'
                    }, {
                        number: '10.1',
                        name: 'puma'
                    }, {
                        number: '10.0',
                        name: 'cheetah'
                    }]
                },
                // android
                {
                    name: 'android',
                    versionSearch: 'android ',
                    versionNames: [
                        // android must be tested before linux
                        {
                            number: '4.1',
                            name: 'jellybean'
                        }, {
                            number: '4.0',
                            name: 'icecream sandwich'
                        }, {
                            number: '3.',
                            name: 'honey comb'
                        }, {
                            number: '2.3',
                            name: 'ginger bread'
                        }, {
                            number: '2.2',
                            name: 'froyo'
                        }, {
                            number: '2.',
                            name: 'eclair'
                        }, {
                            number: '1.6',
                            name: 'donut'
                        }, {
                            number: '1.5',
                            name: 'cupcake'
                        }
                    ]
                },
                // blackberry
                {
                    name: 'blackberry',
                    versionSearch: '(?:blackberry\\d{4}[a-z]?|version)/'
                },
                // blackberry
                {
                    name: 'bb',
                    slugName: 'blackberry',
                    versionSearch: '(?:version)/'
                },
                // blackberry
                {
                    name: 'playbook',
                    slugName: 'blackberry',
                    versionSearch: '(?:version)/'
                },
                // linux
                {
                    name: 'linux'
                },
                // nokia
                {
                    name: 'nokia'
                }
            ],
            browsers: [{
                    name: 'iemobile',
                    versionSearch: 'iemobile/'
                }, // iemobile must be tested before msie
                {
                    name: 'msie',
                    slugName: 'ie',
                    versionSearch: 'msie '
                }, {
                    name: 'firefox',
                    versionSearch: 'firefox/'
                }, {
                    name: 'chrome',
                    versionSearch: 'chrome/'
                }, // chrome must be tested before safari
                {
                    name: 'safari',
                    versionSearch: '(?:browser|version)/'
                }, {
                    name: 'opera',
                    versionSearch: 'version/'
                }
            ],
            engines: [{
                    name: 'trident',
                    versionSearch: 'trident/'
                }, {
                    name: 'webkit',
                    versionSearch: 'webkit/'
                }, // webkit must be tested before gecko
                {
                    name: 'gecko',
                    versionSearch: 'rv:'
                }, {
                    name: 'presto',
                    versionSearch: 'presto/'
                }
            ]
        },
        // [10,)版本就无法判断
        ieVer = (function () {
            var v = 3,
                p = doc.createElement('p'),
                all = p.getElementsByTagName('i');
            while (
                p.innerHTML = '<!--[if gt IE ' + (++v) + ']><i></i><![endif]-->',
                all[0]);
            return v > 4 ? v : 0;
        }()),
        ieAX = win.ActiveXObject,
        ieMode = doc.documentMode,
        isIe = ieAX || ieMode,
        isIe6 = (ieAX && ieVer == 6) || (ieMode == 6),
        isIe7 = (ieAX && ieVer == 7) || (ieMode == 7),
        isIe8 = (ieAX && ieVer == 8) || (ieMode == 8),
        isIe9 = (ieAX && ieVer == 9) || (ieMode == 9),
        isIe10 = ieMode === 10,
        isIe11 = ieMode === 11,
        isChrome = !isIe && _mime("type", "application/vnd.chromium.remoting-viewer"),
        isLiebao = !isIe && !! win.external && !! win.external.LiebaoAutoFill_CopyToClipboard,
        is360ee = !isIe && !isChrome && !isLiebao && _plugins("filename", "pepflashplayer.dll"),
        is360se = !isIe && !is360ee && _mime("suffixes", "dll", "description", /fancy/),
        isSougou = !isIe && _plugins("filename", "NPComBrg310.dll"),
        isFirefox = win.scrollMaxX !== undefined;
    if (isIe6) {
        ieVer = 6;
    } else if (isIe7) {
        ieVer = 7;
    } else if (isIe8) {
        ieVer = 8;
    } else if (isIe9) {
        ieVer = 9;
    } else if (isIe10) {
        ieVer = 10;
    } else if (isIe11) {
        ieVer = 11;
    }
    $.extend({
        ua: function () {
            var args = arguments,
                argL = args.length,
                ua = (argL == 1 && $.type(args[0]) == "string" ? args[0] : UA).toLowerCase(),
                objPlatform = _parse(parseRule.platforms, ua),
                objBrowser = _parse(parseRule.browsers, ua, true),
                objEngine = _parse(parseRule.engines, ua);
            return {
                // 返回ua字符串
                ua: ua,
                // 操作平台
                platform: $.extend({}, objPlatform, {
                    os: win.navigator.platform.toLowerCase(),
                    plugins: win.navigator.plugins
                }),
                // 浏览器内核
                engine: objEngine,
                // 浏览器外壳
                browser: objBrowser,
                // ie
                isIe: !! ieVer,
                isIe6: isIe6,
                isIe7: isIe7,
                isIe8: isIe8,
                isIe9: isIe9,
                isIe10: isIe10,
                isIe11: isIe11,
                ie: ieVer,
                // 内核
                isWebkit: !! objEngine.isWebkit,
                isGecko: !! objEngine.isGecko,
                isTrident: !! objEngine.isTrident,
                // 外壳[优先特征判断]
                isChrome: isChrome,
                is360ee: is360ee,
                is360se: is360se,
                isSougou: isSougou,
                isLiebao: isLiebao,
                isFirefox: isFirefox,
                // 类型
                isMobile: objPlatform.isMobile,
                isTablet: objPlatform.isTablet,
                isDesktop: objPlatform.isDesktop
            };
        }
    });
    /**
     * 解析
     * 参考：https://github.com/terkel/jquery-ua
     * @param  {Array} 需要解析的数据
     * @param  {String} 需要解析的ua字符串
     * @param  {Boolean} 是否为解析浏览器数据
     * @return {Object} 解析后的对象
     * @version 1.0
     * 2013年9月27日13:36:47
     */
    function _parse(rule, ua, isBrowser) {
        var item = {},
            name,
            versionSearch,
            flags,
            versionNames,
            i,
            is,
            ic,
            j,
            js,
            jc;
        if (isBrowser && ieVer) {
            return {
                name: "ie",
                ie: true,
                version: ieVer,
                isIe: true
            }
        }
        for (i = 0, is = rule.length; i < is; i++) {
            ic = rule[i];
            name = ic.name;
            versionSearch = ic.versionSearch;
            flags = ic.flags;
            versionNames = ic.versionNames;
            if (ua.indexOf(name) !== -1) {
                item.name = name.replace(/\s/g, '');
                if (ic.slugName) {
                    item.name = ic.slugName;
                }
                item["is" + _upperCase1st(item.name)] = true;
                item.version = ('' + (new RegExp(versionSearch + '(\\d+((\\.|_)\\d+)*)').exec(ua) || [, 0])[1]).replace(/_/g, '.');
                if (flags) {
                    for (j = 0, js = flags.length; j < js; j++) {
                        item["is" + _upperCase1st(flags[j])] = true;
                    }
                }
                if (versionNames) {
                    for (j = 0, js = versionNames.length; j < js; j++) {
                        jc = versionNames[j];
                        if (item.version.indexOf(jc.number) === 0) {
                            item.fullname = jc.name;
                            item["is" + _upperCase1st(item.fullname)] = true;
                            break;
                        }
                    }
                }
                if (rule === parseRule.platforms) {
                    item.isMobile = /mobile|phone/.test(ua) || item.isBlackberry;
                    item.isMobile = item.isMobile === undefined ? false : true;
                    item.isTablet = /tablet/.test(ua) || item.isIpad || (item.isAndroid && !/mobile/.test(ua));
                    item.isTablet = item.isTablet === undefined ? false : true;
                    if (item.isTablet) item.isMobile = false;
                    item.isDesktop = !item.isMobile && !item.isTablet ? true : false;
                    if (item.ios) {
                        item.fullname = 'ios' + parseInt(item.version, 10);
                        item["is" + _upperCase1st(item.fullname)] = true;
                    }
                }
                break;
            }
        }
        if (!item.name) {
            item['isUnknown'] = true;
            item.name = '';
            item.version = '';
        }
        return item;
    }
    // 大写第一个字母
    function _upperCase1st(string) {
        return string.replace(/^(\w)/, function (w) {
            return w.toUpperCase()
        });
    }
    // 测试mime
    function _mime(where, value, name, nameReg) {
        var mimeTypes = win.navigator.mimeTypes,
            i;
        for (i in mimeTypes) {
            if (mimeTypes[i][where] == value) {
                if (name !== undefined && nameReg.test(mimeTypes[i][name])) return true;
                else if (name === undefined) return true;
            }
        }
        return false;
    }
    // 测试plugins
    function _plugins(where, value) {
        var plugins = win.navigator.plugins,
            i;
        for (i in plugins) {
            if (plugins[i][where] == value) return true;
        }
        return false;
    }
})(jQuery, this);
```
