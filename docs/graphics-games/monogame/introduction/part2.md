---
title: Part 2-Walkinggame 구현
description: 이 연습에서는 게임 논리 및 콘텐츠를 데모로 이동 하는 애니메이션된 스프라이트를 만드는 빈 MonoGame 프로젝트를 터치 입력을 추가 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: F0622A01-DE7F-451A-A51F-129876AB6FFD
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 941b88f9109cf2f3a3485311c52b1250bd08e53f
ms.sourcegitcommit: 2ee36611ef667affee7d417db947fbb614d75315
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54479773"
---
# <a name="part-2--implementing-the-walkinggame"></a>Part 2-Walkinggame 구현

_이 연습에서는 게임 논리 및 콘텐츠를 데모로 이동 하는 애니메이션된 스프라이트를 만드는 빈 MonoGame 프로젝트를 터치 입력을 추가 하는 방법을 보여 줍니다._

이 연습의 이전 부분에는 빈 MonoGame 프로젝트를 만드는 방법을 보여 주었습니다. 간단한 게임 데모를 만들어 이러한 이전 부분에서 빌드합니다. 이 자료에는 다음과 같은 섹션이 포함되어 있습니다.

- 게임 콘텐츠 압축을 풀기
- MonoGame 클래스 개요
- 이 첫 번째 스프라이트 렌더링
- CharacterEntity 만들기
- 게임 CharacterEntity 추가
- 애니메이션 클래스 만들기
- 첫 번째 애니메이션 CharacterEntity 추가
- 이동 된 문자 추가
- 일치 하는 이동 및 애니메이션


## <a name="unzipping-our-game-content"></a>게임 콘텐츠 압축을 풀기

여기서 다루는 게임의 압축을 풉니다 하고자 코드 작성을 시작 하기 전에 *콘텐츠*합니다. 이라는 용어를 자주 사용 하는 게임 개발자 *콘텐츠* visual 아티스트, 게임 디자이너 또는 오디오 디자이너에서 일반적으로 생성 되는 비 코드 파일을 참조 합니다. 일반적인 콘텐츠 유형의 시각적 개체를 표시 하거나 소리를 재생 하거나 AI (인공 지능) 동작을 제어 하는 데 사용 하는 파일을 포함 합니다. 게임 개발에서 팀의 큐브 뷰 콘텐츠는 프로그래머가 아닌 일반적으로 만들어집니다.

여기에 콘텐츠를 찾을 수 있습니다 [github에서](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true)합니다. 이러한 파일을이 연습의 뒷부분에서 액세스 하는 것을 위치에 다운로드 해야 합니다.

## <a name="monogame-class-overview"></a>MonoGame 클래스 개요

먼저 기본 렌더링 하는 데 MonoGame 클래스를 살펴봅니다.

- `SpriteBatch` – 화면으로 2D 그래픽을 그리는 데 사용 합니다. *스프라이트* 2D visual 요소는 화면에 이미지를 표시 하는 데 사용 됩니다. `SpriteBatch` 개체 간에 한 번에 단일 스프라이트를 그릴 수 해당 `Begin` 하 고 `End` 메서드 또는 여러 스프라이트를 함께 그룹화 할 수 있습니다 또는 *일괄 처리 된*합니다.
- `Texture2D` – 런타임 시 이미지 개체를 나타냅니다. `Texture2D` 인스턴스는도 만들 수 있지만 동적으로 런타임 시 자주.png,.bmp 등 파일 형식에서 만들어집니다. `Texture2D` 사용 하 여 렌더링할 때 사용 되는 인스턴스 `SpriteBatch` 인스턴스.
- `Vector2` – 시각적 개체를 배치에 대 한 자주 사용 되는 2 차원 좌표 시스템의 위치를 나타냅니다. MonoGame도 포함 되어 있습니다 `Vector3` 하 고 `Vector4` 만 사용 하지만 `Vector2` 이 연습 합니다.
- `Rectangle` -위치, 너비 및 높이 사용 하 여 네 방향 영역을 합니다. 사용이 부분을 정의 하는 `Texture2D` 애니메이션을 사용 하 여 작업을 시작 하는 경우 렌더링 합니다.

해당 네임 스페이스에도 불구 하 고 MonoGame – Microsoft에서 유지 되지 않습니다 하는 것은 참고 해야 합니다. `Microsoft.Xna` 네임 스페이스는 MonoGame 쉽게 MonoGame을 기존 XNA 프로젝트를 마이그레이션할 수 있도록 합니다.

## <a name="rendering-our-first-sprite"></a>이 첫 번째 스프라이트 렌더링

다음 단일 스프라이트 MonoGame에서 2D 렌더링을 수행 하는 방법을 보여 주는 화면으로 그립니다 됩니다.

### <a name="creating-a-texture2d"></a>Texture2D 만들기

