---
title: 2 부-WalkingGame 구현
description: 이 연습에서는 빈 MonoGame 프로젝트에 게임 논리 및 콘텐츠를 추가 하 여 터치 입력으로 이동 하는 애니메이션 스프라이트의 데모를 만드는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: F0622A01-DE7F-451A-A51F-129876AB6FFD
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 7c7b58266b4f5168fdb231258390fa64278963f8
ms.sourcegitcommit: f255aa286bd52e8a80ffa620c2e93c97f069f8ec
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68680949"
---
# <a name="part-2--implementing-the-walkinggame"></a>2 부-WalkingGame 구현

_이 연습에서는 빈 MonoGame 프로젝트에 게임 논리 및 콘텐츠를 추가 하 여 터치 입력으로 이동 하는 애니메이션 스프라이트의 데모를 만드는 방법을 보여 줍니다._

이 연습의 이전 부분에서는 빈 MonoGame 프로젝트를 만드는 방법을 살펴보았습니다. 간단한 게임 데모를 수행 하 여 이러한 이전 부분을 빌드할 예정입니다. 이 문서에는 다음과 같은 섹션이 포함되어 있습니다.

- 게임 콘텐츠 압축 풀기
- MonoGame 클래스 개요
- 첫 번째 스프라이트 렌더링
- CharacterEntity 만들기
- 게임에 CharacterEntity 추가
- 애니메이션 클래스 만들기
- CharacterEntity에 첫 번째 애니메이션 추가
- 문자 이동 추가
- 일치 하는 이동 및 애니메이션


## <a name="unzipping-our-game-content"></a>게임 콘텐츠 압축 풀기

코드 작성을 시작 하기 전에 게임 *콘텐츠의*압축을 푸는 것이 좋습니다. 게임 개발자는 종종 *콘텐츠* 용어를 사용 하 여 일반적으로 시각적 아티스트, 게임 디자이너 또는 오디오 디자이너에서 만든 비 코드 파일을 참조 합니다. 일반적인 콘텐츠 형식에는 시각적 개체를 표시 하거나, 소리를 재생 하거나, AI (인공 지능) 동작을 제어 하는 데 사용 되는 파일이 포함 됩니다. 게임 개발 팀의 관점에서는 일반적으로 비 프로그래머에 의해 만들어집니다.

여기에 사용 된 콘텐츠는 [github에서](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true)찾을 수 있습니다. 이러한 파일은이 연습의 뒷부분에서 액세스할 위치로 다운로드 해야 합니다.

## <a name="monogame-class-overview"></a>MonoGame 클래스 개요

초보자를 위해 기본 렌더링에 사용 되는 MonoGame 클래스를 살펴보겠습니다.

- `SpriteBatch`– 화면에 2D 그래픽을 그리는 데 사용 됩니다. *스프라이트* 는 화면에 이미지를 표시 하는 데 사용 되는 2d 시각적 요소입니다. 개체 `SpriteBatch` `Begin` 는 및 메서드사이에서한번에하나의스프라이트를그리거나여러스프라이트를함께그룹화하거나일괄처리할수있습니다.`End`
- `Texture2D`– 런타임에 이미지 개체를 나타냅니다. `Texture2D`인스턴스는 런타임에 동적으로 만들어질 수도 있지만 .png 또는 .bmp와 같은 파일 형식에서 생성 되는 경우가 많습니다. `Texture2D`인스턴스는 인스턴스를 사용 하 `SpriteBatch` 여 렌더링할 때 사용 됩니다.
- `Vector2`– 시각적 개체의 위치를 지정 하는 데 자주 사용 되는 2D 좌표 시스템의 위치를 나타냅니다. MonoGame에도 `Vector3` 포함 `Vector4` 되어 있지만이 연습에서는 `Vector2` 만 사용 합니다.
- `Rectangle`– 위치, 너비 및 높이가 있는 4 면 영역입니다. 이를 사용 하 여 애니메이션 작업을 시작할 때 렌더링할 `Texture2D` 의 부분을 정의 합니다.

