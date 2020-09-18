---
title: MusicString 
tags: JFugue
renderNumberedHeading: true
grammar_cjkRuby: true
---

## 简介
由音符、八度、音长、音色（乐器，默认乐器为钢琴）组成
和弦、连音、速冻、控制器、键签名

Jfugue 可以简单并且允许工程师去快速创建音乐的原因是 MusicString，一个特殊格式描述音乐的字符串对象。

例如，播放 C（哆） 音符，可以使用如下简单的程序

```java
Player player = new Player(); 
player.play("C");
```

JFugue 解析 MusicString 并且创建对象标识每一个音符、乐器等。这些对象将用来生成音乐，并且通过扬声器播放。JFugue MusicString 不区分大小写。接下来的例子将会看到大小一致的样式，虽然 JFugue 不需要解析这种特定样式即可正确解析 MusicString，但尽可能让 MusicString 可读。


## 学习部分 MusicString

例子
```java
 Player player = new Player(); 
 player.play("C"); 
 player.play("C7h"); 
 player.play("C5maj7w"); 
 player.play("G5h+B5h+C6q_D6q"); 
 player.play("G5q G5q F5q E5q D5h"); 
 player.play("T[Allegro] V0 I0 G6q A5q V1 A5q G6q"); 
 player.play("V0 Cmajw V1 I[Flute] G4q E4q C4q E4q"); 
 player.play("T120 V0 I[Piano] G5q G5q V9 [Hand_Clap]q Rq");
```

每个通过空格分来的一个或多个字符，被称为一个 Token，一个token 代表一个音符、和弦或停顿；一个一起改变；一个声音或者层的改变；一个速度指示；一个控制事件；一个不变的定义；或者其他，更多的详情在这个章节展开。上面的例子第一个，前四个 MusicString 每个包含一个 Token，并且后面的 MUsicString 每个包含 8 个Token。

## 音符、停顿、和弦

音符和停顿的说明以音符名称或者停顿字符开始，分别是如下
 `C`(哆), `D`(来), `E`(咪), `F`(发), `G`(索), `A`(拉), `B`(西), or `R`(停顿)。除了音符说明本身，还可以追加一些 尖、平、八度、音长、和弦等等。

 一个音符也可以用数字表示，这个可用于创建“算法”音乐，每个音符都可以使用数组只而不是字母。一个数字音符将提供描述一个音符 MIDI 值，在方括号中，例如 `[60]`。八度已经作为因复制的因数，因此在提供音符值的时候不必指定八度（也不可能）。数值不能超过 127

Octave | C | C#/Db | D | D#/Eb | E | F | F#/Gb | G | G#/Ab | A | A#/Bb | B | 
|-|-|-|-|-|-|-|-|-|-|-|-|-|
0 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 
1 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 
2 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 | 
3 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 
4 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 
5 | 60 | 61 | 62 | 63 | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 70 | 
6 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 
7 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 
8 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 
9 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 
10 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127

## 尖、平、自然

可以使用 `#` 字符表示尖音，用 `b` 字符标识平音，`#`或`b`紧跟字符即可，例如 一个 `B-平` 可以表示为 `Bb`,JFugue 也支持 `双尖` 或者 `双平` 分别使用 `##` 和 `bb` 表示。

如果使用 键值标识（Key Signatures），你可以表示一个自然的音符通过在音符后使用 `n`，例如 `B-自然` 可以表示为 `Bn`。如果没有指明 `B` 为一个自然的，JFugue将自动改变音符的只基于键值标识（如果键值为 `F-major`，那么 `B` 将自动被覆盖通过 `B-平`）。后续将对 键值标识丛详细阐述。 

## 八度
默认 音符 5，和弦 3
允许给音符指定八度，使用 0-10 之间的数字表示。例如 `C6` 播放一个 `C` 音节在第六八度。如果没有八度说明，默认一个音节为第五八度，并且默认一个和弦第三八度。 

![enter description here](./images/1600326636988.png)


## 和弦
表示和弦需要先指定和弦的跟，JFugue支持多种和弦，下标中有详细描述

表格中的间隔表示和弦音符，例如，一个大和弦包含三个音符，用 `0,4,7`表示，这里定义的和弦是由跟（0），跟家四个半步（4）和跟加七个半步（7）组成。然而一个 `C-majo` 和弦是有音符 `C,E,G` 组成。

![enter description here](./images/WeChatfa1786a04f650e117a0c60921a770aed.png)

在 MusicString 中指明和弦，提供一个跟音符紧跟着通过上表中“JFugue Name”。例如 播放一个 `C-major` 和弦在默认八度中，可以使用 `C-major` MusicString，这个等效于 C+E+C，JFugue 会根据指定的和弦去填充对应的音符。回想一下，和弦的默认八度是第三个八度，它低于单个音符的默认五度。

若要指定带有和弦的八度，请在和弦根后面跟随八度数。例如，E-flat,6<sup>th</sup>，major 和弦 可以使用 `Eb6maj`。记住八度音阶放置位置的一种简单方法是，八度音阶更详细地描述了根音，因此它应该在根音旁。如果和弦名称后面有一个数字，则该数字与和弦本身相关联：例如，`Cmaj7` 描述的是C大调第七和弦，而不是七八度音阶的C大和弦。

## 和弦反转

和弦反转是演奏和弦音符的另一种方法，通过改变和弦中的那个音作为根音。有时候将其称为“和弦发声”。

第一次倒置表示和弦的常规根音应上移八度，使和弦中的第二个音符成为新的低音音符。第二个倒置表示和弦的根音和第二个音符应高八度，使和弦的第三个音符成为新的贝斯音符。具有三个以上成员的和弦可以进行第三次反转，具有四个以上成员的和弦可以进行四个反转，依此类推。和弦反转的例子如下图

