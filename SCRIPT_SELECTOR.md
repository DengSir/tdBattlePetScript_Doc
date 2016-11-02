# 创建一个脚本选择器

> 你可以为本插件定制一个脚本选择器

## Plugin 对象

`local Plugin = tdBattlePetScript:NewPlugin('You plugin name', [... Ace3 Embeds])`

`Plugin` 是一个 `AceAddon` 对象，我定义了一些方法

### 方法

`Plugin:SetPluginTitle(title)`

> 设置脚本选择器标题

`Plugin:SetPluginIcon(texture)`

> 设置脚本选择器图标

`Plugin:SetPluginNotes(notes)`

> 设置脚本选择器说明

`name = Plugin:GetPluginName()`

> 获取脚本选择器名称

`title = Plugin:GetPluginTitle()`

> 获取脚本选择器标题

`texture = Plugin:GetPluginIcon()`

> 获取脚本选择器图标

`notes = Plugin:GetPluginNotes()`

> 获取脚本选择器说明

`Plugin:EnableWithAddon(addon)`

> 设置脚本选择器依赖的插件，脚本选择器将在该插件加载是被加载

`Plugin:OpenScriptEditor(key, script_name)`

> 打开脚本编辑器

`script_name = Plugin:AllocName()`

> 自动分配脚本名称：新建脚本 1，2，3 ...

`script_object = Plugin:GetScript(key)`

> 获取脚本对象

`Plugin:RemoveScript(key)`

> 移除脚本

`Plugin:AddScript(key, script_object)`

> 添加脚本

`iter = Plugin:IterateScripts()`

> 遍历脚本 `for key, script_object in Plugin:IterateScripts() do end`

### 重载

`key = Plugin:GetCurrentKey()`

> 进入小宠物战斗时调用，返回key或者nil，插件会调用 `Plugin:GetScript(key)`
>
> 查找脚本，如果找到，这个脚本将可以被用户选择
>
> 这是一个核心API

`Plugin:OnTooltipFormatting(tip, key)`

> 格式化鼠标提示时调用

`data = Plugin:OnExport(key)`

> 导出脚本时调用
>
> 如果你的脚本选择器服务的插件有导入导出功能，可以通过重载这个方法，
>
> 返回key所对应的数据，插件导出脚本时会同时导出扩展数据

`Plugin:OnImport(data)`

> 导入脚本时调用
>
> 玩家导入脚本时，如果勾选“继续导入扩展信息”，将调用该方法


## Script 对象

`name = Script:GetName()`

> 获取脚本名称

`author = Script:GetAuthor()`

> 获取脚本作者

`notes = Script:GetNotes()`

> 获取脚本说明

`code = Script:GetCode()`

> 获取脚本代码

`key = Script:GetKey()`

> 获取脚本key

`plugin = Script:GetPlugin()`

> 获取脚本的plugin对象

`script_builded = Script:GetScript()`

> 获取编译好的脚本，基本用不到

`db = Script:GetDB()`

> 获取db，基本用不到

`Script:SetName(name)`

> 设置脚本名称

`Script:SetAuthor(author)`

> 设置脚本作者

`Script:SetNotes(notes)`

> 设置脚本说明

`success = Script:SetCode(code)`

> 设置脚本代码，尝试编译代码

## Demo

```lua
local Addon = tdBattlePetScript:NewPlugin('Rematch', 'AceEvent-3.0', 'AceHook-3.0')

local teamMenu = {
    text = L.WRITE_SCRIPT,
    func = function(_, key, ...)
        Addon:OpenScriptEditor(key, Rematch:GetTeamTitle(key))
    end
}

function Addon:OnInitialize()
    self:EnableWithAddon('Rematch')
    self:SetPluginTitle(L.TITLE)
    self:SetPluginNotes(L.NOTES)
    self:SetPluginIcon([[Interface\Icons\PetJournalPortrait]])
end

function Addon:OnEnable()
    tinsert(Rematch:GetMenu('TeamMenu'), 6, teamMenu)

    self.teamKey = nil
    self:Hook(RematchDialog, 'AcceptOnClick', function(...)
        if RematchDialog.dialogName ~= 'DeleteTeam' then
            return
        end
        if self.teamKey then
            self:RemoveScript(self.teamKey)
        end
    end)
    self:Hook(RematchDialog, 'FillTeam', function(_, _, team)
        if RematchDialog.dialogName ~= 'DeleteTeam' then
            return
        end
        for key, v in pairs(RematchSaved) do
            if team == v then
                self.teamKey = key
                return
            end
        end
    end)

    self:HookScript(RematchJournal, 'OnShow', function(self)
        CollectionsJournal:SetAttribute('UIPanelLayout-width', 870)
        UpdateUIPanelPositions()
    end)
    self:HookScript(RematchJournal, 'OnHide', function(self)
        CollectionsJournal:SetAttribute('UIPanelLayout-width', 710)
        UpdateUIPanelPositions()
    end)
end

function Addon:OnDisable()
    tDeleteItem(Rematch:GetMenu('TeamMenu'), teamMenu)
end

function Addon:GetCurrentKey()
    return RematchSettings.loadedTeam
end

function Addon:GetPetTip(id)
    if not id then
        return ' '
    end
    local _, customName, _, _, _, _, _, name, icon, petType = C_PetJournal.GetPetInfoByPetID(id)
    if not name then
        return ' '
    end
    local health, maxHealth, power, speed, rarity = C_PetJournal.GetPetStats(id)

    return format('|T%s:20|t %s%s|r', icon, ITEM_QUALITY_COLORS[rarity-1].hex, customName or name)
end

function Addon:OnTooltipFormatting(tip, key)
    local saved = RematchSaved[key]
    if not saved then
        tip:AddLine('当前没有队伍匹配到该脚本。', RED_FONT_COLOR:GetRGB())
    else
        tip:AddLine('队伍：' .. Rematch:GetTeamTitle(key), GREEN_FONT_COLOR:GetRGB())
        tip:AddLine(' ')

        for i, v in ipairs(saved) do
            if v[1] ~= 0 then
                tip:AddLine(self:GetPetTip(v[1]), HIGHLIGHT_FONT_COLOR:GetRGB())
            else
                tip:AddLine(format([[|TInterface\AddOns\Rematch\Textures\levelingicon:20|t %s]], L.LEVELING_FIELD), HIGHLIGHT_FONT_COLOR:GetRGB())
            end
        end
    end
end

function Addon:OnExport(key)
    if RematchSaved[key] then
        Rematch:SetSideline(key, RematchSaved[key])
        return Rematch:ConvertSidelineToString()
    end
end

function Addon:OnImport(data)
    Rematch:ShowImportDialog()
    RematchDialog.MultiLine.EditBox:SetText(data)
end

```
