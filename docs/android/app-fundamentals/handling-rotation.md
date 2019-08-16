---
title: 회전 처리
description: 이 항목에서는 Xamarin Android에서 장치 방향 변경을 처리 하는 방법을 설명 합니다. Android 리소스 시스템을 사용 하 여 특정 장치 방향에 대 한 리소스를 자동으로 로드 하는 방법 뿐만 아니라 방향 변경을 프로그래밍 방식으로 처리 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 6D33ADF7-ED81-0256-479D-D9E3787A76B0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 198d667ea52fcad4758c2845e5f2e935d1f74a0b
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69521112"
---
# <a name="handling-rotation"></a>회전 처리

_이 항목에서는 Xamarin Android에서 장치 방향 변경을 처리 하는 방법을 설명 합니다. Android 리소스 시스템을 사용 하 여 특정 장치 방향에 대 한 리소스를 자동으로 로드 하는 방법 뿐만 아니라 방향 변경을 프로그래밍 방식으로 처리 하는 방법을 설명 합니다._


## <a name="overview"></a>개요

모바일 장치는 쉽게 회전 하므로 기본 제공 되는 회전은 모바일 Os의 표준 기능입니다. Android는 사용자 인터페이스가 XML로 선언적으로 만들어지거나 코드에서 프로그래밍 방식으로 생성 되는지에 관계 없이 응용 프로그램 내에서의 회전을 처리 하기 위한 정교한 프레임 워크를 제공 합니다. 회전 된 장치에서 선언적 레이아웃 변경을 자동으로 처리 하는 경우 응용 프로그램은 Android 리소스 시스템과 긴밀 하 게 통합 되는 이점을 누릴 수 있습니다. 프로그래밍 레이아웃의 경우에는 변경 내용을 수동으로 처리 해야 합니다. 이렇게 하면 런타임에 보다 세부적인 제어가 가능 하지만 개발자에 게 더 많은 작업이 수행 됩니다. 응용 프로그램은 활동 다시 시작을 옵트아웃 하 고 방향 변경을 수동으로 제어 하도록 선택할 수도 있습니다.

이 가이드는 다음과 같은 방향 항목을 검사 합니다.

- **선언적 레이아웃 회전** &ndash; Android 리소스 시스템을 사용 하 여 특정 방향에 대 한 레이아웃 및 drawables을 로드 하는 방법을 비롯 하 여 방향 인식 응용 프로그램을 빌드하는 방법입니다.

- **프로그래밍 레이아웃 회전** &ndash; 프로그래밍 방식으로 컨트롤을 추가 하는 방법 뿐만 아니라 방향 변경을 수동으로 처리 하는 방법을 설명 합니다.


## <a name="handling-rotation-declaratively-with-layouts"></a>레이아웃을 사용 하 여 선언적으로 회전 처리

명명 규칙을 따르는 폴더에 파일을 포함 하 여, 해당 방향이 변경 될 때 Android에서 적절 한 파일을 자동으로 로드 합니다.
여기에는에 대 한 지원이 포함 됩니다.

- *리소스 레이아웃* &ndash; 각 방향에 대해 팽창 되는 레이아웃 파일 지정

- *그릴 때 리소스* &ndash; 각 방향에 대해 로드 되는 drawables 지정 하는 것입니다.


### <a name="layout-resources"></a>리소스 레이아웃

기본적으로 **리소스/레이아웃** 폴더에 포함 된 Android XML (axml) 파일은 작업에 대 한 렌더링 보기에 사용 됩니다. 이 폴더의 리소스는 가로 및 세로 방향으로 특별히 제공 되는 추가 레이아웃 리소스가 없는 경우 가로 및 세로 방향으로 모두 사용 됩니다. 기본 프로젝트 템플릿에서 만든 프로젝트 구조를 고려 합니다.

