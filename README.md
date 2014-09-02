## Marvellous Moonglyphs: Match Kana and Kanji

for http://meta.codegolf.stackexchange.com/questions/1847/sandbox-for-proposed-challenges/2123#2123

Dictionary files can also be found [in the dic folder](dics/).

##Tutorial

**Do not read any further if you want to challenge yourself, or do the research yourself.**

Let us consider the simple example again:

 - `成田` [Narita, name of a town]
 - `なりた` [na-ri-ta]
 - `成`, `田`
 - [`成`,`なり`],[`田`,`た`]

`成` can be read `なり` (nari) and `せい` (sei), `田` as `た` (ta) and `でん` (den). The reading of `成田` is `なりた` (narita). Thus:

    　　　　　　　　　　　　　　　　　　／　た（ＴＡ）　　　─　せいた（ＳＥＩＴＡ）
    　　　　　／　せい（ＳＥＩ）　─　田
    　　　　／　　　　　　　　　　　　　＼　でん（ＤＥＮ）　─　せいでん（ＳＥＩＤＥＮ）
    　　　／
    　　／
    成　　　　　　　　　
    　　＼
    　　　＼
    　　　　＼　　　　　　　　　　　　　／　た（ＴＡ）　　　─　なりた（ＮＡＲＩＴＡ）
    　　　　　＼　なり（ＮＡＲＩ）─　田
    　　　　　　　　　　　　　　　　　　＼　でん（ＤＥＮ）　─　なりでん（ＮＡＲＩＤＥＮ）


We conclude that `成` must be read `なり` (nari), and `田` as `た` (ta).


A slightly more advance example:

 - `成田空港` [Narita Airport]
 - `なりたくうこう` [na-ri-ta-ku-u-ko-u]
 - `成田`, `空港`
 - [`成田`,`なりた`] , [`空港`,`くうこう`]

Among others, `成` can be read `なり` (nari), `田` as `た` (ta), `空` as `くう` (kuu), and `港` `こう` (kou). Therefore, the correct answer in this case is

 - [`成田`,`なりた`], [`空港`,`くうこう`]

成田 is read as なりた (narita), and 空港 as くうこう (kuukou)

###Implementing the basic feature

Hint: It is more or less a tree search. Each reading is a branch.

I have broken this feature down into four steps that are intended as a road map. I believe this will make it easier for you to get acquainted with the topic and the problem, but all you need to do is implement step 4. Feel free to implement this however you see fit.

**Step 1**

Traverse the tree.

Take the input as described above, and determine whether or not there exists at least one combination of each `MOONGLYPH`'s reading, such that the `READING` may be obtained. Feel free to ignore the `PART`s for this step.

Examples:

(1)

 - `山起`
 - `やまおこし` [ya-ma-o-ko-shi]
 - `山起`
 - `True` [because 山 = ya-ma; and 起 = o-ko-shi]

(2)

 - `静岡`
 - `つき` [tsu-ki]
 - `静岡`
 - `False` 

The readings of `静` are `sei`, `jou`, `shizu`, and `shidzu`.

The readings of `岡` are `kou` and `oka`.

There is no way to combined these reading and get `tsu-ki`. Thus, your program should indicate that there is no match.

**Step 2**

Remember your position in the tree.

Also output the reading for each `MOONGLYPH`. You may still ignore `PART`s. This means

(1)

 - `山起`
 - `やまおこし` [ya-ma-o-to-shi]
 - `山`,`起`
 - [`山`,`やま`], [`起`,`おこし`]

(2)

 - `静岡`
 - `つき` [tsu-ki]
 - `静`,`岡`
 - []

**Step 3**

Score +15

Traverse the entire tree, and collect all solutions.

