---
title: "그래픽 및 애니메이션"
description: "Android 2 차원 그래픽 및 애니메이션을 지원 하기 위한 풍부 하 고 다양 한 프레임 워크를 제공 합니다. 이 항목 이러한 프레임 워크를 소개 하 고 Xamarin.Android 응용 프로그램에서 사용자 지정 그래픽 및 사용에 대 한 애니메이션을 만드는 방법에 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 80086318-6FE4-4711-9A71-5C8F8C28C754
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: ce51511c58d7d0f5a14e487b57897bfa0e0b20b3
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="graphics-and-animation"></a>그래픽 및 애니메이션

_Android 2 차원 그래픽 및 애니메이션을 지원 하기 위한 풍부 하 고 다양 한 프레임 워크를 제공 합니다. 이 항목 이러한 프레임 워크를 소개 하 고 Xamarin.Android 응용 프로그램에서 사용자 지정 그래픽 및 사용에 대 한 애니메이션을 만드는 방법에 설명 합니다._


## <a name="overview"></a>개요

불구 하 고 제한 된 전원의 일반적으로 장치에서 실행을 가장 높은 정격된 모바일 응용 프로그램 경우가 정교한 사용자 경험 (UX), 완벽 하 게 높은 품질의 그래픽 및 직관적이 고 동적 응답 느낌을 제공 하는 애니메이션 합니다. 모바일 응용 프로그램 추가 정보 보기 및 더 복잡 한 사용자가 응용 프로그램에서 더 많은 기대 하기 시작 했습니다.

다행히 us에 대 한 최신 모바일 플랫폼에 사용 편의성을 유지 하면서 정교한 애니메이션 및 사용자 지정 그래픽을 만들기 위한 매우 강력한 프레임 워크는. 그러면 개발자가 매우 적은 노력으로 풍부한 대화형 작업을 추가할 수 있습니다.

Android에서 UI API 프레임 워크는 다음 두 가지 범주로 대략 분할 될 수 있습니다: 그래픽 및 애니메이션 합니다.

그래픽 2D 및 3D 그래픽을 수행 하는 데 다양 한 접근 방법으로 추가 분할 됩니다. 3D 그래픽은의 번호를 통해 사용할 수 있는 기본 제공 OpenGL ES (모바일의 특정 버전 OpenGL), 같은 프레임 워크 및 MonoGame (크로스 플랫폼 도구 키트는 XNA toolkit과 호환) 등의 타사 프레임 워크입니다. 3D 그래픽 되지 않지만이 문서의 범위 내에서 기본 제공 2D 그리기 기술을 살펴보겠습니다.

Android은 2 차원 그래픽을 만들기 위한 두 개의 서로 다른 API를 제공 합니다. 하나는 높은 수준의 선언적 방법 및 다른 프로그래밍 방식으로 낮은 수준의 API 합니다.

-   **그릴 수 있는 리소스** &ndash; 이러한 XML 파일에 그리기 지침을 포함 하 여 프로그래밍 방식으로 또는 더 일반적으로 사용자 지정 그래픽을 만드는 데 사용 됩니다. 그릴 수 있는 리소스는 일반적으로 지침 또는 2 차원 그래픽을 렌더링 하는 Android에 대 한 작업을 포함 하는 XML 파일로 정의 됩니다. 

-   **캔버스** &ndash; 기본 비트맵에 직접 그리기를 포함 하는 낮은 수준의 API입니다. 표시 되는 매우 세부적으로 제어를 제공 합니다. 

이러한 2 차원 그래픽 기술 외에도 Android 애니메이션을 만드는 여러 가지 방법으로 제공 합니다.

-   **그릴 애니메이션** &ndash; Android 라고 프레임별으로 애니메이션 지원 *그릴 애니메이션*합니다. 이 가장 간단한 애니메이션 API입니다. Android는 순차적으로 로드 하 고 그릴 수 있는 리소스 (매우 유사한 만화) 순서로 표시 됩니다.

