[lag]{简体中文}
[简体中文]{https://github.com/qq292/UE.Documentation/blob/main/PaperZD/PaperZD_zh.md}
[English]{https://github.com/qq292/UE.Documentation/blob/main/PaperZD/PaperZD_en.md}

# 首页

欢迎来到 PaperZD 知识库！

# 01. 概述

PaperZD 的出现是为了回应 Unreal Engine 对 2D 游戏支持不足的问题，它旨在帮助用户像常规 3D 开发中那样，通过绑定逻辑来轻松创建复杂的动画角色，同时将体验调整为更好地服务于 2D 游戏开发。

该插件自带一套"动画蓝图（Animation Blueprint）"实现（配合自定义蓝图编译器），拥有自己的动画图（Animation Graph），可以为每个角色或实体提供易于理解的动画逻辑。

![1-1](https://drfs.ctcontents.com/file/12489212/17569905074320/2775ea/UE.Documentation/PaperZD/image/1-1.png)

同时还支持专用的"动画通知（Animation Notifies）"，可以添加到需要它的每个动画序列中。

![1-2](https://drfs.ctcontents.com/file/12489212/17569905074357/032860/UE.Documentation/PaperZD/image/1-2.png)

除此之外，还提供了大量其他 2D 专属功能，帮助用户创建丰富而复杂的角色，无需自己管理时间轴等繁琐工作。

PaperZD 还允许向系统中添加"扩展（Extensions）"，从而可以使用其他类型的动画源。这些扩展可以解锁对骨骼 2D 动画源的管理能力，如"Spine2D"、"DragonBones"或"Creature2D"（或用户自定义的任何其他源）。目前仅支持"Spine2D"和"Paper2D"。

当前版本的插件（称为 V2.0）大幅重写了旧架构，旨在打造一款既能服务大型项目也能服务小型项目的工具，成为任何旨在 Unreal Engine 中开发 2D 游戏的用户的必备软件。

# 02. 安装

### 如何安装

如果通过 Marketplace/FAB 下载，安装已经完成，只需通过"插件（Plugins）"菜单启用该插件即可（需要重启引擎）。

![2-1](https://drfs.ctcontents.com/file/12489212/17569905080481/5a9c05/UE.Documentation/PaperZD/image/2-1.png)

要将 PaperZD 作为"项目插件（Project Plugin）"安装，请遵循与任何插件相同的步骤，只需将插件添加到项目文件夹内的"Plugins"文件夹中即可。

扩展的安装可以通过添加"插件扩展（Plugin Extensions）"来完成，这些扩展插件赋予 PaperZD 管理 Paper2D 以外其他类型动画源的能力。由于这些工具的性质是引擎外部的，因此这些扩展无法在 Marketplace 中提供，必须单独安装（更多信息请参见"添加对自定义动画源的支持"部分 TODO: Link）。

### 重要说明

#### "仅蓝图（Blueprint Only）"项目的注意事项

> ℹ️ 对于使用 C++ 插件的项目，用户需要能够编译项目。这要求即使是在仅蓝图项目中，也必须安装有效的 VisualStudio 和工具链。

#### C++ 用户的注意事项：

> ℹ️ 如果你需要在游戏项目中导入和使用 PaperZD 类（如继承 PaperZDCharacter 或在 C++ 中创建自定义通知），则需要在项目的"PublicDependencyModules"中添加"PaperZD"模块，否则会触发链接器错误。

#### v1.0 用户的注意事项：

> ℹ️ 每当系统首次加载使用之前版本（v.1.4.3）创建的 AnimationBlueprint 时，它会自动更新该蓝图并为其创建一个"Animation Source"，同时将每个 AnimSequence 重新指向该对象作为父级。由于 Unreal Engine 加载协议的性质，我们无法强制包保存，只能标记为脏状态，需要用户确保它们被正确保存。

# 03. 设置动画对象

## 创建 **AnimationSource**

创建新角色时的第一步是创建所谓的**动画源（Animation Source）**。该对象作为动画的源头，**AnimationBlueprint** 可以使用它来渲染角色并控制其播放。

PaperZD 默认自带一个 **"翻页动画源（Flipbook Animation Source"**，它让系统知道如何管理和渲染翻页动画角色，但也可以创建其他不同的源，它们可以具有其他功能（如基于骨骼从而可以选择启用混合支持）。首先，在内容浏览器中右键单击，进入 **PaperZetaD** 部分并创建一个新的 **Animation Source**：

![3-1](https://drfs.ctcontents.com/file/12489212/17569905132288/8e2cd9/UE.Documentation/PaperZD/image/3-1.png)

> ℹ️ 注意：如果除了 Paper2D Flipbook 之外没有其他支持的 Animation Source，系统将直接创建一个新的资源，否则会要求你先选择要创建哪种类型的 Animation Source。

使用 新创建的 Animation Source（我们将其命名为"MyAnimSource"），我们将开始设置角色的动画：

![3-2](https://drfs.ctcontents.com/file/12489212/17569905132289/359372/UE.Documentation/PaperZD/image/3-2.png)

## **AnimSequence** 的创建和设置

AnimSequence 是包含要渲染的**动画（Animation）**以及其中相关事件元数据（如 **AnimNotifies**）的对象。**AnimSequences** 存在于 **AnimationSource** 内部，因此在创建新的 **AnimSequence** 时需要指定一个 AnimationSource。

可以通过在内容浏览器中右键单击并从下拉菜单中选择来创建新的 **AnimSequence**（就像上一步那样），也可以在 **Animation Source Editor** 内部创建（TODO: 这是 Animation Editor 还是 Animation Blueprint Editor？不管怎样，加上链接）。

从下拉菜单创建 **AnimSequence** 时，系统会要求用户指定该序列应附加到哪个 Source。要创建的 **AnimSequence** 的类型取决于所选的 **AnimationSource**，选择 Paper2D Flipbook 的 **AnimationSource**（TODO: 检查这在 UE 中是否真的是一个东西）将为它们创建一个专门的 **AnimSequence**，用户无需担心。

![3-3](https://drfs.ctcontents.com/file/12489212/17569905132290/26b9e8/UE.Documentation/PaperZD/image/3-3.png)

![3-4](https://drfs.ctcontents.com/file/12489212/17569905132291/04809b/UE.Documentation/PaperZD/image/3-4.png)

我们将这个 AnimSequence 命名为"MyFirstAnimSequence"

![3-5](https://drfs.ctcontents.com/file/12489212/17569905132292/053259/UE.Documentation/PaperZD/image/3-5.png)

之后，双击你的 **AnimationSource** 或 **AnimSequence**，进入 **[Animation Editor](https://github.com/heavybullets/PaperZD/wiki/03.-Setting-up-the-animation-objects#the-animation-editor)**。

## 动画编辑器（The Animation Editor）

首次打开资源时，你会看到以下界面：

![3-6](https://drfs.ctcontents.com/file/12489212/17569905132293/f6c81a/UE.Documentation/PaperZD/image/3-6.png)

在左侧，资源浏览器将显示当前绑定到此 **AnimationSource** 的所有 **AnimSequences**，如果你在上一歩创建了第一个 **AnimSequence**，这里应该显示一个。

![3-7](https://drfs.ctcontents.com/file/12489212/17569905132294/336a43/UE.Documentation/PaperZD/image/3-7.png)

单击此 **AnimSequence** 将打开资源进行编辑，你应该会在屏幕底部中心看到一个新标签页出现。

![3-8](https://drfs.ctcontents.com/file/12489212/17569905132296/c14729/UE.Documentation/PaperZD/image/3-8.png)

在屏幕右侧，**Asset Details（资源详情）** 标签页也应该已填充了你的 **AnimSequence** 的相关信息。

![3-9](https://drfs.ctcontents.com/file/12489212/17569905132297/d8b5cb/UE.Documentation/PaperZD/image/3-9.png)

现在，让我们指定用于渲染此序列的 **Flipbook**。我们将使用 Paper2D 示例内容项目的动画，并选择 **Idle** 动画。选择动画后，中间的 Viewport 和 Timeline 应该会反映新创建的动画。

![3-10](https://drfs.ctcontents.com/file/12489212/17569905132298/c4ded5/UE.Documentation/PaperZD/image/3-10.png)

![3-11](https://drfs.ctcontents.com/file/12489212/17569905132299/ec37bc/UE.Documentation/PaperZD/image/3-11.png)

现在我们已经设置了一个简单的 AnimSequence，可以继续为 **walking** 行为创建另一个。我们不需要从内容浏览器创建，而是可以直接在这个编辑器中创建，点击 **AnimSequence** 列表顶部的 **+Add New** 按钮。这将创建并选择一个带有默认新名称的 **AnimSequence**。

![3-12](https://drfs.ctcontents.com/file/12489212/17569905132274/201173/UE.Documentation/PaperZD/image/3-12.png)

然后我们可以将其 Flipbook 设置为 **Walking** 动画

![3-13](https://drfs.ctcontents.com/file/12489212/17569905132275/5485e1/UE.Documentation/PaperZD/image/3-13.png)

> ℹ️ 注意：通过此方法创建的 **AnimSequences** 默认存储在名为 **AnimSequences** 的**文件夹**中，该文件夹将创建在 **与 AnimationSource 同级** 的位置。此行为可以在 **项目设置** 中的 **PaperZD Editor** 部分进行修改。

我们还可以继续为 **AnimSequences** 重命名为更易识别的名称，分别称为 **Idle** 和 **Walk**。重命名可以在 [Animation Editor](https://github.com/heavybullets/PaperZD/wiki/03.-Setting-up-the-animation-objects#the-animation-editor) 中通过单击动画并按 **F2** 来完成。

![3-14](https://drfs.ctcontents.com/file/12489212/17569905132276/7416e0/UE.Documentation/PaperZD/image/3-14.png =450x)

![3-15](https://drfs.ctcontents.com/file/12489212/17569905132278/3d9f54/UE.Documentation/PaperZD/image/3-15.png =450x)

通过这种方式，我们拥有了创建动画逻辑的骨架，虽然在这个编辑器部分我们还可以做其他事情，但我们会在后面的章节中回到这些内容。

现在，我们将通过创建一个新的 **AnimationBlueprint** 来创建我们的动画逻辑，该蓝图以我们的 **AnimationSource** 为基础。和往常一样，在内容浏览器中右键单击，选择以下选项（在提示时，选择你的 **AnimationSource** 作为基础）。

![3-16](https://drfs.ctcontents.com/file/12489212/17569905132279/db8635/UE.Documentation/PaperZD/image/3-16.png)

我们将它命名为"MyAnimBP"

![3-17](https://drfs.ctcontents.com/file/12489212/17569905132280/95a846/UE.Documentation/PaperZD/image/3-17.png)

此资源将打开"[Animation Blueprint Editor](https://github.com/heavybullets/PaperZD/wiki/03.-Setting-up-the-animation-objects#the-animation-blueprint-editor)"

## 动画蓝图编辑器（The Animation Blueprint Editor）

首次打开 **Animation Blueprint Editor** 时，你会看到一个熟悉的蓝图编辑器窗口，带有 Event Graph 和新增的 **Animation Graph**

![3-18](https://drfs.ctcontents.com/file/12489212/17569905132281/48f081/UE.Documentation/PaperZD/image/3-18.png)

双击 **Animation Graph** 可以修改蓝图渲染最终动画时将遵循的逻辑。

![3-19](https://drfs.ctcontents.com/file/12489212/17569905132282/14e5df/UE.Documentation/PaperZD/image/3-19.png)

作为一种简单的方法，我们将使用 `Select` 节点在两个已创建的动画之间切换，根据角色的速度，移动时显示 **Walk** 动画，静止时显示 Idle 动画。

![3-20](https://drfs.ctcontents.com/file/12489212/17569905132283/fc5a6f/UE.Documentation/PaperZD/image/3-20.png)

还可以使用其他节点来指定更复杂的行为，例如使用动画状态机。这些将在后面的章节中解释。（TODO: Link）

## 动画组件和 PaperZDCharacter

动画逻辑创建完成后，我们需要指定要将其与角色一起使用，为此 PaperZD 自带一个 **PaperZDAnimationComponent**，可以添加到任何类/对象上来驱动其动画。

**AnimationComponent** 需要设置以下数据以确保正确行为：

- **AnimInstanceClass**：这是你创建的 **AnimationBlueprint** 的类，你希望用它来驱动此组件的动画。
- **RenderComponent**：将渲染实际动画的 **PrimitiveComponent**，对于默认行为（使用 Paper2D），这应该是一个 **FlipbookComponent**。

> 注意：**RenderComponent** 应与你指定的 **AnimationBlueprint** 兼容。

对于默认 Paper2D 角色，PaperZD 自带一个 **PaperZDCharacter**，它已经将 **RenderComponent** 设置为主要的 **PaperFlipbook**。如果你的目标是创建一个简单的 Paper2D 角色，那么继承这个类是一个不错的选择。

为了举例说明，我们将获取一个默认的 PaperZD 角色并创建它的子类。该角色将简单地路由左右输入

![3-21](https://drfs.ctcontents.com/file/12489212/17569905132284/137935/UE.Documentation/PaperZD/image/3-21.png)

然后我们将 **AnimInstanceClass** 设置为指向我们创建的 **AnimationBlueprint**。

![3-22](https://drfs.ctcontents.com/file/12489212/17569905132285/e8c305/UE.Documentation/PaperZD/image/3-22.png)

如果我们现在点击播放，我们应该能看到动画在移动和静止时在 **Walk**/**Idle** 之间切换。

![3-23](https://drfs.ctcontents.com/file/12489212/17569905132286/4c60cd/UE.Documentation/PaperZD/image/3-23.png)

# 04. 创建动画事件和通知

要实现丰富的动画，你不仅需要能够在正确的时间和情况下正确显示动画（我们通过使用 **AnimationBlueprint** 实现了这一点），还需要能够在精确的时间点播放效果、触发事件和播放声音。

为此，PaperZD 自带了一套自己的 **AnimNotify** 系统，类似于引擎自带的系统。

打开 **Walk** 动画序列应该会显示我们之前使用的 **[Animation Editor](https://github.com/heavybullets/PaperZD/wiki/04.-Creating-animation-events-and-notifies#the-animation-editor)**，在视口下方有一个 **Timeline**，我们可以用它来设置事件。

在此时间轴上，你可以添加任意数量的 **AnimNotifies** 和 **NotifyStates**。插件自带一些默认的通知，但你始终可以创建自定义蓝图和 C++ 子类来创建自己的行为。

为了说明这一点，我们将创建一个"Source AnimNotify"，这是一种存在于 **AnimationSource** 中的 Notify 类型，可以由继承自该源的任何 **AnimationBlueprint** 实现。

在 **Timeline** 上右键单击，点击 **NewNotify**，在提示时填写名称，我们称之为 **Footstep**

![4-1](https://drfs.ctcontents.com/file/12489212/17569905140635/d82286/UE.Documentation/PaperZD/image/4-1.png)

我们将它移动到正确的位置（就在脚踩到地面的那一刻），然后通过从已创建的通知中选择来创建另一个 **Footstep** 通知。

![4-2](https://drfs.ctcontents.com/file/12489212/17569905140637/eecac6/UE.Documentation/PaperZD/image/4-2.png)

**Timeline** 应该看起来像这样：

![4-3](https://drfs.ctcontents.com/file/12489212/17569905140639/543630/UE.Documentation/PaperZD/image/4-3.png)

通常，仅仅设置通知并正确配置它就足够了，但 **SourceNotifies** 需要由其派生的 **AnimationBlueprints** 来实现。

如果我们回到我们的 **AnimationBlueprint**，只需点击右上角的选择器超链接，我们会看到 **AnimationBlueprint** 已经自动为我们创建了一个新函数。

![4-4](https://drfs.ctcontents.com/file/12489212/17569905140641/e1b124/UE.Documentation/PaperZD/image/4-4.png =450x)

**SourceNotify** 函数将在每个子 **AnimationBlueprint** 上自动创建，它们都将共享相同的命名约定：Receive Notify: [通知名称]。

我们只需双击这个函数，暂时放入一些非常简单的工作流。

![4-5](https://drfs.ctcontents.com/file/12489212/17569905140642/420fd3/UE.Documentation/PaperZD/image/4-5.png)

如果我们编译并播放，我们应该能看到行为在正确的时间被触发。

![4-6](https://drfs.ctcontents.com/file/12489212/17569905140643/e1530a/UE.Documentation/PaperZD/image/4-6.png)

# 05. 自定义 Animation Graph 的输出

**Animation Graph** 带有一组 **Animation Nodes**，用于告诉系统在特定时间应该使用哪个动画。在之前的章节中，我们使用了 `Select Animation by Bool` 来根据布尔值在两个输出之间进行选择。

需要注意的是，进入节点的 **Animation Pin**（绘制成剪纸角色样式），可以是任何 Animation 节点的输出，这些节点可以级联以创建复杂的行为并在节点之间切换。

一个示例如下：

![5-1](https://drfs.ctcontents.com/file/12489212/17569905141565/5197c9/UE.Documentation/PaperZD/image/5-1.png)

**Animation 节点的输出** 只能连接到 **一个动画引脚**，这是为了确保 **Update** 序列按预期运行而不会多次触发。如果你想多次使用同一个节点的输出（比如在多个地方使用状态机），则需要使用 `CacheAnimation` 节点，如下所示。

![5-2](https://drfs.ctcontents.com/file/12489212/17569905141545/a7f5ae/UE.Documentation/PaperZD/image/5-2.png)

![5-3](https://drfs.ctcontents.com/file/12489212/17569905141549/f7408b/UE.Documentation/PaperZD/image/5-3.png)

这个示例只是说明性的，同一个输出可以被使用任意多次，因为我们只是获取它的缓存版本并且只更新一次。

另一个值得关注的点是每个 Animation 节点都有可以更改的设置来修改它们的行为，让我们以 `Play Idle` 节点为例。

![5-4](https://drfs.ctcontents.com/file/12489212/17569905141551/35ee85/UE.Documentation/PaperZD/image/5-4.png)

你可以看到实际上可以更改不同的值，如 **PlayRate**（播放速率）、是否从开头或其他播放位置开始动画，甚至是否循环播放动画。这些变量可以通过选择右侧的复选框使其变为**动态**，这将在节点上创建一个新的 PIN，然后你可以将其绑定到 **Animation Graph** 中的动态变量和逻辑。

![5-5](https://drfs.ctcontents.com/file/12489212/17569905141552/a992ee/UE.Documentation/PaperZD/image/5-5.png)

![5-6](https://drfs.ctcontents.com/file/12489212/17569905141560/1bfeb8/UE.Documentation/PaperZD/image/5-6.png)

在这种情况下，我们可以改变 **Idle** 动画的播放速度，并在角色疲劳时让它运行更慢，甚至完全暂停。

有多个节点和设置，以确保动画以最少的代码量按预期工作。

# 06. 状态机

在之前的章节中，我们创建了一个非常简单的 2 状态动画角色，它仅根据速度在动画之间切换。虽然这很容易理解，但它不能很好地扩展，角色越复杂，动画逻辑就越复杂。

对于这些情况，PaperZD 自带一个针对 2D 使用优化的自定义 `StateMachine` 节点。让我们改进我们的行走角色，并将逻辑迁移到新的状态机中。

首先，我们将在 **Animation Graph** 上创建一个新的 `StateMachine` 节点，为了本示例的目的，我们将其命名为 **Locomotion**。

![6-1](https://drfs.ctcontents.com/file/12489212/17569905142762/54c58d/UE.Documentation/PaperZD/image/6-1.png)

![6-2](https://drfs.ctcontents.com/file/12489212/17569905142763/030ee1/UE.Documentation/PaperZD/image/6-2.png)

> ℹ️ 注意：与其他 Animation Node 一样，`StateMachine` 可以输出到任何其他节点，并像我们在上一章中那样进行级联。

双击 `StateMachine` 允许我们修改其底层图，在其中我们会找到一个 `Out` 节点，它作为状态机的根，从这里我们可以开始构建逻辑。

![6-3](https://drfs.ctcontents.com/file/12489212/17569905142765/1ca12d/UE.Documentation/PaperZD/image/6-3.png)

我们将首先重新创建之前章节的逻辑，为此我们从 `Out` 节点拖拽并放置一个 **State**，这将是我们的 **Idle**。

![6-4](https://drfs.ctcontents.com/file/12489212/17569905142767/c8dac3/UE.Documentation/PaperZD/image/6-4.png)

![6-5](https://drfs.ctcontents.com/file/12489212/17569905142769/8e311f/UE.Documentation/PaperZD/image/6-5.png)

双击该状态将带我们进入状态的内部动画图，它的工作原理与我们一直使用的根图相同；在这里我们只需放置一个 `Play Idle` 节点。

![6-6](https://drfs.ctcontents.com/file/12489212/17569905142770/4224b9/UE.Documentation/PaperZD/image/6-6.png)

之后，退出此 **Animation graph** 回到 **State Machine**，我们从状态的边缘拖拽并创建另一个 State，我们称之为 **Walking**。

![6-7](https://drfs.ctcontents.com/file/12489212/17569905142749/615f35/UE.Documentation/PaperZD/image/6-7.png)

正如你所见，还创建了一个中间节点，这是 Transition，它将控制从 Idle 到 Walking 的状态转换逻辑。我们将双击这里并放入与之前示例相同的逻辑。

![6-8](https://drfs.ctcontents.com/file/12489212/17569905142750/f0664b/UE.Documentation/PaperZD/image/6-8.png)

通过这种方式，当此条件评估为 `true` 时，`Idle` 将切换到 `Walking`。我们还需要另一个条件来返回 `Idle`，所以我们从 `Walking` 节点的边缘拖拽到 `Idle` 并放置反向条件，同时在状态的内部 **Animation graph** 上放置一个 `Play Walk` 节点。

![6-9](https://drfs.ctcontents.com/file/12489212/17569905142752/e73300/UE.Documentation/PaperZD/image/6-9.png)

![6-10](https://drfs.ctcontents.com/file/12489212/17569905142754/a7be88/UE.Documentation/PaperZD/image/6-10.png)

如果我们现在点击播放，我们可以看到状态机的工作方式与之前章节中的示例完全相同。我们现在将在此基础上进行扩展，但首先需要修改我们的角色并添加一些额外的序列。

首先我们将创建我们缺少的其他动画序列，它们是：`Jump`、`Fall`、`Run` 和 `AirTrans`。

![6-11](https://drfs.ctcontents.com/file/12489212/17569905142755/4d7f7f/UE.Documentation/PaperZD/image/6-11.png)

然后我们将在我们的角色中添加一个简单的 `boolean`，用于改变 `IsRunning` 变量。此操作还应改变角色的移动 `max speed`，但我们暂不处理这部分，因为我们只关心渲染而不是游戏逻辑代码。在角色内部我们将添加一个 **Run 机制** 和一个 **Jump 机制**。

![6-12](https://drfs.ctcontents.com/file/12489212/17569905142757/4914fd/UE.Documentation/PaperZD/image/6-12.png)

我们还将进入我们的 **Animation Blueprints event graph** 并为我们的角色创建一个变量，以便在需要时查询它。我们将使用 `OnInit` 事件，该事件在 **AnimInstance**（此蓝图构建到的类）被创建时被调用。

![6-13](https://drfs.ctcontents.com/file/12489212/17569905142758/d08cf9/UE.Documentation/PaperZD/image/6-13.png)

现在，我们可以进入状态机节点的 Walk 并将其内部动画图更改为根据角色是否在奔跑来显示 Walk 或 Run

![6-14](https://drfs.ctcontents.com/file/12489212/17569905142759/1593ea/UE.Documentation/PaperZD/image/6-14.png)

如果我们现在点击播放，我们会看到角色改变了行为，不仅在 **Idle** 或 **Moving** 之间切换，它还能区分是应该 **run** 还是 **walk**。这些逻辑被紧密地打包在易于修改的模块中，确保迭代和创建一样容易。

![6-15](https://drfs.ctcontents.com/file/12489212/17569905142761/8213f6/UE.Documentation/PaperZD/image/6-15.png)

# 07. 使用 JumpNodes 创建紧凑的图

我们已经使用状态机以标准方式驱动角色的动画，到目前为止我们看到的特性与引擎 **AnimationBlueprints** 中状态机的默认行为共享，但有一些 2D 专属功能我们可以用来使 2D 开发更容易、更易于管理，并克服 Paper2D 翻页动画等逐帧动画带来的挑战。

我们将通过在角色上添加 **Jump** 功能来继续角色开发，同时学习这些功能。首先，我们要确保角色能够跳跃，为此我们已经在上一章中添加了所需的代码。

想象一下有一个非常大的动画状态机在驱动你的角色，而你想在其中添加空中动画。空中动画几乎可以在任何状态发生，如果动画图足够大，这意味着你需要从每个状态创建一个到空中状态的连接，如果你想在同一个图上保持行为的话。虽然确实有多种方式可以实现跳跃（如创建单独的状态机，然后在根动画图上切换它们），但作为示例，我们将通过在同一个图上实现 **Jump** 功能来解决这个问题。

PaperZD 自带 `JumpNodes`，这些是"快捷"节点，你可以放在状态机中的任意位置，可以通过代码触发使状态机跳转到该节点。这些节点对于中断状态机的常规流程非常有用，比如受伤或在这种情况下跳跃或处于空中状态，同时仍然将逻辑保留在同一个状态机中。

我们将首先创建我们的空中状态的逻辑，为此我们首先创建一个名为 **Airborne** 的 `JumpNode`，名称很重要，因为它是后续触发该跳跃时使用的标识符。

通过右键单击并添加节点来添加 `JumpNode`，就像添加状态或导体一样，然后将名称更改为 **Airborne**。

![7-1](https://drfs.ctcontents.com/file/12489212/17569905143413/9deb91/UE.Documentation/PaperZD/image/7-1.png)

![7-2](https://drfs.ctcontents.com/file/12489212/17569905143416/a26412/UE.Documentation/PaperZD/image/7-2.png)

之后我们要创建跳跃逻辑：开始跳跃（或处于空中）时，我们首先运行 **Jump** 动画（即使我们可能刚刚坠落），我们将在完成基本逻辑后解释这一点。

**跳跃**之后，角色会开始下落（ apex 完成后），我们将添加 **Falling** 状态。从 **Jumping** 到 **Falling** 的转换发生在下落的上抛速度变为负值时（意味着我们在下落而不是上升）。最后我们到达**着地**状态，即 **Walking** 或 **Idle**，我们连接到 **Walking**，因为我们更可能在移动而不是完全静止，但这可能根据游戏中落地逻辑的工作方式而变化。

![7-3](https://drfs.ctcontents.com/file/12489212/17569905143420/5fd7da/UE.Documentation/PaperZD/image/7-3.png)

我们还将 **Jumping** 和 **Falling** 动画放在它们各自的状态中，确保 `Play Animation` 节点设置为不循环，因为这些动画在设计时已考虑了这一点。

![7-4](https://drfs.ctcontents.com/file/12489212/17569905143423/c4b2c3/UE.Documentation/PaperZD/image/7-4.png)

在 **Jumping** 到 **Falling** 的连接内部：

![7-5](https://drfs.ctcontents.com/file/12489212/17569905143428/5fe23e/UE.Documentation/PaperZD/image/7-5.png)

在 **Falling** 到 **Walking** 的连接内部：

![7-6](https://drfs.ctcontents.com/file/12489212/17569905143430/102298/UE.Documentation/PaperZD/image/7-6.png)

默认情况下，**AnimationBlueprints** 在其类默认值上启用了 **Allow Transitional States** 设置。这使得状态机继续评估状态直到找到稳定状态。在上面的示例中，如果你的角色**坠落**（没有**跳跃**），状态机将首先评估 **Jump** 状态，然后立即评估转换并移动到 **Falling** 状态，因为速度是负的；状态机将在那里停止，因为没有其他转换可用，**Falling 状态将被渲染，Jump 状态不会。** 此示例中的 **Jump** 状态成为一个**过渡状态**，它有助于创建更紧凑的状态机，通过允许状态机自然地流向其稳定状态来消除对额外连接的需求。

现在状态机已经准备好了，我们只需要通过获取存储在 **AnimationComponent** 中的 **AnimInstance** 来触发 `JumpNode`。如前所述，**AnimInstance** 是我们正在构建的 **AnimationBlueprint** 的实例，用于驱动组件的动画。

在我们的角色蓝图我们将添加以下节点，以检测 **Jump** 和 **Falling** 行为，它们都将触发同一个 `JumpNode`，状态机将完成剩余的工作。

![7-7](https://drfs.ctcontents.com/file/12489212/17569905143435/673095/UE.Documentation/PaperZD/image/7-7.png)

如果你现在测试，角色将具有 Jump/Fall 动画设置，但它将没有中间动画，看起来会很僵硬。

![7-8](https://drfs.ctcontents.com/file/12489212/17569905143438/f163ed/UE.Documentation/PaperZD/image/7-8.png)

# 08. 过渡动画

## 创建中间状态及其缺点

骨骼动画相对于逐帧动画和 Paper2D 等框架有一个明显的优势，它们允许动画在状态之间自然混合，并确保动作流程不会显得过于生硬。逐帧动画由于其本质，无法自然混合，需要作为额外资源添加"混合"动画，在两个动画之间播放。

虽然 PaperZD 支持骨骼动画源的混合（前提是扩展已设置），但它也有一些工具可以轻松地添加中间动画，创建流动极其流畅的丰富角色，同时保持图的可管理大小。

在上一章中，我们添加了一个 Jump/Fall 行为，由于缺少中间状态而看起来很僵硬。我们将首先看看不使用 **Transitional Animations** 如何正常添加中间动画，以了解为什么添加中间状态不可持续。

作为参考，我们将在 **Jump** 和 **Fall** 动画之间添加一个额外的中间状态，我们将其命名为 **JumpApex**。

![8-1](https://drfs.ctcontents.com/file/12489212/17569905144026/cda3d3/UE.Documentation/PaperZD/image/8-1.png)

**Jumping** 到 **JumpApex** 的连接与我们之前的相同，当向上速度变为负值时触发。在 **JumpApex** 内部我们添加 **AirTrans** 动画序列**并确保设置为非循环**。最后这一点很重要。

![8-2](https://drfs.ctcontents.com/file/12489212/17569905144028/e29843/UE.Documentation/PaperZD/image/8-2.png)

之后，我们需要设置 **JumpApex** 和 **falling** 之间的转换。因为这是一个中间动画，我们希望在其播放完成后停止播放此动画，为此我们有两种方式可以访问该数据：

1. 获取与我们要检查的动画相关的 `TimeRemaining` 节点

![8-3](https://drfs.ctcontents.com/file/12489212/17569905144029/1cdbc3/UE.Documentation/PaperZD/image/8-3.png)

2. 查询此类编译到的 **AnimInstance** 内部的 **AnimPlayer**，它将给出最重要动画的当前时间。不过这个数据有 1 帧的延迟，所以使用时要小心。

![8-4](https://drfs.ctcontents.com/file/12489212/17569905144030/a30156/UE.Documentation/PaperZD/image/8-4.png)

![8-5](https://drfs.ctcontents.com/file/12489212/17569905144031/47a8b7/UE.Documentation/PaperZD/image/8-5.png)

> ℹ️ 注意：通常优先使用 `TimeRemaining` 节点（如果可用的话），但有时它们不可用，因为它们只能显示与它们相关的转换图。在这些情况下，使用 **AnimPlayer** 是规避这些问题的一个好方法。

> ℹ️ 注意：动画设置为非循环非常重要，因为播放进度可能在我们触发此规则之前循环。在低帧率环境中尤其如此。

> ℹ️ 注意2：**AnimationPlayer** 可以同时管理多个动画，Playback Progress 将引用当前处理中权重最大的动画。这不是非混合动画源（如 Paper2D Flipbooks）需要考虑的事情，因为混合不可用，因此任何时候只有一个动画在运行。

通过此设置，我们现在可以看到由于中间动画，角色的流程得到了显著改善，但这也带来了一个新问题。如果我们在 **AirTrans** 动画的中间突然着地了呢？根据转换规则，动画仍会继续播放直到结束。我们需要添加一个额外的**着地**检查。动画逻辑开始膨胀，我们想要添加更多中间状态来创建更丰富的角色，图就变得越难管理。

我们可以通过用 PaperZD 的自定义功能替换这些中间状态来解决这个问题：**Transitional Animations**。

## 使用 **Transitional Animations** 改善动画流畅度

正如我们在上一章中看到的，添加中间动画是一个繁琐的过程，需要在动画图上添加更多状态（因此更多逻辑），这反过来又使图迅速膨胀，使得即使是中等规模的项目也很难调试和管理。PaperZD 自带一个旨在解决此问题的功能：**Transitional Animations**。

**Transitional Animations** 的工作原理是允许用户在转换节点上添加自定义的可选动画图，此过渡图运行动画的单个循环，**不能**有内部状态机，它旨在成为一个简单的图，用于填充之间所需的动画数据。当动画的单个循环完成时，系统将继续渲染**状态**节点的 Animation Graph。

过渡动画本质上是可选的，如果它们指向的目标状态不再处于活动状态，它们就会停止渲染。只有最后一个过渡图会运行，作为其指向的目标状态的"介绍"。

让我们先回到几步之前，在我们添加中间状态之前。

![8-6](https://drfs.ctcontents.com/file/12489212/17569905144032/0f6504/UE.Documentation/PaperZD/image/8-6.png)

添加过渡图很简单，只需点击你要添加动画的转换，在本例中是 `Jumping to Falling transition`，然后在其详情面板上点击 **Use transitional animations**。你会看到转换本身改变了图标，以反映更改并显示它正在被动画化。

![8-7](https://drfs.ctcontents.com/file/12489212/17569905144034/57e361/UE.Documentation/PaperZD/image/8-7.png)

现在我们可以点击我们启用的复选框旁边的 **Open Animation Graph** 按钮。我们将进入一个动画图。让我们在其中添加 **AirTrans** 动画序列。

![8-8](https://drfs.ctcontents.com/file/12489212/17569905144035/5b45c7/UE.Documentation/PaperZD/image/8-8.png)

> ℹ️ 注意：你会看到在这个图内部，你将无法添加状态机并将动画设置为循环。此图中的每个动画都旨在有结束，因此循环将被禁用。

通过这种方式，过渡动画已正确设置，不再需要任何代码或配置。如果你现在运行示例，你会看到角色像之前一样工作，但它现在也会在着地未完成时中止过渡动画。

过渡动画是 PaperZD 的关键功能，是一种赋予角色更多生命力和动感而又不增加动画图复杂性的强大方式。

# 09. 绑定播放事件

有时仅仅添加动画不足以实现游戏的某些功能，需要对播放进行更细粒度的控制。因此，存在多种管理动画播放的方式，以使用户尽可能轻松地实现目标。

如前所示，播放速率可以在 Animation Graph 上的"逐节点"实例中管理。

![9-1](https://drfs.ctcontents.com/file/12489212/17569905144233/0cd36b/UE.Documentation/PaperZD/image/9-1.png)

通过这种方式，你可以实现**爬梯子**动画，根据角色是否在移动来播放（像旧平台游戏那样），在不动时暂停动画，甚至在下梯子时反向播放。

但有时需要更全局的控制，在每个 `Play Node` 上添加一个变量将不够可扩展，在这些情况下，直接与 **AnimPlayer** 接口更为合理。

PaperZD 的播放控制通过一个作为**播放管理器（Playback manager）**的中心对象来集中管理，它tick动画节点、触发通知并调用与正在运行的动画序列状态相关的事件。你只需调用以下节点即可访问此对象：

![9-2](https://drfs.ctcontents.com/file/12489212/17569905144234/39de3f/UE.Documentation/PaperZD/image/9-2.png)

此对象带有一组有趣的回调和事件，用于管理动画播放的全局状态。

![9-3](https://drfs.ctcontents.com/file/12489212/17569905144235/7fc97f/UE.Documentation/PaperZD/image/9-3.png)

使用此对象，你可以轻松暂停播放、更改播放模式（正向或反向）并查询当前渲染状态（了解当前动画的时间、正在播放哪个动画等）。

> ℹ️ 注意：某些调用如当前动画或动画时间，只考虑混合中权重最大的动画。这仅在骨骼和启用混合的动画源上重要，因为它们可以将多个动画混合到最终混合中。

**AnimPlayer** 还带有一组在播放期间自动调用的事件：

![9-4](https://drfs.ctcontents.com/file/12489212/17569905144237/8645b0/UE.Documentation/PaperZD/image/9-4.png)

其中两个已经被 **AnimationInstance** 实现并且可以直接重写：**Playback Complete**

![9-5](https://drfs.ctcontents.com/file/12489212/17569905144239/37eb8f/UE.Documentation/PaperZD/image/9-5.png)

这些事件（特别是 **Playback Complete**）是 **Transitional Graphs** 检测过渡动画已完成播放的方式。你可以绑定或实现它们以在动画渲染更改时收到通知。

# 10. 添加对多方向游戏的支持（俯视角、等距视角等）

到目前为止，我们已经展示了如何为横版卷轴游戏制作动画。这类游戏通常只有 2 个视角（左/右），实际上每个玩家"状态"只会使用一个动画，相反方向通常是镜像的。实际上这意味着对于每个动作，只有一个"动画"需要渲染。

显然并非总是如此，还有其他类型的 2D 游戏确实需要同一动作的不同方向使用多个动画。

俯视角、等距视角、复古射击游戏等严重依赖通过在不同角度绘制相同动画来模拟假 3D 动画，然后根据角度变量放置它们。这已经在美术部门带来了巨大挑战，根据显示的方向数量，工作量增加了 4 倍甚至 8 倍。显然在实现动画时，我们更愿意尽可能简化添加方向性的过程。

通常引擎和工具为需要为精灵添加方向性的用户提供的解决方案要么是强制他们创建"Blend Spaces"并在图上设置每个动画，要么是强制他们通过代码更改动画；两者都需要太多配置时间，增加了用户在此部分停留的时间，从而增加了这些项目的成本。

PaperZD 尽可能简化了这一步骤，并提供了一个强大的功能来帮助解决这些情况：**Multi-Directional Sequence**s（多方向序列）。

因为 AnimSequences 是播放动画的抽象，严格来说，动画的每个旋转仍然是同一个动画（对于游戏目的而言），我们添加了在序列内部添加"旋转"的能力。

作为示例，我们将进入我们的 Animation Source 并创建一个新的 Animation Sequence，我们将其命名为**"MyDirectionalSequence"**。

![10-1](https://drfs.ctcontents.com/file/12489212/17569905144402/c6d357/UE.Documentation/PaperZD/image/10-1.png)

如果我们打开这个新 Sequence 并查看其右侧的详细信息，我们会看到一个名为"Multi-Directional Sequence"的复选框

![10-2](https://drfs.ctcontents.com/file/12489212/17569905144403/f7579a/UE.Documentation/PaperZD/image/10-2.png)

点击它将显示方向性控件，用于设置 AnimSequence 的方向信息

![10-3](https://drfs.ctcontents.com/file/12489212/17569905144405/21e3e7/UE.Documentation/PaperZD/image/10-3.png)

你会看到这个新控件包含 4 个角，底部"Animation Data"已被替换为"Animation Node 0"。金色部分是当前选中的方向，点击另一个方块将选择该位置

![10-4](https://drfs.ctcontents.com/file/12489212/17569905144406/794b59/UE.Documentation/PaperZD/image/10-4.png)

我们还可以修改方向数量甚至它们的偏移角度

![10-5](https://drfs.ctcontents.com/file/12489212/17569905144410/20e16f/UE.Documentation/PaperZD/image/10-5.png)

因为示例项目不包含俯视角动画，我们将使用我们已有的翻页动画进行演示。将方向性设置为 N = 4，并用不同的动画填充方向。

![10-6](https://drfs.ctcontents.com/file/12489212/17569905144412/b8110d/UE.Documentation/PaperZD/image/10-6.png)

![10-7](https://drfs.ctcontents.com/file/12489212/17569905144397/67288f/UE.Documentation/PaperZD/image/10-7.png)

### 关于方向的说明：

> ℹ️ 方向序列旨在表示同一个动画，因此假设所有序列具有相同的长度。序列的长度将由第一个节点（Node 0）推断。

如果我们专注于控件并按住 Shift 键，我们可以移动预览节点，该节点将显示根据输入到序列的角度会显示哪个动画。

![10-8](https://drfs.ctcontents.com/file/12489212/17569905144399/f38948/UE.Documentation/PaperZD/image/10-8.png)

在这种情况下，即使选中的节点是正确的（Jump），我们显示的也是顶部的（Fall）。

然后可以使用生成的 AnimSequence，通知将正常触发，用于渲染此动画的翻页动画将根据我们必须提供的方向输入而变化。

回到 **Animation Blueprint**，我们现在将让 **Animation Graph** 知道我们希望系统渲染哪个方向（因此将显示哪个翻页动画）。为此，我们只需级联一个**"Set Directionality"**节点，该节点将为正确的动画数据评估设置正确的元数据。

我们将使用角色的速度作为节点的方向输入，这是一个 **FVector2D**，它将镜像**绿色预览节点**。此输入可以是你的项目需要的任何值，例如：

- 相机与敌人前向方向之间的角度差（用于复古射击游戏）。
- 俯视角游戏的 actor 前向向量。
- 最后一个非零速度。
- 或者甚至可以是一个用于简单地渲染不同动画的值（如精灵具有不同状态的更改），它可以是非方向性信息（虽然系统设计时考虑了方向性以使用户更容易创建此类游戏）。

![10-9](https://drfs.ctcontents.com/file/12489212/17569905144400/2acdb9/UE.Documentation/PaperZD/image/10-9.png)

> 注意：更改方向不会重置动画的播放时间。

通过这种方式，我们为角色添加了方向性。方向性信息是按角色而不是按动画设置的，这确保了设置速度很快。即使设置了方向性，系统也不会因为遇到非方向性精灵而失败。

这种实现角色的方式应该迭代快速，只需添加任意数量的方向动画，然后输入你"渲染"动画的角度，系统将为你选择正确的动画进行渲染。

# 11. 使用动画覆盖

## 何时不使用 JumpNodes

在创建你的动画流程时，有时你希望用自己的动画"播放"覆盖动画图的行为，例如在执行"攻击"和其他单次触发状态时。虽然**JumpNodes**确实可以帮助修改 StateMachine 本身的行为，但这些更改不是**触发即忘（Fire and Forget）**的，触发后之前的状态将丢失。这在编码诸如受到攻击的反应或强制进入你希望保持的状态的更改时是可取的，但有时你希望一个动画播放一次然后继续之前的行为，例如"攻击动画"。**Jump Nodes**不是正确的工具，你应该使用 **Animation Overrides**

## 动画覆盖和覆盖槽

简而言之，动画覆盖是一个**触发即忘**的动画，可以被触发，完成后将继续正常的动画流程。**Animation Overrides** 与 **Jump Nodes** 一样通过代码调用，但它们可以接收任何兼容的 AnimationSequence 在 AnimInstance 上播放。有两种调用此功能的方法，唯一的区别是可以接收的回调不同。

![11-1](https://drfs.ctcontents.com/file/12489212/17569905144530/c90f11/UE.Documentation/PaperZD/image/11-1.png)

要使用覆盖，首先我们需要让 AnimationBlueprint 知道在图的哪个位置应该覆盖 AnimGraph 逻辑，我们通过创建 **Override Slot** 节点来完成此操作。

![11-2](https://drfs.ctcontents.com/file/12489212/17569905144531/79ffc6/UE.Documentation/PaperZD/image/11-2.png)

![11-3](https://drfs.ctcontents.com/file/12489212/17569905144532/c057c9/UE.Documentation/PaperZD/image/11-3.png)

这将创建一个新 **Override Slot** 节点，可以通过代码直接调用。

> ℹ️ 添加 **Override Slot** 时，此节点左侧的任何节点在覆盖激活时不会影响最终动画状态，这意味着对于 **Directional Sequences**，最好将方向节点留在右侧，这样即使覆盖激活时，动画方向也能正确设置。

# 12. 创建自定义动画通知

你可以通过继承 **PaperZDAnimNotify** 和 **PaperZDAnimNotifyState** 来创建自己的通知。这些通知可以在不同的动画源之间共享，是构建通用功能的好地方。

![12-1](https://drfs.ctcontents.com/file/12489212/17569905144554/2d8769/UE.Documentation/PaperZD/image/12-1.png)

如果我们创建一个名为"MyNotify"的通知，然后进入 Animation Timeline，我们就能看到它存在。

![12-2](https://drfs.ctcontents.com/file/12489212/17569905144555/62fe76/UE.Documentation/PaperZD/image/12-2.png)

这些蓝图对象将像普通蓝图一样工作，但将能够访问 **"OnReceiveNotify"** 方法，以及在它是 **State Notify** 时的 Start/End 事件。

![12-3](https://drfs.ctcontents.com/file/12489212/17569905144556/fdcaa6/UE.Documentation/PaperZD/image/12-3.png)

在此蓝图中添加公共变量，将使你能够直接在时间轴上修改它们的值。

![12-4](https://drfs.ctcontents.com/file/12489212/17569905144558/96445d/UE.Documentation/PaperZD/image/12-4.png)

![12-5](https://drfs.ctcontents.com/file/12489212/17569905144559/f3585e/UE.Documentation/PaperZD/image/12-5.png)

在实现通知事件时，会给你一个名为"OwningInstance"的成员，这是调用此通知的 AnimInstance。如果从编辑器调用，此对象可能为空。

![12-6](https://drfs.ctcontents.com/file/12489212/17569905144560/a791ed/UE.Documentation/PaperZD/image/12-6.png)

如果需要，你可以获取用于渲染动画的 **"Render Component"**，你可以用它来添加声音、粒子或用于显示的调试框。

![12-7](https://drfs.ctcontents.com/file/12489212/17569905144561/c8c4d1/UE.Documentation/PaperZD/image/12-7.png)

在以下示例中，我们使用 Render 组件位置来识别在游戏播放时应在何处播放声音。在编辑器逻辑中，我们绘制一个调试框，该框仅在 Animation Source Editor 窗口期间显示。这对于创建在编辑器窗口中绘制的"碰撞盒"很有用（以便我们可以根据需要调整它们），而游戏行为实际上会应用碰撞盒而不显示它。

![12-8](https://drfs.ctcontents.com/file/12489212/17569905144562/b0f214/UE.Documentation/PaperZD/image/12-8.png)

![12-9](https://drfs.ctcontents.com/file/12489212/17569905144565/dfa3bd/UE.Documentation/PaperZD/image/12-9.png)

# 13. 为你的角色应用皮肤

随着新的 v2.2 版本，PaperZD 现在允许你为角色应用"皮肤"，使你能够快速更改它们的外观，同时保持之前编码的相同动画逻辑。

更改皮肤是以下情况的完美方式：

- 更改角色的服装。
- 修改复合精灵上的装备或身体部位。
- 为行为相同但外观不同的敌人创建主 AnimationBlueprint。

你可以通过右键单击将受资源影响的 **Animation Source** 并点击"Create AnimationSkin Asset"来创建皮肤

![13-1](https://drfs.ctcontents.com/file/12489212/17569905144841/145eee/UE.Documentation/PaperZD/image/13-1.png)

这将创建一个新的 **AnimationSkin** 文件。此文件负责将播放的动画映射到你想要渲染的自定义翻页动画，允许你在运行时更改角色的显示方式，而无需修改动画逻辑。

该文件预填充了源当前注册的所有动画，使你能够快速将动画映射到另一个翻页动画。

如果在任何时候向源添加了更多动画，你可以快速同步皮肤文件以添加任何未映射的剩余动画。

![13-2](https://drfs.ctcontents.com/file/12489212/17569905144843/0eb548/UE.Documentation/PaperZD/image/13-2.png)

对于多方向角色，动画数组与在编辑器中选择方向时的'Animation Node X'名称匹配，从最顶部的节点开始，按顺时针方向排列。

![13-3](https://drfs.ctcontents.com/file/12489212/17569905144832/6e482b/UE.Documentation/PaperZD/image/13-3.png)

要应用皮肤，只需调用以下节点，指定要使用的资源。

![13-4](https://drfs.ctcontents.com/file/12489212/17569905144836/a83b0f/UE.Documentation/PaperZD/image/13-4.png)

"Layer"参数在创建复合动画时使用，将在下一章中介绍。目前只需使用"Layer 0"，它指向主渲染组件。

# 14. 使用图层创建复合动画

## 构建复合 Animation Source

自 v2.2 版本以来，现在可以创建复合动画序列来渲染分层角色。

![14-1](https://drfs.ctcontents.com/file/12489212/17569905144937/cb7bb5/UE.Documentation/PaperZD/image/14-1.png)

要启用此行为，请导航到 AnimationSource 编辑器左下角并找到新的 **"Layers"** 部分。

![14-2](https://drfs.ctcontents.com/file/12489212/17569905144939/c44c7b/UE.Documentation/PaperZD/image/14-2.png)

在这里，你可以配置你的 **Animation Source** 以允许额外的复合层来渲染与主渲染组件同步的额外动画。在此示例中，我们添加了总共 6 个额外层与角色一起渲染，并给出了正确的偏移量以便它们可以彼此叠加渲染。

## 为每个图层设置动画

现在，我们可以进入任何 **AnimSequence** 的详细信息并设置基础身体动画（现在称为"Animation"），以及要渲染的额外复合层

![14-3](https://drfs.ctcontents.com/file/12489212/17569905144940/2e5c41/UE.Documentation/PaperZD/image/14-3.png)

这使我们能够逐块创建动画，甚至可以为每个身体部位单独应用皮肤，以创建完全可定制的角色，可作为游戏中'角色定制'机制的基础。

> ℹ️ 重要的是要注意，每个动画层都已创建为与主体部分在大小和持续时间上完全匹配，因此它们有效地并行运行并相互叠加，创建复合动画。

## 设置你的角色以使用分层动画

默认情况下，你不需要做太多工作来设置角色以使用你创建的图层，因为工具将自动创建渲染动画并正确分层所需的组件，这是因为我们在 **Animation Component** 上默认启用了以下选项。

![14-4](https://drfs.ctcontents.com/file/12489212/17569905144942/883926/UE.Documentation/PaperZD/image/14-4.png)

让插件自行构建图层是最简单的开始使用复合角色的方式，但这有一些注意事项：

- 图层仅在游戏开始时构建，如果同时加载太多角色可能会对性能产生一些不利影响。
- 因为图层是在 **Begin Play** 时构建的，这些图层在编辑器中不播放时不可见。
- 你无法轻松访问它们来以任何方式修改渲染组件。

因此，建议改为在蓝图中手动创建图层并将其分配到复合图层数组，以让工具知道如何渲染它们。这也使得在编辑器中可视化角色变得更加容易

![14-5](https://drfs.ctcontents.com/file/12489212/17569905144944/432a8b/UE.Documentation/PaperZD/image/14-5.png)

![14-6](https://drfs.ctcontents.com/file/12489212/17569905144945/947de7/UE.Documentation/PaperZD/image/14-6.png)

现在你有了一个完全功能正常的分层角色，其工作方式与普通的非分层角色相同。我们甚至可以通过修改皮肤资源指向的目标图层来为每个图层直接应用皮肤。

![14-7](https://drfs.ctcontents.com/file/12489212/17569905144947/9d0754/UE.Documentation/PaperZD/image/14-7.png)

**注意：**

> ℹ️ 向图层添加皮肤时，请记住 **Layer 0** 是你的主渲染组件，**Layer 1** 及之后是你的复合组件，因此在这种情况下我们正在为"Lower"身体部位应用皮肤。
