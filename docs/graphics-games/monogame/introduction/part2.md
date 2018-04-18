---
title: Part 2-는 WalkingGame 구현
description: 이 연습에서는 터치식 입력 게임 논리 및 콘텐츠를 사용 하 여 이동 하는 애니메이션된 sprite의 데모를 만드는 빈 MonoGame 프로젝트를 추가 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: F0622A01-DE7F-451A-A51F-129876AB6FFD
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: bc4ab2e77bfce9c9ba6043533bcfda5a359d322e
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2018
---
# <a name="part-2--implementing-the-walkinggame"></a>Part 2-는 WalkingGame 구현

_이 연습에서는 터치식 입력 게임 논리 및 콘텐츠를 사용 하 여 이동 하는 애니메이션된 sprite의 데모를 만드는 빈 MonoGame 프로젝트를 추가 하는 방법을 보여 줍니다._

이 연습의 이전 부분에는 빈 MonoGame 프로젝트를 만드는 방법을 보여 주었습니다. 간단한 게임 데모를 늘려 이러한 이전 부분에 작성 합니다. 이 자료에는 다음과 같은 섹션이 포함되어 있습니다.

- 게임 콘텐츠 압축을 풀기
- MonoGame 클래스 개요
- 첫 번째 우리의 Sprite 렌더링
- CharacterEntity 만들기
- 게임에 CharacterEntity 추가
- 애니메이션 클래스 만들기
- CharacterEntity에 첫 번째 애니메이션 추가
- 문자를 추가 이동
- 일치 하는 이동 및 애니메이션


## <a name="unzipping-our-game-content"></a>게임 콘텐츠 압축을 풀기

게임의 압축을 푸는 하고자 코드 작성을 시작 하기 전에 *콘텐츠*합니다. 이라는 용어를 자주 사용 하는 게임 개발자 *콘텐츠* 일반적으로 시각적 아티스트, 게임 디자이너 또는 오디오 디자이너에 의해 생성 되는 비 코드 파일을 참조 합니다. 일반적인 콘텐츠 유형의 시각적 개체를 표시 하거나, 소리를 재생 하거나 인공 인텔리전스 (AI) 동작을 제어 하는 데 사용 하는 파일을 포함 합니다. 게임 개발에서 팀의 관점 콘텐츠는 프로그래머가 아닌 일반적으로 만들어집니다.

여기에 콘텐츠를 찾을 수 [github에서](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true)합니다. 이러한 파일을이 연습의 뒷부분에서 액세스 하는 위치에 다운로드 해야 합니다.

## <a name="monogame-class-overview"></a>MonoGame 클래스 개요

먼저 기본 렌더링에 사용 되는 MonoGame 클래스를 설명 합니다.

- `SpriteBatch` -2 차원 그래픽 화면을 그리는 데 사용 합니다. *스프라이트* 은 화면에 이미지를 표시 하는 데 사용 되는 2D 시각적 요소입니다. `SpriteBatch` 개체 간에 한 번에 하나의 스프라이트를 그릴 수 해당 `Begin` 및 `End` 메서드 또는 여러 스프라이트 함께 그룹화 할 수 있습니다 또는 *일괄 처리 된*합니다.
- `Texture2D` – 런타임에 이미지 개체를 나타냅니다. `Texture2D` 인스턴스가 만들 수 있습니다 동적으로 런타임에 있지만 종종 예:.bmp 또는.png 파일 형식에서 만들어집니다. `Texture2D` 사용 하 여 렌더링 하는 경우 사용 되는 인스턴스 `SpriteBatch` 인스턴스.
- `Vector2` – 시각적 개체를 배치 하기 위한 자주 사용 되는 2 차원 좌표 시스템의 위치를 나타냅니다. MonoGame도 포함 되어 `Vector3` 및 `Vector4` 만 사용 하 여 하지만 `Vector2` 이 연습에서 합니다.
- `Rectangle` -위치, 너비 및 높이와 네 면 영역입니다. 사용할 예정이 부분을 정의 하는 `Texture2D` 애니메이션이 있는 작업을 시작 하는 경우 렌더링 하 합니다.

또한 드릴 네임 스페이스에도 불구 하 고 MonoGame – Microsoft에서 유지 되지 않습니다. `Microsoft.Xna` MonoGame 기존 XNA 프로젝트로 쉽게 수행할 수 있도록 MonoGame에 네임 스페이스가 사용 됩니다.

## <a name="rendering-our-first-sprite"></a>첫 번째 우리의 Sprite 렌더링

다음 단일 스프라이트 MonoGame에서 2D 렌더링을 수행 하는 방법을 보여 주는 화면으로 그립니다 했습니다.

### <a name="creating-a-texture2d"></a>Texture2D 만들기