-   **애니메이션** &ndash; *애니메이션 보기* Android에서 원래 애니메이션 API 이며 모든 버전의 Android에서 사용할 수 있습니다. 이 API는 뷰 개체 에서만 작동 하 고 간단한 변환을 보기에만 수행할 수 있다는 점에서 제한 합니다.
    애니메이션 보기는 일반적으로에 XML 파일에 정의 된 `/Resources/anim` 폴더입니다.

-   **속성 애니메이션** &ndash; 라고 Android 3.0가 애니메이션 API의 새로운 집합이 도입 되었습니다. *속성 애니메이션*합니다. 이러한 새 API 뿐 아니라 개체를 보고, 모든 개체의 속성에 애니메이션을 적용 하는 데 사용할 수 있는 확장 하 고 유연한 시스템을 도입 합니다. 이러한 유연성 애니메이션을을 코드를 쉽게 공유할 수 있도록 고유 클래스에 캡슐화 될 수 있습니다.


보기 애니메이션은 이전 사전 Android 3.0 API (API 수준 11)를 지원 해야 하는 응용 프로그램에 더 적합 합니다. 그렇지 않으면 위에서 언급 된 이유로 응용 프로그램에서 최신 속성 애니메이션 API를 사용 해야 합니다.

그러나 이러한 프레임 워크 모두 실행 가능한 옵션, 가능한 경우, 기본 설정에 부여 해야 속성 애니메이션으로 작업할 수 있는 보다 유연한 api입니다. 속성 애니메이션을 공유 하기 쉽도록 코드를 만들고 코드 유지 관리를 간소화 하는 애니메이션 논리 고유 클래스에 캡슐화를 허용 합니다.


## <a name="accessibility"></a>액세스 가능성

Android 앱 보기 좋게 하려면 그래픽 및 애니메이션 도움말 및을 사용한; 그러나 되기 일부 상호 작용 지원된 확대/축소 또는 screenreaders, 대체 입력된 장치를 통해 발생할 수 있다는 점을 기억해 야 합니다.
또한 일부 상호 작용 오디오 기능 없이 발생할 수 있습니다.

앱은 염두에서 내게 필요한 옵션 디자인 보다 이러한 상황에서 유용: 힌트와 사용자 인터페이스를 지 원하는 탐색을 제공 하 고 텍스트 콘텐츠 또는 UI의 그림 요소에 대 한 설명을 확인 합니다.

