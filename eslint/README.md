# eslint安装指南
> 本文只介绍在mac下sublime的安装...其他的环境还未尝试:(

1、npm全局安装eslint

```javascript
npm install eslint -g
```

2、sublime中安装两个插件——SublimeLinter与SublimeLinter-contrib-eslint<br/>
（调出package control安装）<br/>

3、在sublime中配置sublimeLinter<br/>
配置的地方：Preferences-->Package Settings-->sublimeLinter-->Settings User<br/>
将如下内容复制进去（node的路径需要自己配，在paths中）

```javascript
{
    "user": {
        "debug": true,
        "delay": 0.25,
        "error_color": "D02000",
        "gutter_theme": "Packages/SublimeLinter/gutter-themes/Default/Default.gutter-theme",
        "gutter_theme_excludes": [],
        "lint_mode": "background",
        "linters": {
            "eslint": {
                "@disable": false,
                "args": [],
                "excludes": []
            },
            "jshint": {
                "@disable": false,
                "args": [],
                "excludes": []
            },
            "php": {
                "@disable": false,
                "args": [],
                "excludes": []
            }
        },
        "mark_style": "outline",
        "no_column_highlights_line": false,
        "passive_warnings": false,
        "paths": {
            "linux": [],
            "osx": [
                "/usr/local/bin/node"  //node的路径
            ],
            "windows": []
        },
        "python_paths": {
            "linux": [],
            "osx": [],
            "windows": []
        },
        "rc_search_limit": 3,
        "shell_timeout": 10,
        "show_errors_on_save": false,
        "show_marks_in_minimap": true,
        "syntax_map": {
            "html (django)": "html",
            "html (rails)": "html",
            "html 5": "html",
            "javascript (babel)": "javascript",
            "magicpython": "python",
            "php": "html",
            "python django": "python",
            "pythonimproved": "python"
        },
        "warning_color": "DDB700",
        "wrap_find": true
    }
}
```
4、在项目用配置.eslintrc文件，来配置对应的规则，且用npm安装babel-eslint及对应的eslint规则的npm包