또한 네임 스페이스에도 불구 하 고 MonoGame은 Microsoft에서 유지 관리 되지 않습니다. `Microsoft.Xna` 네임 스페이스는 기존 XNA 프로젝트를 MonoGame로 쉽게 마이그레이션할 수 있도록 MonoGame에 사용 됩니다.

## <a name="rendering-our-first-sprite"></a>첫 번째 스프라이트 렌더링

다음으로는 MonoGame에서 2D 렌더링을 수행 하는 방법을 보여 주기 위해 단일 스프라이트를 화면에 그립니다.

### <a name="creating-a-texture2d"></a>Texture2D 만들기

스프라이트를 렌더링할 때 사용할 `Texture2D` 인스턴스를 만들어야 합니다. 모든 게임 콘텐츠는 플랫폼별 프로젝트에 있는 **content** 라는 폴더에 궁극적으로 포함 됩니다. MonoGame 공유 프로젝트는 콘텐츠를 포함할 수 없습니다. 콘텐츠는 해당 플랫폼과 관련 된 빌드 작업을 사용 해야 하기 때문입니다. 콘텐츠 폴더는 iOS 프로젝트에서, Android 프로젝트의 자산 폴더 내에서 찾을 수 있습니다.

게임 콘텐츠를 추가 하려면 **콘텐츠** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **추가 > 파일 추가** ...를 선택 합니다. .Zip 파일이 추출 된 위치로 이동 하 여 **charactersheet** 파일을 선택 합니다. 폴더에 파일을 추가 하는 방법에 대해 묻는 메시지가 표시 되 면 **복사** 옵션을 선택 해야 합니다.

![](part2-images/image1.png "폴더에 파일을 추가 하는 방법에 대해 묻는 메시지가 표시 되 면 복사 옵션을 선택 합니다.")

이제 콘텐츠 폴더에 charactersheet 파일이 포함 됩니다.

![](part2-images/image2.png "이제 콘텐츠 폴더에 charactersheet 파일이 포함 되어 있습니다.")

다음에는 charactersheet 파일을 로드 하 고를 `Texture2D`만드는 코드를 추가 합니다. 이렇게 하려면 `Game1.cs` 파일을 열고 Game1.cs 클래스에 다음 필드를 추가 합니다.

```csharp
Texture2D characterSheetTexture;
```

