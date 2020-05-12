# 新建定居点
## 定居点的运行方式
游戏中通过两个XML文件来新增定居点,这两个文件其中一个是:定义定居点类别(藏身处,村庄,城镇,城堡等)及拥有者以及有关村镇的繁荣度,产量等相关参数.
这个文件是在游戏目录中的Modules/SandBox/ModuleData/settlements.xml.另一个在代码执行时就产生的文件是距离缓存Modules/SandBox/ModuleSata/Settlements_distance_cache.bin.

这个定义定居点的文件并不会去定义定居点外观,定义外观的文件是Modules/SandBox/SceneObj/Main_map/scene.xscene.


## 对将来SDK的描述
从其他代码来看定居点的建模很明显使用编辑器来完成的,编辑器能放置定居点亦能定义它们的外观以及产生缓存.
就目前而言,没有任何迹象表明定居点是只靠XML来建模的.


## 对距离缓存的描述
距离缓存(distance cache)的定义目前并不明确是什么.不更新这个文件的话AI照样能在添加的定居点有正常行为,依旧会招募,转移俘虏及买卖货物.同时玩家也如此.
距离缓存可能就是由AI的一些决策产生的但也不太确定.它是通过SettlementPositionScript脚本文件中的SaveSettlementDistanceCache()方法生成的.这脚本和函数其实是个目前在游戏中还没用到的类,据说它是地图编辑器衍生出来的.
这个类在SandBox.View.dll中找到.


## 如何在游戏中重载一个定居点
从沙盒模组中建立一个定居点它很可能会重载之前对这个定居点的定义.但是这些改变并不会添加(译者注:原文是append(add)写法,认为是比喻append函数那样添加到列表)到文件中,所以如果改定居点参数之类的话得全盘皆改.

先复制替换Modules/SandBox/ModuleData/settlements.xml文件到Modules/对应Mod文件夹/ModuleData/settlements.xml中, Modules/SandBox/SceneObj/Main_map(文件夹)复制替换Modules/对应Mod文件夹/SceneObj/Main_map.

如果可以的话,用对XML编辑兼容性更好的工具最好,比如Notepad++就好过记事本.
在Modules/对应Mod文件夹/submodule.xml中添加以下XML节点代码<XmlNode>
```
		<XmlNode>
			<XmlName id="Settlements" path="settlements"/>
			<IncludeGameTypes>
				<GameType value = "Campaign"/>
				<GameType value = "CampaignStoryMode"/>
			</IncludeGameTypes>
		</XmlNode> 	
```
完成后就能自动加载大地图了.

在settlements.xml文件中你可以随意自定城镇内容也可以修改现有的城镇(比如修改拥有者,初始繁荣度,产出等).
特别注意settlements.xml文件中每一个条目都有对应的id.

请确保在你mod里的 settlements.xml文件每一个条目在Main_map/scene.xscene 文件里都有对应的实体且没有重复id.

在settlements.xml新建一个城镇和两个村子的条目(即新加内容而不是全部替换)的实例[点此了解更多](https://pastebin.com/BuSbQ6x2)

注意:village_M1_1和village_M1_2这两个条目都有与之依附城镇的条目:
```
trade_bound="Settlement.town_M1" bound="Settlement.town_M1"
```
现在它们看起来毫无区别,但最好是把它们设进你想添加的城镇归属中.
 
你也可以定义村子中特产是什么:
```
village_type="VillageType.fisherman"
```
在Modules/SandBox/ModuleData/spprojects.xml文件中定义村庄特产种类.

这三种新定居点在Main_map/scene.xscene 文件中需要给与之对应的game_entity定义.条目实例[点此了解更多](https://pastebin.com/dXcKT7wf)