참조 [Google의 내게 필요한 옵션 가이드](http://developer.android.com/guide/topics/ui/accessibility/) Android의 내게 필요한 옵션 Api를 활용 하는 방법에 대 한 자세한 내용은 합니다.



## <a name="2d-graphics"></a>2 차원 그래픽

그릴 수 있는 리소스는 Android 응용 프로그램에서 널리 사용 되는 기술입니다. 다른 리소스와 그릴 수 있는 리소스를 선언적으로 &ndash; XML 파일에 정의 합니다. 이 방법은 리소스에서 코드의 확실히 구분할 수 있습니다. 업데이트 또는 Android 응용 프로그램의 그래픽을 변경 하는 코드를 변경할 필요가 없기 때문에 개발 및 유지 관리 단순화할 수 있습니다. 그러나 그릴 수 있는 리소스는 단순 하 고 일반적인 그래픽 요구 하는 데 유용 하지만, 부족 기능과 캔버스 API의 제어 합니다.

다른 기술을 사용 하는 [캔버스](https://developer.xamarin.com/api/type/Android.Graphics.Canvas/) 개체, System.Drawing 또는 iOS의 핵심 그리기 등의 다른 기존 API 프레임 워크와 매우 비슷합니다. 캔버스 개체를 사용 하 여 어떻게 2 차원 그래픽의 제어 권한이 가장 만들어집니다 제공 합니다. 것 그릴 수 있는 리소스는 작동 하지 않거나 사용 하기가 어려울 것 있는 경우에 적합 합니다. 예를 들어 슬라이더의 값과 관련 된 계산을 기초로 모양을 가진 바뀝니다 사용자 지정 슬라이더 컨트롤을 그리는 데 필요한 수 있습니다.

그릴 수 있는 리소스를 먼저 살펴보겠습니다. 더 간단 하며 가장 일반적인 사용자 지정 그리기 사례를 포함 합니다.


### <a name="drawable-resources"></a>그릴 수 있는 리소스

그릴 수 있는 리소스 디렉터리에 XML 파일에 정의 되어 `/Resources/drawable`합니다. PNG 또는 JPEG의를 포함 하는 달리 그릴 수 있는 리소스의 밀도 별 버전을 제공 하는 데 필요한 되지 않습니다.
런타임에 Android 응용 프로그램은 이러한 리소스를 로드 하 고 2 차원 그래픽을 만들려면 이러한 XML 파일에 포함 된 지침을 사용 합니다.
Android 몇 가지 유형의 그릴 수 있는 리소스를 정의합니다.

-   [ShapeDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Shape) &ndash; 는 기본 기 하 도형을 그립니다 하 여 제한 된 집합의 해당 모양에 그래픽 효과 적용 하는 그릴 수 있는 개체입니다. 단추를 사용자 지정 또는 TextViews의 배경 설정 등의 작업을 위한 매우 유용 합니다. 사용 하는 방법의 예가 표시 됩니다는 `ShapeDrawable` 이 문서의 뒷부분에 나오는 합니다.

-   [StateListDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#StateList) &ndash; 위젯/컨트롤의 상태를 기반으로 모양이 변화를 그릴 수 있는 리소스입니다. 예를 들어 단추 여부 누른 여부에 따라 모양이 변경 될 수 있습니다.

-   [LayerDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList) &ndash; 위에서 다른 하나는 다른 여러 drawables 표시를 그릴 수 있는 리소스입니다. 예로 *LayerDrawable* 다음 화면에 표시 됩니다.

    ![LayerDrawable 예제](graphics-and-animation-images/image1.png)

-   [TransitionDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Transition) &ndash; 이것이 *LayerDrawable* 하지만 한 가지 차이점입니다. A *TransitionDrawable* 한 계층 표시 top을 통해 다른 애니메이션 효과를 줄 수 있습니다.

-   [LevelListDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#LevelList) &ndash; 이것은 매우 비슷합니다는 *StateListDrawable* 하 특정 조건에 따라 이미지 표시 됩니다. 그러나 달리는 *StateListDrawable*, *LevelListDrawable* 정수 값에 따라 이미지를 표시 합니다. 예로 *LevelListDrawable* WiFi 신호 강도 표시 하는 것입니다. WiFi 신호 변경의 강도으로 표시 되는 그릴 따라 변경 됩니다.

-   [ScaleDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Scale)/[ClipDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Clip) &ndash; 이름에서 알 수 있듯이 이러한 Drawables 및 형식으로 제공 크기 조정 기능 클리핑 합니다. *ScaleDrawable* 를 그릴 수 있는 동안 다른 크기 조정 되는 *ClipDrawable* 다른 Drawable 클립 합니다.

-   [InsetDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Inset) &ndash; 이 그릴 수 있는 너비 그릴 수 있는 다른 리소스의 양쪽에 적용 됩니다. 뷰 보기의 실제 범위 보다 작은 백그라운드 해야 할 때 사용 됩니다.

-   XML [BitmapDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Bitmap) &ndash; 이 파일은 실제 비트맵에 수행할 되는 지침을 xml에서의 집합입니다. Android 수행할 수 있는 일부 작업은 타일링, 디더링, 및 앤티 앨리어싱을입니다. 레이아웃의 배경 전체 비트맵 바둑판식으로 배열 것이 매우 일반적인 용도 중 하나입니다.


#### <a name="drawable-example"></a>그릴 수 있는 예제

사용 하 여 2 차원 그래픽을 만드는 방법의 간단한 예를 살펴 보겠습니다는 `ShapeDrawable`합니다. A `ShapeDrawable` 네 가지 기본 셰이프 중 하나를 정의할 수 있습니다: 사각형, 타원, 선 및 링입니다. 그라데이션, 색상, 크기 등의 기본적인 효과 적용 하는 것도 가능 합니다. 다음 XML은는 `ShapeDrawable` 에서 찾을 수 있습니다는 *AnimationsDemo* 도우미 프로젝트 (파일에 `Resources/drawable/shape_rounded_blue_rect.xml`).
자주색 그라데이션 배경이 있는 사각형을 정의 하 고 모서리가 둥글게 변합니다.

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

레이아웃 또는 다음 XML에 표시 된 것 처럼 다른 Drawable에서 선언적으로 그릴 수 있는이 리소스를 참조할 수 있습니다.

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

그릴 수 있는 리소스를 프로그래밍 방식으로 적용할 수도 있습니다. 다음 코드 조각에 프로그래밍 방식으로 TextView의 배경색을 설정 하는 방법을 보여 줍니다.

```csharp
TextView tv = FindViewById<TextView>(Resource.Id.shapeDrawableTextView);
tv.SetBackgroundResource(Resource.Drawable.shape_rounded_blue_rect);
```

이 모양을 보려면, 실행의 *AnimationsDemo* 프로젝트 한 주 메뉴에서 도형을 그릴 항목을 선택 합니다. 다음 스크린샷과 유사 하 게 내용이 표시 해야 했습니다.

![그라데이션 및 둥근 모서리가 그릴 수 있는 사용자 지정 배경을 Textview](graphics-and-animation-images/image1.png)

XML 요소 및 그릴 수 있는 리소스의 구문에 대 한 자세한 내용은 참조 하십시오. [Google 설명서](http://developer.android.com/guide/topics/resources/drawable-resource.html#Shape)합니다.


### <a name="using-the-canvas-drawing-api"></a>캔버스 드로잉 API를 사용 하 여

Drawables 강력 하지만는 제한이 있습니다. 몇 가지 항목 불가능 또는 매우 복잡 한 (예: 장치에서 카메라에서 찍은 사진에 필터를 적용). 그릴 수 있는 리소스를 사용 하 여 적목 감소 적용할 매우 어렵습니다.
대신, 캔버스 API에는 선택적으로 색을 변경 하는 그림의 특정 부분에서 매우 세분화 된 제어를 응용을 프로그램 수 있습니다.

캔버스에 자주 사용 되는 한 클래스는 [페인트](https://developer.xamarin.com/api/type/Android.Graphics.Paint/) 클래스입니다. 이 클래스는을 그리는 방법에 대 한 색 및 스타일 정보를 포함 합니다. 이러한 색 및 투명도 작업 수 있도록 사용 됩니다.

캔버스 API가 사용 하는 *복사의 모델* 를 2 차원 그래픽을 그릴 합니다.
작업이 서로 위에 연속 된 계층에 적용 됩니다. 각 작업은 원본 비트맵의 일부 영역을 설명 합니다. 영역 겹치거나 이전에 그려지는 영역, 이전을 완전히 숨기 되거나는 새 페인트 부분적으로 됩니다. 이 다른 많은 그리기 Api System.Drawing, iOS의 핵심 그래픽 작동 하는 동일한 방식으로 합니다.

두 가지 방법으로 가져올 수는 `Canvas` 개체입니다. 첫 번째 방법은 정의가 포함 됩니다는 [비트맵](https://developer.xamarin.com/api/type/Android.Graphics.Bitmap/) 개체 및를 인스턴스화하는 `Canvas` 된 개체입니다. 예를 들어 다음 코드 조각은 기본 비트맵으로 새 캔버스를 만듭니다.

```csharp
Bitmap bitmap = Bitmap.CreateBitmap(100, 100, Bitmap.Config.Argb8888);
Canvas canvas = new Canvas(b);
```

검색 하는 다른 방법은 `Canvas` 로 개체가 있는 상태에서 [OnDraw](https://developer.xamarin.com/api/member/Android.Views.View.OnDraw/) 제공 되는 콜백 메서드는 [보기](https://developer.xamarin.com/api/type/Android.Views.View/) 기본 클래스입니다. Android 뷰 자체를 그려야 하 고 전달 결정할 때이 메서드를 호출 하는 `Canvas` 작업할 보기에 대 한 개체입니다.

캔버스 클래스 프로그래밍 방식으로 그리기 지침을 제공 하는 메서드를 노출 합니다. 예:

-   [Canvas.DrawPaint](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawPaint/p/Android.Graphics.Paint/) &ndash; 지정 된 색과 전체 캔버스의 비트맵을 채웁니다.

-   [Canvas.DrawPath](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawPath/p/Android.Graphics.Path/Android.Graphics.Paint/) &ndash; 지정된 그림판을 사용 하 여 지정 된 기 하 도형을 그립니다.

-   [Canvas.DrawText](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawText/p/System.String/System.Single/System.Single/Android.Graphics.Paint/) &ndash; 지정 된 색상을 사용 하 여 캔버스에 텍스트를 그립니다. 위치에 텍스트를 그리는 `x,y` 합니다.



#### <a name="drawing-with-the-canvas-api"></a>캔버스 API 사용 하 여 그리기

캔버스 api 작업에 예를 들어를 살펴보겠습니다. 다음 코드 조각에는 뷰를 그리는 방법을 보여 줍니다.

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

위의이 코드는 먼저 빨간색 페인트 및 녹색 페인트 개체에 만듭니다. 빨간색, 캔버스의 내용의 채우고 캔버스 너비의 25%가 녹색 사각형을 그리는 캔버스를 지시 합니다. 이 예제에서 볼 수 있습니다 `AnimationsDemo` 이 문서에 대 한 소스 코드에 포함 된 프로젝트입니다. 응용 프로그램을 시작 하는 주 메뉴에서 그리기 항목을 선택 하 여 다음과 비슷한 화면이 해야 했습니다.

![빨간색 페인트 및 녹색 페인트 개체와 함께 화면](graphics-and-animation-images/image3.png)


## <a name="animation"></a>애니메이션

사용자가 같은 응용 프로그램에 이동 하는 것입니다. 애니메이션 편리 하 게 응용 프로그램의 사용자 환경을 개선 하 고 구별 하는 데 도움이 됩니다. 최상의 애니메이션은 사용자가 자연 스러운 우려 때문에 표시 하지 않습니다. Android는 애니메이션에 대 한 다음과 같은 세 가지 API를 제공합니다.

-   **애니메이션 볼** &ndash; 원래 API입니다. 이러한 애니메이션 특정 보기 연결 되 고 보기의 내용에 간단한 변환을 수행할 수 있습니다. 단순성 때문에이 API 작업에도 유용함 같은 알파 애니메이션, 회전 및 등입니다.

-   **속성이 애니메이션** &ndash; 속성 애니메이션 Android 3.0에서 도입 되었습니다. 거의 모든 항목에 애니메이션 효과를 주는 응용 프로그램을 사용 합니다. 속성 애니메이션 데 사용할 수 있는 모든 개체의 속성을 변경할 개체를 화면에 표시 되지 않는 경우에 합니다.

-   **그릴 애니메이션** &ndash; 레이아웃에 영향이 매우 간단한 애니메이션을 적용 하는 데 사용 되는 특별 한 그릴 수 있는 리소스입니다.

일반적으로 속성 애니메이션은 기본 시스템을 사용은 보다 유연 하 고 더 많은 기능을 제공 합니다.


### <a name="view-animations"></a>애니메이션 보기

애니메이션 보기는 뷰로 제한 되며 시작 및 끝 위치, 크기, 회전, 투명도 같은 값에 애니메이션만 수행할 수 있습니다.
이러한 종류의 애니메이션은 일반적으로 이라고 *트윈 애니메이션*합니다. 애니메이션 보기는 두 가지 방법으로 정의할 수 있습니다 &ndash; 코드에서 프로그래밍 방식으로 또는 XML 파일을 사용 하 여 합니다. XML 파일 되는지 애니메이션 보기를 선언 하는 기본 방법은 때 읽기 쉽고 유지 관리가 더 용이 합니다.

애니메이션 XML 파일에 저장 됩니다는 `/Resources/anim` Xamarin.Android 프로젝트의 디렉터리입니다. 이 파일의 루트 요소와 다음 요소 중 하나를 가져야 합니다.

-   `alpha` &ndash; 페이드 인 또는 페이드 아웃 애니메이션입니다.

-   `rotate` &ndash; 회전 애니메이션입니다.

-   `scale` &ndash; 크기 조정 애니메이션입니다.

-   `translate` &ndash; 가로 및/또는 세로 동작입니다.

-   `set` &ndash; 다른 애니메이션 요소 중 하나 이상을 보유할 수 있는 컨테이너입니다.

기본적으로 모든 애니메이션이 XML 파일에 동시에 적용 됩니다. 순차적으로 발생 하는 애니메이션을 하려면 설정는 `android:startOffset` 위에 정의 된 요소 중 하나에 특성입니다.

애니메이션의 변경 비율을 사용 하 여 영향을 줄 수는 *보간*합니다. 보간 accelerated, 반복 또는 decelerated에 애니메이션 효과를 수 있습니다. Android 프레임 워크 (에 국한 되지 않습니다)와 같은 여러 가지 interpolators를 기본적으로 제공합니다.

-   `AccelerateInterpolator` / `DecelerateInterpolator` &ndash; 이러한 interpolators 나 애니메이션의 변경 비율을 낮춥니다.

-   `BounceInterpolator` &ndash; 변경 후에는 반송 합니다.

-   `LinearInterpolator` &ndash; 변경 속도 상수입니다.


다음 XML에서는 이러한 요소 중 일부를 결합 하는 애니메이션 파일의 예를 보여 줍니다.

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

이 애니메이션의 애니메이션 모든 동시에 수행 합니다. 첫 번째 눈금 애니메이션 되며 가로 이미지의 늘이기을 세로로 축소 및 이미지 동시에 시계 반대 방향으로 45도 회전된 되 고 축소, 화면에서 사라지지 합니다.

애니메이션 않아서 한 다음 보기에 적용 하 여 보기에 애니메이션을 프로그래밍 방식으로 적용할 수 있습니다. 도우미 클래스를 제공 하는 android `Android.Views.Animations.AnimationUtils` 애니메이션 리소스 확장할 되며의 인스턴스를 반환 하는 `Android.Views.Animations.Animation`합니다. 이 개체를 호출 하 여 보기에 적용 되 `StartAnimation` 전달는 `Animation` 개체입니다. 다음 코드 조각에서는 이러한 예제를 보여 줍니다.

```csharp
Animation myAnimation = AnimationUtils.LoadAnimation(Resource.Animation.MyAnimation);
ImageView myImage = FindViewById<ImageView>(Resource.Id.imageView1);
myImage.StartAnimation(myAnimation);
```

이제 보기 애니메이션의 작동 방식에 대 한 기본적인 이해 했으므로 속성 애니메이션 이동 수 있습니다.


### <a name="property-animations"></a>속성 애니메이션

속성 애니메이터 Android 3.0에 도입 된 새로운 API 사용 됩니다.
모든 개체의 모든 속성에 애니메이션을 적용 하는 데 사용할 수 있는 범위를 확장 API를 제공 합니다.

모든 속성 애니메이션의 인스턴스에 의해 생성 되는 [애니메이터](https://developer.xamarin.com/api/type/Android.Animation.Animator/) 하위 클래스입니다. 이 클래스를 직접 사용 하지 않는 응용 프로그램, 대신 사용의 서브 클래스 중 하나:

-   [ValueAnimator](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/) &ndash; 이 클래스는 전체 속성 애니메이션 API에서에서 가장 중요 한 클래스입니다. 변경 해야 하는 속성의 값을 계산 합니다. `ViewAnimator` ; 해당 값을 직접 업데이트 되지 않는 대신, 애니메이션된 개체를 업데이트 하는 데 사용할 수 있는 이벤트가 발생 합니다.

-   [ObjectAnimator](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/) &ndash; 이 클래스의 서브 클래스는 `ValueAnimator` 합니다. 이를 대상 개체 및 업데이트 하는 속성을 사용 하 여 애니메이션 개체의 프로세스를 간소화 합니다.

-   [AnimationSet](https://developer.xamarin.com/api/type/Android.Animation.AnimatorSet/) &ndash; 이 클래스는 애니메이션 관계에서 서로를 실행 하는 방법을 조정 합니다. 애니메이션 서로 동시에, 순차적으로 또는 지정 된 지연 시간을 실행할 수 있습니다.


*계산기* 은 애니메이터 애니메이션 중에 새 값을 계산 하는 데 사용 되는 특수 클래스입니다. 기본적으로 Android 다음 계산기를 제공합니다.

-   [IntEvaluator](https://developer.xamarin.com/api/type/Android.Animation.IntEvaluator/) &ndash; 정수 속성 값을 계산 합니다.

-   [FloatEvaluator](https://developer.xamarin.com/api/type/Android.Animation.FloatEvaluator/) &ndash; float 속성 값을 계산 합니다.

-   [ArgbEvaluator](https://developer.xamarin.com/api/type/Android.Animation.ArgbEvaluator/) &ndash; 컬러 속성에 대 한 값을 계산 합니다.

애니메이션을 적용 하는 속성 없으면는 `float`, `int` 컬러, 응용 프로그램을 구현 하 여 자신의 계산기를 만들 수 있습니다 또는 `ITypeEvaluator` 인터페이스입니다. (사용자 지정 평가기 구현이이 항목에서 다루지 않습니다.)

#### <a name="using-the-valueanimator"></a>ValueAnimator를 사용 하 여

모든 애니메이션에 두 가지: 애니메이션된 값을 계산 하 고 다음 일부 개체의 속성에 해당 값을 설정 합니다. 
[ValueAnimator](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/) 값을 계산만 하지만 그 작동 하지 것입니다 개체에 직접 합니다. 대신, 애니메이션 수명 동안에 호출 되는 이벤트 처리기 내 개체를 업데이트 됩니다. 이 디자인에서는 하나의 애니메이션된 값에서 업데이트 하도록 여러 가지 속성.

인스턴스를 가져올 `ValueAnimator` 다음 팩터리 방법 중 하나를 호출 하 여:

-  `ValueAnimator.OfInt`
-  `ValueAnimator.OfFloat`
-  `ValueAnimator.OfObject`

완료 했으면,는 `ValueAnimator` 인스턴스에 있어야 지속 집합과 후 시작 될 수 있습니다. 다음 예제는 1000 밀리초 동안 0에서 1 사이의 값에 애니메이션을 적용 하는 방법을 보여 줍니다.

```csharp
ValueAnimator animator = ValueAnimator.OfInt(0, 100);
animator.SetDuration(1000);
animator.Start();
```

하지만 자체를 위의 코드 조각 사용 하지 않습니다 매우 &ndash; 애니메이터가 실행 되는 업데이트 된 값에 대 한 대상인 합니다. `Animator` 클래스 새 값의 수신기를 알리기 위해 필요 하다 고 결정할 때 업데이트 이벤트를 발생 시킵니다. 응용 프로그램 다음 코드 조각에 나와 있는 것 처럼이 이벤트에 응답 하는 이벤트 처리기를 제공할 수 있습니다.

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

이해를 만들었으므로 이제 해당 `ValueAnimator`에 대 한 자세한 정보 수는 `ObjectAnimator`합니다.

#### <a name="using-the-objectanimator"></a>ObjectAnimator를 사용 하 여

[ObjectAnimator](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/) 의 서브 클래스가 `ViewAnimator` 타이밍 엔진과 값 계산을 결합 하는 `ValueAnimator` 이벤트 처리기를 연결 하는 데 필요한 논리와 합니다. `ValueAnimator` 이벤트 처리기를 명시적으로 연결 하는 응용 프로그램이 필요 &ndash; `ObjectAnimator` 하므로이 단계의 수행해 줍니다.

에 대 한 API `ObjectAnimator` API에 대 한 매우 비슷하지만 `ViewAnimator`, 개체 및 업데이트할 속성의 이름을 제공 해야 하지만 합니다. 다음 예제에서는 사용 하는 예제 `ObjectAnimator`:

```csharp
MyCustomObject myObj = new MyCustomObject();
myObj.SomeIntegerValue = -1;

ObjectAnimator animator = ObjectAnimator.OfFloat(myObj, "SomeIntegerValue", 0, 100);
animator.SetDuration(1000);
animator.Start();
```

이전 코드 조각에서 볼 수 있듯이 `ObjectAnimator` 개체에 애니메이션을 적용 해야 하는 코드를 단순화를 줄일 수 있습니다.


### <a name="drawable-animations"></a>그릴 애니메이션

마지막 애니메이션 API는 그릴 애니메이션 API입니다. 애니메이션을 그릴 수 있는 다른 일련의 그릴 수 있는 리소스 하나를 로드 하 고 순차적으로 표시할 플립 it 만화 비슷합니다.

그릴 수 있는 리소스에 있는 XML 파일에 정의 되어 있는 `<animation-list>` 루트 요소와 일련의 요소 `<item>` 애니메이션에서 각 프레임을 정의 하는 요소입니다. 이 XML 파일에 저장 되는 `/Resource/drawable` 응용 프로그램의 폴더입니다. 다음 XML은 그릴 애니메이션의 예:

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

이 애니메이션은 6 개의 프레임을 통해 실행 됩니다. `android:duration` 특성 선언 각 프레임 시간 표시 됩니다. 다음 코드 조각과 그릴 애니메이션을 만들고 시작 화면에서 단추를 클릭할 때 예를 보여 줍니다.

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

이 시점에서 애니메이션 Android 응용 프로그램에서 사용할 수 있는 Api의 기본 요 설명 했습니다.


## <a name="summary"></a>요약

이 문서는 많은 새로운 개념을 도입 하 고 API의 일부 그래픽 Android 응용 프로그램을 추가할 수 있도록 합니다. 먼저 다양 한 2 차원 그래픽 API를 설명 하 고 Android 캔버스 개체를 사용 하 여 화면에 직접 그리는 데 응용 프로그램을 사용 하는 방법을 살펴보았습니다. XML 파일을 사용 하 여 선언적으로 만들어야 하는 그래픽을 허용 하는 몇 가지 대체 방법을 살펴보았습니다. 그런 다음 곧 준비는 이전 구문과 새 API의 Android에서 애니메이션을 만드는 설명 합니다.



## <a name="related-links"></a>관련 링크

- [애니메이션 데모 (샘플)](https://developer.xamarin.com/samples/monodroid/AnimationDemo)
- [애니메이션 및 그래픽](http://developer.android.com/guide/topics/graphics/index.html)
- [애니메이션을 사용 하 여 모바일 앱 수명을 가져올 수](http://youtu.be/ikSk_ILg3d0)
- [AnimationDrawable](https://developer.xamarin.comhttps://developer.xamarin.com/api/type/Android.Graphics.Drawables.AnimationDrawable/)
- [캔버스](https://developer.xamarin.comhttps://developer.xamarin.com/api/type/Android.Graphics.Canvas/)
- [개체 애니메이터](https://developer.xamarin.comhttps://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/)
- [값 애니메이터](https://developer.xamarin.comhttps://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/)
