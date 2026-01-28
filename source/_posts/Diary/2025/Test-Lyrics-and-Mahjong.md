---

title: This is a test
date: 2025-02-02
categories: 
  - Diary
tags: 
  - random_note
description: 歌词样式的测试，顺便塞了记了记我老家的麻将规则
mermaid: true

---

萌生了“加一个末尾歌词的美化”的想法，写了一个上午差不多调好了，本篇文章是一篇测试文章。

但是还是写一些东西比较好吧，在我老家，麻将的规则比较复杂，记录一下：

首先是通用的基础规则：

1. **基础胡牌规则**：四面子加一雀头，符合这些条件的牌型才算胡牌，注意并不包括十三幺和七对这类特殊胡法。
2. **不可缺门**：即必须同时拥有条、万、筒这三种花色的牌，不能存在清一色、混一色等胡法。
3. **不可缺幺九**：幺九指的是牌面上的一、九，以及所有字牌（这一点与日麻中的“断幺九”正好相反）。
4. **不可缺“大茬”**：这里的“大茬”指的是至少有一组任意刻子（碰/杠）或是中发白的雀头。

接下来说说**听牌规则**：

1. **暗听与打宝**：在听牌状态下，你可以在自己的摸牌阶段选择是否**打宝**，若打，则除了普通的摸牌外，还需要掷骰子额外抽取一张牌作为**宝牌**，并且进入听宝状态，此时只能模切或胡牌。这与日麻的立直规则有些相似，但更加具有游戏的独特性。若不打宝，可维持暗听状态。
2. **宝牌的作用**：听宝状态下，摸到宝牌即可视为胡牌。
3. **宝牌的数量**：若当前持有的宝牌已全部出现在牌河里，则可以选择重新打宝，否则无法更换宝牌。这一点引申出了卡宝的防守策略
4. **特殊宝牌规则**：如果在打宝时抽到了幺鸡宝牌，那很幸运了，直接胡牌。
5. 如果宝牌刚好是你听的牌：那你很幸运了，直接胡牌

状态转移图如下，还是状态转移图方便啊（

{% mermaid stateDiagram %}
    [*] --> 听牌
    暗听 --> 听牌
    听牌 --> 暗听 : 不打宝
    暗听 --> 胡牌 : 正常胡
    听牌 --> 听宝 : 打宝
    听宝 --> 抽宝牌 : 摸宝
    抽宝牌 --> 幺鸡胡 : 幺鸡
    抽宝牌 --> 正常摸牌 : 未胡
    正常摸牌 --> 宝胡 : 摸到宝牌
    抽宝牌 --> 宝胡 : 直接胡
    正常摸牌 --> 胡牌 : 正常胡
    胡牌 --> 结算 : 正常胡得分
    宝胡 --> 结算 : 宝胡得分
    幺鸡胡 --> 结算 : 幺鸡胡得分
{% endmermaid %}



**特殊规则**：

1. **加番规则**：在胡牌的基础上，若满足特定条件（如一杯口、四带一、海底捞、岭上、单吊等），则可以加番。
2. **杠牌加番**：所有杠牌都能加一番，若是暗杠则可以加两番，这与日麻中的杠牌加分类似，但更为直接。
3. **破门清**：若其他三家均未开门（也就是破门清），而你在此情况下胡牌，则计满番。



好，那么好，测试开始：

Test 01 English

<div class="lyrics-container">
    <div class="song-info">
        <p class="song-title"><svg class="w-6 h-6 text-gray-800 dark:text-white" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" width="24"
                                   height="24" fill="currentColor" viewBox="0 0 24 24">
            <path
                  d="M18.6216 3.21667c.2391.1897.3784.47817.3784.78334V15.6667c0 .0412-.0025.0818-.0073.1218.0048.0698.0073.1404.0073.2115 0 1.6569-1.3431 3-3 3s-3-1.3431-3-3 1.3431-3 3-3c.3506 0 .6872.0602 1 .1707V9.2602l-8 1.8667V18l-.00001.0032C8.99824 19.6586 7.65577 21 6 21c-1.65685 0-3-1.3431-3-3s1.34315-3 3-3c.35064 0 .68722.0602 1 .1707V6.33334c0-.46474.32018-.86823.77277-.97384l9.99953-2.33321c.1486-.03477.3012-.03465.4467-.00201.1427.03202.2783.09532.3964.18752.0021.00162.0041.00324.0062.00487Z" />
            </svg> As the World caves in</p>
    </div>
    <div class="lyrics-display">
        <div class="lyric-item">
            <div class="original-lyric">And here it is, our final night alive</div>
            <div class="translated-lyric">就是此刻了 最后一个存活之夜</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">As the earth burns to the ground</div>
            <div class="translated-lyric">地球就这样燃烧殆尽</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">Oh boy it's you that I lie with</div>
            <div class="translated-lyric">男孩，你是我选择依存之人</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">As the atom bomb locks in</div>
            <div class="translated-lyric">末日来袭 爆炸来袭</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">Oh, boy, it's you I watch TV with</div>
            <div class="translated-lyric">男孩 是你还在陪我看电视</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">As the world, as the world caves in</div>
            <div class="translated-lyric">直到世界，在这世界崩塌之际</div>
        </div>
    </div>