만들어야 하는 `Texture2D` 우리의 sprite 렌더링할 때 사용할 인스턴스. 모든 게임 콘텐츠 폴더에 포함 된 궁극적으로 **콘텐츠를** 플랫폼별 프로젝트에 있습니다. 콘텐츠 플랫폼에 특정 빌드 작업을 사용 해야 하는 대로 MonoGame 공유 프로젝트에서 콘텐츠를 포함할 수 없습니다. CocosSharp 개발자가 콘텐츠 폴더 익숙한 개념을 찾을 수 – CocosSharp 및 MonoGame 프로젝트 모두에서 같은 위치에 있습니다. IOS 프로젝트에 있고 Android 프로젝트의 자산 폴더 안에 콘텐츠 폴더를 찾을 수 있습니다.

이 게임의 콘텐츠를 추가 하려면 마우스 오른쪽 단추로 클릭는 **콘텐츠** 폴더를 선택 **추가 > 파일 추가 중...** Content.zip 파일 추출 된 위치로 이동 하 고 선택 된 **charactersheet.png** 파일입니다. 폴더에 파일을 추가 하는 방법에 대 한 요청 되 면 선택 해야는 **복사** 옵션:

![](part2-images/image1.png "폴더에 파일을 추가 하는 방법에 대 한 요청 되 면 복사 옵션을 선택 합니다.")

콘텐츠 폴더에는 이제 charactersheet.png 파일이 들어 있습니다.

![](part2-images/image2.png "콘텐츠 폴더에는 이제 charactersheet.png 파일 포함")

Charactersheet.png 파일을 로드 하 고 만드는 코드를 추가 하겠습니다 다음으로 `Texture2D`합니다. 이 열기 작업을 수행 하는 `Game1.cs` 파일을 Game1.cs 클래스에 다음 필드를 추가 합니다.

```csharp
Texture2D characterSheetTexture;
```

다음으로, 만들겠습니다는 `characterSheetTexture` 에 `LoadContent` 메서드. 수정 하기 전에 `LoadContent` 메서드는 다음과 같습니다.

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
    // TODO: use this.Content to load your game content here
}
```

기본 프로젝트에는 이미 인스턴스화합니다 드릴는 `spriteBatch` 인스턴스를 수행해 줍니다. 사용할 예정이 나중에 반드시 म 않습니다 수를 준비 하는 데 추가 코드는 `spriteBatch` 사용 합니다. 반면에 우리의 `spriteSheetTexture` 초기화 후에 메서드는 하므로 초기화 않아도 `spriteBatch` 만들어집니다.

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
    using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
    {
        characterSheetTexture = Texture2D.FromStream (this.GraphicsDevice, stream);

    }
}
```

이제는 `SpriteBatch` 인스턴스 및 `Texture2D` 당사의 렌더링 코드를 추가할 수 있는 인스턴스는 `Game1.Draw` 메서드:

```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    spriteBatch.Begin ();

    Vector2 topLeftOfSprite = new Vector2 (50, 50);
    Color tintColor = Color.White;

    spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, tintColor);

    spriteBatch.End ();

    base.Draw(gameTime);
}
```

게임을 지금 실행 charactersheet.png에서 만든 질감을 표시 하는 단일 sprite 보여 줍니다.

![](part2-images/image3.png "게임을 지금 실행 charactersheet.png에서 만든 질감을 표시 하는 단일 sprite 보여 줍니다.")

## <a name="creating-the-characterentity"></a>CharacterEntity 만들기

지금까지 단일 스프라이트 화면에 렌더링 하는 코드를 추가 했습니다. 그러나 다른 클래스를 만들지 않고 전체 게임을 개발 하 고 있었습니다 우리 코드 조직 문제에 실행 됩니다.

### <a name="what-is-an-entity"></a>엔터티는 무엇입니까?

각 게임에 대 한 새 클래스를 만드는 게임 코드를 구성 하기 위한 일반적인 패턴은 *엔터티* 유형입니다. 게임 개발에 엔터티 (필수는 일부만)는 다음과 같은 특징을 포함할 수 있는 개체는:

- Sprite, 텍스트 또는 3D 모델 등의 시각적 요소
- 물리 또는 경로 설정 또는 입력에 응답 플레이어 정찰 단위와 같은 모든 프레임 동작
- 만들고 전원을 표시 되 고 플레이어에 의해 수집 되 같이 동적으로 소멸 수

엔터티 조직 시스템 복잡할 수 있으며 대부분의 게임 엔진 엔터티를 관리할 수 있도록 클래스를 제공 합니다. 에서는 전체 게임 일반적으로 개발자의 부분에 더 많은 조직에 필요한 주목할 만한 되기 때문 매우 간단한 엔터티 시스템을 구현 될 합니다.


### <a name="defining-the-characterentity"></a>CharacterEntity 정의

호출할 우리의 엔터티 `CharacterEntity`, 다음과 같은 특징을 갖습니다.

- 자체를 로드 하는 기능 `Texture2D`
- 워크 애니메이션을 업데이트 하려면 호출 메서드가 들어 있는 포함 하 여 자체를 렌더링 하는 기능
- `X` 및 Y 속성 제어 문자의 위치입니다.
- 업데이트 되도록 – 구체적으로, 터치 값 화면 및 위치를 적절 하 게 조정을 읽을 수 있습니다.

