---
title: 그래픽 및 애니메이션
description: Android 2D 그래픽 및 애니메이션을 지원 하기 위한 매우 다양 하 고 다양 한 프레임 워크를 제공 합니다. 이 항목에서는 이러한 프레임 워크를 소개 하 고 Xamarin.Android 응용 프로그램에서 사용자 지정 그래픽 및 사용에 대 한 애니메이션을 만드는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 80086318-6FE4-4711-9A71-5C8F8C28C754
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 7f4f7fd3af1e90307a84037f01ddf8e52b1ee030
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669052"
---
# <a name="graphics-and-animation"></a>그래픽 및 애니메이션

_Android 2D 그래픽 및 애니메이션을 지원 하기 위한 매우 다양 하 고 다양 한 프레임 워크를 제공 합니다. 이 항목에서는 이러한 프레임 워크를 소개 하 고 Xamarin.Android 응용 프로그램에서 사용자 지정 그래픽 및 사용에 대 한 애니메이션을 만드는 방법에 설명 합니다._


## <a name="overview"></a>개요

제한적인된 전원의 일반적으로 장치에서 실행에 불구 하 고 가장 높은 등급이 지정 된 모바일 응용 프로그램 경우가 복잡 한 사용자 환경 (UX), 고품질의 그래픽 및 애니메이션을 직관적이 고 동적 응답 느낌을 제공 하는 사용 하 여 완료 합니다. 모바일 응용 프로그램 더 가져오기 하 고 보다 복잡 한 사용자가 응용 프로그램에서 더 많은 기대 하기 시작 했습니다.

다행히 우리에 게 있어 최신 모바일 플랫폼에 사용 편의성을 유지 하면서 복잡 한 애니메이션 및 사용자 지정 그래픽을 만들기 위한 강력한 프레임 워크는 합니다. 따라서 개발자가 매우 적은 노력으로 풍부한 대화형 작업을 추가할 수 있습니다.

Android에서 UI API 프레임 워크는 대략 두 가지 범주로 분할 될 수 있습니다. 그래픽 및 애니메이션 합니다.

그래픽은 2D 및 3D 그래픽 작업을 수행 하는 다른 방법으로 추가 분할 됩니다. 3D 그래픽은 기본 제공 OpenGL ES (특정의 모바일 버전 OpenGL)와 같은 프레임 워크 및 MonoGame (플랫폼 간 도구 키트는 XNA 도구 키트와 호환)와 같은 타사 프레임 워크의 번호를 통해 사용할 수 있습니다. 3D 그래픽이 문서의 범위에 포함 되지 않지만 기본 제공 2D 그리기 기술을 살펴보겠습니다.

Android는 2D 그래픽을 만들기 위한 두 개의 다른 API를 제공 합니다. 하나는 높은 수준의 선언적 방법 및 기타 하위 수준 API를 프로그래밍 방식:

-   **드로어 블 리소스** &ndash; 이러한 XML 파일에 그리기 지침을 포함 하 여 프로그래밍 방식으로 또는 더 일반적으로 사용자 지정 그래픽을 만드는 데 사용 됩니다. 드로어 블 리소스는 일반적으로 지침 또는 2D 그래픽을 렌더링 하는 Android에 대 한 작업을 포함 하는 XML 파일로 정의 됩니다. 

-   **캔버스** &ndash; 는 기본 비트맵에서 직접 그리기를 포함 하는 낮은 수준의 API입니다. 표시 되는 항목을 통해 매우 세분화 된 제어를 제공 합니다. 

이러한 2D 그래픽 기술을 외에도 Android 애니메이션을 만드는 여러 가지 방법으로 제공 합니다.

-   **Drawable 애니메이션** &ndash; Android에서는 라고 프레임별으로 애니메이션 *Drawable 애니메이션*합니다. 이 가장 간단한 애니메이션 API입니다. Android는 순차적으로 로드 하 고 (마찬가지로, 만화) 순서로 드로어 블 리소스를 표시 합니다.

-   **애니메이션** &ndash; *애니메이션 보기* Android에서 원래 애니메이션 API 이며 모든 버전의 Android에서 사용할 수 있습니다. 이 API는 제한 된 뷰 개체 에서만 작동 하며 해당 뷰를 간단한 변환을 수행할 수 있습니다.
    애니메이션 보기는 일반적으로 XML 파일에 정의 된 `/Resources/anim` 폴더입니다.