</div>


Test 02 Japanese




<div class="lyrics-container">
    <div class="song-info">
        <p class="song-title"><svg class="w-6 h-6 text-gray-800 dark:text-white" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" width="24"
                                   height="24" fill="currentColor" viewBox="0 0 24 24">
            <path
                  d="M18.6216 3.21667c.2391.1897.3784.47817.3784.78334V15.6667c0 .0412-.0025.0818-.0073.1218.0048.0698.0073.1404.0073.2115 0 1.6569-1.3431 3-3 3s-3-1.3431-3-3 1.3431-3 3-3c.3506 0 .6872.0602 1 .1707V9.2602l-8 1.8667V18l-.00001.0032C8.99824 19.6586 7.65577 21 6 21c-1.65685 0-3-1.3431-3-3s1.34315-3 3-3c.35064 0 .68722.0602 1 .1707V6.33334c0-.46474.32018-.86823.77277-.97384l9.99953-2.33321c.1486-.03477.3012-.03465.4467-.00201.1427.03202.2783.09532.3964.18752.0021.00162.0041.00324.0062.00487Z" />
            </svg> 夏令时记录 </p>
    </div>
    <div class="lyrics-display">
        <div class="lyric-item">
            <div class="original-lyric">秘密基地に集まって</div>
            <div class="translated-lyric">在秘密基地中集合</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">笑い合った夏の日に</div>
            <div class="translated-lyric">一齐笑着的夏日</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">「また何処かで思い出して</div>
            <div class="translated-lyric">「能再次在某处回忆起来</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">出逢えるかな」って</div>
            <div class="translated-lyric">再度相遇吗」</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">何度でも描こう</div>
            <div class="translated-lyric">无数次地在心底描绘着</div>
        </div>
    </div>
</div>


Test 03 Special Bold


<div class="lyrics-container">
    <div class="song-info">
        <p class="song-title"><svg class="w-6 h-6 text-gray-800 dark:text-white" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" width="24"
                                   height="24" fill="currentColor" viewBox="0 0 24 24">
            <path
                  d="M18.6216 3.21667c.2391.1897.3784.47817.3784.78334V15.6667c0 .0412-.0025.0818-.0073.1218.0048.0698.0073.1404.0073.2115 0 1.6569-1.3431 3-3 3s-3-1.3431-3-3 1.3431-3 3-3c.3506 0 .6872.0602 1 .1707V9.2602l-8 1.8667V18l-.00001.0032C8.99824 19.6586 7.65577 21 6 21c-1.65685 0-3-1.3431-3-3s1.34315-3 3-3c.35064 0 .68722.0602 1 .1707V6.33334c0-.46474.32018-.86823.77277-.97384l9.99953-2.33321c.1486-.03477.3012-.03465.4467-.00201.1427.03202.2783.09532.3964.18752.0021.00162.0041.00324.0062.00487Z" />
            </svg> 失想话语 </p>
    </div>
    <div class="lyrics-display">
        <div class="lyric-item">
            <div class="original-lyric">たどり着いたのは「未来」で</div>
            <div class="translated-lyric">在终于抵达的“<b>未来</b>”中</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">そう、色めくような世界で</div>
            <div class="translated-lyric">没错，在<b>色彩斑斓的世界</b>中</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">大人になっていく私は</div>
            <div class="translated-lyric">逐渐长成<b>大人</b>的我</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">変わり続けていく</div>
            <div class="translated-lyric">即使不断改变着</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">変わらない想いを 大事に　抱いていく</div>
            <div class="translated-lyric">也有<b>不变的情意 始终紧抱在心中</b></div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">不思議なほどに　この世界は</div>
            <div class="translated-lyric">奇怪的是 在这个世界上</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">「思い出す」のが　難しくて</div>
            <div class="translated-lyric">想要去“<b>回忆</b>”总是很难</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">忘れたくない　言葉を</div>
            <div class="translated-lyric">为了不失去那些 <b>不愿忘记的话</b></div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">失くさないように　伝えて行く</div>
            <div class="translated-lyric">就将它们都传达出去吧</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">いつか誰かと　この世界で</div>
            <div class="translated-lyric">若有一天能<b>与谁</b>在这个世界上</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">笑い合えたら　ちょうど良いなぁ</div>
            <div class="translated-lyric"><b>笑着相遇的话该有多好啊</b></div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">そんなことを　考える</div>
            <div class="translated-lyric">想一想这样的事情</div>
        </div>
        <div class="lyric-item">
            <div class="original-lyric">未来に　理由が見つかりそう</div>
            <div class="translated-lyric">好像就能找到<b>前往未来的理由</b>了</div>
        </div>
    </div>
</div>

就这样！感谢你的阅读~