# 帅宝行名车 · 车源网站

## 项目是什么

西安帅宝行名车的二手车源展示网站，部署在 GitHub Pages：
**https://s2846610867.github.io/cars/**

## 文件结构

| 文件/目录 | 说明 |
|-----------|------|
| `index.html` | 客户看车页，从 cars.json fetch 数据渲染 |
| `admin.html` | 后台管理页，有密码保护，通过 GitHub API 读写 cars.json |
| `cars.json` | 车源数据，唯一数据源 |
| `pics/` | 车辆实拍图片，路径形如 `pics/xxx.jpg` |

### 四个销售的 ?s= 参数链接

| 参数 | 姓名 | 备注 |
|------|------|------|
| （无参数） | 老板 | 默认 |
| `?s=wuxuan` | 武轩 | |
| `?s=yijie` | 毅杰 | |
| `?s=chengjuan` | 程娟 | |

## 数据流

- 客户页（index.html）：页面加载时 `fetch('./cars.json')`，只读
- 后台（admin.html）：通过 GitHub Contents API 读写 cars.json，保存时同步上传图片到 `pics/`

## cars.json 车辆字段

```
id        唯一字符串，如 "A01"
name      车型全称
year      上牌时间，字符串，如 "2019年9月"
km        里程，纯数字字符串，如 "6.5"
disp      排量，如 "2.0T"
gear      变速箱，自由文本，如 "9挡手自一体"
transfer  过户次数，数字字符串，如 "1"
price     价格，纯数字字符串（万元），如 "15.38"
emission  排放标准，如 "国六"
color     车身颜色，如 "黑色"
status    在售=on / 已订=rsv / 已售=sold
note      车况描述，中文全角标点
photos    图片路径数组，第一张为封面，如 ["pics/glc-01.jpg", ...]
```

## 工作铁律

1. **后台保存过车之后、再改任何文件前，必须先 `git pull`**——后台直接写远端，本地可能比远端旧，不 pull 直接推会冲突。
2. **改动 index.html 前先给用户看方案，确认后再动手，不要直接推。**
3. 每次推前确认 `git status` 干净，没有漏掉的文件。

## 已知注意点

- `cars.json` 里文字描述使用中文全角标点（，。：）
- 图片路径不带 `./` 前缀，形如 `pics/glc-01.jpg`
- admin.html 的密码保护用 sessionStorage，关闭标签页后失效
- index.html 里 `mapCar()` 负责把 cars.json 字段转换成页面内部格式，新增字段需同步更新这个函数