다음으로 `characterSheetTexture` `LoadContent` 메서드에서을 만듭니다. 수정 `LoadContent` 메서드는 다음과 같습니다.

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
    // TODO: use this.Content to load your game content here
}
```

기본 프로젝트는 이미 인스턴스를 `spriteBatch` 인스턴스화 하 고 있음을 확인 해야 합니다. 나중에이를 사용 하지만, 사용을 준비 하기 위해 추가 코드를 추가 하지 `spriteBatch` 는 않습니다. 반면에는 초기화가 필요 하므로를 만든 `spriteBatch` 후 초기화 합니다. `spriteSheetTexture`

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

이제 `SpriteBatch` `Game1.Draw` 인스턴스와 `Texture2D` 인스턴스가 있으므로 메서드에 렌더링 코드를 추가할 수 있습니다.

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

게임을 실행 하면 charactersheet에서 생성 된 질감이 표시 되는 단일 스프라이트가 표시 됩니다.

![](part2-images/image3.png "게임을 실행 하면 charactersheet에서 생성 된 질감이 표시 되는 단일 스프라이트가 표시 됩니다.")

## <a name="creating-the-characterentity"></a>CharacterEntity 만들기

지금까지 단일 스프라이트를 화면에 렌더링 하는 코드를 추가 했습니다. 그러나 다른 클래스를 만들지 않고 전체 게임을 개발 하는 경우 코드 조직 문제가 발생 합니다.

### <a name="what-is-an-entity"></a>엔터티 란?

게임 코드를 구성 하는 일반적인 패턴은 각 게임 *엔터티* 형식에 대 한 새 클래스를 만드는 것입니다. 게임 개발의 엔터티는 다음 특징 중 일부를 포함할 수 있는 개체입니다 (모두 필수는 아님).

- 스프라이트, 텍스트 또는 3D 모델과 같은 시각적 요소
- 설정 경로 또는 입력에 응답 하는 플레이어 문자를 patrolling 같은 단위 또는 모든 프레임 동작
- 플레이어에서 표시 하 고 수집 하는 전원과 같이 동적으로 만들고 제거할 수 있습니다.

엔터티 조직 시스템은 복잡할 수 있으며 많은 게임 엔진에서 엔터티를 관리 하는 데 사용할 수 있는 클래스를 제공 합니다. 매우 간단한 엔터티 시스템을 구현 하므로 전체 게임에는 일반적으로 개발자 파트에 대 한 더 많은 조직이 필요 합니다.


### <a name="defining-the-characterentity"></a>CharacterEntity 정의

이 엔터티는를 호출할 `CharacterEntity`때 다음과 같은 특징이 있습니다.

- 자체를 로드 하는 기능`Texture2D`
- 호출 메서드를 포함 하 여 탐색 애니메이션을 업데이트 하는 것을 포함 하 여 자신을 렌더링할 수 있습니다.
- `X`및 Y 속성을 통해 문자 위치를 제어할 수 있습니다.
- 자신을 업데이트 하 고, 터치 스크린에서 값을 읽고 위치를 적절 하 게 조정할 수 있습니다.

게임 `CharacterEntity` 에를 추가 하려면 **WalkingGame** 프로젝트를 마우스 오른쪽 단추로 클릭 하거나 마우스 오른쪽 단추로 클릭 하 고 **새 파일 > 추가**...를 선택 합니다. **빈 클래스** 옵션을 선택 하 고 새 파일의 이름을 **CharacterEntity**로 지정한 다음 **새로 만들기**를 클릭 합니다.

먼저를 `CharacterEntity` `Texture2D` 로드 하 고 자체를 그리기 위해를 로드 하는 기능을 추가 합니다. 새로 추가 `CharacterEntity.cs` 된 파일을 다음과 같이 수정 합니다.

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

위의 코드는 콘텐츠를의 `CharacterEntity`위치 지정, 렌더링 및 로드 하는 책임을 추가 합니다. 위의 코드에서 수행 하는 몇 가지 고려 사항을 살펴보겠습니다.

### <a name="why-are-x-and-y-floats"></a>X 및 Y Float는 무엇 인가요?

게임 프로그래밍을 처음 접하는 개발자는 `float` 형식이 `int` 또는 `double`와 반대로 사용 되는 이유를 궁금할 수 있습니다. 그 이유는 32 비트 값은 하위 수준 렌더링 코드에서 위치를 지정 하는 데 가장 일반적입니다. Double 형식은 정밀도에 64 비트를 차지 하며,이는 개체의 위치를 지정 하는 데 거의 필요 하지 않습니다. 32 비트 차이가 중요 하지 않을 수 있지만, 대부분의 최신 게임에는 각 프레임 (초당 30 개나 60 번)의 위치 값을 처리 해야 하는 그래픽이 포함 됩니다. 그래픽 파이프라인을 절반으로 이동 해야 하는 메모리 양을 잘라내면 게임의 성능에 상당한 영향을 줄 수 있습니다.

`int` 데이터 형식은 위치를 지정 하는 데 적절 한 측정 단위가 될 수 있지만, 엔터티가 배치 되는 방식에 대 한 제한 사항이 있을 수 있습니다. 예를 들어 정수 값을 사용 하면 확대/축소 또는 3D 카메라 (에서 `SpriteBatch`허용 됨)를 지 원하는 게임의 부드러운 움직임을 구현 하기가 더 어려워집니다.

마지막으로, 화면 주위에서 문자를 이동 하는 논리가 게임의 타이밍 값을 사용 하 여이 작업을 수행 하는 것을 확인할 수 있습니다. 지정 된 프레임에 전달 된 시간에 대 한 속도를 곱한 결과에는 대개 정수가 발생 하므로 및 `float` `Y`에 대해 `X` 를 사용 해야 합니다.

### <a name="why-is-charactersheettexture-static"></a>CharacterSheetTexture 정적 인가요?

`characterSheetTexture` 인스턴스는charactersheet파일`Texture2D` 의 런타임 표현입니다. 즉, 원본 **charactersheet** 파일에서 추출 된 각 픽셀의 색 값이 포함 됩니다. 두 개의 `CharacterEntity` 인스턴스를 만든 경우 각 인스턴스는 charactersheet의 정보에 액세스할 수 있어야 합니다. 이 상황에서는 charactersheet를 두 번 로드 하는 성능 비용을 초래 하지 않으며 ram에 중복 된 텍스처 메모리를 저장 하려고 합니다. 필드를 `static` 사용 하면 모든 `CharacterEntity` 인스턴스에서 동일 `Texture2D` 하 게 공유할 수 있습니다.

콘텐츠의 `static` 런타임 표현에 대 한 멤버를 사용 하는 것은 단순화 된 솔루션 이며 한 수준에서 다른 수준으로 이동할 때 콘텐츠 언로드와 같은 큰 게임에서 발생 하는 문제를 해결 하지 않습니다. 이 연습에서 다루지 않는 보다 정교한 솔루션에는 MonoGame의 콘텐츠 파이프라인을 사용 하거나 사용자 지정 콘텐츠 관리 시스템을 만드는 작업이 포함 됩니다.

### <a name="why-is-spritebatch-passed-as-an-argument-instead-of-instantiated-by-the-entity"></a>SpriteBatch가 엔터티에 의해 인스턴스화되지 않고 인수로 전달 되는 이유는 무엇 인가요?

위에서 `Draw` 구현 된 메서드는 `SpriteBatch` 인수를 사용 `characterSheetTexture` 하지만와는 달리는에 의해 `CharacterEntity`인스턴스화됩니다.

`SpriteBatch` 그 이유는 동일한 인스턴스가 모든 `Draw` 호출에 사용 될 때 그리고 단일 집합과 `Begin` `End` 호출 사이에 모든 `Draw` 호출이 수행 될 때 가장 효율적인 렌더링이 가능 하기 때문입니다. 물론,이 게임에는 단일 엔터티 인스턴스만 포함 되지만 여러 엔터티가 동일한 `SpriteBatch` 인스턴스를 사용할 수 있도록 하는 패턴을 사용 하면 더 복잡 한 게임의 이점을 누릴 수 있습니다.


## <a name="adding-characterentity-to-the-game"></a>게임에 CharacterEntity 추가

자신을 렌더링 하는 코드 `CharacterEntity` 와 함께를 추가 했으므로 이제이 새 엔터티의 인스턴스를 사용 `Game1.cs` 하도록의 코드를 바꿀 수 있습니다. 이렇게 하려면 `Texture2D` 필드 `CharacterEntity` 를의 `Game1`필드로 바꿉니다.

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

다음으로에서 `Game1.Initialize`이 엔터티의 인스턴스화를 추가 합니다.

```csharp
protected override void Initialize()
{
    character = new CharacterEntity (this.GraphicsDevice);

    base.Initialize();
}
```

또한 `Texture2D` 이제내`LoadContent` 에서 처리 되므로에서 만들기를 제거 해야 합니다.`CharacterEntity` `Game1.LoadContent`다음과 같이 표시 됩니다.

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
}
```