만들어야는 `Texture2D` 우리의 스프라이트를 렌더링할 때 사용할 인스턴스. 모든 게임 콘텐츠 라는 폴더 안에 궁극적 **콘텐츠를** 플랫폼별 프로젝트에 있는 합니다. 콘텐츠 플랫폼 빌드 작업을 사용 해야 하는 대로 공유 MonoGame 프로젝트 콘텐츠를 포함할 수 없습니다. CocosSharp 개발자 콘텐츠 폴더 익숙한 개념을 찾을 수-CocosSharp 프로젝트와 MonoGame 프로젝트 모두에서 같은 위치에 있습니다. IOS 프로젝트에 Android 프로젝트에서 자산 폴더 안에 콘텐츠 폴더를 찾을 수 있습니다.

여기서 다루는 게임의 콘텐츠를 추가 하려면 마우스 오른쪽 단추로 클릭 합니다 **콘텐츠** 선택한 폴더 **추가 > 파일 추가...** Content.zip 파일을 추출한 위치를 찾아서 선택 합니다 **charactersheet.png** 파일입니다. 폴더에 파일을 추가 하는 방법에 대 한 요청 되 면 선택 해야 합니다 **복사** 옵션:

![](part2-images/image1.png "폴더에 파일을 추가 하는 방법에 대 한 요청 되 면 복사 옵션을 선택 합니다.")

콘텐츠 폴더에는 이제 charactersheet.png 파일이 들어 있습니다.

![](part2-images/image2.png "콘텐츠 폴더에는 이제 charactersheet.png 파일 포함")

코드 charactersheet.png 파일을 로드 한 다음, 추가할 것을 `Texture2D`입니다. 이렇게 하려면이 열고는 `Game1.cs` 파일과 Game1.cs 클래스에 다음 필드를 추가 합니다.

```csharp
Texture2D characterSheetTexture;
```

다음으로 만들겠습니다 합니다 `characterSheetTexture` 에 `LoadContent` 메서드. 수정 하기 전에 `LoadContent` 메서드는 다음과 같습니다.

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
    // TODO: use this.Content to load your game content here
}
```

기본 프로젝트에는 이미 인스턴스화합니다 드릴을 `spriteBatch` 미국에 대 한 인스턴스. 사용이 나중에 있지만 준비 하는 추가 코드를 추가할 없습니다 것을 `spriteBatch` 사용에 대 한 합니다. 다른 한편으로 우리의 `spriteSheetTexture` 후 초기화는 초기화 필요 않으므로 `spriteBatch` 만들어집니다.

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
    using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
    {
        characterSheetTexture = Texture2D.FromStream (this.GraphicsDevice, stream);

    }
}
```

이제는 `SpriteBatch` 인스턴스 및 `Texture2D` 렌더링 코드를 추가할 수 있습니다 하는 인스턴스는 `Game1.Draw` 메서드:

```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    spriteBatch.Begin ();

    Vector2 topLeftOfSprite = new Vector2 (50, 50);
    Color tintColor = Color.White;

    spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, tintColor);

    spriteBatch.End ();

    base.Draw(gameTime);
}
```

이제 게임 실행 charactersheet.png에서 만든 질감을 표시 하는 단일 스프라이트를 보여 줍니다.

![](part2-images/image3.png "이제 게임 실행 charactersheet.png에서 만든 질감을 표시 하는 단일 스프라이트 보여 줍니다.")

## <a name="creating-the-characterentity"></a>CharacterEntity 만들기

지금까지 화면에 단일 스프라이트 렌더링 하는 코드를 추가 했습니다. 그러나 다른 클래스를 만들지 않고 전체 게임을 개발 한다면 코드 조직 문제를 실행 합니다.

### <a name="what-is-an-entity"></a>엔터티는 무엇입니까?

게임 코드를 구성 하기 위한 일반적인 패턴은 각 게임에 대 한 새 클래스를 만들려면 *엔터티* 형식입니다. 게임 개발의 엔터티에 (필수는 일부만) 다음 특징 일부를 포함할 수 있는 개체:

- 스프라이트, 텍스트 또는 3D 모델 같은 시각적 요소
- 물리학 또는 경로 설정 또는 입력에 응답 하는 플레이어 순찰 단위와 같은 모든 프레임 동작
- 만들고 파워업 표시 및 플레이어에서 수집 되와 같은 동적으로 제거할 수 있습니다

엔터티 조직 시스템 복잡할 수 있으며 많은 게임 엔진 엔터티를 관리할 수 있도록 클래스를 제공 합니다. 에서는 전체 게임에 일반적으로 더 많은 조직에 개발자가 필요한 주목할 만한 것 이므로 매우 간단한 엔터티 시스템을 구현 될 것입니다.


### <a name="defining-the-characterentity"></a>CharacterEntity 정의

이 엔터티는 `CharacterEntity`, 다음과 같은 특징을 갖습니다.

- 자체를 로드 하는 기능 `Texture2D`
- 걸어다니는 애니메이션을 업데이트 하는 호출 메서드가 포함 된 포함 하 여 자체를 렌더링 하는 기능
- `X` 및 Y 문자 위치를 제어 하는 속성입니다.
- 특히 자체-업데이트를 읽고 값 터치 화면 위치를 적절 하 게 조정 하는 기능입니다.