Output every possible combination. `PART`s may still be ignored.

 - `網岬` [this is not a real word, it's pretty hard to come up with something]
 - `あみさき` [a-mi-sa-ki]
 - `網`,`岬`
 - [[`網`,`あみ`],[`岬`,`さき`]], [[`網`,`あ`],[`岬`,`みさき`]]

Among others, `網` can be read `あみ` (ami) and `あ` (a).

Among others, `岬` can be read `みさき` (misaki) and `さき` (saki).

Thus, either `網` is read `ami` and `岬` `saki`; or `網` is read `a` and `岬` `misaki`. Both possibilities would result in the combined reading `amisaki` for `網岬`.

**Step 4**

Score +10

Output the reading for each `PART`, and not for each kanji. Remember that all `PART`s combined, in the order they were given, will result in `MOONGLYPH`.

(1)

 - `岩手県宮古市`
 - `いわてけんみやこし` [iwatekenmiyakoshi]
 - `岩手県`,`宮古市`
 - [`岩手県`,`いわてけん`], [`宮古市`,`みやこし`]

`岩手県` is read `いわてけん` (iwateken), and `宮古市` as `みやこし` (mi-ya-ko-shi).

(2)

A longer example, same principle as above.

 - `鹿児島県熊毛郡屋久島町宮之浦` [Kagoshima, "Bear Fur"-District, City of Yakushima, "Shrine Bay"]
 - `かごしまけんくまげぐんやくしまちょうみやのうら`
 - `鹿児島`,`熊毛郡`,`屋久島町`,`宮之浦`
 - [`鹿児島`,`かごしまけん`], [`熊毛郡`,`くまげぐん`], [`屋久島町`,`やくしまちょう`], [`宮之浦`,`みやのうら`]

You can implement this by simply using the output from Step 3, joining the`MOONGLYPH`s corresponding to the parts.



###Parsing the dictionary

An entry from `KANJIDIC` looks like this:

    月 376E N2169 [...] L13 Yyue4 Wweol ゲツ ガツ つき T1 おと がっ す ずき もり {moon}

You don't most of this information. All you need is the first column containing the `MOONGLYPH`; and all columns with `KATAKANA` or `HIRAGANA` - the `READINGS`. The entry above tells us that `月` can be read as either `ゲツ`, `ガツ`, `つき`, `おと`, `がっ`, `す`, `ずき`, or `もり`.

A reading may be specified by either `KATAKANA`s or `HIRAGANA`s, they are to be treated as equivalent. You probably want to change the case to either all `HIRAGANA` or `KATAKANA`. 

`KATAKANA`s correspond to Chinese `ON` readings, `HIRAGANA` to native Japanese `kun` readings. This classification will be used by feature 10, ignore it for now. You may want to save this meta-information for later, however.

    行 U884c Yxing4 [...] Whang コウ ギョウ アン い.く ゆ.く -ゆ.き -ゆき -い.き -いき
    おこな.う おこ.なう T1 いく なみ なめ みち ゆき ゆく {going} {journey} {carry out}

Strip the dashes `-` from each each reading. Take only the part before dot `.` if one is present. Thus, the set of all possible readings for the above entry are:

    コウ, ギョウ, アン, い, ゆ, ゆき, いき, おこな, おこ, いく, なみ, なめ, みち, ゆき, ゆく

See the documentation linked in the dictionary section for details.

---

##Feature 1

[Explanation of Rendaku](http://en.wikipedia.org/wiki/Rendaku) (Voicing).

The initial syllable of a `MOONGLYPH` other than the first one may get voiced, eg `T` -> `D` or `K` -> `G`. `か` (ka) becomes `が` (ga), and `と` becomes `ど` (do). This is a complex phenomenon, but to keep it simple, we are going to assume that this voicing may always occur, except for a `KANA` in initial position.

(1)

 - `東田川郡`
 - `ひがしたがわぐん` [hi-ga-shi-ta-ga-wa-gun]
 - `東`,`田川郡`
 - [`東`,`ひがし`], [`田川郡`,`たがわぐん`]

In this example, `川` only possesses the reading `かわ` [kawa]. In combination with other words or parts, in can voice to `がわ` [gawa]. Thus the reading `たがわぐん` [ta-ga-wa-gun] becomes possible for 田川郡.

(2)

 - `辺地町`
 - `へじまち` [he-ji-ma-chi]
 - `辺地町`
 - [`辺地町`,`へじまち`]

The reading `ち` [ji] for `地` becomes voiced to form `じ` [ji]. Note that `ち` can become either `じ` [ji] or `ぢ` [dji].

(3)

 - `花見`　(flower viewing, especially cherry trees)
 - `ばなみ` [ba-na-mi]
 - `花`,`見`
 - `no_match`

`花` can be read `はな` (hana), but not `ばな` (bana). Voicing (`は` -> `ば`) is not possible, because `ば` occurs in initial position.

Here is a list of all possible alternations introduced by this `Rendaku` voicing (`JSON`):

    {
      "か": [
        "が"
      ],
      "き": [
        "ぎ"
      ],
      "く": [
        "ぐ"
      ],
      "け": [
        "げ"
      ],
      "こ": [
        "ご"
      ],
      "さ": [
        "ざ"
      ],
      "し": [
        "じ"
      ],
      "す": [
        "ず"
      ],
      "せ": [
        "ぜ"
      ],
      "そ": [
        "ぞ"
      ],
      "た": [
        "だ"
      ],
      "ち": [
        "ぢ",
        "じ"
      ],
      "つ": [
        "づ",
        "ず"
      ],
      "て": [
        "で"
      ],
      "と": [
        "ど"
      ],
      "は": [
        "ば",
        "ぱ"
      ],
      "ひ": [
        "び",
        "ぴ"
      ],
      "ふ": [
        "ぶ",
        "ぷ"
      ],
      "へ": [
        "べ",
        "ぺ"
      ],
      "ほ": [
        "ぼ",
        "ぽ"
      ]
    }

---

##Feature 2

Various orthographical punctuation symbols. 

As I mentioned, That is, your program does not need to handle unmatched punctuation. Thus, examples (2) and (3) are not valid.

(1)

 - `瑞穂町（川澄）`
 - `みずほちょう（かわすみ）`
 - `瑞穂町`,`（川澄）`
 - [`瑞穂町`,`みずほちょう`], [`（川澄）`,`（かわすみ）`]

`）` and `（` simply get copied over to the output.

(2)

 - `瑞穂町（川澄）`
 - `みずほちょうかわすみ`
 - `瑞穂町`,`（川澄）`
 - `undefined`

Invalid input, because `READING` does not contain any parentheses `（）`.

(3)

 - `瑞穂町（川澄）`
 - `（みずほちょう）かわすみ`
 - `瑞穂町`,`（川澄）`
 - `undefined`

Invalid input, because the parentheses occur at different positions in `MOONGLYPH`s and `READING`. Depending on how you implement it, your program may or may not output something meaningful for example (2) and (3).

`〒` is the Japanese sign for `zip` or `postal code`. `『』` are Japanese quotation marks. `【】` are alternative quotation marks. `・` is a separator, similar to a slash `/`.

---

##Feature 3

Add support the counter and old genitive marker `ケ`, `ヶ`, and `ヵ`

They are not listed in KANJIDIC.

Consider the following three glyphs: `ケ`, `ヶ`, and `ヵ`. They may possess the following readings (eg branches):

 - `か` [ka]
 - `が` [ga]

In addtion, `ケ` and `ヶ` can also be read as

 - `げ` [ge]
 - `こ` [ko]

Note that, although ケ is the `KATAKANA` ke and thus pronounced `ke`, it is sometimes used instead of `ヶ` because of its graphical similarity - and can therefore be read as `か`, `が`, `げ`, or `こ` (rare) as well. Supporting `KANA`s in general is part of feature 8.

Example:

(1)

 - `戦場ヶ原` [Senjougahara, a character from [BakeMonoGatari](http://www.animenewsnetwork.com/encyclopedia/anime.php?id=10196)]
 - `せんじょうがはら`
 - `戦場`,`ヶ`,`原`
 - [`戦場`,`せんじょう`],[`ヶ`,`が`],[`原`,`はら`]

`戦場` literally means `battlefield`, and `原` means `plains` or `field`. `が ` is an old genitive or possession marker, and thus `Senjōgahara` [can be translated as 'battlefield' but refers to a mythical battle of Mountain Gods, and not to any historical one.](http://en.wikipedia.org/wiki/Senj%C5%8Dgahara)

(2)

 - `金ケ崎町`
 - `かねがさきちょう`
 - `金`, `ケ`, `崎`, `町`
 - [`金`,`かね`], [`ケ`,`が`], [`崎`,`さき`], [`町`,`ちょう`]

(3)

 - `一ヵ月` [a period of one month, 一 = one, 月 = month]
 - `いっかげつ`
 - `一`,`ヵ`,`月`
 - [`一`,`いっ`], [`ヵ`,`か`], [`月`,`げつ`]


---

##Feature 4

Support omitted genitive markers `の` between `MOONGLYPHS`. In old times, people didn't like to write them down, because `MOONGLYPH`s-only is way cooler.

(1)

 - `九戸郡`
 - `くのへぐん`
 - `九`, `戸`, `郡`
 - [`九`,`く`], [`<empty>`,`の`], [`戸`,`へ`], [`郡`,`ぐん`]

(2)

 - `甲斐守町`
 - `かいのかみちょう`
 - `甲斐`, `守`, `町`
 - [`甲斐`,`かい`], [`<empty>`,`の`], [`守`,`かみ`], [`町`,`ちょう`]

Technically, we would expect the spelling to be `九の戸郡`. However, `の` gets omitted at times, especially in proper nouns.

By the same token, `が` gets omitted at times as well, the mechanics are exactly like `の`.

(3)

 - `輿岡町` (City of Koshigaoka, "Palanquin Hill")
 - `こしがおかちょう` [ko-shi-ga-o-ka-cho-u]
 - `輿`, `岡`, `町`
 - [`輿`, `こし`], [`<empty>`,`が`], [`岡`,`おか`] , [`町`, `ちょう`]

---

##Feature 5

[Japanese Numerals](http://en.wikipedia.org/wiki/Japanese_numerals)

Add support for multi-digit numbers, when the `READING` includes the pronunciation of the number.

The easiest way to handle this is by converting the number to its `MOONGLYPH` representation, eg `120` => `百二十`), and then proceed as normal

 - `２０５０番地`
 - `にせんごじゅうばんち`
 - `２０５０`,`番地`
 - [`２０５０`,`にせんごじゅう`], [`番地`,`ばんち`]

Convert `２０５０` to `二千五十`, the readings ar found in the dictionary.

Just like we can say `one-hundred two` or `hundred-two`, `一` (1) is optional before `百` (hundred), `千` (thousand), `万` (1E4), and `億` (1E8). That is, a number like `1111` may be equivalent to any of the following: `千百十一`,  `一百十一`, `千一十一`, or `一千一百十一`. 

---

##Feature 6

Support `KANA`, including the now deprecated `ゑ`, `ゐ`, `ヱ`, and `ヰ`. 

Almost all kana can be ignored, like the punctuation symbols from feature 4.

Or to think about it another way, almost all `KANA`s represent themselves, and are read that way. `に` (ni) is read `に`(ni), and ロ(ro) is read `ろ`(ro). Please remember that all `READING`s are always given in `HIRAGANA`, but that is a convention for this challenge.

There are a few excpetion that date from [before the orthographical reforms](http://en.wikipedia.org/wiki/Japanese_script_reform).

The four `KANA` `ゑ` (we), `ゐ` (wi), `ヱ` (we), and `ヰ` (wi) are read as `え` (e), `い` (i), `え` (e), and `い` (i), respectively.

Please note that while `は` (ha) can also be pronounced `wa`, `へ` (he) as `e`, and `を` (wo) as `o`, they will always appear as `は`, `へ`, and `を` in the `READING` , because these three excpetions still occur in contemporary Japanese. This means that you only need to care about the four kana above.

These four are read as `え`, `い`, `え`, and `い`, respectively.

(1)

 - `流通センター`
 - `りゅうつうせんたー`
 - `流通`, `センター`
 - [`流通,りゅうつう`], [`センター`,`せんたー`]

The `READING` of the `KATAKANA`s `センター` is given by the `HIRAGANA`s `せんたー`.

(2)

 - `ヱビス`　[Japanese god of fishermen, luck, and workingmen](http://en.wikipedia.org/wiki/Ebisu_(mythology)
 - `えびす`
 - `ヱ`,`ビス`
 - [`ヱ`,え], [`びす`,`ビス`]

`ヱ` is read `え`.

(3)

 - `月への旅` [journey to the moon]
 - `つきへのたび`
 - `月`,`への`,`旅`
 - [`月`,`つき`], [`への`,`への`],[`旅`,`旅`]

`月への旅` is pronounced as `tsu-ki-e-no-ta-bi`, but in modern Japanese orthography, this is still written as `つきへのたび` [tsu-ki-he-no-ta-bi]. Thus, `へ` does not require any special treatment.

---

##Feature 7

KANJIDIC2 is EUC-JP encoded. Many of these kanji are rarely used anymore, but they may appear at times, especially in proper names. Make sure your encoding support them.

 - `您` (archaic word and moonglyph meaning `you`)
 - `なんじ`
 - `您`
 - [`您`,`なんじ`]

---

##Feature 8

Each reading of a `MOONGLYPH` is categorized as either `on` (taken from Chinese) or `kun` (native Japanese word). As a rule of thumb, words are read either with `on` or `kun` readings only (exceptions abound). If there are multiple results, sort them such that those with more `on-on` and `kun-kun` readings are listed before mixed `on-kun` and `kun-on` readings. This is going to be heuristical, and requires a metric, see above.

Example:

 - 死糟鈴 [again, made up word because these cases are rare]
 - `しぬかすず`
 - `死`, `糟`, `鈴`
 - [[`死`,`しぬ`], [`糟`,`かす`], [`鈴`,`ず`]], [[`死`,`し`], [`糟`,`ぬか`], [`鈴`,`すず`]]

Both readings for `糟` and `鈴` are `KUN` readings. For `死`, `しぬ` is a `KUN` reading, and `し` an `ON` reading. Thus, the likelihood is computed as follows:

 - KUN-KUN-KUN = 3 (`しぬ`,`かす`,`ず`)
 - ON-KUN-KUN = 1 (`シ`,`ぬか`,`すず`)

In the dictionary files, `on` readings are given as `KATAKANA`, `kun` readings `HIRAGANA`.

---

##Feature 9

The `MOONGLYPH` doubling sign `々`. A word such as `木木` (trees) can be written as `木々`. It may, but usually doesn't, appear multiple times. `木々` should be treated as equivalent to `木木`, `人々` (people) as equivalent to `人人`, and `藤原々々` as equivalent to `藤原原原` etc.

When the `MOONGLYPH` repeater 々 occurs `m*n` times, this may also be equivalent to the last `n` `MOONGLYPHS` occuring `m` times. In real-world examples, cases other than m=1 and especially n=2 are quite rare, but they may occur. The case above corresponds to `n=1` and `m=1`

Consider `終了々々`. The word `終了` consists of two characters and means `(I am) finished`. By adding two repeaters, `々々`, the last `n=2` `MOONGLYPH`s get repeated `m=1` times. Thus, `終了々々` may also be equivalent to `終了終了`. `終了々々々々々々` may be equivalent to `終了終了終了終了`.

(1)

 - `鹿島代々木町` ["Deer" Island, City of Yoyogi "Tree of Ages"]
 - `かしまよよぎちょう`
 - `鹿島`, `代々木町`
 - [`鹿島`,`かしま`] [`代々木町`,`よよぎちょう`]

n=1, m=2.

(2)

 - `人々`
 - `ひとびと`
 - `人`,`々`
 - [`人`,`ひと`], [`々`,`びと`]

n=1, m=1.

Please note that if you did not implement feature 1, the output would be `no match`.

(3)

 - `藤原々々` [Japanese Illustrator]
 - `ふじわらわらわら`
 - `藤`, `原々々`
 - [`藤`,`ふじ`] [`原々々`,`わらわらわら`]

n=1, m=2.

(4)

 - `終了々々々々` [emphatic repetition of `finish`]
 - `しゅうりょうしゅうりょうしゅうりょう`
 - `終了`,`々々`
 - [`終了`,`しゅうりょう`], [`々々`,`しゅうりょう`]

Here, n=2, m=2. 

(5)

 - `終了々々`
 - `しゅうりょうりょうりょう` [not a real reading, only for illustration purposes]
 - `終了`,`々々`
 - [`終了`,`しゅうりょう`], [`々々`,`りょうりょう`]

Here, n=2 and m=1.

---

##Feature 10

This feature assumes you have implemented `KANA`s, feature 8.

[Add support for the voiced kana repeater](http://en.wikipedia.org/wiki/Iteration_mark#Kana) `ゞ`. `MOONGLYPH`s shall not contain more than one `ゞ` in succession, ie they will never contain `ゞゞ` or `ゞゞゞ`, but they may contain `かゞみかゞみ`.

It shall occur only after a `KATAKANA` or `HIRAGANA` that can be, or has been, voiced. `かゞみ` (kagami) and `がゞみ` (gagami) are valid input strings, `みゞ` is not, because `み` (mi) cannot be voiced. See the data given in feature 1.

`かがみ` (kagami, mirror) can be written as `かゞみ`. `ゞ` should be treated to the voiced version of the preceding `KANA`.

(1)
 - `かゞみ` (mirror)
 - `かがみ`
 - `か`, `ゞ`, `み`
 - [`か`,`か`], [`ゞ`,`が`], [`み`,`み`]

`ゞ` occurs after か (ka), which can be voiced to が (ga); and stands for this voiced syllable ga.


(2)

 - `ジゞ` (old man)
 - `じじ` (jiji)
 - `ジ`, `ゞ`
 - [`ジ`,`じ`], [`ゞ`,`じ`]

`ゞ` occurs after `ジ` (ji), which is the voiced version of シ (shi), and stands for this voiced syllable ji.


---

##Final Feature

Support [special gikun readings](http://en.wikipedia.org/wiki/Kanji#Special_readings) by implenting words spanning multiple `MOONGLYPH`s. This means you will need a dictionary file.

`大和` (an old name for Japan) is read as `やまと` (yamato), and it is more than the sum of its parts. This reading *cannot* be represented as a combination of the individual readings of each `MOONGLYPH`. The compound `大和` itself is read as `やまと`.

 - `宮崎市大和町`
 - `みやざきしやまとちょう`
 - `宮崎市`, `大和町`
 - [`宮崎市`,`みやざきし`], [`大和町`,`やまとちょう`]

Up until know, you only had to consider the next `MOONGLYPH` when traversing the tree. This feature implies that you will need to take a look at the next `n` `MOONGLYPH`s as well, and add some branches if that word exists in the dictionary.

The tree might look like this:

    　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　ちょう
    　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　／
    　　　　　　　　　　　　　　　　　　／　わ（ＷＡ）　　　─　だいわ（ＤＡＩＷＡ）　　　─　町　─　まち
    　　　　　／　だい（ＤＡＩ）　─　和
    　　　　／　　　　　　　　　　｜　　＼　わら（ＷＡＲＡ）─　だいわら（ＤＡＩＷＡＲＡ）─　町　─　まち
    　　　／　　　　　　　　　　　｜　　　　　　　　　　　　　　　　　　　　　　　　　　　　　＼
    　　／　　　　　　　　　　　　｜　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　ちょう
    大　　　　　　　　　　　　　和町　ＮＯ＿ＳＵＣＨ＿ＥＮＴＲＹ
    ｜　＼
    ｜　　＼
    ｜　　　＼　　　　　　　　　　　　　　　／　ちょう（ＣＨＯＵ）　─　なりた（ＹＡＭＡＴＯＣＨＯＵ）
    ｜　　　　＼　大和（ＹＡＭＡＴＯ）─　町
    ｜　　　　　　　　　　　　　　　　　　　＼　まち（ＭＡＣＨＩ）　─　やまとまち（ＹＡＭＡＴＯＭＡＣＨＩ)
    ｜
    大和町　ＮＯ＿ＳＵＣＨ＿ＥＮＴＲＹ


[Dictionary File EDict](http://www.edrdg.org/jmdict/edict_doc.html). Use either `edict.gz` or `edict2.gz` (custom format); or `JMdict.gz` or `JMdict_e.gz` (xml). The download page also contains links to the documentation of the dictionary format.

As with Kanjidic, you can ignore most of the information. The dictionary contains words and their kana reading. You may filter out any words that contain anything other than `MOONGLYPH`s. You may also want to filter those entries your programm can map to their reading already.