이름에도 불구 하 고, 메서드는 `LoadContent` 콘텐츠를 로드할 수 있는 유일한 장소가 아닙니다. 많은 게임은 Initialize 메서드에서 콘텐츠 로드를 구현 하거나, 업데이트 코드 에서도 콘텐츠를 구현 합니다. 플레이어가 게임의 특정 지점입니다.

마지막으로 그리기 메서드를 다음과 같이 수정할 수 있습니다.


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

게임을 실행 하는 경우 이제 문자가 표시 됩니다. X 및 Y의 기본값은 0 이므로 문자는 화면의 왼쪽 위 모퉁이에 배치 됩니다.

![](part2-images/image4.png "X 및 Y의 기본값은 0 이므로 문자는 화면의 왼쪽 위 모퉁이에 배치 됩니다.")

## <a name="creating-the-animation-class"></a>애니메이션 클래스 만들기

현재에는 전체 **charactersheet** 파일이 표시됩니다.`CharacterEntity` 한 파일에서 여러 이미지를 정렬 하는 것을 *sprite 시트*라고 합니다. 일반적으로 스프라이트는 스프라이트 시트의 일부만 렌더링 합니다. 이 charactersheet의 일부 `CharacterEntity` 를 렌더링 하도록를 수정 합니다.이 부분은 탐색 애니메이션을 표시 하기 위해 시간이 지남에 따라 변경 됩니다.

