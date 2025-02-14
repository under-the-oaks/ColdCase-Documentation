# Code Authoring

The decision to remove `@author` tags from source code is a debated issue in open source communities.
While some projects, such as Apache, enforce their removal to emphasize collective contribution over individual recognition, this can diminish incentives for developers who seek acknowledgment for their work.
As [Alexander Saint Croix argued in 2004](https://web.archive.org/web/20150226035235/www.theinquirer.net/inquirer/news/1037207/apache-enforces-the-removal-of-author-tags), stripping authorship from source code can erase contributors from history, reducing the visibility of their efforts and potentially discouraging participation.

Nevertheless, we believe such tags are often unwanted noise, as API users typically do not care which specific individual wrote a given part of the code.
Maintaining a clean codebase that highlights collective ownership is our priority.
At the same time, we are required—due to external necessity—to list authors.
Therefore, rather than placing these acknowledgments in the source code, we have opted to provide them here in our documentation.

## Contributors

- <a href="https://github.com/Danmyrer">
    <img src="https://github.com/Danmyrer.png" width="20" alt="Danmyrer"/>
    Danmyrer
</a> **(D)**
- <a href="https://github.com/MaxRBecker">
    <img src="https://github.com/MaxRBecker.png" width="20" alt="MaxRBecker"/>
    MaxRBecker
</a> **(B)**
- <a href="https://github.com/Toninskyy">
    <img src="https://github.com/Toninskyy.png" width="20" alt="Toninskyy"/>
    Toninskyy
</a> **(T)**
- <a href="https://github.com/jean874">
    <img src="https://github.com/jean874.png" width="20" alt="jean874"/>
    jean874
</a> **(JL)**
- <a href="https://github.com/JoniBFB">
    <img src="https://github.com/JoniBFB.png" width="20" alt="JoniBFB"/>
    JoniBFB
</a> **(JC)**
- <a href="https://github.com/Yassine091">
    <img src="https://github.com/Yassine091.png" width="20" alt="Yassine091"/>
    Yassine091
</a> **(Y)**

## Author Reference

<table>
<tr><td>Reference</td><td>Authors</td><td>Co-Authors</td></tr>
<tr><td><code>tech.underoaks.coldcase.game.Direction</code></td><td>D</td><td>M</td></tr>
<tr><td><code>tech.underoaks.coldcase.game.GameController</code></td><td>D</td><td>JL, M</td></tr>
<tr><td><code>tech.underoaks.coldcase.game.Interaction</code></td><td>M</td><td>D</td></tr>
<tr><td><code>tech.underoaks.coldcase.game.LevelManager</code></td><td>JL</td><td>M</td></tr>
<tr><td><code>tech.underoaks.coldcase.game.Levels</code></td><td>JL</td><td>Y</td></tr>
<tr><td><code>tech.underoaks.coldcase.game.PlayerController</code></td><td>M, T</td><td>JL, JC</td></tr>
<tr><td><code>tech.underoaks.coldcase.game.TextureController</code></td><td>M, D</td><td>JL, JC, T</td></tr>
<tr><td><code>tech.underoaks.coldcase.game.TextureFactory</code></td><td>D</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.game.UITextureController</code></td><td>M</td><td>JL</td></tr>
<tr><td><code>tech.underoaks.coldcase.remote.Message</code></td><td>JL</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.remote.Messages</code></td><td>JL</td><td>M</td></tr>
<tr><td><code>tech.underoaks.coldcase.remote.RemoteGameController</code></td><td>JL</td><td>D</td></tr>
<tr><td><code>tech.underoaks.coldcase.remote.WebSocketClient</code></td><td>JL</td><td>D</td></tr>
<tr><td><code>tech.underoaks.coldcase.remote.WebSocketMessagesManager</code></td><td>JL</td><td>M</td></tr>
<tr><td><code>tech.underoaks.coldcase.stages.actors.InventoryActor</code></td><td>M</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.stages.actors.MapActor</code></td><td>M, JL</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.stages.actors.PauseMenu</code></td><td>M, JL</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.stages.AbstractStage</code></td><td>M</td><td>JL</td></tr>
<tr><td><code>tech.underoaks.coldcase.stages.GameStage</code></td><td>M</td><td>JL</td></tr>
<tr><td><code>tech.underoaks.coldcase.stages.HostStage</code></td><td>M</td><td>JL</td></tr>
<tr><td><code>tech.underoaks.coldcase.stages.JoinStage</code></td><td>M</td><td>JL</td></tr>
<tr><td><code>tech.underoaks.coldcase.stages.MainMenuStage</code></td><td>M</td><td>JL</td></tr>
<tr><td><code>tech.underoaks.coldcase.stages.SettingsStage</code></td><td>M</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.stages.StageManager</code></td><td>M</td><td>JL</td></tr>
<tr><td><code>tech.underoaks.coldcase.stages.Stages</code></td><td>M, JL</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tileContent.Door</code></td><td>M</td><td>D, JC</td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tileContent.Door_Trigger</code></td><td>T, M</td><td>D</td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tileContent.GloveItem</code></td><td>JC</td><td>M</td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tileContent.GoalObject</code></td><td>Y, JC</td><td>JL, T</td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tileContent.Hole</code></td><td>JL</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tileContent.InvisibleWall</code></td><td>M</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tileContent.ItemObject</code></td><td>JL, JC</td><td>M</td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tileContent.MovableBlock</code></td><td>JC, M</td><td>D</td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tileContent.MovableBlockTranscendent</code></td><td>D</td><td>JL</td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tileContent.Player</code></td><td>M, JC</td><td>T</td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tileContent.PortalObject</code></td><td>JC</td><td>D</td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tileContent.TestContent</code></td><td>D</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tileContent.TestItem</code></td><td>JL</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tileContent.TestItem02</code></td><td>JL</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tileContent.TileContent</code></td><td>D</td><td>M, JL, Y</td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tileContent.TileContents</code></td><td>D</td><td>JC, M, Y</td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tileContent.TranscendentTestBlock</code></td><td>JL, M</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tileContent.UIContentTileContent</code></td><td>M</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tileContent.UpdateTileContentException</code></td><td>D</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tileContent.VisibilityStates</code></td><td>D</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tileContent.Wall</code></td><td>Y</td><td>D</td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tiles.EmptyTile</code></td><td>D, M</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tiles.GroundTile</code></td><td>M</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tiles.Tile</code></td><td>D</td><td>JL, M, Y</td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tiles.Tiles</code></td><td>D</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.state.updates.AddTileContentUpdate</code></td><td>JL</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.state.updates.ChangeTextureUpdate</code></td><td>M</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.state.updates.EndLevelUpdate</code></td><td>JL</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.state.updates.GameStateUpdate</code></td><td>D</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.state.updates.GameStateUpdateException</code></td><td>D</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.state.updates.MoveUpdate</code></td><td>T, M</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.state.updates.PlayerPassebilityUpdate</code></td><td>M</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.state.updates.RemoveTileContentUpdate</code></td><td>JL</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.state.updates.TestUpdate</code></td><td>D</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.state.updates.UpdateTypes</code></td><td>D</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.state.InteractionChain</code></td><td>D</td><td>M</td></tr>
<tr><td><code>tech.underoaks.coldcase.state.Map</code></td><td>JL, M, D</td><td>JC</td></tr>
<tr><td><code>tech.underoaks.coldcase.state.Snapshot</code></td><td>D</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.Main</code></td><td>M</td><td>T, JC, D, JL</td></tr>
<tr><td><code>tech.underoaks.coldcase.MapGenerator</code></td><td>M</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.data.tileContent.trigger.ItemObjectTest</code></td><td>JC</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.game.GameControllerTest</code></td><td>D</td><td>Y, JL</td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tileContent.DoorTest</code></td><td></td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tileContent.GloveItemTest</code></td><td>D</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tileContent.MovableBlockTest</code></td><td>T</td><td>D</td></tr>
<tr><td><code>tech.underoaks.coldcase.state.tileContent.TileContentsTest</code></td><td>D</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.AddTileContentUpdateTest</code></td><td>JL</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.HeadlessApplicationListener</code></td><td>JC, M</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.HoleTest</code></td><td>JL</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.MainTest</code></td><td></td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.MapGeneratorTest</code></td><td>M</td><td>JL</td></tr>
<tr><td><code>tech.underoaks.coldcase.MapTest</code></td><td>JC</td><td>D, M</td></tr>
<tr><td><code>tech.underoaks.coldcase.MoveUpdateTest</code></td><td>Y</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.PlayerTest</code></td><td>Y</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.RemoveTileContentUpdateTest</code></td><td>JL</td><td></td></tr>
<tr><td><code>tech.underoaks.coldcase.TileTest</code></td><td>JL</td><td></td></tr>
</table>
