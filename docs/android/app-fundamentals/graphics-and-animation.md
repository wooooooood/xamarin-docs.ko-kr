---
title: 그래픽 및 애니메이션
description: Android는 2D 그래픽 및 애니메이션을 지원 하기 위한 매우 풍부 하 고 다양 한 프레임 워크를 제공 합니다. 이 항목에서는 이러한 프레임 워크를 소개 하 고 Xamarin Android 응용 프로그램에서 사용할 사용자 지정 그래픽 및 애니메이션을 만드는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 80086318-6FE4-4711-9A71-5C8F8C28C754
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 0a9921706acc4da076e98b1c42c0624c7f56e62f
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69521200"
---
# <a name="android-graphics-and-animation"></a>Android 그래픽 및 애니메이션

_Android는 2D 그래픽 및 애니메이션을 지원 하기 위한 매우 풍부 하 고 다양 한 프레임 워크를 제공 합니다. 이 항목에서는 이러한 프레임 워크를 소개 하 고 Xamarin Android 응용 프로그램에서 사용할 사용자 지정 그래픽 및 애니메이션을 만드는 방법을 설명 합니다._

## <a name="overview"></a>개요

일반적으로 기능이 제한 된 장치에서 실행 되는 경우에도 가장 높은 등급의 모바일 응용 프로그램에는 직관적이 고 응답성이 뛰어난 동적인 느낌을 제공 하는 고품질의 그래픽과 애니메이션이 포함 된 정교한 UX (사용자 환경)가 포함 되어 있는 경우가 많습니다. 모바일 응용 프로그램은 더 복잡 해지고 응용 프로그램에서 더 많은 작업이 시작 되었습니다.

다행히 최신 모바일 플랫폼은 사용 편의성을 유지 하면서 정교한 애니메이션과 사용자 지정 그래픽을 만들기 위한 매우 강력한 프레임 워크를 포함 하 고 있습니다. 이렇게 하면 개발자가 매우 적은 노력으로 풍부한 대화형 작업을 추가할 수 있습니다.

Android의 UI API 프레임 워크는 대략적으로 다음과 같은 두 가지 범주로 나눌 수 있습니다. 그래픽 및 애니메이션.

그래픽은 2D 및 3D 그래픽을 수행 하는 다양 한 방법으로 추가로 분할 됩니다. 3D 그래픽은 OpenGL ES (OpenGL의 모바일 특정 버전) 및 MonoGame와 같은 타사 프레임 워크 (XNA toolkit와 호환 되는 플랫폼 간 도구 키트)와 같은 다양 한 내장 프레임 워크를 통해 사용할 수 있습니다. 3D 그래픽은이 문서의 범위에 포함 되지 않지만 기본 제공 되는 2D 그리기 기법을 살펴보겠습니다.

Android는 2D 그래픽을 만들기 위한 두 가지 API를 제공 합니다. 하나는 높은 수준의 선언적 방법과 기타 프로그래밍 수준이 낮은 API입니다.

- **그릴 때 리소스** &ndash; 이러한 기능은 XML 파일에 그리기 명령을 포함 하 여 프로그래밍 방식으로 또는 (더 일반적으로) 사용자 지정 그래픽을 만드는 데 사용 됩니다. 그릴 수 있는 리소스는 일반적으로 Android에서 2D 그래픽을 렌더링 하기 위한 명령이 나 작업을 포함 하는 XML 파일로 정의 됩니다. 

- **Canvas** &ndash; 이는 기본 비트맵에서 직접 그리기를 포함 하는 하위 수준 API입니다. 표시 되는 항목에 대해 매우 세분화 된 제어를 제공 합니다. 

Android는 이러한 2D 그래픽 기술 외에도 다양 한 방법으로 애니메이션을 만들 수 있습니다.

- **그릴 때 애니메이션** Android는 그릴 수 있는 애니메이션 이라고 하는 프레임별 애니메이션도 지원 합니다. &ndash; 이는 가장 간단한 애니메이션 API입니다. Android는 순차적으로 그릴 리소스를 순차적으로 로드 하 고 표시 합니다 (예: 카툰).