CharacterEntity 애니메이션의 논리 `Animation` 와 상태를 제어 하는 클래스를 만듭니다. 애니메이션 클래스는 애니메이션 뿐만 `CharacterEntity` 아니라 모든 엔터티에 사용할 수 있는 일반 클래스가 됩니다. Ultimate 클래스는가 자신을 `Rectangle` 그릴 때 `CharacterEntity` 사용할를 제공 합니다. `Animation` 또한 애니메이션을 정의 하 `AnimationFrame` 는 데 사용 되는 클래스를 만듭니다.


### <a name="defining-animationframe"></a>애니메이션 프레임 정의

`AnimationFrame`애니메이션에 관련 된 논리를 포함 하지 않습니다. 데이터를 저장 하는 데에만 사용 됩니다. `AnimationFrame` 클래스를 추가 하려면 **WalkingGame** 공유 프로젝트를 마우스 오른쪽 단추로 클릭 하거나 마우스 오른쪽 단추로 클릭 하 고 **새 파일 > 추가** ...를 선택 합니다. 이름 **애니메이션 프레임** 을 입력 하 고 **새로 만들기** 단추를 클릭 합니다. 다음 코드를 포함 `AnimationFrame.cs` 하도록 파일을 수정 합니다.


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

클래스 `AnimationFrame` 에는 다음 두 가지 정보가 포함 되어 있습니다.

- `SourceRectangle`–에 `Texture2D` `AnimationFrame`의해 표시 되는의 영역을 정의 합니다. 이 값은 픽셀 단위로 측정 되며 왼쪽 위는 (0, 0)입니다.
- `Duration``AnimationFrame` –`Animation`에서 사용 될 때가 표시 되는 기간을 정의 합니다.

### <a name="defining-animation"></a>애니메이션 정의

클래스 `Animation` 에는 `List<AnimationFrame>` 전달 된 시간에 따라 현재 표시 되는 프레임을 전환 하는 논리가 포함 됩니다.

`Animation` 클래스를 추가 하려면 **WalkingGame** 공유 프로젝트를 마우스 오른쪽 단추로 클릭 하거나 마우스 오른쪽 단추로 클릭 하 고 **새 파일 > 추가**...를 선택 합니다. 이름 **애니메이션** 을 입력 하 고 **새로 만들기** 단추를 클릭 합니다. 다음 코드를 포함 `Animation.cs` 하도록 파일을 수정 합니다.


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

`Animation` 클래스의 세부 정보 중 일부를 살펴보겠습니다.

### <a name="frames-list"></a>프레임 목록

멤버 `frames` 는 애니메이션에 대 한 데이터를 저장 하는 것입니다. 애니메이션을 인스턴스화하는 코드는 메서드를 `AnimationFrame` `AddFrame` 통해 `frames` 목록에 인스턴스를 추가 합니다. 보다 완전 한 구현은 수정 `public` `frames`에 대 한 메서드 또는 속성을 제공할 수 있지만이 연습에서 프레임을 추가 하는 기능을 제한 합니다.

### <a name="duration"></a>Duration

Duration은 포함 `AnimationFrame` 된 모든 인스턴스의 기간 `Animation,` 을 추가 하 여 가져온의 총 기간을 반환 합니다. 이 값은 변경할 수 없는 `AnimationFrame` 개체 였던 경우에 캐시 될 수 있지만 애니메이션에 추가 된 후 변경할 수 있는 클래스로 프레임을 구현 했으므로 속성에 액세스할 때마다이 값을 계산 해야 합니다.

