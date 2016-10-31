
# Actions

### ability

> 使用技能（会判断技能不在CD中）

> 使用月火术： `ability(月火术)` `ability(595)`

> 使用第三个技能： `ability(#3)`

### change

> 切换宠物（会判断当前不是该宠物）

> 切换到克洛玛尼斯： `change(克洛玛尼斯)` `change(1152)`

> 切换到第二个宠物： `change(#2)`

> 切换到下一个宠物： `change(next)`

### quit

> 退出战斗（认输）

### standby

> 待命

### if/endif

> 分支语法：在if和endif之间的脚本只在if命令成立时执行
> if和endif必须成对出现

### test

> 测试： `test(HelloWorld)` `test(这个条件命中了)`

# Conditions

## Target

> 目标为一个条件的参数，完整写为 `Owner(Pet)`, `Pet` 可忽略，忽略时为当前宠物

### self

> 自己

> 自已的当前宠物： `self`

> 自己的虚空精灵龙： `self(虚空精灵龙)` `self(557)`

> 自己的第一个宠物： `self(#1)`

### ally

> 同 `self`

### enemy

> 敌方，写法与`self`一样

## Function

> 条件函数分为两类：
>
> 1. Compare 比较类
>
>   > 语法： `Owner(Pet).Function operator value`
>   >
>   > 比较类需要指定运算符及比较值
>   >
>   > 运算符(operator)： `=` `!=` `>` `>=` `<` `<=`
>
> 2. Boolean 布尔类
>
>   > 语法： `[operator]Owner(Pet).Function`
>   >
>   > 布尔类只有一个单目运算符`!` 表示逻辑非
>
> 有些条件函数需要指定参数

### dead (Boolean)

> 判断目标是否死亡

> `self.dead` `!enemy(#1).dead`

### hp (Compare)


> 判断目标血量
>
> 自己的第一个宠物血量小于100： `self(#1).hp < 100`

### hp.full (Boolean)

> 判断目标血量是否满
>
> 敌人的当前宠物是否满血： `enemy.hp.full`

### hpp (Compare)

> 判断血量百分比
>
> 自己的克洛玛尼斯的血量是否大于50%： `self(克洛玛尼斯).hpp > 50`

### aura.exists (Boolean)

> 判断光环（Buff，Debuff）是否存在
>
> 自己的当前宠物是否昏迷： `self.aura(昏迷).exists`

### aura.duration (Compare)

> 判断光环的剩余轮数
>
> 敌方当前宠物的黑爪是否大于或等于1轮： `enemy.aura(黑爪).duration >= 1`

### weather (Boolean)

> 判断当前天气
>
> 当前天气是否月光 `weather(月光)` `!weather(奥术之风)`

### weather.duration (Compare)

> 判断当前天气剩余轮数
>
> 当前天气是否是月光并轮数小于3： `weather(月光).duration < 3`

### active (Boolean)

> 判断当前激活宠物
>
> 自己当前宠物是否是克洛玛尼斯： `self(克洛玛尼斯).active`

### ability.usable (Boolean)

> 技能是否可用
>
> 敌方当前宠物技能钻地是否可用： `enemy.ability(钻地).usable`

### ability.duration (Compare)

> 技能冷却剩余轮数
>
> 自己的虚空精灵龙月火术冷却剩余小于或等于一轮： `self(虚空精灵龙).ability(月火术).duration <= 1`

### round (Boolean)

> 判断轮数
>
> 1. 不指定目标时为战斗总轮数
>
> 2. 指定目标时为当前宠物上场第几轮
>
> `round = 1` `self.round < 3` `enemy.round = 1`