추가 하는 `CharacterEntity` 게임을 마우스 오른쪽 단추 클릭 또는 컨트롤-클릭 합니다 **WalkingGame** 프로젝트를 마우스 **추가 > 새 파일...** . 선택 된 **빈 클래스** 옵션 및 새 파일 이름을 **CharacterEntity**, 클릭 **새로 만들기**합니다.

먼저 기능을 추가한 합니다 `CharacterEntity` 로드 하는 `Texture2D` 뿐만 아니라 자체 그리기 합니다. 새로 추가 된 수정 하겠습니다 `CharacterEntity.cs` 다음과 같이 파일:

```csharp
using System;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Input.Touch;


namespace WalkingGame
{
    public class CharacterEntity
    {
        static Texture2D characterSheetTexture;

        public float X
        {
            get;
            set;
        }

        public float Y
        {
            get;
            set;
        }

        public CharacterEntity (GraphicsDevice graphicsDevice)
        {
            if (characterSheetTexture == null)
            {
                using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
                {
                    characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
                }
            }
        }

        public void Draw(SpriteBatch spriteBatch)
        {
            Vector2 topLeftOfSprite = new Vector2 (this.X, this.Y);
            Color tintColor = Color.White;
            spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, tintColor);
        }
    }
}
```

위의 코드를 추가 하는 위치, 렌더링 및 콘텐츠를 로드 해야는 `CharacterEntity`합니다. 위의 코드에서 수행 된 몇 가지 고려 사항의을 잠시를 살펴보겠습니다.

### <a name="why-are-x-and-y-floats"></a>이유는 X 및 Y 부동?

개발자는 게임 프로그래밍 이유를 궁금해 수는 `float` 형식이 아닌 사이트별로 사용 되 `int` 또는 `double`합니다. 이유는 32 비트 값의 하위 수준 렌더링 코드에 배치 하는 것에 대 한 가장 일반적인 된다는 것입니다. Double 형식에는 64 비트 전체 자릿수는 개체의 위치를 지정 하는 데 필요한 거의 차지 합니다. 32 비트 차이 불필요 한 보일 수 있지만, 많은 최신 게임 그래픽이 포함 수십만 위치 값을 처리 해야 하는 각 프레임 (30 또는 초당 60 번). 이동 해야 하는 메모리의 양을 잘라내기 그래픽을 통해 절반으로 파이프라인 수 상당한 영향을 미칠 게임의 성능에 있습니다.

`int` 데이터 형식에는 적절 한 위치에 대 한 측정 단위 수 있지만 엔터티가 배치 되는 방식에 제한 사항이 서비스할 수 있습니다. 예를 들어 정수 값을 사용 하 여 더욱 어렵게 게임 확대/축소 또는 3D 카메라 지에 대 한 부드러운 이동을 구현 하려면 (에서 허용 되는 `SpriteBatch`).

마지막으로,이 문자 화면에서 이리저리 이동 하는 논리 게임의 타이밍 값을 사용 하 여 수행 됩니다 하는 것이 살펴보겠습니다. 지정된 된 프레임에서 얼마나 많은 시간이 경과 하 여 개발 속도 곱한 결과를 거의 하면 정수를 사용 해야 하므로 `float` 에 대 한 `X` 고 `Y`입니다.

### <a name="why-is-charactersheettexture-static"></a>이유는 무엇 characterSheetTexture 정적?

합니다 `characterSheetTexture` `Texture2D` charactersheet.png 파일의 런타임 표현 합니다. 원본에서 추출 된 각 픽셀의 색상 값 포함 하는 다시 말해 **charactersheet.png** 파일입니다. 여기서 다루는 게임 2 개를 만든 경우 `CharacterEntity` 인스턴스를 각각 charactersheet.png에서 정보 액세스 해야 합니다. 이런에서 charactersheet.png를 두 번 로드 하는 성능 비용이 발생 하려고 하지 않으며 중복 질감 메모리가 ram에 저장 할까요. 사용 하는 `static` 필드를 사용 하면 동일한 공유 `Texture2D` 모든 `CharacterEntity` 인스턴스.

사용 하 여 `static` 콘텐츠의 런타임 표현에 대 한 멤버는 단순한 솔루션 및 한 수준에서 다른 컴퓨터로 이동 하는 경우 언로드 콘텐츠와 같은 대규모 게임에서 발생 하는 문제를 해결 하지는 않습니다. 이 연습의 범위를 벗어나는 더욱 정교한 솔루션을 사용자 지정 콘텐츠 관리 시스템을 만들거나 MonoGame의 콘텐츠 파이프라인을 사용 하 여 포함 합니다.

### <a name="why-is-spritebatch-passed-as-an-argument-instead-of-instantiated-by-the-entity"></a>이유은 SpriteBatch 전달으로 엔터티에 의해 인수 Instead of 인스턴스화됩니다.