-   **속성 애니메이션** &ndash; Android 3.0 애니메이션 API의 새 집합 이라고 *속성 애니메이션*합니다. 이러한 새 API의 모든 개체의 속성에 애니메이션 효과 주기 뿐 아니라 개체를 보기를 사용할 수 있는 확장 하 고 유연한 시스템을 도입 했습니다. 이 유연성 덕분에 코드를 쉽게 공유할 수 있도록 고유 클래스에 캡슐화 하는 애니메이션입니다.


보기 애니메이션은 이전 사전 Android 3.0 API (API 레벨 11)를 지원 해야 하는 응용 프로그램에 더 적합 합니다. 그렇지 않으면 위에서 설명한 이유로 응용 프로그램 최신 속성 애니메이션 API를 사용 해야 합니다.

그러나 이러한 프레임 워크 모두 실행 가능한 가능한 경우 기본 설정에 부여 해야 속성 애니메이션을 사용 하는 유연한 API를 그대로 옵션입니다. 속성 애니메이션을 더 쉽게 공유 코드 및 코드 유지 관리를 간소화 하는 애니메이션 논리 고유 클래스에 캡슐화를 허용 합니다.


## <a name="accessibility"></a>액세스 가능성

그래픽 및 애니메이션 Android 앱을 더 좋게 하는 데 도움이 되 고 재미로 사용할 수 있습니다. 그러나 것 있습니다, 다른 입력된 장치를 통해 또는 보조 확대/축소를 사용 하 여 일부 상호 작용 수행 되도록 고려해 야 합니다.
또한 일부 상호 작용은 오디오 기능 없이 발생할 수 있습니다.

앱은 접근성을 염두에서에 두고 디자인 된 경우 이러한 상황에서 좀더: 힌트 및 사용자 인터페이스에서 탐색 지원을 제공 하 고 텍스트 콘텐츠 또는 ui 요소에 대 한 설명을 확인 합니다.

