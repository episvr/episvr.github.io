---
title: 自表达的代码（命名篇）
date: 2025-04-09
categories: 
  - Learning
tags:
  - random_note
  - coding
description: 课上不会教但是感觉必须要会的又一座大山。众所周知：组织得好的代码可以省去大量写文档注释的时间，看着赏心悦目，而且便于调 Bug。那么怎么写出这种并非“非人类”的代码呢？
---
你一定读过很多糟糕的代码，包括但不限于：
- 鼠标滚轮测试程序（方法太长）
- 脑筋急转弯程序（逻辑判定复杂）
- 记忆力测验程序（一大堆常量）
- 眼神测验程序（类似的变量名）
- 逻辑思维测验程序（循环引用式的包含关系）
读这些代码是痛苦的（指不停的翻代码，画流程图），写这些代码也是痛苦的（写着写着就想推翻重写）。
那么怎么有效降低这种痛苦呢？写出一目了然，简洁明了，不言自明的代码就好了，这就是这篇文章介绍的 **自表达代码**

## WHY
写注释写文档自然是一种不错的解决办法，但是最近跑论文代码的 Epi 发现了一个事实：跟着 README 一步一步配置环境有 95% 的可能性会在某一布因为莫名其妙的错误崩掉。

> 注释，文档，README都会骗人，只有代码不会骗人。
 
因为只有代码是最新的最有时效的，一旦代码更新后没有及时更新文档和注释，结果就是灾难的。更何况文档可没有debug功能，读的时候甚至会觉得很对。

所以说：文档也要写，代码也要干净，这样才叫健全。

## WHAT
想一想：如果要建立一个班级 `Grade` 类，那么获取学生数量的方法怎么命名才算合适呢？
```cpp
getStudentsCount();
countStudents();
students.size();
size();
studentsCount();
```

看起来好像都不错，不过仔细分析的话——
```cpp
grade.getStudentsCount(); // get 是多余的
grade.countStudents();    // 感觉是动态计数
grade.students.size();    // 还行？
grade.size();             // 歧义
grade.studentsCount();    // 最佳！
```

对比之下`studentCount()`的命名最直观易懂。

阅读下面的两段代码：

```cpp
switch (keyCode) {
    case KEYCODE_SPACE:
        // 处理空格键
        break;
    case KEYCODE_LEFT:
        // 处理左箭头键
        break;
    case KEYCODE_RIGHT:
        // 处理右箭头键
        break;
    case KEYCODE_UP:
        // 处理上箭头键
        break;
    case KEYCODE_DOWN:
        // 处理下箭头键
        break;
    case KEYCODE_BUTTON1:
        // 处理按钮 1
        break;
    case KEYCODE_BUTTON2:
        // 处理按钮 2
        break;
    case KEYCODE_BUTTON3:
        // 处理按钮 3
        break;
    case KEYCODE_BUTTON4:
        // 处理按钮 4
        break;
    default:
        // 处理其他按键
        break;
}
```

```cpp
bool onKeyDown(int keyCode, const KeyEvent& event) {
    if (event.isPrintingKey()) {
        return appendInput(keyCode);
    }
    else if (isArrowKey(keyCode)) {
        return moveCursor(keyCode);
    }
    else if (isChannelKey(keyCode)) {
        return appendInput(convertChannelToNumber(keyCode));
    }
    else if (isFunctionalKey(keyCode)) {
        return processFunctionalKey(keyCode);
    }
    return false;
}
```

虽然第一种代码有很完整的注释，但是似乎还是第二种代码更好读一点，这就是自表达代码的优秀之处。

## HOW
首先能做到的就是命名的改进了。
### 命名的基础原则：
- 不仅要考虑声明和定义的情况，还要考虑代码被调用的情况
- 尽量使用合适易懂无歧义的英文名命名，辅以数字和下划线
	- 比如`EISU_KANA`(英数かな)就不如`ALPHABET_NUMBER_KANA`
	- 比如`fasten`就不如`accelerate`，`reSearch`就不如`searchAgain`
- 虽然有代码补全，但是名称长度不要过长
- 不要采用无意义的变量名（循环变量除外）
	- 无意义的变量名： `a,b,c,tmp,retVal` 等
- 单词间要有合适的分割
	- 如`fireWall`,`created_person`
- 除常量外，大小写混合拼写
	- 即使是 FTP 这样的缩写也要写成 `Ftp`
- 命名和行为要一致

### 接口的命名原则：
- 不要用`I`开头
- 以`able`结尾表示具备某种能力，但并不是所有的接口都能以`able`结尾。

### 类的命名
- 基类不用`Base`开头或结尾，这并没有意义，使用更有意义的`Model, View, Control`等
- 子类的命名最好与父类建立起某种推测关系
- 抽象类可以增加`Abstract`开头，但是如果类本身就够抽象就不用加
- 兄弟类要有明显的区别词语

### 方法的命名
- 动名型：动词是方法的灵魂要选的合适，把条件移到参数中会让代码更具有扩展性。
	- 比如`filterStudents(subject)` 就比 `listMathStudents(),listGeoStudents()`要合适。