추가 하려면는 `CharacterEntity` 우리의 게임, 마우스 오른쪽 단추로 클릭 또는 컨트롤 클릭 하 고 **WalkingGame** 선택한 프로젝트 **추가 > 새 파일...** . 선택 된 **빈 클래스** 옵션 및 새 파일 이름을 **CharacterEntity**, 클릭 **새로**합니다.

수 있는 기능을 추가 하겠습니다 먼저는 `CharacterEntity` 로드 하는 `Texture2D` 뿐만 아니라 자체 그리기에 대 한 합니다. 새로 추가한 수정 됩니다. 여기서 `CharacterEntity.cs` 다음과 같이 파일:

```csharp
using System;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Input.Touch;


namespace WalkingGame
{
    public class CharacterEntity
    {
        static Texture2D characterSheetTexture;

        public float X
        {
            get;
            set;
        }

        public float Y
        {
            get;
            set;
        }

        public CharacterEntity (GraphicsDevice graphicsDevice)
        {
            if (characterSheetTexture == null)
            {
                using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
                {
                    characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
                }
            }
        }

        public void Draw(SpriteBatch spriteBatch)
        {
            Vector2 topLeftOfSprite = new Vector2 (this.X, this.Y);
            Color tintColor = Color.White;
            spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, tintColor);
        }
    }
}
```

위치, 렌더링 및 콘텐츠를 로드 하의 책임을 추가 하는 위의 코드는 `CharacterEntity`합니다. 위의 코드에서 수행 되는 몇 가지 고려 사항은 간단히 살펴보겠습니다.

### <a name="why-are-x-and-y-floats"></a>이유는 X 및 Y 부동 소수점?

게임 프로그래밍을 처음 접하는 개발자도 이유 궁금할는 `float` 반대인 유형을 사용 중인 `int` 또는 `double`합니다. 32 비트 값이 낮은 수준의 렌더링 코드의 위치 지정에 대 한 가장 일반적인 원인은입니다. Double 형식의 하는 개체의 위치를 지정 하는 데 필요한 거의 정밀도 대 한 64 비트를 차지 합니다. 32 비트 차이 무효 보일 수 있지만, 대부분의 최신 게임 그래픽이 포함 수만 위치 값을 처리 해야 하는 각 프레임 (30 또는 초 당 60 시간). 이동 해야 하는 메모리 양을 잘라내기 그래픽을 통해 파이프라인 절반으로 영향을 줄 수 상당한 게임의 성능에 있습니다.

`int` 데이터 형식을 적절 한 위치 지정에 대 한 측정 단위 수 그러나 엔터티 배치 하는 방법에 제한 사항을 배치할 수 것입니다. 예를 들어 정수 값을 사용 하 여 더 어려워집니다 게임을 확대 하거나 3D 카메라 지원에 대 한 부드러운 이동을 구현 하려면 (에서 허용 하는 `SpriteBatch`).

마지막으로,이 문자는 화면으로 이동 하는 논리 게임의 타이밍 값을 사용 하 여 수행 합니다 표시 됩니다. 지정된 된 프레임에 얼마나 많은 시간이 경과 하 여 개발 속도 곱한 결과를 거의 됩니다 정수를 사용 해야 하므로 `float` 에 대 한 `X` 및 `Y`합니다.

### <a name="why-is-charactersheettexture-static"></a>이유는 characterSheetTexture 정적?

`characterSheetTexture` `Texture2D` charactersheet.png 파일의 런타임 표현 합니다. 원본에서 추출 된 각 픽셀의 색상 값 포함 하는 즉, **charactersheet.png** 파일입니다. 게임을 두 개 만든 경우 `CharacterEntity` 인스턴스를 각 charactersheet.png에서 정보 액세스 해야 합니다. Charactersheet.png를 두 번 로드의 성능 비용을 발생 시 키 지 원하는 없게이 경우에도 중복 된 질감 메모리를 ram에 저장 하려고 합니다. 사용 하는 `static` 필드를 사용 하면 동일한 공유 하려면 `Texture2D` 모든 `CharacterEntity` 인스턴스.

사용 하 여 `static` 내용의 런타임 표현에 대 한 멤버는 단순한 솔루션 및 한 수준에서 이동할 때 언로드 콘텐츠와 같은 대규모 게임에서 발생 하는 문제를 해결 하지는 않습니다. 이 연습에서 다루지 않는 더욱 정교한 솔루션 MonoGame의 콘텐츠 파이프라인을 사용 하 여 만들거나 사용자 지정 콘텐츠 관리 시스템을 포함 합니다.

### <a name="why-is-spritebatch-passed-as-an-argument-instead-of-instantiated-by-the-entity"></a>이유은 SpriteBatch 전달으로 엔터티에서 인수 Instead of 인스턴스화됩니다.

