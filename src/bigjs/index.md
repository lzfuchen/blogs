
# big.js

## 属性

DP
默认值:20
涉及除法的运算结果的最大小数位数，它只与div和sqrt方法有关，pow指数为负时的方法

RM
范围 0 | 1 | 2 | 3
默认值：1（四舍五入）
Big.roundDown === 0 舍去
Big.roundHalfUp === 1 四舍五入
Big.roundHalfEven === 2 ??
Big.roundUp === 3 进一

NE
默认值：7
负指数值等于和低于它 toString返回指数表示法

PE
默认值：21
指数表示法来表示21位及以上的正指数

## 方法

abs，取绝对值。
cmp，compare的缩写，即比较函数。
div，除法。
eq，equal的缩写，即相等比较。
gt，大于。
gte，小于等于，e表示equal。
lt，小于。
lte，小于等于，e表示equal。
minus，减法。
mod，取余。
plus，加法。
pow，次方。
prec，按精度舍入，参数表示整体位数。
round，按精度舍入，参数表示小数点后位数。
sqrt，开方。
times，乘法。
toExponential，转化为科学计数法，参数代表精度位数。
toFied，补全位数，参数代表小数点后位数。
toJSON和toString，转化为字符串。
toPrecision，按指定有效位数展示，参数为有效位数。
toNumber，转化为JavaScript中number类型。
valueOf，包含负号（如果为负数或者-0）的字符串