![![enter description here](https://markdown.xiaoshujiang.com/img/spinner.gif "[[[1600328891276]]]" )](./images/1600328899345.png)


和弦反转也可以明确的指出将要成为新低音音符的音符来描述。可能在分页乐谱中看到 C/E 和弦。 C-Major 表示用`E`音符作为基础音符。

有两种方法可以再 JFugue 中指定和弦反转。第一个与指示和弦的第一，第二，第三等倒置一致。如上街所述，指出和弦（例如 `Cmaj` 为 CMajor）然后再每个反转音符后加入 ^。如上图所示，第一次反转变成`Cmaj^`，第二次反转变成`Cmaj^^`。带有更多音符的和弦可能会进行其他反转。

第二种方法与指示和弦的新低音音符一致，请按照上一节中的指示说出和弦（对于C-Major，为`Cmaj`），然后使用尖号字符^，后跟新的低音音符。例如将 `E` 作为新的低音音符的 C-Major 反转将为 `Cmaj^E`，将 `G` 作为新的低音音符的 C-Major 反转将为 `Cmaj^G`

## 音长
默认为 四分（`q`）

音长表示音符将播放多长时间，它放置在八度音阶之后（如果指定了和弦，则放置在和弦之后），或紧接在音符本身之后（如果省略了八度）或将音符指定为值。音长可由下面字母之一表示，如果未指定工期，将使用默认的四分。

Duration | Character
|-|-|
whole | w
half | h
quarter | q
eighth | i
sixteenth | s
thirty-second | t
sixty-fourth | x
one-twenty-eighth | o

例如，一个 `C6` 音符，半分音长即为 `C6h`。一个 D-flat major 音符， 全音长即为 `DbmajW`.

点线的音长可以使用音长后使用`.`字符来指定。带点的半音使用 `h` 后面加英文句号`h.`来表示。一个点表示原始持续时间的 1.5 倍。半分加线等于四分之半分加四分。

持续时间可以彼此附加以创建较长持续时间的音符。这类似音乐中的一个 “tie”，例如播放 `D6` 三个节拍，可以使用 `D6www`。

持续时间也可以通过数字指定。这种情况下，提供等于整个音符部分的十进制。要表示数字持续时间，请使用斜杠字符，后跟一个十进制值。例如播放 A4 音符四分音长，可以使用 `A4/0.25`。1.0 表示整个时间，小数位数大于 1.0 表示音符跨多个小节。例如 上面给出的例子 `D6www` 等价于 `D6/3.0`。使用数字作为音长可能对”算法“ 音乐有用处。它们也会在JFugue解析MIDI文件时生成MusicStrings时创建。下面是一些关于音长的例子。
```java
 player.play("Aw"); // A5 whole note 
 player.play("E7h"); // E7 half note
 player.play("[60]wq"); // Middle-C (C5) whole+quarter note
 player.play("G8i."); // G8 dotted-eighth note
 player.play("Bb6/0.5"); // B-flat, 6th octave, half note
// C-major chord, second inversion, 7th octave, quarter note 
 player.play("C7maj^^q");
```

## 三连音和其他连音
连音是一组音符，音符的音长会被调节，使得该组音符的持续时间与下一个最长的音符音长一致，如下图![enter description here](./images/1600331983785.png)

三连音是连音一种特殊形式，该组有是哪个音符。三连音是最常见的连音符，其他连音符也可能存在（在音乐理论和 JFugue中）。三连音，三个音符以接下来最大的音长相同的方式演奏。这事一个 3:2 三连音。一个三连音组成四分音符，正如上图所属，这意味着该组音符将在一个半音符中播放，所以每个音符将在正常音长的 2/3 中播放。

例如更多音符 - ，一个五重奏，包括五个音符，如果他们是一个 5:4 连音，那么五个四分音符将播放在同样的音长中作为整个音符 每个音符将播放正常四分音长的 4/5。

在JFugue中指定连音符，使用型号 `*`，在连音符的音符持续时间符号后。三连音，这样做就可以了。其他连音，星号后面必须紧跟描述连音的比例，例如 `5:4` 。每个连音在音符中都必须有连音符号，并且连音符的每个比例都相同（如果不是不会影响解析，只是音乐挺起来回变得很怪）。

例子，这些行中的每行将以三重奏的形式演奏三个四分音符，四分之三的组的持续时间为四分之三（等于二分音符）
```java
player.play("Eq* Fq* Gq*"); // These two lines create
player.play("Eq*3:2 Fq*3:2 Gq*3:2"); // equivalent music
```
 这五个八分音符（五重奏）将在四个八分音符（相同的一半音符）的持续时间内演奏。
 ```java
 player.play("Ci*5:4 Ei*5:4 Gi*5:4 Ei*5:4 Gi:5*4");
 ```
 
 ## 小节（tie）
 
 在乐谱中，tie 链接两个相同音高的音符。并只是将两个音符作为一个音符演奏，总持续时间等于并列音符的持续时间之和。乐谱中经常使用 tie 来描绘音符，该音符的持续时间跨越两个小节之间的小节线，如下图。tie通产被用来创建音符的持续音长，所以不能用注释代替掉，例如半音符加八分音符。
 ![enter description here](https://raw.githubusercontent.com/mousezheng/source-code-analysis/master/img/2020_9_17_MusicString_WeChated2001ff948f2619d2954d867a8a9bbb.png)
 
 在 JFugue 破折号 `-` 备用来表示 tie。对于一个 tie 开始的音符，追加破折号到其音长的结尾。对于在 tie 结尾的音符，将破折号放在持续时间的开头，如果一个音符在一系列连在一起的音符中间，使用两个破折号，持续时间前后都加。使用破折号表示 tie 是否 ”跟随“ 音符的音长，是否”继续“音符的音长， 节点是否在 tie 中，这种情况下，tie 即”跟随“又”继续“持续时间。它将使用”测量“符号（垂直线或者竖线字符 ”|“），如下图
 ![enter description here](./images/WeChatccda1e33b05d536a32ac6c5a7434b3ea.png)
 
 ## Attack(起) and Decay(落) 速度
 默认 64 attack, 64 decay
 
 音符可以指定起音及落音速度，这些速度表明音符“预热”到其全部音量需要多长时间，并且从峰值”消散“，例如 一个音符需要一个很长的起音和一个很快的落音，就像建立的时候需要一段时间，很快关闭。长时间发作的音符听起来有些空灵，具有长衰减声的音符像被敲击的铃铛或吉他弦一样，在被敲击后会继续产生共鸣。
 
 音符的起音和落音可以分别使用字母`a`和`d`来指定，每个字母后面紧跟数字 0-127，默认为 0。较低的值表示起音或者落音的更快，较高的值表示其更长。起落音可单独使用（如果他们一块出现，需要先指定起音）
 
 例如，下面这些带有起落速度的音符
 ```java
player.play("C5qa0d127"); // Sharp attack, long decay
player.play("E3wwd0"); // Default attack, sharp decay
player.play("C7maja30"); // C7, E7, and G7 (components of 
// C7maj) will all play with an 
// of attack 30
 ```
 ## 音符演奏旋律及和声
 这事演奏旋律的音符 -，一个接一个–用空格分隔的单个标记表示。至此，所有MusicStrings示例都显示了旋律演奏的音符。
 ![enter description here](https://raw.githubusercontent.com/mousezheng/source-code-analysis/master/img/2020_9_17_MusicString_WeChata5b7d1dc68950c9c83aede8f92854119.png)如下图所示，
 
 音符也可以与其他音符一起和谐演奏，这可以通过将标记与加号`+`结合使用来指明而不是空格，如下图所示。当然，和弦中的音符自动和声演奏，`+`标记可让您和声演奏任何音符。
 ![enter description here](https://raw.githubusercontent.com/mousezheng/source-code-analysis/master/img/2020_9_17_MusicString_WeChatb6a1f29f7d1dc5d4ff4a6af84e312dd3.png)
 
还会发现在某些情况下要和声地演奏一个音符而两个或多个音符会以旋律演奏。表示在与其他音符和声演奏时应一起演奏的音符，使用下划线字符`_`连接应一起演奏的音符。`C5`音符会连续播放，而`E5`和`G5`音符会依次播放。
![enter description here](./images/WeChate977c301d03089a76121381c69330283.png)

和弦和休息音也可以和声或组合演奏使用加号和下划线字符作为连接符的和声/旋律，只有音符，和弦和休止符可以使用`+`和`_`字符。

## 计量
JFugue MusicStrings的创建旨在使音乐创作变得容易，它们不是为提供代表乐谱的完整语法而开发的。在MusicString中指示小节线`|`不会影响MusicString的音乐输出。但是，在MusicString中指示小节之间的中断通常很有用。要指示条形线，请使用垂直线（或竖线）字符`|`，该字符必须与MusicString中的其他标记分隔并带有空格。

## 键值标识（Key Signature）
默认为: C-major

一个键值标识被用来指明JFugue以特定的键或音阶播放MusicString。指明键值标识，使用字母`K`，后面紧跟键值的根，然后是 `maj` 或 `min` 主要或次要规模。例如 `KCbmaj` 将被记做键至为 C-flat major.

JFugue 将自动为受键值标识影响的音符调整音符值。例如，如果设置键值为 `F-major`，那么播放一个 B 音符在 MusicString 中，JFugure 将自动替换 `B` 为` B-flat`。如果想 `B` 保持自然形式，必须在每个音符后通过使用自然符号标识`n`，这种情况，演奏 `B` 作为自然音符需要令牌 `n`。

## 仪器
默认为  Piano（钢琴）

JFugue 产生的音乐使用 MIDI 来渲染由 Java Sound 音库中的乐器播放的音频，MIDI 规范描述了128种不同的乐器并且可能支持更多。大多数 MIDI 设备的前128个乐器使用相同的定义，尽管声音的质量因设备和音库而异。例如，MIDI 乐器`#0`通常代表一架钢琴，但是各种 MIDI 设备渲染的钢琴声音可能不同。

在 JFugue 的 MusicString 中选择这些乐器，使用工具令牌，它是`I`字符，后跟0到127之间的工具编号。例如要指定钢琴，可以输入 MusicString `I0`。或者 JFugue 使用仪器名称定义了可用于指定仪器的常量。这往往更容易阅读和记住，例如 钢琴常熟为 PIANO，因此用于指定钢琴的 MusicString 也可能显示为`I[Piano]`。也可以定义自己的常量，本章后面将更详细地描述常量。如下表包含仪器编号及 JFugue 常数的列表。某些乐器可能包含多个常熟，可以使用任何一个常数，它们都将解析为相同的仪器编号。一下常量也可以不区分大小写

|id|name|
|-|-|
Piano |钢琴
0 | PIANO or ACOUSTIC_GRAND 
1 | BRIGHT_ACOUSTIC 
2 | ELECTRIC_GRAND 
3 | HONKEY_TONK 
4 | ELECTRIC_PIANO or ELECTRIC_PIANO1 
5 | ELECTRIC_PIANO2 
6 | HARPISCHORD 
7 | CLAVINET 
Chromatic Percussion |打击乐器
8 | CELESTA 
9 | GLOCKENSPIEL 
10 | MUSIC_BOX 
11 | VIBRAPHONE 
12 | MARIMBA 
13 | XYLOPHONE 
14 | TUBULAR_BELLS 
15 | DULCIMER 
Organ |琴
16 | DRAWBAR_ORGAN 
17 | PERCUSSIVE_ORGAN 
18 | ROCK_ORGAN 
19 | CHURCH_ORGAN 
20 | REED_ORGAN 
21 | ACCORIDAN 
22 | HARMONICA 
23 | TANGO_ACCORDIAN 
Guitar |吉他
24 | GUITAR or NYLON_STRING_GUITAR 
25 | STEEL_STRING_GUITAR 
26 | ELECTRIC_JAZZ_GUITAR 
27 | ELECTRIC_CLEAN_GUITAR 
28 | ELECTRIC_MUTED_GUITAR
29 | OVERDRIVEN_GUITAR 
30 | DISTORTION_GUITAR 
31 | GUITAR_HARMONICS 
Bass | 贝斯
32 | ACOUSTIC_BASS 
33 | ELECTRIC_BASS_FINGER
34 | ELECTRIC_BASS_PICK 
35 | FRETLESS_BASS 
36 | SLAP_BASS_1 
37 | SLAP_BASS_2 
38 | SYNTH_BASS_1 
39 | SYNTH_BASS_2 
Strings | 弦
40 | VIOLIN 
41 | VIOLA 
42 | CELLO 
43 | CONTRABASS 
44 | TREMOLO_STRINGS 
45 | PIZZICATO_STRINGS 
46 | ORCHESTRAL_STRINGS 
47 | TIMPANI 
Ensemble | 合奏
48 | STRING_ENSEMBLE_1 
49 | STRING_ENSEMBLE_2 
50 | SYNTH_STRINGS_1 
51 | SYNTH_STRINGS_2 
52 | CHOIR_AAHS 
53 | VOICE_OOHS 
54 | SYNTH_VOICE 
55 | ORCHESTRA_HIT 
Brass | 铜器
56 | TRUMPET 
57 | TROMBONE 
58 | TUBA 
59 | MUTED_TRUMPET 
60 | FRENCH_HORN 
61 | BRASS_SECTION 
62 | SYNTHBRASS_1 
63 | SYNTHBRASS_2 
Reed | 竹
64 | SOPRANO_SAX 
65 | ALTO_SAX 
66 | TENOR_SAX 
67 | BARITONE_SAX 
68 | OBOE 
69 | ENGLISH_HORN 
70 | BASSOON 
71 | CLARINET 
Pipe | 管
72 | PICCOLO 
73 | FLUTE 
74 | RECORDER 
75 | PAN_FLUTE 
76 | BLOWN_BOTTLE 
77 | SKAKUHACHI 
78 | WHISTLE 
79 | OCARINA 
Synth Lead |
80 | LEAD_SQUARE or SQUARE 
81 | LEAD_SAWTOOTH or SAWTOOTH 
82 | LEAD_CALLIOPE or CALLIOPE 
83 | LEAD_CHIFF or CHIFF 
84 | LEAD_CHARANG or
CHARANG |
85 | LEAD_VOICE or VOICE 
86 | LEAD_FIFTHS or FIFTHS 
87 | LEAD_BASSLEAD or BASSLEAD 
Synth Pad | 合成
88 | PAD_NEW_AGE or NEW_AGE 
89 | PAD_WARM or WARM 
90 | PAD_POLYSYNTH or
POLYSYNTH | 塑料
91 | PAD_CHOIR or CHOIR 
92 | PAD_BOWED or BOWED 
93 | PAD_METALLIC or METALLIC 
94 | PAD_HALO or HALO 
95 | PAD_SWEEP or SWEEP 
Synth Effects | 合成器
96 | FX_RAIN OR RAIN 
97 | FX_SOUNDTRACK or SOUNDTRACK 
98 | FX_CRYSTAL or CRYSTAL 
99 | FX_ATMOSPHERE or ATMOSPHERE 
100 | FX_BRIGHTNESS or BRIGHTNESS 
101 | FX_GOBLINS or GOBLINS 
102 | FX_ECHOES or ECHOES 
103 | FX_SCI-FI or SCI-FI 
Ethnic | 种族
104 | SITAR 
105 | BANJO 
106 | SHAMISEN 
107 | KOTO 
108 | KALIMBA 
109 | BAGPIPE 
110 | FIDDLE 
111 | SHANAI 
Percussive | 打击乐器
112 | TINKLE_BELL 
113 | AGOGO 
114 | STEEL_DRUMS 
115 | WOODBLOCK 
116 | TAIKO_DRUM 
117 | MELODIC_TOM 
118 | SYNTH_DRUM 
119 | REVERSE_CYMBAL 
Sound Effects | 声音特效
120 | GUITAR_FRET_NOISE 
121 | BREATH_NOISE 
122 | EASHORE 
123 | BIRD_TWEET 
124 | TELEPHONE_RING 
125 | HELICOPTER 
126 | APPLAUSE 
127 | GUNSHOT

## 音色（Voice）
音乐通常会分解成多种声音，也称为频道（ channels）或曲目（tracks）。每个声音都包含一个旋律，通常会用特定的乐器演奏。例如一个解释音乐，节能有不同的鼓声、萨克管、贝斯、钢琴等。或者在钢琴独奏中，可以为高音谱使用一种声音，为低音谱使用一种声音。

MIDI支持16个同时通道，JFugue通过音色命令来表示。音色命令是 `V`，后面紧跟数字 0-15。

MIDI 允许随意打开或者关闭任意通道来播放音乐，这样就可以专注于音乐的一部分，或听到没有特定声音的歌曲的声音。

## MIDI 打击轨道
第 10 个 MIDI 通道（即`V9`）比较特殊，它是唯一能够为非彩色打击乐器产生声音的通道，通常是鼓。在第十通道中，每个音符分配给不同的打击乐器，例如，如果第 10 通道给了 `A5` 音符，将不会演奏 A5 ，二十播放拨浪鼓的声音。

JFugue 提供了另一种在 `V9` 中指定注释的方法，可以更简单的为指定鼓声。可以使用常量来表达乐器，而不是输入 `V9 A5q` 来代表拨浪鼓。可以使用 `V9 [Hi_Bongo]q`来替代。代表打击乐声音的常量列表如下表所示：

Note Value | JFugue Constant 
|-|-|
35 | ACOUSTIC_BASE_DRUM
36 | BASS_DRUM 
37 | SIDE_KICK 
38 | ACOUSTIC_SNARE 
39 | HAND_CLAP 
40 | ELECTRIC_SNARE 
41 | LOW_FLOOR_TOM 
42 | CLOSED_HI_HAT 
43 | HIGH_FLOOR_TOM 
44 | PEDAL_HI_TOM 
45 | LOW_TOM 
46 | OPEN_HI_HAT 
47 | LOW_MID_TOM 
48 | HI_MID_TOM 
49 | CRASH_CYMBAL_1 
50 | HIGH_TOM 
51 | RIDE_CYMBAL_1 
52 | CHINESE_CYMBAL 
53 | RIDE_BELL 
54 | TAMBOURINE 
55 | SPLASH_CYMBAL 
56 | COWBELL 
57 | CRASH_CYMBAL_2 
58 | VIBRASLAP 
59 | RIDE_CYMBAL_2 
60 | HI_BONGO 
61 | LOW_BONGO 
62 | MUTE_HI_CONGA 
63 | OPEN_HI_CONGA 
64 | LOW_CONGO 
65 | HIGH_TIMBALE 
66 | LOW_TIMBALE 
67 | HIGH_AGOGO 
68 | LOW_AGOGO 
69 | CABASA 
70 | MARACAS 
71 | SHORT_WHISTLE 
72 | LONG_WHISTLE 
73 | SHORT_GUIRO 
74 | LONG_GUIRO 
75 | CLAVES 
76 | HI_WOOD_BLOCK 
77 | LOW_WOOD_BLOCK 
78 | MUTE_CUICA 
79 | OPEN_CUICA 
80 | MUTE_TRIANGLE 
81 | OPEN_TRIANGLE

创建打击乐器的”和弦“跟其他音符的规则一致，例如`V9 [Hand_Clap]q+[Crash_Cymbal_1]q` 将在四分音长中同时演奏这两种声音

## 层（Layer）

一层提供了一种方法，用于指定要以相同声音播放的单独旋律。层不属于 MIDI 规范的一部分，是 JFugue的特色。引入层是为了客服第十通道 MIDI （演奏打击乐器）的难题。特别要提醒，如果有很多旋律每个旋律都有自己的节奏，很难讲这些组合成声音的”和弦“。使用层，可以轻松的将多种旋律融合起来。

此外，层也可以用在其他声音中，可以利用它来模拟从 MIDI 系统中同时获取 16 个以上的旋律。它还可以用于同一音轨中发送多个事件，在弹奏音符时改变音高轮，产生对演奏音符的调制。

类似音色，层用 `L` 来表示，后面可以跟数字 0-15

## 速度（Tempo）
默认每分钟 120 次，大致为快板

速度是指一首音乐的播放速度。它通常是 MusicString 中设置的第一件事，因为它适用于跟随节奏命令的所有音乐事件。

节拍代表每分钟节拍（BPM），在 JFugue 的旧版本中，速度表示“每季度脉冲数”（PPQ），表示要给一个四分音符多少个时钟周期。PPQ 与 BPM 成反比，当然 PPQ 不够直观，因此，JFugue现在支持使用BPM表达速度。幸好，最常见的速度是 120 ，恰好是 PPQ 和 BPM 的等等效值，(120 PPQ = 120 BPM)。

速度的标识为 `T`，后面紧跟数字或者使用常量，例如`T[Adagio]`，下表示关于速度的常亮。
JFugue Constant | Beats Per Minute (BPM) 
|-|-|
Grave | 40 
Largo | 45 
Larghetto | 50 
Lento | 55 
Adagio | 60 
Adagietto | 65 
Andante | 70 
Andantino | 80 
Moderato | 95 
Allegretto | 110 
Allegro (default) | 120 
Vivace | 145 
Presto | 180 
Pretissimo | 220

## 变音轮（Pitch Wheel）
音调轮用于将一个点的音调改变百分之一半或几分，变音轮可用于更改单个音符的向下或向上方向的频率 8192 分

音高转盘可用于在音乐中创建类似 Theremin 的效果。JFugue 还使用变音轮对音符进行正弦调整，使某些东方风格的音乐易于播放。

用于调节变音轮的命令是 `&` 后面紧跟数字 0-16383，0-8191 会使音调降低，8193-16383 使音调变高。重置变音轮使其不变，可以使用`&8192`

## 频道压力（Channel Pressure）
许多MIDI设备都能够对给定通道上播放的所有音符施加“压力”

频道压力的命令为 + 后面紧跟数字 0-127，它适用于 MusicString 中使用的最新语音令牌指示的频道。

在通道压力情况下，由于解析方式不同，请勿将此标记与`+`的使用相混淆以使音符和谐地相连，将`+`放在开头。

## 复音压力（Polyphonic Pressure）
和弦压力，也称为按键压力，是施加到单个音符的压力。这是比“通道压力”更高级的功能，并且并非所有的MIDI设备都支持它。

复印压力的命令为 `*` 其后是键值（即音符值），指定为0到127之间的值，然后是逗号，最后是0到127之间的压力值。

例如，施加 75 的压力 到 `C5` 音符上可以使用 `*60，75`，注意此处的命令不支持音符值，所以使用 `*C5,75` 会发生错误。

通道压力与复音压力之间的差异在于，通道压力会同等地应用于给定通道中演奏的所有音符，而和弦压力则分别应用于通道中的每个音符。切记 JFugue 命令在通道压力与复音压力之间的区别的一种方法是，代表通道压力的加号 `+`代表比星号字符`*`代表复音压力稍微更简单。

## 控制事件（Controller Events）
MIDI规范定义了大约100个控制器事件，这些事件用于指定控制音乐声音的各种设置。 这些功能包括踏板，左右平衡，滑音（音符彼此滑入），颤音等等。关于完整的文档可以参考 MIDI 规范。

JFugue 控制命令使用 `X` 
```java
Xcontroller_number=value 
X37=18 
X[Chorus_Level]=64
 ```
如果您熟悉MIDI控制器，则可能知道有 14 个同时具有“粗”和“细”设置的控制器。这些控制器实质上具
有16位数据，而不是其他大多数控制器的典型 8 位（一个字节）。可以通过两种方式指定粗略设置和精细设置。
第一种方式

```java
 X[Foot_Pedal_Coarse]=10 
 X[Foot_Pedal_Fine]=65
 ```

当然，JFugue可以比这好用，对于具有粗略和精细成分的 14 个控制器事件中的任何一个，您可以同时指定两个值：

```java
X[Foot_Pedal]=1345
```
想要音量设置为 10200 或者 16383，无需弄清楚10200的高字节和低字节，只需要使用 `X[Volume]=10200`，JFugue 将值分为高字节和低字节。

很多控制器具有两个设置 ON/OFF，通常 ON 表示 127，OFF 表示为 0，JF 定义两个常亮 ON/OFF 可以使用常量来代替如 `X[Local_Keyboard]=ON`。JFugue 默认为 DEFAULT 为 64。可以和 X 一块使用的常亮如下：

Controller | JFugue-Constant
|-|-|
0 | BANK_SELECT_COARSE
1 | MOD_WHEEL_COARSE
2 | BREATH_COARSE
4 | FOOT_PEDAL_COARSE
5 | PORTAMENTO_TIME_COARSE
6 | DATA_ENTRY_COARSE
7 | VOLUME_COARSE
8 | BALANCE_COARSE
10 | PAN_POSITION_COARSE
11 | EXPRESSION_COARSE
12 | EFFECT_CONTROL_1_COARSE
13 | EFFECT_CONTROL_2_COARSE
16 | SLIDER_1
17 | SLIDER_2
18 | SLIDER_3
19 | SLIDER_4
32 | BANK_SELECT_FINE
33 | MOD_WHEEL_FINE
34 | BREATH_FINE
36 | FOOT_PEDAL_FINE
37 | PORTAMENTO_TIME_FINE
38 | DATA_ENTRY_FINE
39 | VOLUME_FINE
40 | BALANCE_FINE
42 | PAN_POSITION_FINE
43 | EXPRESSION_FINE
44 | EFFECT_CONTROL_1_FINE
45 | EFFECT_CONTROL_2_FINE
64 | HOLD_PEDAL-or-HOLD
65 | PORTAMENTO
66 | SUSTENUTO_PEDAL-or-SUSTENUTO
67 | SOFT_PEDAL-or-SOFT
68 | LEGATO_PEDAL-or-LEGATO
69 | HOLD_2_PEDAL-or-HOLD_2
70 | SOUND_VARIATION
71 | SOUND_TIMBRE
72 | SOUND_RELEASE_TIME
73 | SOUND_ATTACK_TIME
74 | SOUND_BRIGHTNESS
75 | SOUND_CONTROL_6
76 | SOUND_CONTROL_7
77 | SOUND_CONTROL_8
78 | SOUND_CONTROL_9
79 | SOUND_CONTROL_!10
80 | GENERAL_BUTTON_1
81 | GENERAL_BUTTON_2
82 | GENERAL_BUTTON_3
83 | GENERAL_BUTTON_4
91 | EFFECTS_LEVEL
92 | TREMULO_LEVEL
93 | CHORUS_LEVEL
94 | CELESTE_LEVEL
95 | PHASER_LEVEL
96 | DATA_BUTTON_INCREMENT
97 | DATA_BUTTON_DECREMENT
98 | NON_REGISTERED_COARSE
99 | NON_REGISTERED_FINE
100 | REGISTERED_COARSE
101 | REGISTERED_FINE
120 | ALL_SOUND_OFF
121 | ALL_CONTROLLERS_OFF
122 | LOCAL_KEYBOARD
123 | ALL_NOTES_OFF
124 | OMNI_MODE_OFF
125 | OMNI_MODE_ON
126 | MONO_OPERATION
127 | POLY_OPERATION


组合的控制器常数。 可以将整数分配给如下表整数，而JFugue将找出高字节和低字节：
Combined-Contoller | JFugue-Constant
|-|-|
16383 | BANK_SELECT
161 | MOD_WHEEL
290 | BREATH
548 | FOOT_PEDAL
677 | PORTAMENTO_TIME
806 | DATA_ENTRY
935 | VOLUME
1074 | BALANCE
1322 | PAN_POSITION
1451 | EXPRESSION
1580 | EFFECT_CONTROL_1
1709 | EFFECT_CONTROL_2
12770 | NON_REGISTERED
13028 | REGISTERED

## 常数（Constants）
在编写音乐时，主要任务是有没的旋律，而不是被随机无意义的数字所干扰。设置 VOLUME 并使用FLUTE ，不必记住 VOLUME 是控制器编号 935（或 VOLUME 由粗略值和精细值组成） FLUTE 是乐器编号73。为了使数字有意义，并且易于使用，JFugue 定义了很多常量并在 MustringString 转换成音乐时中解析。

设置常数命令如下：
```java
$WORD=DEFINITION
```
或者 `$ELEC_GRAND=2`，当然 JFugue 也可以定义 ELECTRIC_GRAND  =  2。但是也许您想使用一个较短的名称，或者您可能会为该乐器使用一个更易记的名称，例如 ELEC。当您想引用此特定乐器时，可以在 MusicString 中使用您的简称。

JFugue为乐器名称，打击乐器，速度和控制器事件之类的东西定义了一堆常数（大约375）。创建这些常量的类是 JFugueDefinitions

如果需要设置或者更改常量，假设您想用自己喜欢的乐器弹奏一些音乐，您可以将FAV_INST定义为0，然后随时使用`I[Fav_Inst]`。如果乐器发生了变化，那么在音乐字符串中所要做的就是改变FAV_INST 的定义，不必在引用乐器的每个位置上进行更改。

可以在需要数字的任何地方使用常数，但八度值除外（因为只将音符本身和八度指定为一个数字），并且使用 一个持续时间常数，您必须使用十进制持续时间值（并在持续时间前加上斜杠/）。

在 MusicString 中使用常量时，需要将单词放在方括号中。

## 时间信息（Timing Information）
当从乐谱上抄音符时，通过休息和音符持续时间的组合，可以创建音符之间具有适当时间延迟的音乐。但是从 MIDI 文件中读取音乐时，不能保证音符以这种正确的方式相互跟随。因为 JFugue 使用时间令牌来表示序列中播放音符及其他标识的毫秒数。创建音乐时不会用到此功能，但是如果将音乐从 MIDI 转换成 MusicString 时就会用到它。

时间信息的指令是 `&` 后面加一个毫秒数。当指令后面有时间信息命令时就需要被播放。

时间不需要是连续的，完整的 JFugue MusicString 在呈现音乐之前已解析，并且表示任何时间的定时信息将在播放期间的正确时间播放。

## MusicString Style（样式）
建议遵循以下准则，以帮助您创建易于阅读和共享的 MusicStrings，虽然 MusicString 不区分大小写，但使用大写和小写字符来最大化 MusicString 的可读性。

1. 使用大写字母表示代表指令的字符：`I, V, L, T, X, K`（分别指 Voice, Layer, Tempo, Controller, and Key Signature, respectively）
2. 使用大写作为音符，`C, D, E, F, G, A, B, R`
3. 指定和弦时，请使用小写字母：`maj, min, aug` 等
4. 注释持续时间使用小写字母 `w, h, q, i, s, t, x, o`。如果您持续使用和弦后的持续时间，则在音符持续时间中使用大写字母可能更易读
5. 使用混合大小写（或者成为驼峰）代表乐器名称，percussion names, tempo names, or controller names 分别是 `I[Piano], [Hand_Clap], T[Adagio] , X[Hold_Petal]`
6. 在定义和引用常量时使用全大写加下划线形式`$MY_WORD=10`
7. 每个指令间留一个空格，但是如果要写多种声音的音乐，如下所示，将每个声音放在自己的行上并使用空格使音符对齐非常有用。
8. 使用竖线字符（也称为竖线）`|`来指示 MusicString 中的小节

以下是一些符合规范的例子
```java
Player player = new Player(); 
 // First two measures of "Für Elise", by Ludwig van Beethoven 
 player.play("V0 E5s D#5s | E5s D#5s E5s B4s D5s C5s " + 
"					V1 Ri             | Riii                                         "); 
 // First a few simple chords 
 player.play("T[Vivace] I[Rock_Organ] Db4minH C5majW C4maj^^");
```

## JFugue 元素：使用对象代替 MusicString

至此，已经了解如何使用 JFugue 的符号创建 MusicSting，还可以通过创建许多单独的音符对象并将它们添加在一起来构造歌曲，这种方式创建音乐非常繁琐。制作MusicString很容易，让JFugue在幕后创建对象。

然而，很多情况下需要通过单个实例对象来创建音乐。或者需要构建一个音符的循环，或者需要在编译是才传递具体的乐器类型。

JFugue 提供了通过实例化一个类并将音乐元素添加到 Pattern 中来创建任何音乐元素的功能。在下一章中，您将学到更多有关Patterns的知识，但与此同时，您需要知道的是Pattern是一种可以由播放器播放的音乐，您可以向其中添加音乐元素。

下面是创建音乐时可以调用的各种事件，可以使用预定义的常量来表示值，例如 `Tempo.ADAGIO`或者`Instrument.PIANO`。

```java
// Create a new Voice instance 
Voice voice = new Voice(byte voiceValue);
// Create a new Layer instance 
Layer layer = new Layer(byte layerValue);
// Create a new Tempo instance (two examples) 
Tempo tempo = new Tempo(int tempoInBPM); 
Tempo tempo = new Tempo(Tempo.ADAGIO);
// Create a new Instrument instance (two examples) 
Instrument instrument = new Instrument(byte instrumentValue);
Instrument instrument = new Instrument(Instrument.PIANO);
// Create a new Note instance (four examples) 
Note note = new Note(byte value, long durationInMilliseconds); 
Note note = new Note(byte value, double decimalDuration);
Note note = new Note(byte value, long durationInMilliseconds, byte attackVelocity, byte decayVelocity);
Note note = new Note(byte value, double decimalDuration, byte attackVelocity, byte decayVelocity);
// Create a new Pitch Bend instance 
PitchBend pitchBend = new PitchBend(byte leastSignificantByte, byte mostSignificantByte); 
// Create a new Channel Pressure instance 
ChannelPressure channelPressure = new ChannelPressure(bytepressure); 
// Create a new Polyphone Pressure instance 
PolyphonicPressure polyPressure = new PolyphonicPressure(bytekey, byte pressure);
// Create a new Measure instance 
// (which is purely decorative and results in no music) 
Measure measure = new Measure();
// Create a new Key Siganture instance 
// keySig: a value from -7 to +7. -7 means 7 flats, +7 
// means 7 sharps, 0 means no flats or sharps. 
// scale: 0 for major, 1 for minor 
// This follows the MIDI Specification on Key Signature. 
KeySignature keySig = new KeySignature(byte keySig, bytescale); 
// Create a new Controller instance 
// Note that for controllers that have a Coarse and Fine 
// setting, each of those settings has to be 
// instantiated individually. 
Controller controller = new Controller(byte index, bytevalue); 
// Create a new Time instance 
// milliseconds: The millisecond position of the next 
// musical event
Time time = new Time(long milliseconds);
```

创建这些对象后可以使用 `addElement(JFugueElement element)`

```java
 Pattern pattern = new Pattern(); 
 pattern.addElement(voice); 
 pattern.addElement(instrument); 
 pattern.addElement(note); 
 Player player = new Player(); 
 player.play(pattern);
 ```
 
 这种模式产生的音乐将等同于从 MusicString 产生的音乐，其中包含与 JFugue 表示法相同的音乐事件：
 ```java
 Player player = new Player(); 
 player.play("V0 I[Piano] C5q");
 ```
 
 还可以创建一个音符值和持续时间数组，并使用它们来构造大量的音符对象，然后将音符对象添加到模板中并播放该模板。
 
 ```java
 // Define the value and decimal duration for each note 
byte[] noteValues = new byte[] 
 { 64, 69, 72, 71, 64, 71, 74, 72, 76, 68, 76 }; 
double[] durations = new double[] 
 { 0.0625, 0.0625, 0.0625, 0.0625, 0.0625, 0.0625, 0.0625, 
 0.125, 0.125, 0.125, 0.125 }; 
 1.
 // Create a Pattern and set the tempo and instrument
Pattern pattern = new Pattern(); 
pattern.addElement(new Tempo(110)); 
pattern.addElement(new Instrument(Instrument.HARPISCHORD)); 
 
 // Build up the pattern using the note values and durations
for (int i=0; i < noteValues.length; i++) { 
	 Note note = new Note(noteValues[i], durations[i]); 
	 pattern.addElement(note); 
} 

 // Play the pattern
Player player = new Player(); 
player.play(pattern);
```

在您复制已知音乐的情况下，这种创建音乐的方法并不是最佳选择。但是，通过算法创建音乐时可能会有用，这更多的是为了合理化，而非实际使用。

## 获取音符协助（Getting Assistance with Notes）
有人可能会说音符对于创造音乐至关重要，在MusicString之外的各种情况下都使用音符。例如，在第7章中，您将学习间隔符号，它使您可以根据间隔（音符之间的差异）来指定音乐，而不是具体音符本身。然后，您将根便笺传递给Interval表示法类，以根据提供的时间间隔创建便笺的特定实例。

正如您所知，可以使用 `C5` 等符号在 MusicString 中指定音符。可以使用MIDI音符值来指定音符，例如`[60]`。MusicStringParser 能够将`C5`之类的东西转换为有意义的音符值。大多数方法不受MusicStringParser支持，这些方法需要音符值。问题在于，像C5这样的符号会在一段时间后变得非常舒适。

为了使音乐创作尽可能简单，Note 类具有许多静态方法，这些方法可以产生给定 MusicString 类记法的 MIDI 音符值。此外，您可能发现有必要将音符值转换为字符串，查找特定十进制持续时间的持续时间字母，等等。Note类包含以下随时可以使用的静态方法：
```java
 // Returns a MusicString represention of the given MIDI note value. 
 // For example, given 60, this method returns C5. 
public static String getStringForNote(int noteValue) 
 // Returns a MusicString represention of the given MIDI note value 
 // and decimal duration. For example, given 60 and 0.5, 
 // this method returns C5h. 
public static String getStringForNote(int noteValue, double decimalDuration) 
 // Returns the frequency, in Hertz, for the given note value. 
 // For example, the frequency for A5 (MIDI note 69) is 440.0 
public static double getFrequencyForNote(int noteValue)
```

JFugue 还包含以下用于将十进制值转换为表示持续时间的 MusicString 的方法，反之亦然。很多方法中的一种 `getStringForDuration(double decimalDuration)`，返回给定十进制持续时间的MusicString表示形式。例如 给定0.5，此方法返回`h`。此方法仅转换单个持续时间值（不支持符合，3.0）代表整个，一半，四分之一，八分之一，十六分，三十二秒，六十四分之一和一百二十八分的持续时间；以及与这些持续时间相关的点缀持续时间（例如 0.75 代表 `h`）。此方法不转换合并持续时间（例如 0.625代表`hi`）任何大于1.0的持续时间（例如，4.0 代表 `wwww`）。对于这些值，原始的十进制持续时间以字符串形式返回，并以/开头，以使返回的值成为有效的MusicString持续时间指示符。

第二种方法，`getDecimalForDuration(String stringDuration)`，行为相似：它接受一个表示单个持续时间字符或带点缀的持续时间字符的String，并返回对应于该持续时间的十进制值。此方法不适用于合并的持续时间字符串，例如`hi`或`wwww`。

最后，Note 类包含一个公共的静态 String 数组 NOTES，该数组包含一个八度音阶中十二个音符的每一个的字符串表示形式：

```java
public static final String[] NOTES = new String[] { "C", "C#", "D", "Eb", "E", "F", "F#", "G", "G#", "A", "Bb", "B" };
```

## 将乐谱转录为JFugue MusicString
本节介绍如何将乐谱转换为JFugue表示法，在本演示中，我们将使用 “Spring” 这首音乐。以下示例使用了Pattern类，您将在下一章中详细了解。现在，您只需要知道Pattern类是一个包含MusicString的对象。


首先要注意的是，有两个谱号，高音谱号和低音谱号，这意味着我们需要将音乐输入两种声音-即可以和谐播放的两种音乐。我们会将高音谱号的音符放入音色0，将低音谱号的音符放入音色1。这里有一个节奏，所以我们也可以输入它。所以 我们会有这样一个 MusicString：
```java
 Pattern pattern = new Pattern("T[Allegro]"); 
 pattern.add("V0 notes-for-treble-clef"); 
 pattern.add("V1 notes-for-bass-clef"); 
 Player player = new Player(); 
 Player.play(pattern);
 ```
 活页乐谱中有一个时间签名，但是JFugue当前不包含时间表示。这是因为时间标识对于了解小节中出现多少音符非常重要。然而，JFugue能够播放节奏和音符持续时间的音符。事实上，在JFugue的符号中，连竖线也是可选的。
 
 让我们开始输入高音谱号的音符，我们首先看到 `C` 音符，四分音长。回顾图2.2，该音符在第五个八度音阶中。这意味着我们有`C5q`。
 
 接下来是一条条线，指示第一个小节的结束。我们将在音乐字符串中添加一个竖线符号`|`； 这将有助于音乐的清晰度。
 
 然后我们看到`C`和`E`音符和谐地演奏，这些又是四分音符。这些注释的可以表示为`E5q + C5q`。并且 我们连续有三个。
 
 接下来是两个八分音符，`D`和`C`。尽管它们被禁止在一起，但是该 bar 是风格上的，并且不会改变八音符的演奏方式。我们需要将`D5i`和`C5i`添加到我们的 MusicString 中。
 
 然后还有另一个测量 bar，因此添加另一个管道符号。然后有两个音符，`E`和`G`，以半点缀的虚线持续演奏。我们需要添加`Eh.+ Gh.` 到 MusicString 。
 
 此时，我们的MusicString应该如下所示：
 ```java
 V0 C5q | E5q+C5q E5q+C5q E5q+C5q D5i C5i | Eh.+Gh.
 ```
 继续，有第八个G和F音符，所以添加 `G5i F5i`。
 
 接下来的八个音符是我们已经输入的音符的副本，我们在这里有几种选择。最明显的选择是我们可以重新输入音符，我们可以将重复的音符放在自己的模式中，并在看到这组八个音符时使用该模式。或者，我们也可以使用 Pattern 类上的方法来重复已经输入的音符子集。由于在下一章之前不会详细讨论“模式”，所以我们将“模式”选项放在一边，只需要重新键入（或复制并粘贴）便笺即可。
 
 因此，我们的MusicString现在看起来像这样：
 ```java
 V0 C5q | E5q+C5q E5q+C5q E5q+C5q D5i C5i | Eh.+Gh. G5i F5i | 
E5q+C5q E5q+C5q E5q+C5q D5i C5i | Eh.+Gh. G5i F5i
 ```
 
 现在我们可以制作低音谱号了。 首先，要休息四分之一。将其余部分添加到 MusicString 中非常重要，这样谱号才能正确排列。将`Rq`添加到低音谱号。 也添加一条线。
 
 接下来，我们看到一堆C音符，持续时间为一半。根据图2.2，这些注释位于八度 4中。将这些音符添加到低音谱号，MusicString 应该如下所示：
 
 ```java
 V1 Rq | C4h C4h | C4h C4h | C4h C4h | C4h C4h
 ```
 
 因此，程序本身应如下所示：
 
 ```java
 Pattern pattern = new Pattern("T[Allegro]"); 
 pattern.add("V0 C5q | E5q+C5q E5q+C5q E5q+C5q D5i C5i | 
Eh.+Gh. G5i F5i | E5q+C5q E5q+C5q E5q+C5q D5i C5i | Eh.+Gh. G5i 
F5i"); 
 pattern.add("V1 Rq | C4h C4h | C4h C4h | C4h C4h | C4h C4h");
 ```
 
 由于MusicString中允许有多余的空格，因此您可以将谱号隔开，以便它们更清晰地排列：
 
 ```java
 
 // This looks better on a larger display! 
 Pattern pattern = new Pattern("T[Allegro]"); 
 pattern.add("V0 C5q | E5q+C5q E5q+C5q E5q+C5q D5i C5i | Eh.+Gh. G5i F5i | E5q+C5q E5q+C5q E5q+C5q D5i C5i | Eh.+Gh. G5i F5i"); 
 pattern.add("V1 Rq | C4h C4h | C4h C4h | C4h C4h | C4h C4h  "); 
 // Now, play the music! 
 Player player = new Player(); 
 Player.play(pattern);
 ```
 
 现在可以将音乐转录为 JFugue。
 
 ## 将MIDI转换为 JFugue MusicStrings
 
 JFugue可以将MIDI文件转换为可以探索和操纵的MusicString。
 
 正如您将在第8章中了解到的那样，JFugue的架构使得任何Parser都可以解释数据并将音乐事件触发到任何ParserListener。例如，JFugue的MusicStringParser可以向JFugue的MidiRenderer触发事件（实现 ParserListener），从而将MusicStrings转换为MIDI。
 
 JFugue可用的解析器之一是MidiParser。JFugue可用的ParserListener之一是MusicStringRenderer。
 
 JFugue将MIDI转换为 MusicString 的功能非常好。这意味着您可以获取MIDI文件并以各种有趣的方式进行播放：
 - 从现有歌曲中跳动
 - 从歌曲中获取一个声音，翻转并反转它
 - 采样一首歌曲并将其用作循环
 - 学习特定歌曲的音符（打印出MusicString）
 - 对歌曲进行数学分析以识别其音调结构
 - 创建一个Hidden Markov Model，该模型可以为给定的艺术家识别一个音符跟随另一个音符的可能性； 然后用它来创作歌手风格的新歌，
 - 还有其他无限的可能性

这个调用很简单，但是幕后的事情很复杂。首先，要将 MIDI 文件转换为 MusicString，请使用`loadMidi()`命令：
```java
Player player = new Player(); 
 Pattern pattern = player.loadMidi(new File("music-file.mid"));
```

与`playMidiDirectly()`一样，如果读取 MIDI 文件有问题，则 `loadMidi()`命令可能会引发 IOException 或InvalidMidiDataException，因此请务必捕获这些异常。

在幕后，JFugue正在做很多事情：
1. 获取MIDI文件格式，并从中获取音序的时间和分辨率，
2. 将 MidiParser 连接到 MusicStringRenderer
3. 从 MIDI 文件中获取一个序列并进行解析
4. 从MusicStringRenderer返回一个新的Pattern

生成的 Pattern 包含一个与 MIDI 文件非常相似的 MusicString 。 如果查看 MusicString，则会看到许多熟悉的命令。

从MIDI文件创建的MusicString与可能自己创建的MusicString之间存在一些差异。首先 所有音符值和持续时间均使用其数值表示，因此用`[60]/0.25`代替了`C5q`，同样，您将看到乐器的数值（例如，用`I0`代替`I[Piano]`）和其他带有值的MusicString命令。

更令人惊讶的是，您会看到许多“时间”命令-一个标志 `@`，随后是播放下一个命令的时间（以毫秒为单位）。通常，几乎不会在自己创建的 MusicString 中使用Time命令。构建自己的 MusicString 时，您将使用音符时长和休止符来适当地隔开音乐。MIDI对音乐的定义不同：音符可以随时打开或关闭，而不一定在规定的持续时间内。而且，MIDI中没有明确的休止符； 只有“注解”和“注解”事件可以在不确定的时间发生。这就是为什么您会看到“时间”命令的原因，也是为什么您会看到音符持续时间不适合典型的整个下半年等的原因。代替[60] /0.25，您更有可能会看到类似[60] /0.0242555568的信息


## FilePlayer
JFugue FilePlayer（org.jfugue.extras.FilePlayer）旨在用作命令行实用程序，它将JFugue模式文件作为输入并播放MIDI

可以在模式上使用 `saveMusicString()` 方法创建JFugue模式文件，也可以在任何文本编辑器中创建一个JFugue模式文件。

无论哪种情况，每行都可以包含MusicString。 文件中的所有MusicStrings将被串联在一起以创建一个新的Pattern实例

以井号（`＃`）开头的行被视为注释。 请注意，注释行必须以井号**开头**； 如果英镑符号位于行中的其他位置，则JFugue会忽略它，因为代表清晰音符的键也是 pound。

JFugue模式文件不支持使用Rhythm，IntervalNotation或MicrotoneNotation类创建的音乐。以下是一个示例JFugue模式文件，其中包含贝多芬“FürElise”的前几个小节
```txt
# 
# "Fur Elise", Ludwig van Beethoven 
# Transcribed into JFugue by David Koelle 
# http://www.jfugue.org 
# 
T200 
V0 E5s D#5s | E5s D#5s E5s B4s D5s C5s | A4i Rs C4s E4s A4s | 
B4i Rs E4s G#4s B4s | C5i Rs E4s E5s D#5s | E5s D#5s E5s B4s 
D5s C5s | A4i Rs C4s E4s A4s | B4i Rs E4s C5s B4s | A4q 
V1 Ri | Riii | A2s E2s A3s Rsi | 
E2s E3s G#3s Rsi | A2s E2s A3s Rsi | Riii | A2s E2s A3s 
Rsi | E2s E3s G#3s Rsi | Riii
```


## Midi2JFugue
Midi2JFugue 程序（org.jfugue.extras.Midi2JFugue）使用 JFugue 的解析器和渲染器将 MIDI 文件转换为 JFugue 模式文件。
与 FilePlayer 一样，它旨在用作命令行实用程序。

传递给 Midi2JFugue 的参数包括现有 MIDI 文件的文件名和目标模板文件的文件名。

然后可以使用 FilePlayer 播放模式文件，或使用 `Pattern.loadMusicString(File)` 方法将其加载到 JFugue 模式中


## 例子

### 播放经典的音乐
```java
 Player player = new Player(); 
 player.play("C D E F G A B"); 
 player.close(); 
```
### 保存音乐为 MIDI 文件

```java
Player player = new Player(); 
Pattern pattern = new Pattern("A5q B5q C5q");
player.saveMidi(pattern, new File("MySong.midi"));
```
### 加载播放 MIDI 文件

```java
Player player = new Player();
player.playMidiDirectly(new File("MySong.midi"));
```

### 保存  Pattern


```java
Pattern pattern = new Pattern("A5q B5q C5q");
pattern.saveMusicString(new File("pattern.jfugue"));
```

### 加载 Pattern

```java
Player player = new Player(); 
Pattern pattern = null;
 pattern = Pattern.loadMusicString(new File("pattern.jfugue")); 
 player.play(pattern);
```

### 如何加载 MIDI 文件并将其转换为 JFugue MusicString
这将获取一个MIDI文件并将其转换为JFugue模式，然后可以根据需求对其做修改，如果您对它的工作方式感兴趣，那么它是JFugue解析器-渲染器体系结构的一个很好的例子。

```java
 Player player = new Player(); 
 Pattern pattern = null; 
try { 
 	pattern = player.loadMidi(new File("MySong.midi")); 
 	System.out.println(pattern); // Show the pattern
 } catch (IOException e) 
 { 
// handle IO Exception
 } catch (InvalidMidiDataException e) 
 { 
// handle Invalid MIDI Data Exception
 }
```
### 结合 Patterns

```java
 Pattern pattern1 = new Pattern("A5q"); 
 Pattern pattern2 = new Pattern("C5q C5q G5q"); 
 pattern1.add(pattern2); // Add patterns together
 pattern1.add("F6h"); // Add a MusicString to a pattern
```

### 重复 Patterns

```java
 Pattern pattern1 = new Pattern("A5q C5q G5q"); 
// Repeat this pattern 4 times
 pattern1.repeat(4); 
// Repeat twice the pattern starting at position 4
// (results in A5q C5q G5q C5q G5q C5q G5q)
 pattern1.repeat(2, 4); 
// Repeat twice the subset of the pattern from position 4
// through position 6 (results in A5q C5q G5q C5q C5q)
 pattern1.repeat(2, 4, 6);
```
### 创建匿名 ParserListener

```java
public class GetInstrumentsUsedTool extends ParserListenerAdapter 
{ 
	private List<Instrument> instruments; 
	public GetInstrumentsUsedTool()
 { 
	instruments = new ArrayList<Instrument>(); 
 } 
@Override
public void instrumentEvent(Instrument instrument) 
 { 
	if (!instruments.contains(instrument)) { 
		instruments.add(instrument); 
	 } 
 } 
public List<Instrument> getInstrumentsUsed(Pattern pattern) 
 { 
	 MusicStringParser parser = new MusicStringParser(); 
	 parser.addParserListener(this); 
	 parser.parse(pattern); 
	return instruments; 
 } 
}
```
### 创建解析器 Parser
1. 创建解析器的子类
2. 创建一个`parse()`方法，该方法获取您要解析的任何对象，并在解析某些音乐事件时触发事件
3. 在适当的时候构造JFugue元素，并使用Parser类中可用的fireXxxxEvent（）方法将它们触发到任何ParserListener：

```java
protected void fireVoiceEvent(Voice event) 
protected void fireTempoEvent(Tempo event) 
protected void fireInstrumentEvent(Instrument event) 
protected void fireLayerEvent(Layer event) 
protected void fireTimeEvent(Time event) 
protected void fireKeySignatureEvent(KeySignature event) 
protected void fireMeasureEvent(Measure event) 
protected void fireControllerEvent(Controller event) 
protected void fireChannelPressureEvent(ChannelPressure  event) 
protected void firePolyphonicPressureEvent(PolyphonicPressure event) 
protected void firePitchBendEvent(PitchBend event) 
protected void fireNoteEvent(Note event) 
protected void fireParallelNoteEvent(Note event) 
protected void fireSequentialNoteEvent(Note event)
```

### 创建渲染器 Renderer
1. 创建一个类实现 ParserListener
2. 覆盖其方法

```java
public void voiceEvent(Voice voice); 
public void tempoEvent(Tempo tempo); 
public void instrumentEvent(Instrument instrument); 
public void layerEvent(Layer layer); 
public void measureEvent(Measure measure); 
public void timeEvent(Time time); 
public void keySignatureEvent(KeySignature keySig); 
public void controllerEvent(Controller controller); 
public void channelPressureEvent(ChannelPressure channelPressure); 
public void polyphonicPressureEvent(PolyphonicPressure polyphonicPressure); 
public void pitchBendEvent(PitchBend pitchBend); 
public void noteEvent(Note note); 
public void parallelNoteEvent(Note note); 
public void sequentialNoteEvent(Note note);
```
### 链接解析器及渲染器

```java
YourParser parser = new YourParser(); 
YourRenderer renderer = new YourRenderer(); 
 parser.addParserListener(renderer); 
 parser.parse(whatever object your parser parses);
```
### 解析 MIDI 并渲染为 MusicString

```java
MidiParser parser = new MidiParser(); 
 MusicStringRenderer renderer = new MusicStringRenderer(); 
 parser.addParserListener(renderer); 
 parser.parse(MIDI sequence);
```
### 解析并 MusicString 渲染为 MIDI

```java
MusicStringParser parser = new MusicStringParser(); 
 MidiRenderer renderer = new MidiRenderer(); 
 parser.addParserListener(renderer); 
 parser.parse(MusicString);
```
### 创建韵律 Rhythm

```java
Rhythm rhythm = new Rhythm(); 
// Set up your substitutions. Examples:
 rhythm.addSubstitution('O', "[ACOUSTIC_BASS_DRUM]s"); 
 rhythm.addSubstitution('o', "[ACOUSTIC_SNARE]s"); 
 rhythm.addSubstitution('\'', "[CLOSED_HI_HAT]s"); 
 rhythm.addSubstitution('`', "[OPEN_HI_HAT]s"); 
 rhythm.addSubstitution('.', "Rs");
 // Create layers using your substitutions. Examples:
 rhythm.setLayer(1, "O.OO...O.OO....O"); 
 rhythm.setLayer(2, "....o.......o..."); 
 rhythm.setLayer(3, "'.`.'.`.'.`.'.`."); 
// Generate a Pattern from the Rhythm
 Pattern pattern = rhythm.getPattern(); 
// Play the pattern!
 Player player = new Player(); 
 player.play(pattern);
```

### 使用间隔符号

```java
// Specify a MusicString using intervals 
 IntervalNotation riff = new IntervalNotation( 
"<1>q <5>q <8>q <1>q+<5>q+<8>q <1>majq"); 
 // Get a Pattern specifically tailored to the C5 note 
 Pattern pattern = riff.getPatternForRootNote("C5"); 
 // Play the pattern 
 Player player = new Player(); 
 player.play(pattern); 
 // Compare the result against a pattern that explicitly 
 // uses C5, just to demonstrate that this works! 
 player.play("C5q E5q G5q C5q+E5q+G5q C5majq"); 
 // Get riffs for multiple root notes, then play them together 
 Pattern p1 = riff.getPatternForRootNote("C5"); 
 Pattern p2 = riff.getPatternForRootNote("E5"); 
 Pattern p3 = riff.getPatternForRootNote("C5"); 
 Pattern p4 = riff.getPatternForRootNote("G5"); 
 Pattern fullPattern = new Pattern(p1, p2, p3, p4); 
 new Player().play(fullPattern);
```

### 结合间隔和旋律

```java
Rhythm rhythm = new Rhythm(); 
// Step 1a. Hammer out your beat – this example is 8-beat 
rhythm.setLayer(1, "O.OO...O.OO....O"); 
rhythm.setLayer(2, "....o.......o..."); 
rhythm.setLayer(3, "^.`.^.`.^.`.^.`."); 
rhythm.setVoice(1, "1...234.....11..");
rhythm.setVoice(2, "W V ....W "); 
// Step 1b. Set voice details (like instruments) 
rhythm.setVoiceDetails(1, "I[Piano]"); 
rhythm.setVoiceDetails(2, "I[String_Ensemble_2]"); 
// Step 2. Identify instruments to use in the beat 
// (ensure the MusicString for each is the same duration) 
rhythm.addSubstitution('O', "[ACOUSTIC_BASS_DRUM]s"); 
rhythm.addSubstitution('o', "[ACOUSTIC_SNARE]s"); 
rhythm.addSubstitution('^', "[CLOSED_HI_HAT]s"); 
rhythm.addSubstitution('`', "[OPEN_HI_HAT]s"); 
rhythm.addSubstitution('.', "Rs"); 
rhythm.addSubstitution('1', "<1>s"); 
rhythm.addSubstitution('2', "<2>s"); 
rhythm.addSubstitution('3', "<3>s"); 
rhythm.addSubstitution('4', "<4>s"); 
rhythm.addSubstitution('W', "<1>qa120d120"); 
rhythm.addSubstitution('V', "<4>qa120d120"); 
// Step 3. Get the Pattern, repeat it, and play it 
Pattern p1 = rhythm.getPatternWithInterval (new Pattern("C3")); 
Pattern p2 = rhythm.getPatternWithInterval (new Pattern("E3")); 
Pattern p3 = rhythm.getPatternWithInterval (new Pattern("C3")); 
Pattern p4 = rhythm.getPatternWithInterval (new Pattern("G3")); 
Pattern pattern = new Pattern(p1, p2, p3, p4); 
pattern.repeat(2); 
Player player = new Player(); 
player.play(pattern);
```

### 使用微调符号

```java
MicrotoneNotation microtone = new MicrotoneNotation(); 
 // Map your desired frequencies to keys. Examples:
 microtone.put("Be", 400.00); 
 microtone.put("Bf", 405.50); 
 microtone.put("Bt", 415.67); 
 microtone.put("Bv", 429.54); 
 // Create a pattern containing your keys in brackets 
 Pattern pattern = microtone.getPattern("<Be>q <Bt>q <Bf>q 
<Bv>q"); 
 Player player = new Player(); 
 player.play(pattern);
```

### 发送 MIDI 到外接设备

```java
 DeviceThatWillReceiveMidi device = null; 
try { 
 	device = new DeviceThatWillReceiveMidi(); 
 } catch (MidiUnavailableException e) { 
// handle MIDI Unavailable Exception
 } 
 Sequence sequence = null; 
try { 
 	sequence = MidiSystem.getSequence(new File("MySong.mid")); 
 } catch (InvalidMidiDataException e) 
 { 
// handle Invalid MIDI Data Exception
 } catch (IOException e) 
 { 
// handle IO Exception
 } 
 device.sendSequence(sequence);
```
### 发送  Pattern 到外部设备

```java
 DeviceThatWillReceiveMidi device = null; 
try { 
 	device = new DeviceThatWillReceiveMidi(); 
 } catch (MidiUnavailableException e) { 
// handle MIDI Unavailable Exception
 } 
 Player player = new Player(); 
 Pattern pattern = new Pattern("A5q B5q C5q"); 
 Player.play(pattern); 
 Sequence sequence = player.getSequencer().getSequence(); 
 device.sendSequence(sequence);
```
### 从外部设备录音

```java
DeviceThatWillTransmitMidi device = null; 
try { 
 	device = new DeviceThatWillTransmitMidi(); 
 } catch (MidiUnavailableException e) { 
// handle MIDI Unavailable Exception
 }
 System.out.println("Listening for 5 seconds..."); 
 device.startListening(); 
// Wait long enough to play a few notes on the keyboard
try { 
 	Thread.sleep(5000); 
 } catch (InterruptedException e) 
 { 
// handle Interrupted Exception
 } 
// Close the device (at program exit)
 device.stopListening(); 
 System.out.println("Done listening"); 
 Pattern pattern = device.getPatternFromListening(); 
 System.out.println("Pattern from listening: "+pattern); 
 Player player = new Player(); 
 player.play(pattern); 
 player.close();
```
### 使用 JFugue 加载音库
[音乐库参考链接](http://gervill.dev.java.net)

```java
// Make sure gervill.jar is in your classpath
 Synthesizer synth = MidiSystem.getSynthesizer(); 
 Soundbank soundbank = MidiSystem.getSoundbank(new
File(soundbank filename)); 
 Sequencer sequencer = 
player.getSequencerConnecedToSynthesizer(synth); 
 Pattern pattern = new Pattern(your pattern); 
 Player player = new Player(sequencer); 
 player.play(pattern);
```