- 与返回值的联系：
	- 返回值是列表就尽量用复数名词
	- 返回值是真假就用`is，has`等词开头
	- 如果返回值不确定，就用`Object`或者其父类名称
	- 如果是Getter，就使用get的对象作为名词
- 考虑被调用时的样子
- 执行类动词的使用：`do,process,execute,run`等名词很抽象，最好选用其他的词来代替。除非这是一个抽象类的实现方法，之后会被子类实现。
	- 比如`doLogin`就是`authenticate`，`doRegister`就是`createUser`
- 适当使用介词会提高可读性
- 不要加下划线
- 不要夹带无意义的临时字符
	- 比如不要因为域变量名称为`mAbortKey`就把方法命名成`getMAbortKey`

### 变量的命名
- 域变量：可以适当加前缀但是不要无脑加
	- 比如`pos.x, pos.y`很清楚，写成`pos.mX, pos.mY`就有点如鲠在喉的感觉。
- 参数变量：避免与域变量相同造成混淆
- 循环变量：下标循环用`ijk`，`forEach`循环采用元素内容命名。
- 局部变量：不要使用`temp`,`tmp`,`result`,`list`,`map`等意义不明的词，采用其真实意思命名。

### 常量的命名
- 全大写命名，多单词采用下划线分割
- 不要序号式命名，要利用开头词进行分组
	- 比如`LAYER_ROMAJI, LAYER_HIRAGANA,LAYER_KANJI`显然比`LAYER_0, LAYER_1, LAYER_2`更好

### 相似的命名
- 类和内容的重复：包装的容器和包装的内容重名，寻找合适的集合名词。
	- 比如 `Questionaire`比 `Questions` 更好
- 整体和部分：只有量的差异导致的重名，可以通过前缀`matched,grouped`区分。
- 类型转换：可以通过连续的调用略去中间变量，如`list.ketSet().iterator()`
---

ok暂时就说到这，这个话题还有不少东西可以说，比如注释的风格，代码风格，代码结构，设计模式，测试代码，甚至英语能力。之后可能会继续更新（挖坑）~


---
> 先生、人生相談です。

今天放的歌是这个：
<div class="lyrics-container">
    <div class="song-info">
        <p class="song-title">
            <svg class="w-6 h-6 text-gray-800 dark:text-white" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" width="24"
                 height="24" fill="currentColor" viewBox="0 0 24 24">
                <path
                    d="M18.6216 3.21667c.2391.1897.3784.47817.3784.78334V15.6667c0 .0412-.0025.0818-.0073.1218.0048.0698.0073.1404.0073.2115 0 1.6569-1.3431 3-3 3s-3-1.3431-3-3 1.3431-3 3-3c.3506 0 .6872.0602 1 .1707V9.2602l-8 1.8667V18l-.00001.0032C8.99824 19.6586 7.65577 21 6 21c-1.65685 0-3-1.3431-3-3s1.34315-3 3-3c.35064 0 .68722.0602 1 .1707V6.33334c0-.46474.32018-.86823.77277-.97384l9.99953-2.33321c.1486-.03477.3012-.03465.4467-.00201.1427.03202.2783.09532.3964.18752.0021.00162.0041.00324.0062.00487Z"/>
            </svg>
            ヒッチコック
        </p>
    </div>
    <div class="lyrics-display">
        <div class="lyric-item">
            <div class="original-lyric">「胸が痛んでも嘘がつけるのは何でなんでしょうか。</div>
            <div class="translated-lyric">「即使心中很痛也要撒谎是为什么呢。</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">悪い人ばかりが得をしてるのは何でなんでしょうか。</div>
            <div class="translated-lyric">总是坏人得到好处是为什么呢。</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">幸せの文字が￥を含むのは何でなんでしょうか。</div>
            <div class="translated-lyric">幸福的文字中包含￥是为什么呢。</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">一つ線を抜けば辛さになるのはわざとなんでしょうか。」</div>
            <div class="translated-lyric">去掉一条线就会变成辛苦是故意的吗。」</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">青春って値札が背中に貼られていて</div>
            <div class="translated-lyric">后背被贴上了名为青春的标签</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">ヒッチコックみたいなサスペンスをどこか期待していた</div>
            <div class="translated-lyric">内心某处期待着希区柯克般的悬疑</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">「先生、どうでもいいんですよ。</div>
            <div class="translated-lyric">「老师，怎样都可以了啊。</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">生きてるだけで痛いんですよ。</div>
            <div class="translated-lyric">只是活着就很痛苦了啊。</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">ニーチェもフロイトもこの穴の埋め方は書かないんだ。</div>
            <div class="translated-lyric">尼采和弗洛伊德都没有写填住这个洞口的方法啊。</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">ただ夏の匂いに目を瞑って、</div>
            <div class="translated-lyric">只是在夏天的味道里闭上双眼、</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">雲の高さを指で描こう。</div>
            <div class="translated-lyric">用手指描绘云的高度。</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">想い出だけが見たいのは我儘ですか。」</div>
            <div class="translated-lyric">只想看着回忆是一种任性吗。」</div>
        </div>
    </div>
</div>


------
感谢你的阅读~