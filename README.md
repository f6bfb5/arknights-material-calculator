# arknights-material-calculator

## Project setup

```
npm install
```

### Compiles and hot-reloads for development

```
npm run serve
```

### Compiles and minifies for production

```
npm run build
```

### Lints and fixes files

```
npm run lint
```

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).

## Arknights

[【アークナイツ】素材入手場所まとめ | 素材アイテム一覧 - GameWith](https://gamewith.jp/arknights/article/show/183912)
[【アークナイツ】昇進素材のおすすめ周回場所 | 効率のいい集め方 - GameWith](https://gamewith.jp/arknights/article/show/183726)

a: "エステル素材",
b: "アケトン素材",
c: "源岩素材",
d: "異鉄素材",
e: "装置素材",
f: "糖原素材",
g: "RMA 素材",
h: "合成コール素材",
i: "砥石素材",
j: "マンガン素材",
k: "ゲル素材",
l: "熾合金素材",
m: "複合"

A. エステル素材
a-1 1 エステル原料 酯原料 mate_1 "X" 4-5
a-2 2 初級エステル 聚酸酯 mate_10 "3a1" 3-2
a-3 3 中級エステル 聚酸酯組 mate_27 "4a2" 7-4
a-4 4 上級エステル 聚酸酯塊 mate_47 "2a3+1e3+1h1" 7-4

B. アケトン素材
b-1 1 アケトン試剤 雙酮 mate_3 "X" 4-5
b-2 2 初級アケトン 酮凝集 mate_12 "3b1" 4-5
b-3 3 中級アケトン 酮凝集組 mate_29 "4b2" 3-1
b-4 4 上級アケトン 酮陣列 mate_49 "2d3+1f3+1i1" 3-1

C. 源岩素材
c-1 源岩鉱 源岩 mate_4 1 "X" 4-4
c-2 初級源岩 固源岩 mate_8 2 "3c1" 1-7
c-3 中級源岩 固源岩組 mate_25 3 "5c2" 2-4
c-4 上級源岩 提純源岩 mate_45 4 "4c3" 2-4

D. 異鉄素材
d-1 異鉄の欠片 異鐵碎片 mate_2 1 "X" S4-1
d-2 初級異鉄 異鐵 mate_9 2 "3d1" S4-1
d-3 中級異鉄 異鐵組 mate_26 3 "4d2" S4-1
d-4 上級異鉄 異鐵塊 mate_46 4 "2d3+1e3+1a3" S4-1

E. 装置素材
e-1 破損装置 破損裝置 mate_5 1 "X" 4-4
e-2 初級装置 裝置 mate_11 2 "3e1" 4-4
e-3 中級装置 全新裝置 mate_28 3 "4e2" 7-15
e-4 上級装置 改量裝置 mate_48 4 "2e3+1c3+1i1" 7-15

F. 糖原素材
f-1 ブドウ糖 代糖 mate_6 1 "X" 7-15
f-2 初級糖原 糖 mate_13 2 "3f1" 7-8
f-3 中級糖原 糖組 mate_30 3 "4f2" 4-2
f-4 上級糖原 糖聚塊 mate_50 4 "2f3+1d3+1j1" 4-2

G. RMA 素材
g-1 RMA70-12 RMA70-12 mate_44 3 "X" 4-9
g-2 RMA70-24 RMA70-24 mate_64 4 "1g1+2c3+1b3" 4-9

H. 合成コール素材
h-1 合成コール 扭轉醇 mate_41 3 "X" 4-4
h-2 上級合成コール 白馬醇 mate_61 4 "1h1+1f3+1g1" 4-4

I. 砥石素材
i-1 砥石 研磨石 mate_43 3 "X" 7-17
i-2 上級砥石 五水研磨石 mate_63 4 "1i1+1d3+1e3" 7-17

J. マンガン素材
j-1 マンガン 輕錳礦 mate_42 3 "X" 3-2
j-2 上級マンガン 三水錳礦 mate_62 4 "2j1+1a3+1h1" 3-2

K. ゲル素材
k-1 人工ゲル 凝膠 mate_90 3 "X" 7-8
k-2 融合ゲル 聚合凝膠 mate_93 3 "X" 7-8

L. 熾合金素材
l-1 熾合金 熾合金 mate_91 3 "X" S3-6
l-2 上級熾合金 熾合金塊 mate_92 3 "X" S3-6

M. 複合
m-1 D32 鋼 D32 鋼 mate_65 5 "1j2+1i2+1g2" X
m-2 ナノフレーク 雙極納米片 mate_66 5 "1e4+2h2" X
m-3 融合剤 聚合劑 mate_67 5 "1c4+1d4+1b4" X

[
0: {
...
id: (...)
...
need: (...)
own: (...)
...
recipe: (...)
},
(...)
]

e-3 own 3 need 0 "4e2"
e-4 own 0 need 1 "2e3+1c3+1i1"

1. 取出需求 > 0
   let materials_need = this.materials.filter((item) => item.need > 0);
   let materials_own = this.materials.filter((item) => item.own > 0);

2. 換算成總共需要的素材表
   let needTable = new Map();
   materials_need.forEach((material) =>
   material.recipe.split("+") // "2e3+1c3+1i1" -> ["2e3", "1c3", "1i1"]
   .forEach((recipe) => {
   let key = recipe.slice(1); // "2e3" -> "e3"
   let value = material.need \* parseInt(recipe.slice(0,1)); // "2e3" -> 2
   if( needTable.has(key) ) needTable[key] = needTable[key] + value;
   else needTable[key] = value;
   })
   console.log(needTable);

3. 素材表 \* 需求量 - 已持有

materials_need.forEach((item) =>
needTable[item.id] \* (item.need))

4. 依照換算素材稀有基準 由上往下換

5. 加總回傳一新陣列

- [Button Group | Components | BootstrapVue](https://bootstrap-vue.org/docs/components/button-group)
- [Components Basics — Vue.js](https://vuejs.org/v2/guide/components.html)
- [Object.keys, values, entries](https://javascript.info/keys-values-entries)
- [9 Ways To Remove 🗑️ Elements From A JavaScript Array 📇\[Examples\]](https://love2dev.com/blog/javascript-remove-from-array/)
- [how to have multiple inputs in a component - Get Help - Vue Forum](https://forum.vuejs.org/t/how-to-have-multiple-inputs-in-a-component/50888)