`Draw` 메서드는 위에 구현 되는 `SpriteBatch` 인수를 반면 하지만 `characterSheetTexture` 의해 인스턴스화됩니다를 `CharacterEntity`.

가장 효율적인 렌더링은 가능한 원인은 이므로 동일한 `SpriteBatch` 인스턴스 모두에 사용 됩니다 `Draw` 호출을 언제 모든 `Draw` 단일 집합 간의 호출이 수행 되는지 `Begin` 및 `End` 호출 합니다. 물론 게임이 단일 엔터티 인스턴스를 포함 됩니다 있지만 동일한 것을 사용 하는 여러 엔터티를 허용 하는 패턴에서 복잡 한 게임 이익을 `SpriteBatch` 인스턴스.


## <a name="adding-characterentity-to-the-game"></a>게임 CharacterEntity 추가

추가한 했으므로 우리의 `CharacterEntity` 자체 렌더링을 위한 코드를 사용 하 여 코드를 바꿀 수 것 `Game1.cs` 이 새 엔터티의 인스턴스를 사용 합니다. 에서는 바꿉니다이 작업을 수행 하는 `Texture2D` 필드를 `CharacterEntity` 필드에 `Game1`:

```csharp
public class Game1 : Game
{
    GraphicsDeviceManager graphics;
    SpriteBatch spriteBatch;
    // New code:
    CharacterEntity character;

    public Game1()
    {
    ...
```

다음으로,이 엔터티를의 인스턴스화 추가한 `Game1.Initialize`:

```csharp
protected override void Initialize()
{
    character = new CharacterEntity (this.GraphicsDevice);

    base.Initialize();
}
```

제거 해야 합니다 `Texture2D` 에서 만들 `LoadContent` 는 이제 내에 처리 되므로 `CharacterEntity`합니다. `Game1.LoadContent` 다음과 같이 표시 됩니다.

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
}
```

이름과 달리는 주목할 만한 가치가 `LoadContent` 사용할 수 없는 콘텐츠를 로드할 수 있습니다 – 콘텐츠 많은 게임 구현 콘텐츠로 사용 하는 업데이트 코드 또는 해당 초기화 메서드에서 로드는 필요 하지 않습니다 플레이어에 도달할 때까지 위치 통해서만 메서드를 게임의 특정 지점입니다.

마지막으로 Draw 메서드가 다음과 같이 수정할 수 있습니다.


```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    // We'll start all of our drawing here:
    spriteBatch.Begin ();

    // Now we can do any entity rendering:
    character.Draw(spriteBatch);
    // End renders all sprites to the screen:
    spriteBatch.End ();

    base.Draw(gameTime);
}
```

게임을 실행 하는 경우 이제 문자를 살펴보겠습니다. X 및 Y를 0으로 기본적으로 하므로 화면의 왼쪽된 위 모퉁이 대 한 문자 배치 됩니다.

![](part2-images/image4.png "X 및 Y를 0으로 기본적으로 하므로 다음 문자 위치가 화면의 왼쪽된 위 모퉁이 대 한")

## <a name="creating-the-animation-class"></a>애니메이션 클래스 만들기

현재 우리의 `CharacterEntity` 전체 표시 **charactersheet.png** 파일입니다. 이 배열을 하나의 파일에 여러 이미지 라고 하는 *스프라이트 시트*합니다. 일반적으로 스프라이트 스프라이트 시트의 일부분만 렌더링 됩니다. 수정 하겠습니다 합니다 `CharacterEntity` 이 부분을 렌더링 하 **charactersheet.png**,이 부분은 걸어다니는 애니메이션을 표시 하는 시간이 지남에 따라 변경 됩니다.

만들겠습니다는 `Animation` 논리 및 CharacterEntity 애니메이션의 상태를 제어 하는 클래스입니다. 애니메이션 클래스 뿐 아니라 모든 엔터티에 대 한 사용 될 수 있는 일반 클래스 됩니다 `CharacterEntity` 애니메이션 합니다. Ultimate는 `Animation` 클래스를 제공 합니다는 `Rectangle` 는 `CharacterEntity` 자체를 그릴 때 사용 됩니다. 또한 만듭니다는 `AnimationFrame` 애니메이션을 정의 하는 데 사용할 클래스입니다.


### <a name="defining-animationframe"></a>AnimationFrame 정의

`AnimationFrame` 애니메이션에 관련 된 모든 논리는 포함 되지 않습니다. 우리가 사용할 것이 데이터를 저장 하기 위한 것입니다. 추가할 합니다 `AnimationFrame` 클래스, 마우스 오른쪽 단추로 클릭 하 여 컨트롤-클릭 합니다 **WalkingGame** 공유 프로젝트 및 선택 **추가 > 새 파일...** 이름을 입력 **AnimationFrame** 을 클릭 합니다 **새로 만들기** 단추입니다. 수정 된 `AnimationFrame.cs` 파일에 다음 코드를 포함 합니다.


```csharp
using System;
using Microsoft.Xna.Framework;