- **애니메이션 보기** 보기 애니메이션은 android의 원본 애니메이션 API 이며 모든 버전의 android에서 사용할 수 있습니다. &ndash; 이 API는 뷰 개체 에서만 작동 하 고 해당 보기에 대 한 간단한 변환만 수행할 수 있도록 제한 됩니다.
    뷰 애니메이션은 일반적으로 `/Resources/anim` 폴더에 있는 XML 파일에 정의 됩니다.

- **속성 애니메이션** Android 3.0에는 *속성 애니메이션*이라는 새로운 애니메이션 API 집합이 도입 되었습니다. &ndash; 이러한 새 API에는 뷰 개체 뿐만 아니라 개체의 속성에 애니메이션 효과를 주는 데 사용할 수 있는 확장 가능 하 고 유연한 시스템이 도입 되었습니다. 이러한 유연성을 통해 애니메이션은 코드를 보다 쉽게 공유할 수 있는 고유한 클래스로 캡슐화 할 수 있습니다.


보기 애니메이션은 이전의 Android 이전 3.0 API (API 수준 11)를 지원 해야 하는 응용 프로그램에 더 적합 합니다. 그렇지 않으면 위에서 설명한 이유로 응용 프로그램에서 최신 속성 애니메이션 API를 사용 해야 합니다.

이러한 프레임 워크는 모두 사용할 수 있는 옵션 이지만 가능 하면 속성 애니메이션에 대 한 우선 순위를 지정 해야 합니다 .이는 작업을 수행 하는 더 유연한 API입니다. 속성 애니메이션을 사용 하면 코드를 보다 쉽게 공유 하 고 코드 유지 관리를 간소화 하는 고유한 클래스에서 애니메이션 논리를 캡슐화 할 수 있습니다.


## <a name="accessibility"></a>액세스 가능성

그래픽 및 애니메이션은 Android 앱을 사용 하는 데 유용 하 고 재미 있게 만듭니다. 그러나 screenreaders, 대체 입력 장치 또는 확대/축소를 통해 일부 상호 작용이 발생 한다는 것을 기억해 야 합니다.
또한 일부 상호 작용은 오디오 기능 없이 발생할 수 있습니다.

사용자 인터페이스에서 힌트 및 탐색 지원을 제공 하 고 UI의 그림 요소에 대 한 텍스트 내용 또는 설명이 있는지 확인 하 여 이러한 상황에서 응용 프로그램을 더 사용할 수 있습니다.