`Draw` 하나 위에 구현 된 메서드는 `SpriteBatch` 반대로 하지만 인수는 `characterSheetTexture` 의해 인스턴스화됩니다는 `CharacterEntity`합니다.

그 이유는 가장 효율적인 렌더링은 가능한 때문에 동일한 `SpriteBatch` 인스턴스 모두에 사용 됩니다 `Draw` 호출, 시기 및 모든 `Draw` 단일 집합 간의 호출이 수행 되는지 `Begin` 및 `End` 호출 합니다. 물론, 게임 단일 엔터티 인스턴스를 포함 하지만 더 복잡 한 게임은 동일한 사용 하는 여러 엔터티를 허용 하는 패턴에서 이익을 얻을 `SpriteBatch` 인스턴스.


## <a name="adding-characterentity-to-the-game"></a>게임에 CharacterEntity 추가

추가한 했으므로 우리의 `CharacterEntity` 렌더링 자체에 대 한 코드로 코드를 대체할 수 있습니다 `Game1.cs` 새이 엔터티의 인스턴스를 사용 합니다. 에서는 바꿉니다이 작업을 수행 하는 `Texture2D` 필드와 한 `CharacterEntity` 필드에 `Game1`:

```csharp
public class Game1 : Game
{
    GraphicsDeviceManager graphics;
    SpriteBatch spriteBatch;
    // New code:
    CharacterEntity character;

    public Game1()
    {
    ...
```

다음으로,이 엔터티를 인스턴스화 추가 `Game1.Initialize`:

```csharp
protected override void Initialize()
{
    character = new CharacterEntity (this.GraphicsDevice);

    base.Initialize();
}
```

또한 제거 해야는 `Texture2D` 에서는 만들 `LoadContent` 이제 내에 처리 된 후 우리의 `CharacterEntity`합니다. `Game1.LoadContent` 다음과 같이 표시 됩니다.

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
}
```

이름과 달리 하 주목할는 `LoadContent` 메서드는 유일한 공간 여기서 콘텐츠를 로드할 수 – 콘텐츠 많은 게임 구현 해당 초기화 메서드에서 또는 내용으로 업데이트 코드에도 로드는 필요 하지 않습니다 플레이어에 도달할 때까지 하지는 게임의 특정 지점입니다.

마지막으로 그리기 메서드를 다음과 같이 수정할 수 있습니다.


```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    // We'll start all of our drawing here:
    spriteBatch.Begin ();

    // Now we can do any entity rendering:
    character.Draw(spriteBatch);
    // End renders all sprites to the screen:
    spriteBatch.End ();

    base.Draw(gameTime);
}
```

게임을 실행 하는 경우 이제 문자를 살펴봅니다. X 및 Y의 기본값 0, 이후 문자 화면의 왼쪽된 위 모퉁이 대 한 배치 됩니다.

![](part2-images/image4.png "X 및 Y의 기본값 0, 이후 다음 문자 위치가 화면의 왼쪽된 위 모퉁이 대 한")

## <a name="creating-the-animation-class"></a>애니메이션 클래스 만들기

현재 우리의 `CharacterEntity` 전체 표시 **charactersheet.png** 파일입니다. 이 배열을 하나의 파일에 여러 이미지가 라고는 *sprite 시트*합니다. 일반적으로 스프라이트 sprite 시트의 일부분만 렌더링 됩니다. 수정 됩니다. 여기서는 `CharacterEntity` 이 부분 렌더링 **charactersheet.png**,이 부분 워크 애니메이션을 표시 하는 시간이 지남에 따라 변경 됩니다.

만듭니다는 `Animation` 논리 및 CharacterEntity 애니메이션의 상태를 제어 하는 클래스입니다. 애니메이션 클래스 뿐 아니라 모든 엔터티에 대 한 사용할 수 있는 일반 클래스 됩니다 `CharacterEntity` 애니메이션 합니다. Ultimate는 `Animation` 클래스가 제공할는 `Rectangle` 있는 `CharacterEntity` 자체를 그릴 때 사용 됩니다. 도 만듭니다는 `AnimationFrame` 애니메이션을 정의에 사용 되는 클래스입니다.


### <a name="defining-animationframe"></a>AnimationFrame 정의

`AnimationFrame` 애니메이션에 관련 된 모든 논리를 포함 되지 않습니다. 사용할 예정 것만 데이터를 저장 합니다. 추가 하려면는 `AnimationFrame` 클래스 오른쪽 단추로 클릭 하거나 컨트롤 클릭 하 고 **WalkingGame** 공유 프로젝트 및 선택 **추가 > 새 파일...** 이름을 입력 **AnimationFrame** 클릭는 **새로** 단추입니다. 수정 합니다는 `AnimationFrame.cs` 다음 코드를 포함 되도록 파일:


```csharp
using System;
using Microsoft.Xna.Framework;