namespace WalkingGame
{
    public class AnimationFrame
    {
        public Rectangle SourceRectangle { get; set; }
        public TimeSpan Duration { get; set; }
    }
}
```

`AnimationFrame` 클래스는 두 가지 정보를 포함 합니다.

- `SourceRectangle` --의 영역을 정의 하는 중 합니다 `Texture2D` 하 여 표시 되는 `AnimationFrame`합니다. 이 값은 맨 위 왼쪽은 (0, 0)와 픽셀 단위로 측정 됩니다.
- `Duration` – 정의 기간을 `AnimationFrame` 에 사용 되는 경우 표시 되는 `Animation`합니다.

### <a name="defining-animation"></a>애니메이션 정의

합니다 `Animation` 클래스에 포함 됩니다는 `List<AnimationFrame>` 프레임 시간 경과 따라 현재 표시를 전환 하는 논리와 합니다.

추가할 합니다 `Animation` 클래스, 마우스 오른쪽 단추로 클릭 하 여 컨트롤-클릭 합니다 **WalkingGame** 공유 프로젝트 및 선택 **추가 > 새 파일...** . 이름을 입력 **애니메이션** 을 클릭 합니다 **새로 만들기** 단추입니다. 수정 된 `Animation.cs` 파일 이므로 다음 코드를 포함 합니다.


```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Microsoft.Xna.Framework;

namespace WalkingGame
{
    public class Animation
    {
        List<AnimationFrame> frames = new List<AnimationFrame>();
        TimeSpan timeIntoAnimation;

        TimeSpan Duration
        {
            get
            {
                double totalSeconds = 0;
                foreach (var frame in frames)
                {
                    totalSeconds += frame.Duration.TotalSeconds;
                }

                return TimeSpan.FromSeconds (totalSeconds);
            }
        }

        public void AddFrame(Rectangle rectangle, TimeSpan duration)
        {
            AnimationFrame newFrame = new AnimationFrame()
            {
                SourceRectangle = rectangle,
                Duration = duration
            };

            frames.Add(newFrame);
        }