### <a name="update"></a>업데이트

메서드 `Update` 는 모든 프레임을 호출 해야 합니다. 즉, 전체 게임이 업데이트 될 때마다이 메서드를 호출 해야 합니다. 이는 현재 표시 된 프레임 `timeIntoAnimation` 을 반환 하는 데 사용 되는 멤버를 늘리기 위한 것입니다. 의 `Update` 논리는가 전체 `timeIntoAnimation` 애니메이션 기간 보다 크지 않도록 방지 합니다.

클래스를 `Animation` 사용 하 여 탐색 애니메이션을 표시 하므로 끝에 도달 했을 때이 애니메이션을 반복 하려고 합니다. 이는이 `timeIntoAnimation` `Duration` 값 보다 큰지 여부를 확인 하 여 수행할 수 있습니다. 이 경우 시작 부분으로 다시 이동 하 여 나머지를 유지 하므로 반복 애니메이션이 생성 됩니다.

### <a name="obtaining-the-current-frames-rectangle"></a>현재 프레임의 사각형 가져오기

`Animation` 클래스의 용도는 궁극적으로 스프라이트를 그릴 때 `Rectangle` 사용할 수 있는를 제공 합니다. 현재 클래스 `Animation` 에는 `timeIntoAnimation` 멤버를 변경 하는 논리가 포함 되어 있으며이는 표시 되어야 `Rectangle` 하는을 가져오는 데 사용 됩니다. 클래스에서 속성을 `CurrentRectangle` 만들어이 작업을 수행 합니다. `Animation` 이 속성을 클래스에 `Animation` 복사 합니다.

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

## <a name="adding-the-first-animation-to-characterentity"></a>CharacterEntity에 첫 번째 애니메이션 추가

에 `CharacterEntity` 는 현재 `Animation` 표시 되는에 대 한 참조 뿐만 아니라 탐색 및 발생에 대 한 애니메이션도 포함 됩니다.

먼저 지금까지 작성 된 기능을 `Animation,` 테스트 하는 데 사용 하는 첫 번째를 추가 합니다. CharacterEntity 클래스에 다음 멤버를 추가 해 보겠습니다.

```csharp
Animation walkDown;
Animation currentAnimation;
```

다음으로 `walkDown` 애니메이션을 정의 하겠습니다. 이렇게 하려면 생성자를 다음과 `CharacterEntity` 같이 수정 합니다.

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

앞서 언급 했 듯이 시간 기반 애니메이션을 `Animation.Update` 재생 하려면를 호출 해야 합니다. 또한도 할당 `currentAnimation`해야 합니다. 지금은 `currentAnimation` 를에 `walkDown`할당 하지만 이동 논리를 구현할 때 나중에이 코드를 바꿀 예정입니다. 다음과 같이에 `CharacterEntity` 메서드 `Update` 를 추가 합니다.


```csharp
public void Update(GameTime gameTime)
{
    // temporary - we'll replace this with logic based off of which way the
    // character is moving when we add movement logic
    currentAnimation = walkDown;

    currentAnimation.Update (gameTime);
}
```

이제를 `currentAnimation` 할당 하 고 업데이트 했으므로 그리기를 수행 하는 데 사용할 수 있습니다. 다음과 같이 수정 `CharacterEntity.Draw` 합니다.

```csharp
public void Draw(SpriteBatch spriteBatch)
{
    Vector2 topLeftOfSprite = new Vector2 (this.X, this.Y);
    Color tintColor = Color.White;
    var sourceRectangle = currentAnimation.CurrentRectangle;

    spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, sourceRectangle, Color.White);
}
```

`CharacterEntity` 애니메이션을 얻기 위한 마지막 단계는에서 `Game1`새로 추가 `Update` 된 메서드를 호출 하는 것입니다. 다음과 `Game1.Update` 같이 수정 합니다.

```csharp
protected override void Update(GameTime gameTime)
{
    character.Update (gameTime);
    base.Update(gameTime);
}
```