[![기본 프로젝트 템플릿 구조](handling-rotation-images/00.png)](handling-rotation-images/00.png#lightbox)

이 프로젝트는 **리소스/레이아웃** 폴더에 단일 **Main. axml** 파일을 만듭니다. 활동의 `OnCreate` 메서드가 호출 되 면 아래 xml에 표시 된 것 처럼 단추를 선언 하는 늘어납니다에 정의 된 뷰를 **만듭니다** .

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:orientation="vertical"
  android:layout_width="fill_parent"
  android:layout_height="fill_parent">
<Button  
  android:id="@+id/myButton"
  android:layout_width="fill_parent" 
  android:layout_height="wrap_content" 
  android:text="@string/hello"/>
</LinearLayout>
```

장치를 가로 방향으로 회전 하는 경우 아래 스크린샷에 표시 `OnCreate` 된 것 처럼 활동의 메서드가 다시 호출 되 고 동일한 **Main. axml** 파일이 팽창 됩니다.

[![가로 방향으로 동일한 화면](handling-rotation-images/01-sml.png)](handling-rotation-images/01.png#lightbox)


#### <a name="orientation-specific-layouts"></a>방향 별 레이아웃

레이아웃 폴더 외에도 기본적으로 세로를 사용 하 고 이름이 인 `layout-land`폴더를 포함 하 여 *레이아웃-포트* 를 명시적으로 지정할 수 있습니다. 응용 프로그램은 코드를 변경 하지 않고도 가로에서 필요한 뷰를 정의할 수 있습니다.

**Main. axml** 파일에 다음 XML이 포함 되어 있다고 가정 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="fill_parent"
  android:layout_height="fill_parent">
  <TextView
    android:text="This is portrait"
    android:layout_height="wrap_content"
    android:layout_width="fill_parent" />
</RelativeLayout>
```

추가 **Main. axml** 파일이 포함 된 레이아웃 육지 라는 폴더가 프로젝트에 추가 되 면, 않아서는 레이아웃을 가로 방향으로 지정 하면 이제 Android에서 새로 추가 된 **기본. axml** 을 로드 합니다. 다음 코드가 포함 된 **주. axml** 파일의 가로 버전을 고려 합니다. 간단 하 게 하기 위해이 XML은 코드의 기본 세로 버전과 유사 하지만에서 `TextView`다른 문자열을 사용 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="fill_parent"
  android:layout_height="fill_parent">
  <TextView
    android:text="This is landscape"
    android:layout_height="wrap_content"
    android:layout_width="fill_parent" />
</RelativeLayout>
```

이 코드를 실행 하 고 장치를 세로에서 가로 방향으로 회전 하면 다음과 같이 새로운 XML 로드를 보여 줍니다.

[![세로 및 가로 스크린샷 세로 모드 인쇄](handling-rotation-images/02.png)](handling-rotation-images/02.png#lightbox)


### <a name="drawable-resources"></a>그릴 때 리소스

회전 하는 동안 Android는 그릴 수 있는 리소스를 레이아웃 리소스와 유사 하 게 처리 합니다. 이 경우 시스템은 각각 **리소스/그릴** 때 및 **리소스/그릴** 때 발생 하는 폴더에서 drawables을 가져옵니다.

예를 들어, 프로젝트의 **Resources/그릴** 수 있는 폴더에는 원숭이 라는 이미지가 포함 됩니다. 여기에서 그릴 수 있는 이미지는 `ImageView` 다음과 같이 in XML에서 참조 됩니다.

```xml
<ImageView
  android:layout_height="wrap_content"
  android:layout_width="wrap_content"
  android:src="@drawable/monkey"
  android:layout_centerVertical="true"
  android:layout_centerHorizontal="true" />
```

**리소스/그릴**수 있는 다른 버전의 **원숭이 .png** 가 포함 되어 있다고 가정 하겠습니다. 레이아웃 파일을 사용 하는 것과 마찬가지로 장치가 회전 될 때 아래와 같이 그릴 때 지정 된 방향으로 변경 됩니다.

[![세로 및 가로 모드에 표시 되는 다른 버전의 원숭이. png](handling-rotation-images/03.png)](handling-rotation-images/03.png#lightbox)


## <a name="handling-rotation-programmatically"></a>프로그래밍 방식으로 회전 처리

코드에 레이아웃을 정의 하는 경우도 있습니다. 이는 기술적 제한, 개발자 기본 설정 등 다양 한 이유로 발생할 수 있습니다. 프로그래밍 방식으로 컨트롤을 추가 하는 경우 응용 프로그램은 XML 리소스를 사용할 때 자동으로 처리 되는 장치 방향에 대해 수동으로 계정을 만들어야 합니다.


### <a name="adding-controls-in-code"></a>코드에 컨트롤 추가

응용 프로그램에서 프로그래밍 방식으로 컨트롤을 추가 하려면 다음 단계를 수행 해야 합니다.

- 레이아웃을 만듭니다.
- 레이아웃 매개 변수를 설정 합니다.
- 컨트롤을 만듭니다.
- 컨트롤 레이아웃 매개 변수를 설정 합니다.
- 레이아웃에 컨트롤을 추가 합니다.
- 레이아웃을 콘텐츠 뷰로 설정 합니다.

예를 들어, 다음 코드와 같이에 추가 `TextView` `RelativeLayout`된 단일 컨트롤로 구성 된 사용자 인터페이스를 살펴보겠습니다.

```csharp
protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);
                        
  // create a layout
  var rl = new RelativeLayout (this);

  // set layout parameters
  var layoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.FillParent);
  rl.LayoutParameters = layoutParams;
        
  // create TextView control
  var tv = new TextView (this);

  // set TextView's LayoutParameters
  tv.LayoutParameters = layoutParams;
  tv.Text = "Programmatic layout";

  // add TextView to the layout
  rl.AddView (tv);
        
  // set the layout as the content view
  SetContentView (rl);
}
```

이 코드는 `RelativeLayout` 클래스의 인스턴스를 만들고 해당 `LayoutParameters` 속성을 설정 합니다. 클래스 `LayoutParams` 는 다시 사용 가능한 방식으로 컨트롤의 위치를 지정 하는 방법을 캡슐화 하는 Android의 방법입니다. 레이아웃의 인스턴스를 만든 후에는 컨트롤을 만들어 추가할 수 있습니다. 컨트롤에는 `LayoutParameters` `TextView` 이 예제의와 같은도 있습니다. 가 만들어지면를 `RelativeLayout` 에 추가 하 고를 `TextView` 콘텐츠 뷰로 설정 `RelativeLayout` 하면 다음과 같이 응용 프로그램이 표시 됩니다. `TextView`

[![세로 및 가로 모드에서 표시 되는 증가 카운터 단추](handling-rotation-images/04.png)](handling-rotation-images/04.png#lightbox)


### <a name="detecting-orientation-in-code"></a>코드의 방향 감지

가 호출 될 `OnCreate` 때마다 응용 프로그램이 각 방향에 대해 다른 사용자 인터페이스를 로드 하려고 시도 하는 경우 (장치가 회전 될 때마다 발생 함) 방향을 검색 한 다음 원하는 사용자 인터페이스 코드를 로드 해야 합니다. Android에는 `WindowManager`아래와 같이 `WindowManager.DefaultDisplay.Rotation` 속성을 통해 현재 장치 회전을 결정 하는 데 사용할 수 있는 라는 클래스가 있습니다.

```csharp
protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);
                        
  // create a layout
  var rl = new RelativeLayout (this);

  // set layout parameters
  var layoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.FillParent);
  rl.LayoutParameters = layoutParams;
                        
  // get the initial orientation
  var surfaceOrientation = WindowManager.DefaultDisplay.Rotation;
  // create layout based upon orientation
  RelativeLayout.LayoutParams tvLayoutParams;
                
  if (surfaceOrientation == SurfaceOrientation.Rotation0 || surfaceOrientation == SurfaceOrientation.Rotation180) {
    tvLayoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
  } else {
    tvLayoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
    tvLayoutParams.LeftMargin = 100;
    tvLayoutParams.TopMargin = 100;
  }
                        
  // create TextView control
  var tv = new TextView (this);
  tv.LayoutParameters = tvLayoutParams;
  tv.Text = "Programmatic layout";
        
  // add TextView to the layout
  rl.AddView (tv);
        
  // set the layout as the content view
  SetContentView (rl);
}
```

이 코드는 `TextView` 다음과 같이를 화면 왼쪽 위에서 100 픽셀 배치 하도록 설정 하 고, 가로로 회전할 때 새 레이아웃에 자동으로 애니메이션 효과를 줍니다.

[![뷰 상태는 가로 및 세로 모드에서 유지 됩니다.](handling-rotation-images/05.png)](handling-rotation-images/05.png#lightbox)


### <a name="preventing-activity-restart"></a>활동 다시 시작 방지

응용 프로그램은에서 `OnCreate`모든 항목을 처리 하는 것 외에도 다음과 같이를 `ActivityAttribute` 설정 `ConfigurationChanges` 하 여 방향이 변경 될 때 활동이 다시 시작 되지 않도록 할 수 있습니다.

```csharp
[Activity (Label = "CodeLayoutActivity", ConfigurationChanges=Android.Content.PM.ConfigChanges.Orientation | Android.Content.PM.ConfigChanges.ScreenSize)]
```
이제 장치가 회전 될 때 활동이 다시 시작 되지 않습니다. 이 경우 방향 변경을 수동으로 처리 하기 위해 활동은 메서드를 `OnConfigurationChanged` 재정의 하 고 아래 활동의 새 구현에서와 같이 전달 되는 `Configuration` 개체에서 방향을 결정할 수 있습니다.

```csharp
[Activity (Label = "CodeLayoutActivity", ConfigurationChanges=Android.Content.PM.ConfigChanges.Orientation | Android.Content.PM.ConfigChanges.ScreenSize)]
public class CodeLayoutActivity : Activity
{
  TextView _tv;
  RelativeLayout.LayoutParams _layoutParamsPortrait;
  RelativeLayout.LayoutParams _layoutParamsLandscape;
                
