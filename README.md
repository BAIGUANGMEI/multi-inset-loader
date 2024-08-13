# multi-inset-loader

在使用大佬制作的vue-inset-loader的包时遇到跨平台的问题，因此在大佬的基础上修改了一点，重新打包方便使用。

原版的地址：https://github.com/1977474741/vue-inset-loader

#### 编译阶段在sfc模板指定位置插入自定义内容，适用于webpack构建的vue应用，常用于小程序需要全局引入组件的场景。（由于小程序没有开放根标签，没有办法在根标签下追加全局标签，所以要使用组件必须在当前页面引入组件标签）

### 第一步 安装

    npm i multi-inset-loader

### 第二步 vue.config.js注入loader

    module: {
        rules: [
          {
            test: /\.vue$/,
            use:{
                loader: "vue-inset-loader"
                // // 针对Hbuilder工具创建的uni-app项目
                // loader: path.resolve(__dirname,"./node_modules/multi-inset-loader")
            }
          }
        ]
    },
    // 支持自定义pages.json文件路径
    // options: {
    //     pagesPath: path.resolve(__dirname,'./src/pages.json')
    // }

### 第三步 pages.json配置文件中添加insetLoader

    "insetLoader": {
        "config":{
            "confirm": "<BaseConfirm ref='confirm'></BaseConfirm>",
            "abc": "<BaseAbc ref='BaseAbc'></BaseAbc>"
        },
        // 全局配置
        "label":["confirm"],
        "rootEle":"div"
    },
    "pages": [
        {
            "path": "pages/tabbar/index/index",
            "style": {
                "navigationBarTitleText": "测试页面",
                // 单独配置，用法跟全局配置一致，优先级高于全局
                "label": ["confirm","abc"],
                "rootEle":"div"
            }
        },
    ]

###  配置说明

 - `config` (default: `{}`)
    定义标签名称和内容的键值对
 - `label`(default: `[]`)
    需要全局引入的标签，打包后会在所有页面引入此标签
 - `rootEle`(default: `"div"`)
    根元素的标签类型，缺省值为div，支持正则，比如匹配任意标签 ".*"

 ✔ `label` 和 `rootEle` 支持在单独页面的style里配置，优先级高于全局配置