        public void Update(GameTime gameTime)
        {
            double secondsIntoAnimation = 
                timeIntoAnimation.TotalSeconds + gameTime.ElapsedGameTime.TotalSeconds;


            double remainder = secondsIntoAnimation % Duration.TotalSeconds;

            timeIntoAnimation = TimeSpan.FromSeconds (remainder);
        }
    }
}
```

세부 정보 중 일부에 대해 살펴보겠습니다는 `Animation` 클래스입니다.

### <a name="frames-list"></a>프레임 목록

`frames` 멤버는이 애니메이션에 대 한 데이터를 저장 하는 항목입니다. 애니메이션을 인스턴스화하는 코드를 추가 합니다 `AnimationFrame` 인스턴스를 합니다 `frames` 를 통해 목록는 `AddFrame` 메서드. 더 완전 한 구현을 제공할 수 있습니다 `public` 메서드 또는 속성을 수정 하기 위한 `frames`, 하지만이 연습에 대 한 프레임을 추가 하는 기능 제한 됩니다.

### <a name="duration"></a>기간

총 기간을 반환 하는 기간을 `Animation,` 포함 된 모든 기간을 추가 하 여 얻을 수 있는 `AnimationFrame` 인스턴스. 하는 경우이 값을 캐시할 수 `AnimationFrame` 변경할 수 없는 개체 있었지만 AnimationFrame 애니메이션에 추가 된 후 변경 될 수 있는 클래스로 구현에서는 있으므로 속성에 액세스할 때마다이 값을 계산 해야 합니다.

### <a name="update"></a>업데이트

`Update` 메서드 (즉, 전체 게임은 업데이트 될 때마다) 프레임 마다 호출 해야 합니다. 용도 증가 `timeIntoAnimation` 현재 표시 된 프레임을 반환 하는 데 사용 되는 멤버입니다. 논리 `Update` 방지는 `timeIntoAnimation` 없도록 이전 애니메이션의 지속 시간 보다 더 큰 합니다.

사용 하므로 `Animation` 끝에 도달 하면 반복이 애니메이션 하고자 하는 다음 걸어다니는 애니메이션을 표시 하는 클래스입니다. 확인 하 여이 작업을 수행할 수 있습니다 것을 `timeIntoAnimation` 보다 큰는 `Duration` 값입니다. 이 경우 처음으로 순환 하 고 나머지 부분에서는 애니메이션을 반복의 결과 유지 합니다.

### <a name="obtaining-the-current-frames-rectangle"></a>현재 프레임의 사각형을 가져오기

용도 `Animation` 클래스는 궁극적으로 제공 하는 `Rectangle` 스프라이트를 그릴 때 사용할 수 있습니다. 현재는 `Animation` 변경에 대 한 논리를 포함 하는 클래스를 `timeIntoAnimation` 멤버는 가져올 하는 데 사용 됩니다 `Rectangle` 표시 합니다. 만들어이 작업을 수행 합니다는 `CurrentRectangle` 속성에는 `Animation` 클래스입니다. 이 속성에 복사 합니다 `Animation` 클래스:

```csharp
public Rectangle CurrentRectangle
{
    get
    {
        AnimationFrame currentFrame = null;

        // See if we can find the frame
        TimeSpan accumulatedTime = new TimeSpan();
        foreach(var frame in frames)
        {
            if (accumulatedTime + frame.Duration >= timeIntoAnimation)
            {
                currentFrame = frame;
                break;
            }
            else
            {
                accumulatedTime += frame.Duration;
            }
        }

        // If no frame was found, then try the last frame, 
        // just in case timeIntoAnimation somehow exceeds Duration
        if (currentFrame == null)
        {
            currentFrame = frames.LastOrDefault ();
        }

        // If we found a frame, return its rectangle, otherwise
        // return an empty rectangle (one with no width or height)
        if (currentFrame != null)
        {
            return currentFrame.SourceRectangle;
        }
        else
        {
            return Rectangle.Empty;
        }
    }
}
```

## <a name="adding-the-first-animation-to-characterentity"></a>첫 번째 애니메이션 CharacterEntity 추가

합니다 `CharacterEntity` 애니메이션에 대 한 탐색 및 고정, 및 현재에 대 한 참조를 포함 하는 `Animation` 표시 합니다.

먼저 첫 추가한 `Animation,` 지금 작성 기능을 테스트 하는 데 사용 됩니다. CharacterEntity 클래스에 다음 멤버를 추가 해 보겠습니다.

```csharp
Animation walkDown;
Animation currentAnimation;
```

그런 다음 정의 `walkDown` 애니메이션 합니다. 이 수정 작업을 수행 하는 `CharacterEntity` 다음과 같은 생성자:

```csharp
public CharacterEntity (GraphicsDevice graphicsDevice)
{
    if (characterSheetTexture == null)
    {
        using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
        {
            characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
        }
    }

    walkDown = new Animation ();
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (16, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (32, 0, 16, 16), TimeSpan.FromSeconds (.25));
}
```

호출 해야 앞에서 설명한 대로 `Animation.Update` 애니메이션을 재생 하는 데 시간을 기준으로 합니다. 또한 할당 해야 합니다 `currentAnimation`합니다. 이제 할당 하겠습니다에 대 한 합니다 `currentAnimation` 에 `walkDown`,에서는 됩니다 수 교체이 코드 나중에 이동 논리를 구현 하는 경우. 추가 합니다 `Update` 메서드를 `CharacterEntity` 다음과 같이 합니다.


```csharp
public void Update(GameTime gameTime)
{
    // temporary - we'll replace this with logic based off of which way the
    // character is moving when we add movement logic
    currentAnimation = walkDown;

    currentAnimation.Update (gameTime);
}
```

이제는 `currentAnimation` 되 고 할당 하 고 업데이트는 그리기를 수행 하는 데 사용할 수 것입니다. 수정 `CharacterEntity.Draw` 다음과 같습니다.

```csharp
public void Draw(SpriteBatch spriteBatch)
{
    Vector2 topLeftOfSprite = new Vector2 (this.X, this.Y);
    Color tintColor = Color.White;
    var sourceRectangle = currentAnimation.CurrentRectangle;

    spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, sourceRectangle, Color.White);
}
```

시작 하기 위한 마지막 단계는 `CharacterEntity` 새로 추가 된를 호출 하는 애니메이션 `Update` 메서드에서 `Game1`합니다. 수정 `Game1.Update` 다음과 같습니다.

```csharp
protected override void Update(GameTime gameTime)
{
    character.Update (gameTime);
    base.Update(gameTime);
}
```

이제는 `CharacterEntity` 담당할 해당 `walkDown` 애니메이션 합니다.

![](part2-images/image5.gif "이제는 CharacterEntity walkDown 애니메이션이 재생")

## <a name="adding-movement-to-the-character"></a>이동 된 문자 추가

다음으로 추가할 예정 이동 터치 컨트롤을 사용 하는 문자입니다. 사용자가 화면을 터치 하면 문자 화면 라이브 위치 지점 방향으로 이동 합니다. 없는 터치 감지 되 면 문자 위치에 강조 합니다.

### <a name="defining-getdesiredvelocityfrominput"></a>GetDesiredVelocityFromInput 정의

MonoGame의 사용 `TouchPanel` 터치 스크린의 현재 상태에 대 한 정보를 제공 하는 클래스입니다. 확인 하는 메서드를 추가 해 보겠습니다는 `TouchPanel` 원하는 속도 문자를 반환 합니다.


```csharp
Vector2 GetDesiredVelocityFromInput()
{
    Vector2 desiredVelocity = new Vector2 ();

    TouchCollection touchCollection = TouchPanel.GetState();

    if (touchCollection.Count > 0)
    {
        desiredVelocity.X = touchCollection [0].Position.X - this.X;
        desiredVelocity.Y = touchCollection [0].Position.Y - this.Y;

        if (desiredVelocity.X != 0 || desiredVelocity.Y != 0)
        {
            desiredVelocity.Normalize();
            const float desiredSpeed = 200;
            desiredVelocity *= desiredSpeed;
        }
    }

    return desiredVelocity;
}
```

`TouchPanel.GetState` 메서드가 반환 되는 `TouchCollection` 사용자가 여 화면을 터치 하는 위치에 대 한 정보를 포함 하는 합니다. 사용자가 화면을 터치 하지 않는 경우 해당 `TouchCollection` 는 비워 둘 경우에서는 문자를 이동 해서는 안 됩니다.

사용자가 화면을 터치 하는 경우 이동 첫 번째 터치를 향해 문자 즉, `TouchLocation` 인덱스 0입니다. 처음에 문자 위치와 첫 번째 터치 위치 간의 차이과 동일 하 게 원하는 속도 설정 하겠습니다.

```csharp
        desiredVelocity.X = touchCollection [0].Position.X - this.X;
        desiredVelocity.Y = touchCollection [0].Position.Y - this.Y;
