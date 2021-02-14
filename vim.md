

    zr: reduces fold level throughout the buffer
    zR: opens all folds
    zm: increases fold level throughout the buffer
    zM: folds everything all the way
    za: open a fold your cursor is on
    zA: open a fold your cursor is on recursively
    zc: close a fold your cursor is on
    zC: close a fold your cursor is on recursively

    :help vim-markdwon
[[ "跳转上一个标题
]] "跳转下一个标题
]c "跳转到当前标题
]u "跳转到副标题
zr "打开下一级折叠
zR "打开所有折叠
zm "折叠当前段落
zM "折叠所有段落
:Toc "显示目录

由于我们还会使用到Latex数学公式，所以可以在配置文件中加上

let g:vim_markdown_math = 1