  protected override void OnCreate (Bundle bundle)
  {
    // create a layout
    // set layout parameters
    // get the initial orientation

    // create portrait and landscape layout for the TextView
    _layoutParamsPortrait = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
                
    _layoutParamsLandscape = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
    _layoutParamsLandscape.LeftMargin = 100;
    _layoutParamsLandscape.TopMargin = 100;
                        
    _tv = new TextView (this);
                        
    if (surfaceOrientation == SurfaceOrientation.Rotation0 || surfaceOrientation == SurfaceOrientation.Rotation180) {
      _tv.LayoutParameters = _layoutParamsPortrait;
    } else {
      _tv.LayoutParameters = _layoutParamsLandscape;
    }
                        
    _tv.Text = "Programmatic layout";
    rl.AddView (_tv);
    SetContentView (rl);
  }
                
  public override void OnConfigurationChanged (Android.Content.Res.Configuration newConfig)
  {
    base.OnConfigurationChanged (newConfig);
                        
    if (newConfig.Orientation == Android.Content.Res.Orientation.Portrait) {
      _tv.LayoutParameters = _layoutParamsPortrait;
      _tv.Text = "Changed to portrait";
    } else if (newConfig.Orientation == Android.Content.Res.Orientation.Landscape) {
      _tv.LayoutParameters = _layoutParamsLandscape;
      _tv.Text = "Changed to landscape";
    }
  }
}
```

여기서 레이아웃 `TextView's` 매개 변수는 가로 및 세로 모두에 대해 초기화 됩니다. 방향이 변경 될 때 활동이 다시 생성 되지 않으므로 `TextView` 클래스 변수에는 자체와 함께 매개 변수가 포함 됩니다. 이 코드에서는 여전히의 `surfaceOrientartion` `OnCreate` 를 사용 하 여의 초기 레이아웃 `TextView`을 설정 합니다. 이후에는 `OnConfigurationChanged` 모든 이후 레이아웃 변경 내용을 처리 합니다.