가리킵니다 [Google의 내게 필요한 옵션 가이드](https://developer.android.com/guide/topics/ui/accessibility/) Android의 내게 필요한 옵션 Api를 활용 하는 방법에 대 한 자세한 내용은 합니다.



## <a name="2d-graphics"></a>2 차원 그래픽

드로어 블 리소스는 Android 응용 프로그램에서 널리 사용 되는 기술입니다. 다른 리소스와 드로어 블 리소스는 선언적 &ndash; XML 파일에 정의 합니다. 이 방법은 리소스에서 코드를 구분할 수 있습니다. 업데이트 하거나 Android 응용 프로그램에서 그래픽을 변경 하려면 코드를 변경 하는 데 필요한 하지 않기 때문에 개발 및 유지 관리 간소화할 수 있습니다. 그러나 드로어 블 리소스는 대부분 간단 하 고 일반적인에 대 한 그래픽 요구 사항에 대 한 유용한, 없기 기능과 캔버스 API의 제어 합니다.

다른 기법을 사용 하는 [캔버스](https://developer.xamarin.com/api/type/Android.Graphics.Canvas/) 개체, System.Drawing 또는 iOS의 핵심 그리기와 같은 다른 기존 API 프레임 워크에 매우 비슷합니다. 캔버스 개체를 사용 하는 방법을 2 차원 그래픽의 대부분의 컨트롤이 만들어집니다 제공 합니다. 드로어 블 리소스 작동 하지 것입니다 또는 작업이 어려울 수 있는 경우에는 것이 적합 합니다. 예를 들어 구성 요소를 슬라이더의 값과 관련 된 계산을 기초로 모양을 가진 바뀝니다 사용자 지정 슬라이더 컨트롤을 그리는 데 필요한 수 있습니다.

먼저 드로어 블 리소스를 검토해 보겠습니다. 더 간단 하며 가장 일반적인 사용자 지정 그리기 사례를 설명 합니다.


### <a name="drawable-resources"></a>드로어 블 리소스

그릴 수 있는 리소스 디렉터리에 XML 파일에 정의 된 `/Resources/drawable`합니다. PNG 또는 JPEG의 포함을 달리 드로어 블 리소스의 밀도 별 버전을 제공 하는 데 필요한 아닙니다.
런타임에 Android 응용 프로그램을 이러한 리소스를 로드를 2D 그래픽을 만들려면 이러한 XML 파일에 포함 된 지침을 사용 합니다.
Android는 여러 다른 유형의 드로어 블 리소스를 정의합니다.

-   [ShapeDrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#Shape) &ndash; 해당 모양에 그래픽 효과 대 한 제한 된 집합을 적용 하는 기본 기 하 도형을 그립니다는 그릴 수 있는 개체입니다. 단추 사용자 지정 또는 TextViews의 배경을 설정 등에 대 한 매우 유용 합니다. 사용 하는 방법의 예제를 살펴보겠습니다는 `ShapeDrawable` 이 문서의 뒷부분에 나오는.

-   [StateListDrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#StateList) &ndash; 위젯/컨트롤의 상태에 따라 모양이 변경 되는 드로어 블 리소스입니다. 예를 들어, 단추 여부 눌린 여부에 따라 모양이 변경 될 수 있습니다.

-   [LayerDrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList) &ndash; 이 드로어 블 리소스를 다른를 기반으로 한 다른 여러 드로어 블 스택 됩니다. 예는 *LayerDrawable* 다음 스크린샷에 표시 됩니다.

    ![LayerDrawable 예제](graphics-and-animation-images/image1.png)

-   [TransitionDrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#Transition) &ndash; 이것이 *LayerDrawable* 하지만 차이점 중 하나입니다. A *TransitionDrawable* 한 계층 표시 위쪽 다른 애니메이션 효과를 줄 수 있습니다.

-   [LevelListDrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#LevelList) &ndash; 이것은 매우 비슷합니다는 *StateListDrawable* 는 특정 조건에 따라 이미지로 표시 됩니다. 그러나 달리를 *StateListDrawable*의 *LevelListDrawable* 정수 값을 기반으로 이미지를 표시 합니다. 예는 *LevelListDrawable* WiFi 신호 강도 표시 하는 것입니다. WiFi 신호 변경의 강도으로 표시 되는 drawable 따라 변경 됩니다.

-   [ScaleDrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#Scale)/[ClipDrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#Clip) &ndash; 이러한 드로어 블 제공 크기 조정 및 자르기 기능 이름에서 알 수 있듯이, 합니다. *ScaleDrawable* 다른 Drawable, while 크기가 조정 됩니다 합니다 *ClipDrawable* 다른 Drawable 클립 됩니다.

-   [InsetDrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#Inset) &ndash; 이 Drawable 음각 다른 드로어 블 리소스의 양쪽에 적용 됩니다. 뷰를 뷰의 실제 범위 보다 작은 배경이 해야 하는 경우 사용 됩니다.

-   XML [BitmapDrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#Bitmap) &ndash; 이 파일은 명령의 XML에는 실제 비트맵에서 수행할 수 있는 집합입니다. Android에서 수행할 수 있는 일부 작업은 타일링, 디더링, 및 앤티앨리어싱입니다. 이 사용 되는 매우 일반적인 중 하나를 레이아웃의 백그라운드에서 비트맵을 바둑판식으로 배열입니다.


#### <a name="drawable-example"></a>Drawable 예제

사용 하 여 2D 그래픽을 만드는 방법의 간단한 예제를 살펴보겠습니다는 `ShapeDrawable`합니다. `ShapeDrawable` 네 가지 기본 도형 중 하나를 정의할 수 있습니다: 사각형, 타원, 선 및 링 합니다. 그라데이션, 색상, 크기 등 기본 효과 적용 하는 것도 가능 합니다. 다음 XML은는 `ShapeDrawable` 에서 찾을 수 있습니다 합니다 *AnimationsDemo* 도우미 프로젝트 (파일에 `Resources/drawable/shape_rounded_blue_rect.xml`).
자주색 배경에 그라데이션 효과 사용 하 여 사각형을 정의 하 고 모서리가 둥근:

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android" android:shape="rectangle">
<!-- Specify a gradient for the background -->
<gradient android:angle="45"
          android:startColor="#55000066"
          android:centerColor="#00000000"
          android:endColor="#00000000"
          android:centerX="0.75" />

<padding android:left="5dp"
          android:right="5dp"
          android:top="5dp"
          android:bottom="5dp" />

<corners android:topLeftRadius="10dp"
          android:topRightRadius="10dp"
          android:bottomLeftRadius="10dp"
          android:bottomRightRadius="10dp" />
</shape>
```

이 드로어 블 리소스 레이아웃 또는 다음 XML에 표시 된 대로 다른 Drawable에서 선언적으로 참조할 수 있습니다.

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:background="#33000000">
    <TextView android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              android:layout_centerInParent="true"
              android:background="@drawable/shape_rounded_blue_rect"
              android:text="@string/message_shapedrawable" />
</RelativeLayout>
```

드로어 블 리소스를 프로그래밍 방식으로 적용할 수도 있습니다. 다음 코드 조각은 배경의 TextView 프로그래밍 방식으로 설정 하는 방법을 보여 줍니다.

```csharp
TextView tv = FindViewById<TextView>(Resource.Id.shapeDrawableTextView);
tv.SetBackgroundResource(Resource.Drawable.shape_rounded_blue_rect);
```

어떤 유사 하 게 확인 하려면 실행 합니다 *AnimationsDemo* 프로젝트 및 주 메뉴에서 도형을 그릴 수 있는 항목을 선택 합니다. 다음 스크린샷과 비슷하게 표시 되어야 합니다.

![그라데이션 및 둥근 모퉁이 사용 하 여 그릴 수 있는 사용자 지정 배경 사용 하 여 Textview](graphics-and-animation-images/image1.png)

XML 요소 및 드로어 블 리소스의 구문에 대 한 자세한 내용은 참조 하세요. [Google 설명서](https://developer.android.com/guide/topics/resources/drawable-resource.html#Shape)합니다.


### <a name="using-the-canvas-drawing-api"></a>캔버스 그리기 API를 사용 하 여

드로어 블은 강력 하지만 해당 제한 사항이 있습니다. 몇 가지 항목 불가능 하거나 매우 복잡 한 (예: 장치에서 카메라에 의해 수행 된 그림에 필터를 적용). 드로어 블 리소스를 사용 하 여 적목 감소를 적용 하는 것이 어려울 것입니다.
대신, 캔버스 API 응용 프로그램을을 선택적으로 그림의 특정 부분에서 색을 변경 하려면 매우 세부적으로 제어할 수 있습니다.

캔버스를 사용 하 여 일반적으로 사용 되는 하나의 클래스는 [페인트](https://developer.xamarin.com/api/type/Android.Graphics.Paint/) 클래스입니다. 이 클래스는을 그리는 방법에 대 한 색 및 스타일 정보를 포함 합니다. 이러한 색상 및 투명도 작업을 제공 하는 것이 됩니다.

캔버스 API를 사용 하는 *페인 터의 모델* 2D 그래픽을 그릴 합니다.
작업은 서로 기반으로 연속 된 계층에 적용 됩니다. 각 작업에서는 기본 비트맵의 일부 영역을 설명 합니다. 영역 이전에 그려지는 영역을 중첩 하는 경우 새 페인트 부분적으로 또는 완전히 이전 모호 하 게 합니다. System.Drawing 및 iOS의 핵심 그래픽 등 다른 많은 그리기 Api 작동 하는 동일한 방식으로 이것이입니다.

두 가지 방법으로 가져오려고를 `Canvas` 개체입니다. 첫 번째 방법은 정의 하는 것을 [비트맵](https://developer.xamarin.com/api/type/Android.Graphics.Bitmap/) 개체를 인스턴스화하는 `Canvas` 개체를 합니다. 예를 들어, 다음 코드 조각은 기본는 비트맵을 사용 하 여 새 캔버스를 만듭니다.

```csharp
Bitmap bitmap = Bitmap.CreateBitmap(100, 100, Bitmap.Config.Argb8888);
Canvas canvas = new Canvas(b);
```

가져오는 다른 방법을 `Canvas` 하 여 합니다 [OnDraw](https://developer.xamarin.com/api/member/Android.Views.View.OnDraw/) 제공 되는 콜백 메서드를 [보기](https://developer.xamarin.com/api/type/Android.Views.View/) 기본 클래스입니다. Android 뷰 자체를 그려야 하 고 전달 결정할 때이 메서드를 호출 하는 `Canvas` 개체를 사용 하려면 뷰에 대 한 합니다.

캔버스 클래스는 프로그래밍 방식으로 draw 지침을 제공 하는 메서드를 노출 합니다. 예를 들어:

-   [Canvas.DrawPaint](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawPaint/p/Android.Graphics.Paint/) &ndash; 지정된 그리기를 사용 하 여 전체 캔버스의 비트맵을 채웁니다.

-   [Canvas.DrawPath](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawPath/p/Android.Graphics.Path/Android.Graphics.Paint/) &ndash; 지정된 그림판을 사용 하 여 지정 된 기 하 도형을 그립니다.

-   [Canvas.DrawText](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawText/p/System.String/System.Single/System.Single/Android.Graphics.Paint/) &ndash; 지정 된 색을 사용 하 여 캔버스에 텍스트를 그립니다. 위치에서 텍스트를 그리는 `x,y` 합니다.



#### <a name="drawing-with-the-canvas-api"></a>캔버스 API 사용 하 여 그리기

작업에서 캔버스 API의 예를 들어를 살펴보겠습니다. 다음 코드 조각에는 뷰를 그리는 방법을 보여 줍니다.

```csharp
public class MyView : View
{
    protected override void OnDraw(Canvas canvas)
    {
        base.OnDraw(canvas);
        Paint green = new Paint {
            AntiAlias = true,
            Color = Color.Rgb(0x99, 0xcc, 0),
        };
        green.SetStyle(Paint.Style.FillAndStroke);

        Paint red = new Paint {
            AntiAlias = true,
            Color = Color.Rgb(0xff, 0x44, 0x44)
        };
        red.SetStyle(Paint.Style.FillAndStroke);

        float middle = canvas.Width * 0.25f;
        canvas.DrawPaint(red);
        canvas.DrawRect(0, 0, middle, canvas.Height, green);
    }
}
```

이 코드는 먼저 빨간색 페인트 및 녹색 그리기 개체를 만듭니다. 빨간색으로를 사용 하 여는 canvas의 내용을 채우고 캔버스 너비의 25%는 녹색 사각형을 그리려면 캔버스를 지시 합니다. 이 예제에서 볼 수 `AnimationsDemo` 이 기사의 소스 코드를 사용 하 여 포함 된 프로젝트입니다. 응용 프로그램을 시작 하 고 주 메뉴에서 그리기 항목을 선택 하 여 다음과 비슷한 화면이 해야 합니다.

![빨간색 페인트 및 녹색 그리기 개체를 사용 하 여 화면](graphics-and-animation-images/image3.png)


## <a name="animation"></a>애니메이션

사용자가 같은 응용 프로그램에서 이동 하는 것입니다. 애니메이션은 편리 하 게 응용 프로그램의 사용자 환경을 개선 하 고 두드러져 보이도록 합니다. 최상의 애니메이션은 자연 스러운 때문에 사용자가 확인 하지 것입니다. Android는 애니메이션에 대 한 다음 세 가지 API를 제공합니다.

-   **애니메이션 볼** &ndash; 원래 API입니다. 이러한 애니메이션 특정 보기에 연결 되 고 보기의 내용에 간단한 변환을 수행할 수 있습니다. 단순성으로 인해이 API를 작업 하는 데 유용 하지만 같은 알파 애니메이션, 회전 등입니다.

-   **속성 애니메이션** &ndash; 속성 애니메이션 Android 3.0에서 도입 되었습니다. 거의 모든 요소에 애니메이션 효과를 응용 프로그램을 사용 수 있습니다. 속성 애니메이션 개체를 화면에 표시 되지 않더라도 모든 개체의 속성을 변경 하려면 사용할 수 있습니다.

-   **Drawable 애니메이션** &ndash; 레이아웃에이 매우 간단한 애니메이션을 적용 하는 데 사용 되는 특별 한 드로어 블 리소스 적용 합니다.

일반적으로 속성 애니메이션은 기본 시스템 사용은 보다 유연 하 고 더 많은 기능을 제공 합니다.


### <a name="view-animations"></a>애니메이션 보기

애니메이션 보기는 보기에 제한 되 고 애니메이션 시작 및 종료 지점, 크기, 회전 및 투명도 같은 값에만 수행할 수 있습니다.
이러한 유형의 애니메이션은 일반적으로 이라고 *트윈 애니메이션*합니다. 보기 애니메이션은 두 가지 방법으로 정의할 수 있습니다 &ndash; 코드 또는 XML 파일을 사용 하 여 프로그래밍 방식으로 합니다. XML 파일 되는지 보기 애니메이션을 선언 하는 기본 방법은 때 더 쉽게 읽고 유지 관리 합니다.

애니메이션 XML 파일에 저장 됩니다는 `/Resources/anim` Xamarin.Android 프로젝트의 디렉터리입니다. 이 파일을 루트 요소로 다음 요소 중 하나를 가져야 합니다.

-   `alpha` &ndash; 페이드 또는 페이드 아웃 애니메이션입니다.

-   `rotate` &ndash; 회전 애니메이션입니다.

-   `scale` &ndash; 크기 조정 애니메이션입니다.

-   `translate` &ndash; 가로 및/또는 세로 동작입니다.

-   `set` &ndash; 다른 애니메이션 요소 중 하나 이상을 보유할 수 있는 컨테이너입니다.

기본적으로 XML 파일에서 모든 애니메이션을 동시에 적용 됩니다. 순차적으로 발생 하는 애니메이션을 설정 합니다 `android:startOffset` 위에 정의 된 요소 중 하나에 대 한 특성입니다.

사용 하 여 애니메이션의 변동률을 적용할 수는 *보간*합니다. 보간 가속, 반복 또는 decelerated 애니메이션 효과를 수 있습니다. Android 프레임 워크 (에 국한 되지 않음)와 같은 여러 interpolators를 기본적으로 제공합니다.

-   `AccelerateInterpolator` / `DecelerateInterpolator` &ndash; 이러한 interpolators 애니메이션의 변동률을 늘리거나 설정 합니다.

-   `BounceInterpolator` &ndash; 변경 후에 바운스합니다.

-   `LinearInterpolator` &ndash; 변경 속도가 상수입니다.


다음 XML에서는 이러한 요소 중 일부를 결합 하는 애니메이션 파일의 예제를 보여 줍니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android=http://schemas.android.com/apk/res/android
     android:shareInterpolator="false">

    <scale android:interpolator="@android:anim/accelerate_decelerate_interpolator"
           android:fromXScale="1.0"
           android:toXScale="1.4"
           android:fromYScale="1.0"
           android:toYScale="0.6"
           android:pivotX="50%"
           android:pivotY="50%"
           android:fillEnabled="true"
           android:fillAfter="false"
           android:duration="700" />

    <set android:interpolator="@android:anim/accelerate_interpolator">
        <scale android:fromXScale="1.4"
               android:toXScale="0.0"
               android:fromYScale="0.6"
               android:toYScale="0.0"
               android:pivotX="50%"
               android:pivotY="50%"
               android:fillEnabled="true"
               android:fillBefore="false"
               android:fillAfter="true"
               android:startOffset="700"
               android:duration="400" />

        <rotate android:fromDegrees="0"
                android:toDegrees="-45"
                android:toYScale="0.0"
                android:pivotX="50%"
                android:pivotY="50%"
                android:fillEnabled="true"
                android:fillBefore="false"
                android:fillAfter="true"
                android:startOffset="700"
                android:duration="400" />
    </set>
</set>
```

이 애니메이션의 애니메이션 모두 동시에 수행 합니다. 첫 번째 확장 애니메이션 이미지를 가로로 확장 되며 세로 축소 하 고 이미지는 동시에 시계 반대 방향으로 45도 회전 되 고 축소 화면에서 사라지지 합니다.

애니메이션 값 및 다음 뷰를 적용 하 여 보기에 애니메이션을 프로그래밍 방식으로 적용할 수 있습니다. 도우미 클래스를 제공 하는 android `Android.Views.Animations.AnimationUtils` 애니메이션 리소스를 확장 되며의 인스턴스를 반환 하는 `Android.Views.Animations.Animation`합니다. 이 개체를 호출 하 여 보기에 적용 됩니다 `StartAnimation` 전달 된 `Animation` 개체입니다. 다음 코드 조각은이 예를 보여 줍니다.

```csharp
Animation myAnimation = AnimationUtils.LoadAnimation(Resource.Animation.MyAnimation);
ImageView myImage = FindViewById<ImageView>(Resource.Id.imageView1);
myImage.StartAnimation(myAnimation);
```

이제 보기 애니메이션의 작동 방법에 대 한 기본 이해 했으므로 속성 애니메이션을 이동할 수 있습니다.


### <a name="property-animations"></a>속성 애니메이션

속성 애니메이터 Android 3.0에 도입 된 새 API 사용 됩니다.
모든 개체의 모든 속성에 애니메이션 효과를 사용할 수 있는 보다 확장성이 뛰어난 API를 제공 합니다.

모든 속성 애니메이션의 인스턴스에 의해 생성 되는 [애니메이터](https://developer.xamarin.com/api/type/Android.Animation.Animator/) 하위 클래스입니다. 응용 프로그램에서이 클래스를 직접 사용 하지 않는 경우의 서브 클래스 중 하나를 사용 하는 대신:

-   [ValueAnimator](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/) &ndash; 이 클래스는 전체 속성 애니메이션 API에서에서 가장 중요 한 클래스입니다. 변경 해야 하는 속성의 값을 계산 합니다. `ViewAnimator` 직접 이러한 값을 업데이트 하지는 않습니다 대신 애니메이션된 개체를 업데이트 하는 이벤트가 발생 합니다.

-   [ObjectAnimator](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/) &ndash; 이 클래스의 서브 클래스는 `ValueAnimator` 합니다. 대상 개체를 업데이트 하는 속성을 그대로 사용 하 여 개체를 애니메이션의 프로세스를 간소화 하는 것이 것입니다.

-   [AnimationSet](https://developer.xamarin.com/api/type/Android.Animation.AnimatorSet/) &ndash; 이 클래스는 애니메이션 관계에서 서로를 실행 하는 어떻게 오케스트레이션 하는 일을 담당 합니다. 애니메이션에는 서로 동시에, 순차적으로 또는 지정된 된 지연 시간을 사용 하 여 실행할 수 있습니다.


*계산기* 애니메이터 애니메이션 중에 새 값을 계산에 사용 되는 특수 클래스입니다. 기본적으로 Android는 다음 계산기를 제공합니다.

-   [IntEvaluator](https://developer.xamarin.com/api/type/Android.Animation.IntEvaluator/) &ndash; 정수 속성에 대 한 값을 계산 합니다.

-   [FloatEvaluator](https://developer.xamarin.com/api/type/Android.Animation.FloatEvaluator/) &ndash; float 속성 값을 계산 합니다.

-   [ArgbEvaluator](https://developer.xamarin.com/api/type/Android.Animation.ArgbEvaluator/) &ndash; 컬러 속성에 대 한 값을 계산 합니다.

애니메이션 효과가 적용 되는 속성에는 없는 경우는 `float`, `int` 컬러를 응용 프로그램을 구현 하 여 자신의 계산기를 만들 수 있습니다 또는 `ITypeEvaluator` 인터페이스입니다. (사용자 지정 평가 자가 구현은이 항목의 범위를 벗어납니다.)

#### <a name="using-the-valueanimator"></a>ValueAnimator를 사용 하 여

모든 애니메이션 하는 방법은 두 가지가: 애니메이션이 적용 된 값을 계산한 다음 일부 개체의 속성에 해당 값을 설정 합니다. 
[ValueAnimator](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/) 이 작동 하지 것입니다 개체에 직접 하지만 값만 계산 됩니다. 대신 개체를 애니메이션 수명 동안에 호출 되는 이벤트 처리기 내에서 업데이트 됩니다. 이 디자인 한 애니메이션이 적용 된 값에서 업데이트할 여러 속성이 있습니다.

인스턴스를 가져올 `ValueAnimator` 다음 팩터리 메서드 중 하나를 호출 하 여:

-  `ValueAnimator.OfInt`
-  `ValueAnimator.OfFloat`
-  `ValueAnimator.OfObject`

완료 되 면,는 `ValueAnimator` 인스턴스에 있어야 해당 기간을 설정 하 고 다음 시작할 수 있습니다. 다음 예제에는 0에서 1 사이의 값 1000 밀리초의 범위에 애니메이션 효과를 주는 방법을 보여 줍니다.

```csharp
ValueAnimator animator = ValueAnimator.OfInt(0, 100);
animator.SetDuration(1000);
animator.Start();
```

자체에 위의 코드 조각 그다지 유용 하지 않지만 &ndash; 애니메이터가 실행될지 이지만 업데이트 된 값에 대 한 대상이 없습니다. `Animator` 클래스 업데이트 이벤트 수신기의 새 값을 알리는 데 필요 하다 고 결정할 때 발생 합니다. 응용 프로그램에는 다음 코드 조각에 나와 있는 것 처럼이 이벤트에 응답할 이벤트 처리기를 제공할 수 있습니다.

```csharp
MyCustomObject myObj = new MyCustomObject();
myObj.SomeIntegerValue = -1;

animator.Update += (object sender, ValueAnimator.AnimatorUpdateEventArgs e) =>
{
    int newValue = (int) e.Animation.AnimatedValue;
    // Apply this new value to the object being animated.
    myObj.SomeIntegerValue = newValue;
};
```

이해를 만들었으므로 `ValueAnimator`, 수에 대해 자세히 알아보세요는 `ObjectAnimator`합니다.

#### <a name="using-the-objectanimator"></a>ObjectAnimator를 사용 하 여

[ObjectAnimator](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/) 서브 클래스입니다 `ViewAnimator` 타이밍 엔진 및 값 계산을 결합 하 여 `ValueAnimator` 이벤트 처리기를 연결 하는 데 필요한 논리를 사용 하 여 합니다. 합니다 `ValueAnimator` 이벤트 처리기를 명시적으로 연결 하는 응용 프로그램이 필요 &ndash; `ObjectAnimator` 우리 회사에이 단계의 처리 됩니다.

에 대 한 API `ObjectAnimator` 에 대 한 API에 매우 비슷합니다 `ViewAnimator`, 개체 및 업데이트 속성의 이름을 제공 해야 하지만 합니다. 다음 예제에서는 사용 하는 예로 `ObjectAnimator`:

```csharp
MyCustomObject myObj = new MyCustomObject();
myObj.SomeIntegerValue = -1;

ObjectAnimator animator = ObjectAnimator.OfFloat(myObj, "SomeIntegerValue", 0, 100);
animator.SetDuration(1000);
animator.Start();
```

이전 코드 조각에서 알 수 있듯이 `ObjectAnimator` 줄이고 개체에 애니메이션을 적용 하는 데 필요한 코드를 단순화할 수 있습니다.


### <a name="drawable-animations"></a>Drawable 애니메이션

마지막 애니메이션 API는 Drawable 애니메이션 API입니다. Drawable 애니메이션 다른 일련의 드로어 블 리소스 하나를 로드 하 고 순차적으로 표시할 대칭 이동 후 it 만화와 비슷합니다.

드로어 블 리소스 있는 XML 파일에 정의 되어는 `<animation-list>` 루트 요소와 일련의 요소로 `<item>` 애니메이션의 각 프레임을 정의 하는 요소입니다. 이 XML 파일에 저장 되는 `/Resource/drawable` 응용 프로그램의 폴더입니다. 다음 XML은 drawable 애니메이션의 예:

```xml
<animation-list xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:drawable="@drawable/asteroid01" android:duration="100" />
  <item android:drawable="@drawable/asteroid02" android:duration="100" />
  <item android:drawable="@drawable/asteroid03" android:duration="100" />
  <item android:drawable="@drawable/asteroid04" android:duration="100" />
  <item android:drawable="@drawable/asteroid05" android:duration="100" />
  <item android:drawable="@drawable/asteroid06" android:duration="100" />
</animation-list>
```

이 애니메이션은 6 개의 프레임을 통해 실행 됩니다. `android:duration` 특성은 각 프레임 표시할 기간을 선언 합니다. 다음 코드 조각은 Drawable 애니메이션을 만들고 시작 화면에서 단추를 클릭할 때 예를 보여 줍니다.

```csharp
AnimationDrawable _asteroidDrawable;

protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    _asteroidDrawable = (Android.Graphics.Drawables.AnimationDrawable)
    Resources.GetDrawable(Resource.Drawable.spinning_asteroid);

    ImageView asteroidImage = FindViewById<ImageView>(Resource.Id.imageView2);
    asteroidImage.SetImageDrawable((Android.Graphics.Drawables.Drawable) _asteroidDrawable);

    Button asteroidButton = FindViewById<Button>(Resource.Id.spinAsteroid);
    asteroidButton.Click += (sender, e) =>
    {
        _asteroidDrawable.Start();
    };
}
```

이 시점에서 Android 응용 프로그램에서 사용 가능한 Api 애니메이션의 foundations 설명 했습니다.


## <a name="summary"></a>요약

이 문서는 여러 가지 새로운 개념이 도입 및 Android 응용 프로그램에 일부 그래픽을 추가 하는 데 API. 먼저 다양 한 2D 그래픽 API를 설명 하 고 Android Canvas 개체를 사용 하 여 화면으로 직접 그릴 응용 프로그램을 사용 하는 방법을 보여 줍니다. XML 파일을 사용 하 여 선언적으로 만들어야 하는 그래픽을 허용 하는 몇 가지 대체 기술을 살펴보았습니다. 이동는 이전 및 새 API의 Android에서 애니메이션 만들기에 대 한 설명입니다.



## <a name="related-links"></a>관련 링크

- [애니메이션 데모 (샘플)](https://developer.xamarin.com/samples/monodroid/AnimationDemo)
- [애니메이션 및 그래픽](https://developer.android.com/guide/topics/graphics/index.html)
- [애니메이션을 사용 하 여 모바일 앱에 활기를](http://youtu.be/ikSk_ILg3d0)
- [AnimationDrawable](https://developer.xamarin.com/api/type/Android.Graphics.Drawables.AnimationDrawable/)
- [캔버스](https://developer.xamarin.com/api/type/Android.Graphics.Canvas/)
- [개체 애니메이터](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/)
- [값 애니메이터](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/)