이제는 `CharacterEntity` `walkDown` 애니메이션을 재생 합니다.

![](part2-images/image5.gif "이제 CharacterEntity가 walkDown 애니메이션을 재생 합니다.")

## <a name="adding-movement-to-the-character"></a>문자 이동 추가

다음으로 터치 컨트롤을 사용 하 여 문자 이동을 추가 합니다. 사용자가 화면에 닿을 때 문자는 화면이 이동 하는 지점 쪽으로 이동 됩니다. 검색 된 접촉이 없으면 문자는 적용 되지 않습니다.

### <a name="defining-getdesiredvelocityfrominput"></a>GetDesiredVelocityFromInput 정의

터치 스크린의 현재 상태에 `TouchPanel` 대 한 정보를 제공 하는 MonoGame의 클래스를 사용 합니다. 를 `TouchPanel` 확인 하 고 문자의 원하는 속도를 반환 하는 메서드를 추가 해 보겠습니다.


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

메서드 `TouchPanel.GetState` 는 사용자가 `TouchCollection` 화면에 접촉 하는 위치에 대 한 정보를 포함 하는을 반환 합니다. 사용자가 화면을 건드리지 않는 경우는 비어 있습니다. `TouchCollection` 이 경우에는 문자를 이동 하지 않아야 합니다.

사용자가 화면을 터치 하는 경우 첫 번째 터치로 문자를 이동 합니다. 즉, `TouchLocation` 인덱스 0에서로 이동 합니다. 처음에는 문자 위치와 첫 번째 터치 위치의 차이와 같도록 원하는 속도를 설정 합니다.

```csharp
        desiredVelocity.X = touchCollection [0].Position.X - this.X;
        desiredVelocity.Y = touchCollection [0].Position.Y - this.Y;
```

다음은 문자를 동일한 속도로 이동 하는 수치 연산입니다. 이 점이 중요 한 이유를 설명 하기 위해 사용자가 문자가 위치한 위치에서 500 픽셀을 벗어나 화면을 접촉 하는 상황을 살펴보겠습니다. `desiredVelocity.X` 가 설정 된 첫 번째 줄은 500 값을 할당 합니다. 그러나 사용자가 문자에서 100 단위 까지만 화면에 접촉 하는 경우는 `desiredVelocity.X` 100으로 설정 됩니다. 결과적으로 문자 이동 속도가 문자에서 접촉 하는 거리에 응답 하 게 됩니다. 문자를 항상 동일한 속도로 이동 하려면 desiredVelocity을 수정 해야 합니다.

`if (desiredVelocity.X != 0 || desiredVelocity.Y != 0)` 문은 속도가 0이 아닌지 여부를 확인 합니다. 즉, 사용자가 문자의 현재 위치와 동일한 위치에 접촉 하 고 있지 않은지 확인 합니다. 그렇지 않으면 터치의 거리에 관계 없이 문자의 속도를 상수로 설정 해야 합니다. 이는 속도 벡터를 정규화 하 여 길이가 1 인 속도 벡터를 표준화 하 여 수행 합니다. 속도 벡터 1은 문자가 1 픽셀에서 초당 이동 한다는 것을 의미 합니다. 값을 원하는 속도의 200에 곱하여 속도를 높이는 것이 좋습니다.


### <a name="applying-velocity-to-position"></a>위치에 속도 적용

에서 `GetDesiredVelocityFromInput` 반환 된 속도는 런타임에 적용 되는 `X` 문자 및 `Y` 값에 적용 해야 합니다. 메서드를 다음과 같이 `Update` 수정 합니다.

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

여기에서 구현 된 기능을 *시간 기반* 이동 이라고 합니다 ( *프레임 기반* 이동과 반대). 시간 기반 이동은에 `velocity` `gameTime.ElapsedGameTime.TotalSeconds`저장 된 마지막 업데이트 이후 경과 된 시간을 기준으로 속도 값 (이 경우 변수에 저장 된 값)을 곱합니다. 게임에서 초당 프레임 수가 더 적으면 프레임 간에 경과 된 시간이 발생 합니다. 즉, 시간 기반 이동을 사용 하는 개체가 프레임 속도에 관계 없이 항상 동일한 속도로 이동 합니다.

