# tks0002
一个合作项目


# 对话框指令

|	指令	|	用途	|	例子	|	备注	|
|	---	|	---	|	---	|	---	|
| -wtf | 无冷却无耗蓝
| -unwtf | 取消wtf模式
| -refresh | 刷新冷却和单位状态
|	myid	|	在控制台打印出自己steamID，方便复制	|	myid	|		|
|	ship NAME true	|	使关键词NAME的羁绊生效。	|	ship wuhu true	|	NAME的列表在“羁绊判断的关键词”中	|
|	ship NAME	|	使关键词NAME的羁绊失效。	|	ship wuhu	|	NAME的列表在“羁绊判断的关键词”中	|
|   hero lvlup  |   全场单位升1级   | hero lvlup | |
|   hero waveup  |   触发技能中的needwaveup   | hero waveup | |

# 轮子

## 打印变动部分

在一个修改器中的参数 `params` 实际是一个键值对的数组。因为他们是不断循环触发的，我们可以通过下面这个段代码来只打印其中变动的部分。
```lua
function modifier_example:OnTakeDamage( params)
    self.paramstable = self.paramstable or {}
    for k,v in pairs(params) do
        if  self.paramstable[k] ~= v then
            self.paramstable[k]  = v
            print("OnHealReceived", k, v, IsServer())
        end
    end
end
```

## 改变伤害的马甲单位
比如我们需要释放电属性的伤害，但是单位本身的攻击属性不是电属性，那么我们可以创建一个马甲单位制造这个伤害。
```lua
local dummy = CreateUnitByName( "npc_damage_dummy", Vector(0,0,0), false, parent, parent, parent:GetTeamNumber() )
dummy.attack_type  = "electrical"
dummy:AddNewModifier(dummy, nil, 'modifier_kill', {duration = 0.1} )
```
- 注意在 `ApplyDamage` 的时候，attacker不再是 `caster` 或者 `parent` ,而是我们上面创建的 `dummy` 。
- `dummy` . `attack_type` 要赋值为实际需要的伤害属性。
- 第三行添加的修改器是在`0.1`秒后自杀。如果是个由他发出的持续伤害，那么就吧`0.1`改为实际的持续时间。


## 锁定值范围的小技巧
`Clamp`也是很好用的方法，( 浮动值, 最小值 , 最大值 )
```lua
Clamp( 2, 0, 10) == 2
Clamp(12, 0, 10) == 10
Clamp(-5, 0, 10) == 0
```