```

뒤에 오는 수학 같은 속도로 이동 된 문자를 유지 하는 많은 경우 을이 중요 한 이유는 보여 주기 위해 사용자의 문자 위치에서 화면 500 픽셀에 접촉 되어 있는 경우를 생각해 보겠습니다. 첫 번째 줄 위치 `desiredVelocity.X` 는 집합의 값이 500 할당 합니다. 그러나 사용자는 문자, 100 명의 단위의 거리 여 화면을 터치 된 경우 해당 `desiredVelocity.X `100으로 설정 됩니다. 결과 문자의 이동 속도 응답 방법 멀리 터치 포인트에서 문자는 것입니다. 항상 같은 속도로 이동 하는 문자, 것 이므로 desiredVelocity 수정 해야 합니다.

`if (desiredVelocity.X != 0 || desiredVelocity.Y != 0)` 사용자 문자의 현재 위치와 동일한 위치를 건드리지 않고는 있는지 검사 하는 경우 속도 비-0 – 즉 문을 확인 합니다. 그렇지 않으면 다음 필요한 경우 터치 문자 속도 멀리 떨어진 곳에 관계 없이 상수를 설정 됩니다. 길이는 1 중에 발생 하는 속도 벡터를 정규화 하 여이 작업을 수행할 것입니다. 1 이면 문자에 초당 1 픽셀씩 이동 하는 개발 속도 벡터입니다. 에서는에서는 속도 200 원하는 속도 따라 값을 곱하여 합니다.


### <a name="applying-velocity-to-position"></a>위치로 개발 속도 적용합니다.

반환 된 속도 `GetDesiredVelocityFromInput` 문자를 적용 해야 `X` 및 `Y` 값을 런타임에 적용 합니다. 수정 된 `Update` 같이 메서드:

```csharp
public void Update(GameTime gameTime)
{

    var velocity = GetDesiredVelocityFromInput ();

    this.X += velocity.X * (float)gameTime.ElapsedGameTime.TotalSeconds;
    this.Y += velocity.Y * (float)gameTime.ElapsedGameTime.TotalSeconds;

    // temporary - we'll replace this with logic based off of which way the
    // character is moving when we add movement logic
    currentAnimation = walkDown;

    currentAnimation.Update (gameTime);
}
```

여기서 구현 했습니다 무엇 이라고 *시간 기반* 이동 (반대인 *프레임 기반* 이동). 이동 시간을 기준으로 곱합니다 속도 값 (값에 저장 된 경우에는 `velocity` 변수)에 저장 된 마지막 업데이트 이후 얼마나 많은 시간이 경과 하 여 `gameTime.ElapsedGameTime.TotalSeconds`입니다. 게임에서 초당 프레임 수가 실행 되 면 프레임 간에 경과 된 시간 이동 – 최종 결과 프레임 속도 관계 없이 같은 속도로 이동 시간을 기준으로 사용 하 여 개체는 항상 이동 합니다.

게임, 이제 실행 하는 경우 문자 터치 위치 변화는 살펴보겠습니다.

![](part2-images/image6.gif "터치 위치로 이동 하는 문자")

## <a name="matching-movement-and-animation"></a>일치 하는 이동 및 애니메이션

이 문자를 이동 하 고 단일 애니메이션을 재생 한 후 수는 애니메이션의 나머지 부분을 정의 하 고 문자의 움직임을 반영 하기 위해 사용 합니다. 때 완료 해야 8 애니메이션 총에서:

- 아래쪽, 왼쪽, 탐색을 하 고 오른쪽에 대 한 애니메이션
- 고정에 대 한 애니메이션 여전히 및 왼쪽과 오른쪽 아래로, 연결

### <a name="defining-the-rest-of-the-animations"></a>애니메이션의 나머지 부분을 정의합니다.

먼저 추가 합니다 `Animation` 인스턴스를 `CharacterEntity` 추가한 있는 동일한 위치에서이 애니메이션의 모든 클래스를 `walkDown`합니다. 완료 되 면이 `CharacterEntity` 같은 `Animation` 멤버:

```csharp
Animation walkDown;
Animation walkUp;
Animation walkLeft;
Animation walkRight;