응용 프로그램을 실행할 때 Android는 장치 회전이 발생 함에 따라 사용자 인터페이스 변경 사항을 로드 하 고 작업을 다시 시작 하지 않습니다.


## <a name="preventing-activity-restart-for-declarative-layouts"></a>선언 레이아웃에 대 한 작업 다시 시작 방지

XML에서 레이아웃을 정의 하는 경우 장치 회전으로 인 한 작업 다시 시작을 방지할 수도 있습니다. 예를 들어, 성능상의 이유로 작업을 다시 시작 하는 것을 방지 하려는 경우이 방법을 사용할 수 있으며, 다른 방향에 대 한 새 리소스를 로드할 필요가 없습니다.

이렇게 하려면 프로그래밍 방식 레이아웃에 사용 하는 것과 동일한 절차를 따릅니다. 이전에서 `ConfigurationChanges` 했던 `ActivityAttribute`것 처럼에서를 설정 하면 됩니다. `CodeLayoutActivity` 방향 변경을 위해 실행 해야 하는 모든 코드는 `OnConfigurationChanged` 메서드에서 다시 구현할 수 있습니다.


## <a name="maintaining-state-during-orientation-changes"></a>방향 변경 중 상태 유지 관리

회전을 선언적으로 또는 프로그래밍 방식으로 처리 하는지에 관계 없이 모든 Android 응용 프로그램은 장치 방향이 변경 될 때 상태를 관리 하기 위해 동일한 기술을 구현 해야 합니다. Android 장치를 회전할 때 시스템이 실행 중인 작업을 다시 시작 하므로 상태 관리가 중요 합니다. Android는 특정 방향에 맞게 특별히 디자인 된 레이아웃 및 drawables과 같은 대체 리소스를 쉽게 로드할 수 있도록 합니다. 작업을 다시 시작 하면 로컬 클래스 변수에 저장 했을 수 있는 모든 일시적 상태가 손실 됩니다. 따라서 활동이 상태 기반 인 경우 응용 프로그램 수준에서 상태를 유지 해야 합니다. 응용 프로그램은 방향 변경에 대해 유지 하려는 응용 프로그램 상태를 저장 하 고 복원 하는 것을 처리 해야 합니다.

Android에서 상태를 유지 하는 방법에 대 한 자세한 내용은 [활동 수명 주기](~/android/app-fundamentals/activity-lifecycle/index.md) 가이드를 참조 하세요.


## <a name="summary"></a>요약

이 문서에서는 Android의 기본 제공 기능을 사용 하 여 회전 작업을 수행 하는 방법을 살펴보았습니다. 먼저, Android 리소스 시스템을 사용 하 여 방향 인식 응용 프로그램을 만드는 방법을 설명 했습니다. 그런 다음 코드에 컨트롤을 추가 하는 방법 뿐만 아니라 방향 변경을 수동으로 처리 하는 방법을 제공 했습니다.



## <a name="related-links"></a>관련 링크

- [회전 데모 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-rotationdemo)
- [작업 수명 주기](~/android/app-fundamentals/activity-lifecycle/index.md)
- [런타임 변경 내용 처리](https://developer.android.com/guide/topics/resources/runtime-changes.html)
- [빠른 화면 방향 변경](http://android-developers.blogspot.com/2009/02/faster-screen-orientation-change.html)
