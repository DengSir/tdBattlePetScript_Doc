
以圣光精华为例，我们使用虚空精灵龙+克洛玛尼斯，使用月火术+生命交换+嚎叫+能量涌动过关

上场需要使用技能月火术

```
ability(月火术)
```

> 每个命令一行
>
> 上述命令解释为：施放月火术

接下来我们需要给月火术加一个条件，条件为只在不是月光天气下施放月火术

```
ability(月火术) [ !weather(月光) ]
```

> 每个命令可以条件执行，条件使用`[]`括起来
>
> 上述命令解释为：当天气不是月光时，施放月火术

接下来我们给施放月火术添加另一个条件

```
ability(月火术) [ !weather(月光) & self(虚空精灵龙).ability(月火术).usable ]
```

> 多个条件使用`&`分开，表示逻辑与，没有逻辑或
>
> 上述命令解释为：当天气不是月光且月火术可用时，施放月火术
>
> 实际上不需要为技能判断是否可用，每个动作命令都会判断可行性，这里只作示例

下面我们再添加一个技能

```
ability(月火术) [ !weather(月光) & self(虚空精灵龙).ability(月火术).usable ]
ability(生命交换) [ self.ability(生命交换).usable ]
```

> 每次运行脚本，只会有一个命令被执行

下面我们将要切换宠物

```
ability(月火术) [ !weather(月光) & self.ability(月火术).usable ]
ability(生命交换) [ self.ability(生命交换).usable ]
change(克洛玛尼斯)
```

以上这个脚本为虚空精灵龙的脚本，我们现在把这个脚本限定在虚空精灵龙下

```
if [ self(虚空精灵龙).active ]
    ability(月火术) [ !weather(月光) & self.ability(月火术).usable ]
    ability(生命交换) [ self.ability(生命交换).usable ]
    change(克洛玛尼斯)
endif
```

> 使用if/endif可以把一组命令包含起来
>
> 缩进不是必须的，但这会使用脚本更清晰

我们继续补全克洛玛尼斯的脚本

```
if [ self(虚空精灵龙).active ]
    ability(月火术) [ !weather(月光) & self.ability(月火术).usable ]
    ability(生命交换) [ self.ability(生命交换).usable ]
    change(克洛玛尼斯)
endif
if [ self(克洛玛尼斯).active ]
    ability(嚎叫) [ !enemy.aura(嚎叫).exists ]
    ability(能量涌动)
endif
```

实际上这场战斗很简单，并没有任何不确定因素（几率昏迷，暴击导致己方死亡等），只需要按照月火术-生命交换-换宠-嚎叫-能量涌动这个流程来，就一定可以结束战斗，并且所有使用的技能都有CD，所以我们可以把脚本简化成：

```
ability(月火术)
ability(生命交换)
change(克洛玛尼斯)
ability(嚎叫)
ability(能量涌动)
```