namespace WalkingGame
{
    public class AnimationFrame
    {
        public Rectangle SourceRectangle { get; set; }
        public TimeSpan Duration { get; set; }
    }
}
```

`AnimationFrame` 클래스 라는 두 가지 정보를 포함 합니다.

- `SourceRectangle` – 영역을 정의 `Texture2D` 으로 표시 됩니다는 `AnimationFrame`합니다. 이 값은 위 왼쪽 되 고 (0, 0)를 픽셀 단위로 지정 합니다.
- `Duration` -정의 기간는 `AnimationFrame` 에서 사용 될 때 표시 되는 `Animation`합니다.

### <a name="defining-animation"></a>애니메이션을 정의합니다.

`Animation` 클래스에 포함 됩니다는 `List<AnimationFrame>` 전환 프레임 시간 경과 따라 현재 표시 되어 논리 뿐만 아니라 합니다.

추가 하려면는 `Animation` 클래스 오른쪽 단추로 클릭 하거나 컨트롤 클릭 하 고 **WalkingGame** 공유 프로젝트 및 선택 **추가 > 새 파일...** . 이름을 입력 **애니메이션** 클릭는 **새로** 단추입니다. 수정 합니다는 `Animation.cs` 파일 이므로 다음 코드를 포함 합니다.


```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Microsoft.Xna.Framework;

namespace WalkingGame
{
    public class Animation
    {
        List<AnimationFrame> frames = new List<AnimationFrame>();
        TimeSpan timeIntoAnimation;

        TimeSpan Duration
        {
            get
            {
                double totalSeconds = 0;
                foreach (var frame in frames)
                {
                    totalSeconds += frame.Duration.TotalSeconds;
                }

                return TimeSpan.FromSeconds (totalSeconds);
            }
        }

        public void AddFrame(Rectangle rectangle, TimeSpan duration)
        {
            AnimationFrame newFrame = new AnimationFrame()
            {
                SourceRectangle = rectangle,
                Duration = duration
            };

            frames.Add(newFrame);
        }