지금 게임을 실행 하는 경우 문자가 터치 위치로 이동 하는 것을 알 수 있습니다.

![](part2-images/image6.gif "문자가 터치 위치로 이동 하 고 있습니다.")

## <a name="matching-movement-and-animation"></a>일치 하는 이동 및 애니메이션

문자 이동 및 단일 애니메이션 재생을 마치면 애니메이션의 나머지를 정의한 다음이를 사용 하 여 문자의 움직임을 반영할 수 있습니다. 완료 되 면 총 8 개의 애니메이션을 갖게 됩니다.

- 위쪽, 아래쪽, 왼쪽 및 오른쪽으로 이동 하는 애니메이션
- 계속 해 서 위쪽, 아래쪽, 왼쪽 및 오른쪽으로 향하는 애니메이션

### <a name="defining-the-rest-of-the-animations"></a>나머지 애니메이션 정의

먼저가 추가 `Animation` `CharacterEntity` 된것과동일한위치에있는모든애니메이션에대한인스턴스를클래스`walkDown`에 추가 합니다. 이 작업을 수행 하면에 `CharacterEntity` 는 다음 `Animation` 멤버가 포함 됩니다.

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

이제 다음과 같이 `CharacterEntity` 생성자에서 애니메이션을 정의 합니다.

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

이 연습을 더 짧게 유지 하기 위해 위의 코드가 `CharacterEntity` 생성자에 추가 되었다는 점에 유의 해야 합니다. 게임은 일반적으로 문자 애니메이션의 정의를 고유한 클래스로 구분 하거나 XML 또는 JSON과 같은 데이터 형식에서이 정보를 로드 합니다.

다음으로, 문자가 이동 하는 방향에 따라 또는 문자가 방금 중지 된 경우 마지막 애니메이션에 따라 애니메이션을 사용 하도록 논리를 조정 합니다. 이렇게 하려면 메서드를 수정 합니다 `Update` .


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

애니메이션을 전환 하는 코드는 두 개의 블록으로 분할 됩니다. 첫 번째는 속도가와 같지 `Vector2.Zero` 않은지 여부를 확인 합니다 .이는 문자 이동 여부를 알려 줍니다. 문자를 이동 하는 경우 `velocity.X` 및 `velocity.Y` 값을 확인 하 여 재생할 탐색 애니메이션을 결정할 수 있습니다.

문자가 이동 하지 않는 경우에는 문자 `currentAnimation` 를 단일 애니메이션으로 설정 해야 합니다 `currentAnimation` . 그러나가 탐색 애니메이션 이거나 애니메이션이 설정 되지 않은 경우에만이를 수행 합니다. 가 4 개의 탐색 애니메이션 중 하나가 아닌 경우 문자는 이미 존재 하므로 변경할 `currentAnimation`필요가 없습니다. `currentAnimation`

이 코드의 결과는 문자를 탐색 하는 동안 적절 하 게 애니메이션 효과를 적용 한 다음 중지 될 때 이동한 마지막 방향을 face 하는 것입니다.

![](part2-images/image7.gif "이 코드의 결과는 문자를 탐색 하는 동안 적절 하 게 애니메이션 효과를 적용 한 다음, 중지할 때 이동한 마지막 방향을 face 하는 것입니다.")

## <a name="summary"></a>요약

이 연습에서는 MonoGame를 사용 하 여 스프라이트, 이동 개체, 입력 검색 및 애니메이션을 포함 하는 플랫폼 간 게임을 만드는 방법을 살펴보았습니다. 범용 애니메이션 클래스 만들기에 대해 설명 합니다. 또한 코드 논리를 구성 하기 위해 문자 엔터티를 만드는 방법을 보여 주었습니다.

## <a name="related-links"></a>관련 링크

- [CharacterSheet Image 리소스 (샘플)](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true)
- [탐색 게임 완료 (샘플)](https://docs.microsoft.com/samples/xamarin/mobile-samples/walkinggamemg/)
