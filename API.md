
# Actions

### ability

> **使用技能**
>
> 使用技能的隐含条件：
> 1. 技能在技能栏里
> 2. 技能可用
>
> 使用技能可以用名称/id/序号进行定位
>
> 使用月火术： `ability(月火术)`
>
> 使用月火术(id)： `ability(595)`
>
> 使用第三个技能： `ability(#3)`

### change

> **切换宠物**
>
> 切换宠物的隐含条件
> 1. 宠物活着
> 2. 当前宠物不是定位到的宠物
>
> 切换宠物可以用名称/id/序号进行定位
>
> 使用名称/id定位宠物的规则
>
> 1. 当前宠物是否符合
>
> 2. 从1-3查找宠物是否符合
>
> 切换到克洛玛尼斯： `change(克洛玛尼斯)`
>
> 切换到克洛玛尼斯(id)： `change(1152)`

> 切换到第二个宠物： `change(#2)`

> 切换到下一个宠物： `change(next)`

### quit

> **退出战斗（认输）**

### standby

> **待命**

### if/endif

> **分支语法**
>
>在if和endif之间的脚本只在if命令成立时执行
>
> if和endif必须成对出现

### test

> **测试**
>
> `test(HelloWorld)` `test(这个条件命中了)`

# Conditions

## Target

> 目标为一个条件(Condition)的参数，完整y语法为 `Owner(Pet)`, `Pet` 可忽略，忽略时为当前宠物
>
> 可以用名称/id/序号进行定位
>
> 使用名称/id定位宠物的规则
>
> 1. 当前宠物是否符合
>
> 2. 从1-3查找宠物是否符合

### self

> **自己**
>
> 自已的当前宠物： `self`
>
> 自己的虚空精灵龙： `self(虚空精灵龙)` `self(557)`
>
> 自己的第一个宠物： `self(#1)`

### ally

> 同 `self`

### enemy

> **敌方**
>
> 写法与`self`一样

## Function

> 条件函数分为两类：
>
> 1. Boolean 布尔
>
>   > 语法： `[operator]Owner(Pet).Function`
>   >
>   > 运算符(operator)： `!`
>
> 2. Compare 比较
>
>   > 语法： `Owner(Pet).Function operator value`
>   >
>   > 比较类需要指定运算符及比较值
>   >
>   > 运算符(operator)： `=` `!=` `>` `>=` `<` `<=` `~` `!~`
>
> 3. Equality 相等性
>
>   > 语法：与Compare相同，但只能进行相等性运算
>   >
>   > 运算符(operator)： `=` `!=` `~` `!~`
>
> 有些条件函数需要指定参数
>
> 运算符
>
> 1. `=` ：等于
> 2. `!=`：不等于
> 3. `>` ：大于
> 4. `>=`：大于或等于
> 5. `<` ：小于
> 6. `<=`：小于或等于
> 7. `~` ：包含于（类似python的 `in`）
> 8. `!~`：不包含于（类似python的 `not in`）
>
> `~`的value指定多个值，用`,`分开，只要一个符合就返回true `self.type ~ 飞行,小动物`
>
> `!~`与`~`相反

### dead (Boolean)

> **判断目标是否死亡**

> `self.dead` `!enemy(#1).dead`

### hp (Compare)


> **判断目标血量**
>
> 自己的第一个宠物血量小于100： `self(#1).hp < 100`

### hp.full (Boolean)

> **判断目标血量是否满**
>
> 敌人的当前宠物是否满血： `enemy.hp.full`

### hpp (Compare)

> **判断血量百分比**
>
> 自己的克洛玛尼斯的血量是否大于50%： `self(克洛玛尼斯).hpp > 50`

### aura.exists (Boolean)

> **判断光环（Buff，Debuff）是否存在**
>
> 自己的当前宠物是否昏迷： `self.aura(昏迷).exists`

### aura.duration (Compare)

> **判断光环的剩余轮数**
>
> 敌方当前宠物的黑爪是否大于或等于1轮： `enemy.aura(黑爪).duration >= 1`

### weather (Boolean)

> **判断当前天气**
>
> 当前天气是否月光 `weather(月光)` `!weather(奥术之风)`

### weather.duration (Compare)

> **判断当前天气剩余轮数**
>
> 当前天气是否是月光并轮数小于3： `weather(月光).duration < 3`

### active (Boolean)

> **判断当前激活宠物**
>
> 自己当前宠物是否是克洛玛尼斯： `self(克洛玛尼斯).active`

### ability.usable (Boolean)

> **技能是否可用**
>
> 敌方当前宠物技能钻地是否可用： `enemy.ability(钻地).usable`

### ability.duration (Compare)

> **技能冷却剩余轮数**
>
> 自己的虚空精灵龙月火术冷却剩余小于或等于一轮： `self(虚空精灵龙).ability(月火术).duration <= 1`

### ability.strong (Boolean)

> **技能是否重击**
>
> `self.ability(奥术冲击).strong`

### ability.weak (Boolean)

> **技能是否轻击**
>
> `enemy.ability(#1).weak`

### ability.type (Equality)

> **技能种类**
>
> `self.ability(#1) = 魔法` `self.ability(#3) !~ 魔法,亡灵`

### round (Boolean)

> **判断轮数**
>
> 1. 不指定目标时为战斗总轮数
>
> 2. 指定目标时为当前宠物上场第几轮
>
> `round = 1` `self.round < 3` `enemy.round = 1`

### played (Boolean)

> **判断宠物是否上过场**
>
> 双方的第一个宠物一定是上过场的
>
> `self(#3).played` `!enemy(泰莉).played`


### speed (Compare)

> **判断宠物的速度**
>
> `self.speed < 292`

### speed.fast (Boolean)

> **判断目标是否更快**
>
> `enemy.speed.fast`

### speed.slow (Boolean)

> **判断目标是否更慢**
>
> `self.speed.slow`

### level (Compare)

> **判断目标等级**
>
> `self.level < 25`

### level.max (Boolean)

> **判断目标是否满级**
>
> `self(#3).level.max`

### power (Compare)

> **判断目标攻击**
>
> `self.power > 100`

### type (Equality)

> **判断宠物类型**
>
> `self(#2).type = 飞行` `enemy(#2) ~ 飞行,亡灵` `self.type = 2`

### quality (Compare)

> **判断宠物质量**
>
> `self.quality > 弱小` `self.quality = 4`

## Type

> 1 = 人型
>
> 2 = 龙类
>
> 3 = 飞行
>
> 4 = 亡灵
>
> 5 = 小动物
>
> 6 = 魔法
>
> 7 = 元素
>
> 8 = 野兽
>
> 9 = 水栖
>
> 10 = 机械

## Quality

> 1 = 弱小
>
> 2 = 普通
>
> 3 = 优秀
>
> 4 = 精良
>
> 5 = 史诗
>
> 6 = 传奇