        public void Update(GameTime gameTime)
        {
            double secondsIntoAnimation = 
                timeIntoAnimation.TotalSeconds + gameTime.ElapsedGameTime.TotalSeconds;


            double remainder = secondsIntoAnimation % Duration.TotalSeconds;

            timeIntoAnimation = TimeSpan.FromSeconds (remainder);
        }
    }
}
```

세부 정보 중 일부에 대해 살펴보겠습니다는 `Animation` 클래스입니다.

### <a name="frames-list"></a>프레임 목록

`frames` 멤버는 애니메이션에 대 한 데이터를 저장 하는 기능입니다. 애니메이션을 인스턴스화하는 코드를 추가 합니다 `AnimationFrame` 인스턴스는 `frames` 통해 목록는 `AddFrame` 메서드. 제공할 수 있을 것 보다 완벽 한 구현을 `public` 메서드 또는 속성 수정에 대 한 `frames`, 하지만이 연습에 대 한 프레임을 추가 하는 기능 제한 합니다.

### <a name="duration"></a>기간

전체 지속 시간을 반환 하는 기간은 `Animation,` 포함 된 모든의 기간을 추가 하 여 얻을 수 있는 `AnimationFrame` 인스턴스. 경우이 값을 캐시할 수 `AnimationFrame` 변경할 수 없는 개체를 찾았지만 AnimationFrame 애니메이션에 추가 된 후 변경할 수 있는 클래스와 구현한 있으므로 속성에 액세스할 때마다이 값을 계산 해야 합니다.

### <a name="update"></a>업데이트

`Update` 메서드 모든 프레임 (즉, 게임 중 업데이트 될 때마다)를 호출 해야 합니다. 용도를 늘리는 것입니다는 `timeIntoAnimation` 현재 표시 된 프레임을 반환 하는 데 사용 되는 멤버입니다. 논리 `Update` 방지는 `timeIntoAnimation` 전체 애니메이션의 지속 시간 보다 더 큰 하에서 합니다.

사용할 예정 있으므로 `Animation` 이 애니메이션의 끝에 도달한 경우 반복을 포함 하려는 다음 워크 애니메이션을 표시 하는 클래스입니다. 확인 하 여이 작업을 수행할 수 있습니다는 `timeIntoAnimation` 보다 크면는 `Duration` 값입니다. 이 경우 처음으로 순환 하 고 나머지를 애니메이션을 반복에 유지 합니다.

### <a name="obtaining-the-current-frames-rectangle"></a>현재 프레임의 사각형을 가져오기

용도 `Animation` 클래스는 궁극적으로 제공 하는 `Rectangle` 스프라이트 그릴 때 사용할 수 있는 합니다. 현재는 `Animation` 변경에 대 한 논리를 포함 하는 클래스는 `timeIntoAnimation` 멤버를 얻으려고 하에서는 `Rectangle` 표시 해야 합니다. 만들어서 수행 합니다는 `CurrentRectangle` 속성에는 `Animation` 클래스입니다. 이 속성에 복사는 `Animation` 클래스:

```csharp
public Rectangle CurrentRectangle
{
    get
    {
        AnimationFrame currentFrame = null;

        // See if we can find the frame
        TimeSpan accumulatedTime;
        foreach(var frame in frames)
        {
            if (accumulatedTime + frame.Duration >= timeIntoAnimation)
            {
                currentFrame = frame;
                break;
            }
            else
            {
                accumulatedTime += frame.Duration;
            }
        }

        // If no frame was found, then try the last frame, 
        // just in case timeIntoAnimation somehow exceeds Duration
        if (currentFrame == null)
        {
            currentFrame = frames.LastOrDefault ();
        }

        // If we found a frame, return its rectangle, otherwise
        // return an empty rectangle (one with no width or height)
        if (currentFrame != null)
        {
            return currentFrame.SourceRectangle;
        }
        else
        {
            return Rectangle.Empty;
        }
    }
}
```

## <a name="adding-the-first-animation-to-characterentity"></a>CharacterEntity에 첫 번째 애니메이션 추가

`CharacterEntity` 탐색 하 고 순위, 뿐만 아니라 현재에 대 한 참조에 대 한 애니메이션 포함 될 `Animation` 표시 되 고 있습니다.

첫째, 첫 추가 `Animation,` 는 지금까지 작성 된 대로 기능을 테스트 하려면 사용 합니다. CharacterEntity 클래스에 다음 멤버를 추가 해 보겠습니다.

```csharp
Animation walkDown;
Animation currentAnimation;
```

그런 다음 정의 `walkDown` 애니메이션 합니다. 이 수정 작업을 수행 하는 `CharacterEntity` 생성자를 다음과 같이 합니다.

```csharp
public CharacterEntity (GraphicsDevice graphicsDevice)
{
    if (characterSheetTexture == null)
    {
        using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
        {
            characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
        }
    }

    walkDown = new Animation ();
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (16, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (32, 0, 16, 16), TimeSpan.FromSeconds (.25));
}
```

호출 해야 듯이, `Animation.Update` 재생할 시간 기준 애니메이션에 대 한 합니다. 할당 해야는 `currentAnimation`합니다. 이제 할당 하겠습니다에 대 한는 `currentAnimation` 를 `walkDown`, 우리 합니다 수 교체이 코드 나중에 이동 논리를 구현 하는 경우. 추가 `Update` 메서드를 `CharacterEntity` 다음과 같습니다.


```csharp
public void Update(GameTime gameTime)
{
    // temporary - we'll replace this with logic based off of which way the
    // character is moving when we add movement logic
    currentAnimation = walkDown;

    currentAnimation.Update (gameTime);
}
```

이제는 `currentAnimation` 되 고 할당 하 고 업데이트 우리의 그리기를 수행 하려면 사용할 수 있습니다. 수정 합니다 `CharacterEntity.Draw` 다음과 같습니다.

```csharp
public void Draw(SpriteBatch spriteBatch)
{
    Vector2 topLeftOfSprite = new Vector2 (this.X, this.Y);
    Color tintColor = Color.White;
    var sourceRectangle = currentAnimation.CurrentRectangle;

    spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, sourceRectangle, Color.White);
}
```

시작 하기 위한 마지막 단계는 `CharacterEntity` 새로 추가 된 호출 하는 것에 애니메이션 적용 `Update` 메서드에서 `Game1`합니다. 수정 `Game1.Update` 다음과 같습니다.

```csharp
protected override void Update(GameTime gameTime)
{
    character.Update (gameTime);
    base.Update(gameTime);
}
```

이제는 `CharacterEntity` 재생 될 해당 `walkDown` 애니메이션:

![](part2-images/image5.gif "이제는 CharacterEntity walkDown 애니메이션이 재생 됩니다")

## <a name="adding-movement-to-the-character"></a>문자를 추가 이동

다음으로 추가 되므로 이동을 터치 컨트롤을 사용 하 여이 문자를 합니다. 사용자가 화면을 터치 하면 문자 화면 접촉 될 지점 방향으로 이동 합니다. 없는 터치 검색 되 면 문자 위치에 강조 합니다.

### <a name="defining-getdesiredvelocityfrominput"></a>GetDesiredVelocityFromInput 정의

사용할 예정 MonoGame의 `TouchPanel` 터치 스크린의 현재 상태에 대 한 정보를 제공 하는 클래스입니다. 확인 하는 메서드를 추가 해 보겠습니다는 `TouchPanel` 하 고 원하는 개발 속도 문자를 반환 합니다.


```csharp
Vector2 GetDesiredVelocityFromInput()
{
    Vector2 desiredVelocity = new Vector2 ();

    TouchCollection touchCollection = TouchPanel.GetState();

    if (touchCollection.Count > 0)
    {
        desiredVelocity.X = touchCollection [0].Position.X - this.X;
        desiredVelocity.Y = touchCollection [0].Position.Y - this.Y;

        if (desiredVelocity.X != 0 || desiredVelocity.Y != 0)
        {
            desiredVelocity.Normalize();
            const float desiredSpeed = 200;
            desiredVelocity *= desiredSpeed;
        }
    }

    return desiredVelocity;
}
```

`TouchPanel.GetState` 메서드가 반환 되는 `TouchCollection` 사용자는 화면을 터치가 위치 하는 방법에 대 한 정보가 들어 있는입니다. 사용자가 화면을 터치 하지 않는 경우 하면 `TouchCollection` 가 비게 됩니다는 쿼리에서 म 문자도 이동 하지 않아야 합니다.

첫 번째 터치 쪽으로 문자 즉 이동할 것은 사용자가 화면을 터치 하는 경우, `TouchLocation` 인덱스 0에 있습니다. 처음에 문자 위치와 첫 번째 터치 위치 간의 차이를 원하는 속도 바로잡을 수 있습니다:

```csharp
        desiredVelocity.X = touchCollection [0].Position.X - this.X;
        desiredVelocity.Y = touchCollection [0].Position.Y - this.Y;