Animation standDown;
Animation standUp;
Animation standLeft;
Animation standRight;

Animation currentAnimation;
```

이제에서 애니메이션 정의 `CharacterEntity` 다음과 같은 생성자:

```csharp
public CharacterEntity (GraphicsDevice graphicsDevice)
{
    if (characterSheetTexture == null)
    {
        using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
        {
            characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
        }
    }

    walkDown = new Animation ();
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (16, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (32, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkUp = new Animation ();
    walkUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (160, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (176, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkLeft = new Animation ();
    walkLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (64, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (80, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkRight = new Animation ();
    walkRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (112, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (128, 0, 16, 16), TimeSpan.FromSeconds (.25));

    // Standing animations only have a single frame of animation:
    standDown = new Animation ();
    standDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standUp = new Animation ();
    standUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standLeft = new Animation ();
    standLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standRight = new Animation ();
    standRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
}
```

위의 코드에 추가 된를 드릴 합니다 `CharacterEntity` 이 연습을 더 짧게 유지 하는 생성자입니다. 일반적으로 하 게 게임 자체 클래스로 문자가 애니메이션의 정의 별도 못할을 하거나 XML 또는 JSON 등 데이터 형식에서이 정보를 로드 합니다.

그런 다음 문자는 이전 단계에서 중지 하는 경우 문자를 이동 하는 방향에 따라 또는 마지막 애니메이션에 따라 애니메이션을 사용 하는 논리를 조정 됩니다. 이렇게 하려면 수정 된 `Update` 메서드:


```csharp
public void Update(GameTime gameTime)
{
    var velocity = GetDesiredVelocityFromInput ();

    this.X += velocity.X * (float)gameTime.ElapsedGameTime.TotalSeconds;
    this.Y += velocity.Y * (float)gameTime.ElapsedGameTime.TotalSeconds;


    if (velocity != Vector2.Zero)
    {
        bool movingHorizontally = Math.Abs (velocity.X) > Math.Abs (velocity.Y);
        if (movingHorizontally)
        {
            if (velocity.X > 0)
            {
                currentAnimation = walkRight;
            }
            else
            {
                currentAnimation = walkLeft;
            }
        }
        else
        {
            if (velocity.Y > 0)
            {
                currentAnimation = walkDown;
            }
            else
            {
                currentAnimation = walkUp;
            }
        }
    }
    else
    {
        // If the character was walking, we can set the standing animation
        // according to the walking animation that is playing:
        if (currentAnimation == walkRight)
        {
            currentAnimation = standRight;
        }
        else if (currentAnimation == walkLeft)
        {
            currentAnimation = standLeft;
        }
        else if (currentAnimation == walkUp)
        {
            currentAnimation = standUp;
        }
        else if (currentAnimation == walkDown)
        {
            currentAnimation = standDown;
        }
        else if (currentAnimation == null)
        {
            currentAnimation = standDown;
        }

        // if none of the above code hit then the character
        // is already standing, so no need to change the animation.
    }

    currentAnimation.Update (gameTime);
}
```

애니메이션을 전환 하는 코드는 다음과 같은 두 가지 요소로로 세분화 됩니다. 첫 번째 검사 속도 같지 않은 경우 `Vector2.Zero` –이 알려줍니다. 문자 인지 이동 합니다. 문자를 이동, 경우를 볼 수는 `velocity.X` 및 `velocity.Y` 걸어다니는 애니메이션 재생을 확인 하는 값입니다.

문자는 이동 하지 경우 문자를 설정 하려고 `currentAnimation` 고정 애니메이션 –에 그렇게 하는 경우 있지만 `currentAnimation` 는 탐색은 애니메이션이 나 애니메이션 설정 되지 않은 경우. 경우는 `currentAnimation` 중 하나가 아닌 4 개의 워크 애니메이션, 다음 문자는 변경할 필요가 없습니다 있도록 이미 서 있는 `currentAnimation`합니다.

이 코드의 결과 다음과 문자 제대로 walking 때 애니메이션을 적용 되며 다음의 방향이 마지막 중지 되 면 검색 되었습니다.

![](part2-images/image7.gif "이 코드의 결과 문자를 제대로 walking 때 애니메이션을 적용 되며 다음의 방향이 마지막 중지 되 면 검색 된")

## <a name="summary"></a>요약

이 연습에서는 스프라이트, 개체 이동, 입력된 감지 및 애니메이션을 사용 하 여 게임 만드는 플랫폼 간 MonoGame을 사용 하는 방법을 살펴보았습니다. 범용 애니메이션 클래스 만들기는 내용을 다룹니다. 또한 코드 논리를 구성 하는 것에 대 한 문자 엔터티를 만드는 방법에 알아보았습니다.

## <a name="related-links"></a>관련 링크

- [CharacterSheet 이미지 리소스 (샘플)](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true)
- [탐색 게임 완료 (샘플)](https://developer.xamarin.com/samples/mobile/WalkingGameMG/)