Android의 접근성 Api를 활용 하는 방법에 대 한 자세한 내용은 [Google의 접근성 가이드](https://developer.android.com/guide/topics/ui/accessibility/) 를 참조 하세요.



## <a name="2d-graphics"></a>2D 그래픽

그릴 수 있는 리소스는 Android 응용 프로그램의 인기 있는 기술입니다. 다른 리소스와 마찬가지로 그릴 수 있는 리소스는 &ndash; 선언적으로 XML 파일에 정의 됩니다. 이 방법을 사용 하면 리소스에서 코드를 명확 하 게 분리할 수 있습니다. 이렇게 하면 Android 응용 프로그램에서 그래픽을 업데이트 하거나 변경 하기 위해 코드를 변경할 필요가 없으므로 개발 및 유지 관리가 간단해 집니다. 그러나 그릴 수 있는 리소스는 간단 하 고 일반적인 그래픽 요구 사항에 유용 하지만 Canvas API의 기능과 제어는 없습니다.

[Canvas](xref:Android.Graphics.Canvas) 개체를 사용 하는 다른 기술은 시스템 Drawing 또는 IOS의 핵심 드로잉과 같은 기존의 다른 API 프레임 워크와 매우 비슷합니다. Canvas 개체를 사용 하 여 2D 그래픽이 생성 되는 방식을 가장 많이 제어할 수 있습니다. 그릴 수 있는 리소스가 작동 하지 않거나 작업 하기가 어려운 상황에 적합 합니다. 예를 들어 슬라이더의 값과 관련 된 계산에 따라 모양이 변경 되는 사용자 지정 슬라이더 컨트롤을 그려야 할 수 있습니다.

먼저 그릴 수 있는 리소스를 살펴보겠습니다. 가장 간단 하 고 가장 일반적인 사용자 지정 그리기 사례를 포함 합니다.


### <a name="drawable-resources"></a>그릴 때 리소스

그릴 수 있는 리소스는 디렉터리 `/Resources/drawable`의 XML 파일에 정의 됩니다. PNG 또는 JPEG를 포함 하는 것과 달리 그릴 수 있는 리소스의 밀도 별 버전을 제공할 필요는 없습니다.
런타임에 Android 응용 프로그램은 이러한 리소스를 로드 하 고 이러한 XML 파일에 포함 된 지침을 사용 하 여 2D 그래픽을 만듭니다.
Android는 여러 가지 형식의 그릴 수가 있는 리소스를 정의 합니다.

- [ShapeDrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#Shape) &ndash; 이 개체는 기본 도형을 그리고 해당 도형에 제한 된 그래픽 효과 집합을 적용 하는 그릴 수 있는 개체입니다. 단추 사용자 지정 또는 TextViews의 배경 설정과 같은 작업에 매우 유용 합니다. 이 문서의 뒷부분에서를 `ShapeDrawable` 사용 하는 방법에 대 한 예제를 확인할 수 있습니다.

- [Statelistdrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#StateList) 때 &ndash; 위젯/컨트롤의 상태에 따라 모양을 변경 하는 그릴 수 있는 리소스입니다. 예를 들어 단추는 눌러져 있는지 여부에 따라 모양이 변경 될 수 있습니다.

- [LayerDrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList) &ndash; 다른 모든 drawables을 스택에 있는이 그릴 수 있는이 리소스는 다른 모든 drawables 가장 합니다. 다음 스크린샷에서는 *LayerDrawable* 의 예를 보여 줍니다.

    ![LayerDrawable 예제](graphics-and-animation-images/image1.png)

- [TransitionDrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#Transition) 이는 LayerDrawable 하지만 한 가지 차이점이 있습니다. &ndash; *TransitionDrawable* 는 한 레이어에 애니메이션 효과를 줄 수 있습니다.

- [Levellistdrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#LevelList) 때 이는 특정 조건에 따라 이미지를 표시 한다는 점에서 사용 가능한 statelistdrawable 매우 비슷합니다. &ndash; 그러나 *Statelistdrawable*때와 달리 *levellistdrawable* 때는 정수 값을 기준으로 이미지가 표시 됩니다. *Levellistdrawable* 수 있는 예제는 WiFi 신호의 강도를 표시 하는 것입니다. WiFi 신호의 강도가 변경 되 면 표시 되는 그릴 수 있는 것이 그에 따라 변경 됩니다.

- [ScaleDrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#Scale)/[ClipDrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#Clip)는 &ndash;이름이 암시 하므로 이러한 drawables 크기 조정 및 클리핑 기능을 모두 제공 합니다. *ScaleDrawable* 는 다른 그릴 수 있는 다른 것으로 크기를 조정할 수 있습니다.

- [InsetDrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#Inset) &ndash; 이 경우 그릴 수 있는 다른 리소스의 측면에 음각이 적용 됩니다. 보기의 실제 범위 보다 작은 배경이 보기에 필요할 때 사용 됩니다.

- Xml [BitmapDrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#Bitmap) &ndash; 이 파일은 실제 비트맵에서 수행 되는 명령 집합 (xml)입니다. Android에서 수행할 수 있는 작업에는 바둑판식 배열, 디더링 및 앤티앨리어싱을 사용할 수 있습니다. 이를 사용 하는 가장 일반적인 방법 중 하나는 레이아웃의 배경 전체에 비트맵을 바둑판식으로 배열 하는 것입니다.

#### <a name="drawable-example"></a>그릴 때 예제

을 `ShapeDrawable`사용 하 여 2d 그래픽을 만드는 방법에 대 한 간단한 예제를 살펴보겠습니다. 는 `ShapeDrawable` 사각형, 타원, 선 및 링의 네 가지 기본 도형 중 하나를 정의할 수 있습니다. 그라데이션, 색, 크기 등의 기본 효과를 적용할 수도 있습니다. 다음 XML `ShapeDrawable` 은 *AnimationsDemo* 자매 프로젝트 (파일 `Resources/drawable/shape_rounded_blue_rect.xml`)에서 찾을 수 있는입니다.
자주색 그라데이션 배경과 둥근 모퉁이가 있는 사각형을 정의 합니다.

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

다음 XML에 표시 된 것 처럼 레이아웃 또는 다른 방법으로이 그릴 수 있는 리소스를 선언적으로 참조할 수 있습니다.

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

그릴 수 있는 리소스를 프로그래밍 방식으로 적용할 수도 있습니다. 다음 코드 조각에서는 TextView의 배경을 프로그래밍 방식으로 설정 하는 방법을 보여 줍니다.

```csharp
TextView tv = FindViewById<TextView>(Resource.Id.shapeDrawableTextView);
tv.SetBackgroundResource(Resource.Drawable.shape_rounded_blue_rect);
```

이 모양을 확인 하려면 *AnimationsDemo* 프로젝트를 실행 하 고 주 메뉴에서 그릴 수 있는 모양 항목을 선택 합니다. 다음 스크린샷에서 유사한 내용이 표시 되어야 합니다.

![그라데이션 및 모퉁이가 둥근 모퉁이가 있는 사용자 지정 배경을 사용 하는 Textview](graphics-and-animation-images/image1.png)

XML 요소 및 그릴 수가 있는 리소스의 구문에 대 한 자세한 내용은 [Google 설명서](https://developer.android.com/guide/topics/resources/drawable-resource.html#Shape)를 참조 하세요.


### <a name="using-the-canvas-drawing-api"></a>Canvas Drawing API 사용

Drawables은 강력 하지만 제한 사항이 있습니다. 특정 항목은 불가능 하거나 매우 복잡할 수 있습니다 (예: 장치의 카메라에서 찍은 그림에 필터 적용). 그릴 수 있는 리소스를 사용 하 여 적목 현상 감소를 적용 하기가 매우 어렵습니다.
대신 Canvas API를 사용 하면 응용 프로그램에서 그림의 특정 부분에 있는 색을 선택적으로 변경할 수 있는 매우 세분화 된 제어를 사용할 수 있습니다.

Canvas와 함께 일반적으로 사용 되는 클래스 중 하나는 [Paint](xref:Android.Graphics.Paint) 클래스입니다. 이 클래스는를 그리는 방법에 대 한 색 및 스타일 정보를 포함 합니다. 이러한 항목은 색 및 투명도를 제공 하는 데 사용 됩니다.

Canvas API는 *복사의 모델* 을 사용 하 여 2d 그래픽을 그립니다.
작업은 연속 된 계층의 서로 위에 적용 됩니다. 각 작업은 기본 비트맵의 일부 영역을 포함 합니다. 영역이 이전에 그린 영역과 겹치면 새 그리기가 부분적으로 또는 완전히 보이지 않습니다. 이는 시스템 그리기 및 iOS의 핵심 그래픽과 같은 다른 많은 그리기 Api가 작동 하는 방식과 동일 합니다.

개체를 `Canvas` 가져오는 방법에는 두 가지가 있습니다. 첫 번째 방법은 [Bitmap](xref:Android.Graphics.Bitmap) 개체를 정의한 다음이 개체를 사용 하 `Canvas` 여 개체를 인스턴스화하는 것입니다. 예를 들어 다음 코드 조각에서는 기본 비트맵이 있는 새 캔버스를 만듭니다.

```csharp
Bitmap bitmap = Bitmap.CreateBitmap(100, 100, Bitmap.Config.Argb8888);
Canvas canvas = new Canvas(b);
```

개체를 `Canvas` 가져오는 다른 방법은 [뷰](xref:Android.Views.View) 기본 클래스를 제공 하는 [OnDraw](xref:Android.Views.View.OnDraw*) 콜백 메서드를 사용한 것입니다. Android는 뷰가 작업을 수행 하기 위해 자신을 그려야 하 고 `Canvas` 개체를 전달 해야 한다고 결정할 때이 메서드를 호출 합니다.

Canvas 클래스는 그리기 명령을 프로그래밍 방식으로 제공 하는 메서드를 노출 합니다. 예를 들어:

- [Canvas.DrawPaint](xref:Android.Graphics.Canvas.DrawPaint*)&ndash;는 전체 비트맵을 지정 된 그리기로 채웁니다.

- [Canvas.](xref:Android.Graphics.Canvas.DrawPath*) &ndash; 지정 된 그리기를 사용 하 여 지정 된 기하학적 모양을 그립니다.

- [Canvas.DrawText](xref:Android.Graphics.Canvas.DrawText*)&ndash; 는 지정 된 색을 사용하여 텍스트를 그립니다. 텍스트를 위치 `x,y` 에 그립니다.

#### <a name="drawing-with-the-canvas-api"></a>Canvas API를 사용 하 여 그리기

작업 중인 Canvas API의 예제를 살펴보겠습니다. 다음 코드 조각에서는 뷰를 그리는 방법을 보여 줍니다.

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

위의이 코드는 먼저 빨강 페인트 및 녹색 그리기 개체를 만듭니다. 캔버스의 콘텐츠를 빨강으로 채운 다음 캔버스에서 캔버스 너비의 25% 인 녹색 사각형을 그리도록 지시 합니다. 이에 대 한 예제는이 문서에 `AnimationsDemo` 대 한 소스 코드와 함께 제공 되는 프로젝트에서 볼 수 있습니다. 응용 프로그램을 시작 하 고 주 메뉴에서 그리기 항목을 선택 하면 다음과 같은 화면이 나타납니다.

![빨간색 그리기 및 녹색 그리기 개체가 있는 화면](graphics-and-animation-images/image3.png)


## <a name="animation"></a>애니메이션

응용 프로그램에서 이동 하는 등의 사용자 애니메이션은 응용 프로그램의 사용자 환경을 개선 하 고 확장 하는 데 도움이 되는 유용한 방법입니다. 가장 좋은 애니메이션은 자연스럽 게 생각 하기 때문에 사용자가 확인 하지 않는 애니메이션입니다. Android는 애니메이션에 대해 다음과 같은 세 가지 API를 제공 합니다.

- **애니메이션 보기** &ndash; 이것은 원래 API입니다. 이러한 애니메이션은 특정 뷰에 연결 되며 뷰의 내용에 대 한 간단한 변환을 수행할 수 있습니다. 이 API는 간단 하 게 하기 때문에 알파 애니메이션, 회전 등과 같은 작업에도 유용 합니다.

- **속성 애니메이션** &ndash; 속성 애니메이션은 Android 3.0에서 도입 되었습니다. 응용 프로그램에서 거의 모든 항목에 애니메이션 효과를 주는 데 사용할 수 있습니다. 속성 애니메이션은 개체가 화면에 표시 되지 않는 경우에도 개체의 모든 속성을 변경 하는 데 사용할 수 있습니다.

- **그릴 때 애니메이션** &ndash; 이는 레이아웃에 매우 간단한 애니메이션 효과를 적용 하는 데 사용 되는 특별 한 그릴 수 있는 리소스입니다.

일반적으로 속성 애니메이션은 더 유연 하 고 더 많은 기능을 제공 하므로 사용할 기본 설정 시스템입니다.


### <a name="view-animations"></a>애니메이션 보기

보기 애니메이션은 뷰로 제한 되며 시작점과 끝점, 크기, 회전 및 투명도와 같은 값에 대 한 애니메이션만 수행할 수 있습니다.
이러한 유형의 애니메이션은 일반적으로 *트윈 애니메이션*이라고 합니다. 뷰 애니메이션은 코드에서 프로그래밍 방식 &ndash; 으로 또는 XML 파일을 사용 하 여 두 가지 방법으로 정의할 수 있습니다. XML 파일은 보기 애니메이션을 보다 읽기 쉽고 유지 관리가 쉽도록 선언 하는 기본 방법입니다.

애니메이션 XML 파일은 xamarin.ios 프로젝트의 `/Resources/anim` 디렉터리에 저장 됩니다. 이 파일에는 루트 요소로 다음 요소 중 하나가 있어야 합니다.

- `alpha`&ndash; 페이드 인 또는 페이드 아웃 애니메이션입니다.

- `rotate`&ndash; 회전 애니메이션입니다.

- `scale`&ndash; 크기 조정 애니메이션입니다.

- `translate`&ndash; 가로 및/또는 세로 동작입니다.

- `set`&ndash; 하나 이상의 다른 애니메이션 요소를 포함할 수 있는 컨테이너입니다.

기본적으로 XML 파일의 모든 애니메이션은 동시에 적용 됩니다. 애니메이션을 순차적으로 발생 시키려면 위에 정의 `android:startOffset` 된 요소 중 하나에 특성을 설정 합니다.

*보간 기*를 사용 하 여 애니메이션의 변경 률에 영향을 줄 수 있습니다. 보간 기를 사용 하 여 애니메이션 효과를 가속화, 반복 또는 decelerated 수 있습니다. Android 프레임 워크는 다음과 같은 다양 한 interpolators를 제공 합니다 (이에 국한 되지 않음).

- `AccelerateInterpolator`이러한interpolators애니메이션의변경률을늘리거나줄입니다/. `DecelerateInterpolator` &ndash;

- `BounceInterpolator`&ndash; 변경 내용이 끝에 바운스 됩니다.

- `LinearInterpolator`&ndash; 변경 비율은 일정 합니다.


다음 XML에서는 이러한 몇 가지 요소를 결합 하는 애니메이션 파일의 예를 보여 줍니다.

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

이 애니메이션은 모든 애니메이션을 동시에 수행 합니다. 첫 번째 눈금 애니메이션은 이미지를 가로로 늘이고 세로로 축소 합니다. 그러면 이미지가 동시에 시계 반대 방향으로 45 회전 하 고 축소 되어 화면에서 사라지지 않습니다.

애니메이션을 않아서 한 다음 뷰에 애니메이션을 적용 하 여 프로그래밍 방식으로 뷰에 애니메이션을 적용할 수 있습니다. Android는 애니메이션 리소스를 `Android.Views.Animations.AnimationUtils` 확장 하 고의 `Android.Views.Animations.Animation`인스턴스를 반환 하는 도우미 클래스를 제공 합니다. 이 개체는를 호출 `StartAnimation` 하 고 개체를 `Animation` 전달 하 여 뷰에 적용 됩니다. 다음 코드 조각에서는이에 대 한 예를 보여 줍니다.

```csharp
Animation myAnimation = AnimationUtils.LoadAnimation(Resource.Animation.MyAnimation);
ImageView myImage = FindViewById<ImageView>(Resource.Id.imageView1);
myImage.StartAnimation(myAnimation);
```

이제 보기 애니메이션의 작동 방식에 대 한 기본적인 내용을 이해 했으므로 속성 애니메이션으로 이동할 수 있습니다.


### <a name="property-animations"></a>속성 애니메이션

속성 애니메이터는 Android 3.0에 도입 된 새로운 API입니다.
모든 개체의 속성에 애니메이션 효과를 주는 데 사용할 수 있는 보다 확장 가능한 API를 제공 합니다.

모든 속성 애니메이션은 [애니메이터](xref:Android.Animation.Animator) 하위 클래스의 인스턴스를 통해 생성 됩니다. 응용 프로그램은이 클래스를 직접 사용 하지 않고 대신 클래스의 하위 클래스 중 하나를 사용 합니다.

- [Valueanimator](xref:Android.Animation.ValueAnimator) &ndash; 이 클래스는 전체 속성 애니메이션 API에서 가장 중요 한 클래스입니다. 변경 해야 하는 속성의 값을 계산 합니다. 는 `ViewAnimator` 이러한 값을 직접 업데이트 하지 않고 애니메이션 개체를 업데이트 하는 데 사용할 수 있는 이벤트를 발생 시킵니다.

- [Objectanimator](xref:Android.Animation.ObjectAnimator) 이 클래스는의 `ValueAnimator` 서브 클래스입니다. &ndash; 업데이트를 위해 대상 개체 및 속성을 허용 하 여 개체에 애니메이션 효과를 주는 프로세스를 간소화 합니다.

- [애니메이션 집합](xref:Android.Animation.AnimatorSet) &ndash; 이 클래스는 애니메이션이 서로 관련 하 여 실행 되는 방식을 오케스트레이션 하는 일을 담당 합니다. 애니메이션은 동시에 또는 순차적으로 실행 되거나 지정 된 지연 시간 동안 실행 될 수 있습니다.


*계산기* 는 애니메이터에서 애니메이션 중에 새 값을 계산 하는 데 사용 하는 특수 클래스입니다. Android에서 제공 하는 기본 제공 되는 계산기는 다음과 같습니다.

- [IntEvaluator](xref:Android.Animation.IntEvaluator) &ndash; 정수 속성의 값을 계산 합니다.

- [FloatEvaluator](xref:Android.Animation.FloatEvaluator) &ndash; Float 속성의 값을 계산 합니다.

- [Argbevaluator](xref:Android.Animation.ArgbEvaluator) &ndash; 색 속성의 값을 계산 합니다.

애니메이션 효과가 적용 되는 속성이 `float`, `int` 또는 색이 아니면 응용 프로그램은 `ITypeEvaluator` 인터페이스를 구현 하 여 고유한 계산기를 만들 수 있습니다. 사용자 지정 계산기를 구현 하는 것은이 항목의 범위를 벗어나는 것입니다.

#### <a name="using-the-valueanimator"></a>ValueAnimator 사용

애니메이션에는 애니메이션이 적용 된 값을 계산 하 고 일부 개체의 속성에 해당 값을 설정 하는 두 부분이 있습니다. 
[Valueanimator](xref:Android.Animation.ValueAnimator) 는 값만 계산 하지만 개체에서 직접 작동 하지 않습니다. 대신, 개체는 애니메이션 수명 중에 호출 되는 이벤트 처리기 내에서 업데이트 됩니다. 이 설계를 통해 하나의 애니메이션 된 값에서 여러 속성을 업데이트할 수 있습니다.

다음 팩터리 메서드 중 하나 `ValueAnimator` 를 호출 하 여의 인스턴스를 가져옵니다.

- `ValueAnimator.OfInt`
- `ValueAnimator.OfFloat`
- `ValueAnimator.OfObject`

이 작업을 완료 `ValueAnimator` 한 후에는 인스턴스에 기간이 설정 되어 있어야 하며 시작 될 수 있습니다. 다음 예제에서는 1000 밀리초의 범위에 대해 0에서 1 사이의 값에 애니메이션 효과를 주는 방법을 보여 줍니다.

```csharp
ValueAnimator animator = ValueAnimator.OfInt(0, 100);
animator.SetDuration(1000);
animator.Start();
```

하지만 위의 코드 조각은 매우 유용 &ndash; 하지 않습니다. 즉, 애니메이터는 실행 되지만 업데이트 된 값의 대상이 없습니다. 클래스 `Animator` 는 수신기에 새 값을 알려야 한다고 결정할 때 업데이트 이벤트를 발생 시킵니다. 응용 프로그램은 다음 코드 조각과 같이이 이벤트에 응답 하는 이벤트 처리기를 제공할 수 있습니다.

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

이제 `ValueAnimator`를 이해 했으므로에 대해 자세히 알아볼 `ObjectAnimator`수 있습니다.

#### <a name="using-the-objectanimator"></a>ObjectAnimator 사용

[Objectanimator](xref:Android.Animation.ObjectAnimator) 는의 타이밍 엔진과 `ViewAnimator` `ValueAnimator` 값 계산을 이벤트 처리기를 연결 하는 데 필요한 논리와 결합 하는의 서브 클래스입니다. 에서 `ValueAnimator` 이벤트 처리기 &ndash;를명시적으로 연결하는데필요한응용프로그램에서는이단계를수행합니다.`ObjectAnimator`

용 `ObjectAnimator` api는의 `ViewAnimator`api와 매우 비슷하지만 업데이트할 속성의 이름과 개체를 제공 해야 합니다. 다음 예에서는를 사용 하 `ObjectAnimator`는 예를 보여 줍니다.

```csharp
MyCustomObject myObj = new MyCustomObject();
myObj.SomeIntegerValue = -1;

ObjectAnimator animator = ObjectAnimator.OfFloat(myObj, "SomeIntegerValue", 0, 100);
animator.SetDuration(1000);
animator.Start();
```

위의 코드 조각 `ObjectAnimator` 에서 볼 수 있듯이는 개체에 애니메이션 효과를 주는 데 필요한 코드를 줄이고 단순화할 수 있습니다.


### <a name="drawable-animations"></a>그릴 때 애니메이션

최종 애니메이션 API는 그릴 수 있는 애니메이션 API입니다. 그릴 수 있는 애니메이션은 그릴 수 있는 일련의 리소스를 차례로 로드 하 고 대칭 이동 하는 만화와 비슷하게 순차적으로 표시 합니다.

그릴 수 있는 리소스는 `<animation-list>` 요소를 루트 요소로 포함 하는 XML 파일 및 애니메이션의 각 프레임을 정의 하는 일련의 `<item>` 요소로 정의 됩니다. 이 XML 파일은 응용 프로그램의 `/Resource/drawable` 폴더에 저장 됩니다. 다음 XML은 그릴 수 있는 애니메이션의 예입니다.

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

이 애니메이션은 6 개의 프레임을 통해 실행 됩니다. 특성 `android:duration` 은 각 프레임이 표시 되는 기간을 선언 합니다. 다음 코드 조각에서는 그릴 수 있는 애니메이션을 만들고 사용자가 화면에서 단추를 클릭 하면 시작 하는 예제를 보여 줍니다.

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

이 시점에서는 Android 응용 프로그램에서 사용할 수 있는 애니메이션 Api의 기초에 대해 설명 했습니다.


## <a name="summary"></a>요약

이 문서에는 Android 응용 프로그램에 그래픽을 추가 하는 데 도움이 되는 많은 새로운 개념과 API가 도입 되었습니다. 먼저 다양 한 2D 그래픽 API에 대해 설명 하 고 Android에서 캔버스 개체를 사용 하 여 응용 프로그램을 화면에 직접 그리는 방법을 보여 주었습니다. 또한 XML 파일을 사용 하 여 그래픽을 선언적으로 만들 수 있도록 하는 몇 가지 다른 기법을 살펴보았습니다. 그런 다음 Android에서 애니메이션을 만들기 위한 이전 및 새 API에 대해 설명 했습니다.



## <a name="related-links"></a>관련 링크

- [애니메이션 데모 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/animationdemo)
- [애니메이션 및 그래픽](https://developer.android.com/guide/topics/graphics/index.html)
- [애니메이션을 사용 하 여 Mobile Apps 개발](http://youtu.be/ikSk_ILg3d0)
- [AnimationDrawable](xref:Android.Graphics.Drawables.AnimationDrawable)
- [캔버스](xref:Android.Graphics.Canvas)
- [개체 애니메이터](xref:Android.Animation.ObjectAnimator)
- [값 애니메이터](xref:Android.Animation.ValueAnimator)