```

뒤에 오는 다소 같은 속도로 이동 문자를 유지 합니다는 수학입니다. 을이 중요 한 이유는 보여 주기 위해 사용자 문자 위치한 반대쪽 화면 500 픽셀에 연결 되어 있는 경우를 생각해 봅시다. 첫 번째 줄 where `desiredVelocity.X` 은 집합 500의 값을 할당 합니다. 그러나 사용자는 해당 문자를에서 100 명의 단위 떨어진 위치 화면을 터치 된 경우, 그런 다음 `desiredVelocity.X `100으로 설정 됩니다. 결과 문자의 이동 속도가 응답 하는 방법을 멀리 떨어져 터치 점은 문자에서 것입니다. 항상 같은 속도로 이동할 문자 할 것 이므로 desiredVelocity 수정 해야 합니다.

`if (desiredVelocity.X != 0 || desiredVelocity.Y != 0)` 사용자 문자의 현재 위치와 같은 지점을 건드리지 않고가 있는지 검사 하는 경우 개발 속도 비-0 – 즉, 문이 확인 하 고 있습니다. 그렇지 않은 해야 합니다. 터치 멀리 떨어져 방법에 관계 없이 일정 되도록 문자의 속도 설정할 경우입니다. 그 결과 길이는 1 하면 속도 벡터를 정규화 하 여이 위해. 개발 속도 벡터 1 이면 문자는 초 당 1 픽셀에서 이동 합니다. 에서는 것이 속도를 향상 200 원하는 속도 값을 곱한.


### <a name="applying-velocity-to-position"></a>개발 속도 위치에 적용

반환 된 개발 속도 `GetDesiredVelocityFromInput` 문자로 적용 되어야 `X` 및 `Y` 값을 런타임 시 적용 합니다. 수정 합니다는 `Update` 메서드를 다음과 같이 합니다.

```csharp
public void Update(GameTime gameTime)
{

    var velocity = GetDesiredVelocityFromInput ();

    this.X += velocity.X * (float)gameTime.ElapsedGameTime.TotalSeconds;
    this.Y += velocity.Y * (float)gameTime.ElapsedGameTime.TotalSeconds;

    // temporary - we'll replace this with logic based off of which way the
    // character is moving when we add movement logic
    currentAnimation = walkDown;

    currentAnimation.Update (gameTime);
}
```

여기서 구현 했습니다 무엇 이라고 *시간 기반* 이동 (반대인 *프레임 기반* 이동). 시간 기반 이동을 곱합니다 속도 값 (값에 저장 된 경우에는 `velocity` 변수)에 저장 되는 마지막 업데이트 이후 시간 경과 하 여 `gameTime.ElapsedGameTime.TotalSeconds`합니다. 게임 초당 프레임 수가에서 실행 되 면 프레임 사이의 경과 시간 위로 올라가면서 – 최종 결과 프레임 속도 상관 없이 같은 속도로 항상 시간 기반 이동을 사용 하 여 개체 이동 합니다.

게임을 지금 실행 터치 위치 변화는 보겠습니다.

![](part2-images/image6.gif "문자는 터치 위치 쪽으로 이동")

## <a name="matching-movement-and-animation"></a>일치 하는 이동 및 애니메이션

이동 하 고 단일 애니메이션을 재생 우리의 문자 상태, 우리의 애니메이션의 나머지 부분을 정의 다음 문자의 움직임을 반영 하는 데 사용할 수 있습니다. 때 완료 되는 8 개의 애니메이션에에서 있는 총:

- 아래쪽, 왼쪽,를 탐색 하 고 오른쪽에 대 한 애니메이션
- 순위에 대 한 애니메이션 여전히 및 왼쪽과 오른쪽 아래로 향하는

### <a name="defining-the-rest-of-the-animations"></a>애니메이션의 나머지 부분을 정의합니다.

먼저 추가 `Animation` 인스턴스는 `CharacterEntity` 추가 위치 같은 위치에이 애니메이션의 모든 클래스를 `walkDown`합니다. 이렇게 우리는 `CharacterEntity` 다음 갖습니다 `Animation` 구성원:

```csharp
Animation walkDown;
Animation walkUp;
Animation walkLeft;
Animation walkRight;

Animation standDown;
Animation standUp;
Animation standLeft;
Animation standRight;

Animation currentAnimation;
```

애니메이션을 정의 하겠습니다 이제는 `CharacterEntity` 생성자를 다음과 같이 합니다.

```csharp
public CharacterEntity (GraphicsDevice graphicsDevice)
{
    if (characterSheetTexture == null)
    {
        using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
        {
            characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
        }
    }

    walkDown = new Animation ();
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (16, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (32, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkUp = new Animation ();
    walkUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (160, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (176, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkLeft = new Animation ();
    walkLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (64, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (80, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkRight = new Animation ();
    walkRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (112, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (128, 0, 16, 16), TimeSpan.FromSeconds (.25));

    // Standing animations only have a single frame of animation:
    standDown = new Animation ();
    standDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standUp = new Animation ();
    standUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standLeft = new Animation ();
    standLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standRight = new Animation ();
    standRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
}
```

위의 코드에 추가 된 드릴는 `CharacterEntity` 이 연습을 짧게 유지 하는 생성자입니다. 게임은 일반적으로는 자체 클래스로 문자 애니메이션 정의 구분 되거나 됩니다 XML, JSON과 같은 데이터 형식에서이 정보를 로드.

다음으로 문자는 이전 단계에서 중지 하는 경우 문자는 이동 방향에 따라 또는 마지막 애니메이션에 따라 애니메이션을 사용 하는 논리를 조정 합니다 합니다. 이 위해 수정 합니다는 `Update` 메서드:


```csharp
public void Update(GameTime gameTime)
{
    var velocity = GetDesiredVelocityFromInput ();

    this.X += velocity.X * (float)gameTime.ElapsedGameTime.TotalSeconds;
    this.Y += velocity.Y * (float)gameTime.ElapsedGameTime.TotalSeconds;


    if (velocity != Vector2.Zero)
    {
        bool movingHorizontally = Math.Abs (velocity.X) > Math.Abs (velocity.Y);
        if (movingHorizontally)
        {
            if (velocity.X > 0)
            {
                currentAnimation = walkRight;
            }
            else
            {
                currentAnimation = walkLeft;
            }
        }
        else
        {
            if (velocity.Y > 0)
            {
                currentAnimation = walkDown;
            }
            else
            {
                currentAnimation = walkUp;
            }
        }
    }
    else
    {
        // If the character was walking, we can set the standing animation
        // according to the walking animation that is playing:
        if (currentAnimation == walkRight)
        {
            currentAnimation = standRight;
        }
        else if (currentAnimation == walkLeft)
        {
            currentAnimation = standLeft;
        }
        else if (currentAnimation == walkUp)
        {
            currentAnimation = standUp;
        }
        else if (currentAnimation == walkDown)
        {
            currentAnimation = standDown;
        }
        else if (currentAnimation == null)
        {
            currentAnimation = standDown;
        }

        // if none of the above code hit then the character
        // is already standing, so no need to change the animation.
    }

    currentAnimation.Update (gameTime);
}
```

애니메이션을 전환 하는 코드는 두 개의 블록 나뉩니다. 첫 번째 속도 같지 않은 경우 확인 `Vector2.Zero` –이 알려줍니다. 문자를 이동 하는 여부. 문자 이동 경우 볼 수는 `velocity.X` 및 `velocity.Y` 을 재생 하는 워크 애니메이션을 확인할 값입니다.

문자 이동 하지 않는 경우는 문자를 설정 하려고 `currentAnimation` 순위 애니메이션 –에 수행할 경우 하지만 `currentAnimation` 는 탐색은 애니메이션이 나 애니메이션 설정 되지 않은 경우. 경우는 `currentAnimation` 가 4 개의 워크 애니메이션 중 하나는 문자는 변경할 필요가 없습니다 이미 서 있는 `currentAnimation`합니다.

이 코드의 결과는 문자를 보면에 애니메이션을 제대로 적용 되며이 중지 되 면 검색 된 대로 마지막 방향이:

![](part2-images/image7.gif "이 코드의 결과 문자를 보면에 애니메이션을 제대로 적용 되며이 중지 되 면 검색 된 대로 마지막 방향이")

## <a name="summary"></a>요약

이 연습에서는 스프라이트, 개체 이동, 입력된 감지, 애니메이션와 게임 플랫폼 간 만들려는 MonoGame와 작업 하는 방법을 배웠습니다. 범용 애니메이션 클래스를 만드는 내용을 다룹니다. 또한 코드 논리를 구성 하기 위한 문자 엔터티를 만드는 방법을 배웠습니다.

## <a name="related-links"></a>관련 링크

- [CharacterSheet 이미지 리소스 (샘플)](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true)
- [순환 게임 완료 (샘플)](https://developer.xamarin.com/samples/mobile/WalkingGameMG/)
