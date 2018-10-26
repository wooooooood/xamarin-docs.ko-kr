---
title: 회전 처리
description: 이 항목에서는 Xamarin.Android에서 장치 방향 변경 내용을 처리 하는 방법을 설명 합니다. 이 변경 방향을 프로그래밍 방식으로 처리 하는 방법으로 특정 장치 방향에 대 한 리소스를 자동으로 로드 하려면 Android 리소스 시스템을 사용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 6D33ADF7-ED81-0256-479D-D9E3787A76B0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: b2361c04ae627dd68d98f9a6bca1238f1694aaa1
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50118788"
---
# <a name="handling-rotation"></a>회전 처리

_이 항목에서는 Xamarin.Android에서 장치 방향 변경 내용을 처리 하는 방법을 설명 합니다. 이 변경 방향을 프로그래밍 방식으로 처리 하는 방법으로 특정 장치 방향에 대 한 리소스를 자동으로 로드 하려면 Android 리소스 시스템을 사용 하는 방법에 설명 합니다._


## <a name="overview"></a>개요

모바일 장치를 쉽게 회전 하기 때문에 기본 제공 회전 모바일 Os의 표준 기능입니다. Android는 XML 또는 프로그래밍 방식으로 코드에서 사용자 인터페이스를 선언적으로 만든 있는지 여부를 응용 프로그램의 경우에서 회전을 처리 하기 위한 복잡 한 프레임 워크를 제공 합니다. 회전 된 장치에서 선언적 레이아웃 변경 내용을 자동으로 처리 하는 경우 응용 프로그램은 Android 리소스 시스템과 긴밀 한 통합에서 이용할 수 있습니다. 프로그래밍 방식으로 레이아웃에 대 한 변경 내용은 수동으로 처리 되어야 합니다. 따라서 보다 세부적으로 제어 더 많은 작업이 런타임에 짧아지며 개발자. 응용 프로그램 방향 변경 내용의 수동 컨트롤을 작업 다시 시작을 건너뛰도록 선택할 수도 있습니다.

이 가이드에는 다음 방향 항목 검사:

-   **선언적 레이아웃 회전** &ndash; 레이아웃 및 특정 방향에 대 한 드로어 블을 로드 하는 방법을 비롯 한 방향 인식 응용 프로그램을 빌드하는 Android 리소스 시스템을 사용 하는 방법입니다.

-   **프로그래밍 방식으로 레이아웃 회전** &ndash; 방향 변경 내용을 수동으로 처리 하는 방법 뿐만 아니라 컨트롤을 프로그래밍 방식으로 추가 하는 방법입니다.


## <a name="handling-rotation-declaratively-with-layouts"></a>레이아웃을 사용 하 여 선언적으로 회전 처리

파일 명명 규칙을 따르는 폴더에 넣어 Android 자동으로 파일을 로드 적절 한 방향을 변경 하는 경우.
에 대 한 지원이 포함 됩니다.

-   *레이아웃 리소스* &ndash; 각 방향에 대해 팽창 된 레이아웃 파일을 지정 합니다.

-   *드로어 블 리소스* &ndash; 각 방향에 대해 로드 되는 드로어 블을 지정 합니다.


### <a name="layout-resources"></a>레이아웃 리소스

기본적으로 Android XML (AXML) 파일에 포함 된 **리소스/레이아웃** 폴더 활동에 대 한 뷰 렌더링에 사용 됩니다. 이 폴더의이 리소스는 환경에 대 한 추가적인 레이아웃 리소스가 제공 되는 경우 가로 및 세로 방향에 사용 됩니다. 기본 프로젝트 템플릿에서 만든 프로젝트 구조를 고려 합니다.

[![기본 프로젝트 템플릿 구조](handling-rotation-images/00.png)](handling-rotation-images/00.png#lightbox)

이 프로젝트 하나를 만듭니다 **Main.axml** 파일을 **리소스/레이아웃** 폴더입니다. 때 활동의 `OnCreate` 메서드가 호출 되 면에서 정의 된 뷰를 확장 **Main.axml,** 선언 하는 단추 아래 XML에 표시 된 대로:

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

가로 방향으로, 작업의 장치를 회전 하는 경우 `OnCreate` 메서드를 다시 호출 및 동일한 **Main.axml** 아래 스크린샷에 표시 된 것 처럼 파일 팽창 됩니다.

[![동일 하지만 화면 가로 방향](handling-rotation-images/01-sml.png)](handling-rotation-images/01.png#lightbox)


#### <a name="orientation-specific-layouts"></a>레이아웃 방향 별

레이아웃 폴더 외에도 (세로 기본값으로 하며 명시적으로 명명 수도 있습니다 *레이아웃 포트* 라는 폴더를 포함 하 여 `layout-land`), 응용 프로그램 코드 없이 환경에서 작업 하는 경우 필요한 뷰를 정의할 수 있습니다 변경 내용입니다.

가정 합니다 **Main.axml** 파일 같은 XML이 포함 합니다.

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

폴더 이름이 추가 포함 하는 레이아웃 랜드 **Main.axml** 파일을 프로젝트에 추가 됩니다 새로 추가 된 로드 하는 Android에서 이제 하면 환경에서 작업 하는 경우 레이아웃 값 **Main.axml 합니다.** 가로 버전의 **Main.axml** 다음 코드를 포함 하는 파일 (간단히 하기 위해이 XML은 코드의 기본 세로 버전와 유사 하지만 다른 문자열을 사용 하 여는 `TextView`):

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

이 코드를 실행 하 고 세로에서 가로로 장치 회전 아래와 같이 새 XML 로드를 보여 줍니다.

[![세로 모드로 인쇄 하는 세로 및 가로 스크린 샷](handling-rotation-images/02.png)](handling-rotation-images/02.png#lightbox)


### <a name="drawable-resources"></a>드로어 블 리소스

회전 하는 동안 Android 처리 드로어 블 리소스 마찬가지로 레이아웃 리소스 합니다. 이 경우 시스템에서 드로어 블을 가져옵니다 합니다 **리소스/drawable** 하 고 **리소스/drawable land** 폴더 각각.

예를 들어, 프로젝트에서 Monkey.png 라는 이미지를 포함 합니다 **리소스/drawable** 폴더에 위치를 그릴 수 있는에서 참조 되는 `ImageView` 다음과 같은 xml:

```xml
<ImageView
  android:layout_height="wrap_content"
  android:layout_width="wrap_content"
  android:src="@drawable/monkey"
  android:layout_centerVertical="true"
  android:layout_centerHorizontal="true" />
```

추가로 가정 하는 다른 버전의 **Monkey.png** 아래에 포함 됩니다 **리소스/drawable land**합니다. 레이아웃 파일의 장치가 있을 때 처럼 회전, 지정 된 방향에 대 한 드로어 블 변경을 아래와 같이:

[![다른 버전의 가로 및 세로 모드에 표시 된 Monkey.png](handling-rotation-images/03.png)](handling-rotation-images/03.png#lightbox)


## <a name="handling-rotation-programmatically"></a>회전을 프로그래밍 방식으로 처리

경우에 따라 코드에서 레이아웃 정의합니다. 이 다양 한 기술적 제한 사항을, 개발자 환경 설정 등의 이유로 발생할 수 있습니다. 컨트롤에 프로그래밍 방식으로 추가 하는 경우 응용 프로그램 XML 리소스를 사용할 때 자동으로 처리 하는 장치 방향에 대해 수동으로 고려해 야 합니다.


### <a name="adding-controls-in-code"></a>코드에서 컨트롤 추가

컨트롤에 프로그래밍 방식으로 추가 하려면 응용 프로그램에서는 다음 단계를 수행 해야 합니다.

-  레이아웃을 만듭니다.
-  레이아웃 매개 변수를 설정 합니다.
-  컨트롤을 만듭니다.
-  컨트롤 레이아웃 매개 변수를 설정 합니다.
-  레이아웃 컨트롤을 추가 합니다.
-  콘텐츠 보기 레이아웃을 설정 합니다.

예를 들어, 단일 구성 된 사용자 인터페이스는 것이 좋습니다 `TextView` 에 추가 된 컨트롤을 `RelativeLayout`다음 코드에 표시 된 것 처럼 합니다.

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

이 코드의 인스턴스를 만듭니다는 `RelativeLayout` 클래스 집합과 해당 `LayoutParameters` 속성입니다. `LayoutParams` 클래스는 다시 사용할 수 있는 방식으로 컨트롤 배치 되는 방식을 캡슐화 하는 Android의 방법입니다. 레이아웃의 인스턴스를 만든 후 컨트롤을 생성 하 고 추가할 수 있습니다. 컨트롤 `LayoutParameters`와 같은 `TextView` 이 예제입니다. 후는 `TextView` 만들어지면 추가 하는 `RelativeLayout` 설정 합니다 `RelativeLayout` 콘텐츠 보기 결과를 표시 하는 응용 프로그램으로는 `TextView` 표시 된 것 처럼:

[![세로 및 가로 모드로 표시 하는 증분 카운터 단추](handling-rotation-images/04.png)](handling-rotation-images/04.png#lightbox)


### <a name="detecting-orientation-in-code"></a>코드에서 검색 방향

응용 프로그램을 각 방향에 대 한 다른 사용자 인터페이스를 로드 하려고 하는 경우 때 `OnCreate` 호출 됩니다 (이 일이 발생 될 때마다 장치 회전) 방향에 감지 하 고 원하는 사용자 인터페이스 코드를 다음 로드 합니다. Android 라는 클래스에는 `WindowManager`, 확인을 통해 현재 장치 회전에 사용할 수 있는 `WindowManager.DefaultDisplay.Rotation` 속성을 아래와 같이:

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

설정 하는이 코드는 `TextView` 다음과 같이 가로로 회전 하는 경우 자동으로 새 레이아웃에 애니메이션, 화면의 왼쪽 상단에서의 픽셀 위치 지정된 100 되도록 합니다.

[![가로 및 세로 모드에서 뷰 상태 유지](handling-rotation-images/05.png)](handling-rotation-images/05.png#lightbox)


### <a name="preventing-activity-restart"></a>작업 다시 시작 방지

모든 항목을 처리 하는 것 외에도 `OnCreate`, 응용 프로그램에서 설정 하 여 방향을 변경 하는 경우 다시 시작 작업을 방해할 수 있습니다 `ConfigurationChanges` 에 `ActivityAttribute` 다음과 같습니다:

```csharp
[Activity (Label = "CodeLayoutActivity", ConfigurationChanges=Android.Content.PM.ConfigChanges.Orientation | Android.Content.PM.ConfigChanges.ScreenSize)]
```
이제 장치를 회전할 때 작업 다시 시작 되지 않습니다. 수동으로 처리 하기 위해 방향 변경이 경우 작업을 재정의할 수는 `OnConfigurationChanged` 메서드에서 방향을 결정 하 고는 `Configuration` 예: 아래 활동의 새 구현에서는 전달 되는 개체:

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

여기서는 `TextView's` 가로 및 세로 레이아웃 매개 변수가 초기화 됩니다. 클래스 변수와 함께 매개 변수를 보유할는 `TextView` 자체 활동 다시 만들어지지 것입니다 방향이 변경 되 면 때문입니다. 코드를 사용 하 여 계속 합니다 `surfaceOrientartion` 에 `OnCreate` 에 대 한 초기 레이아웃을 설정 하는 `TextView`합니다. 그 후 `OnConfigurationChanged` 모든 후속 레이아웃 변경 내용을 처리 합니다.

응용 프로그램을 실행 하는 경우 Android 장치 회전 발생 하 고 작업을 다시 시작 되지 않으면 변경 하는 사용자 인터페이스를 로드 합니다.


## <a name="preventing-activity-restart-for-declarative-layouts"></a>선언적 레이아웃에 대 한 작업 다시 시작 방지

작업 다시 시작 장치 회전과 인해 xml에서 레이아웃 정의 하는 경우에 방지할 수 있습니다. 예를 들어, 작업 다시 시작을 방지 하려는 경우이 방법을 사용할 수 있습니다 (성능상의 이유로 아마도) 다른 방향에 대 한 새 리소스를 로드할 필요가 없습니다.

이 위해 프로그래밍 방식으로 레이아웃으로 사용 하는 동일한 절차를 따릅니다. 설정 하기만 하면 됩니다 `ConfigurationChanges` 에 `ActivityAttribute`에서 수행한 것 처럼는 `CodeLayoutActivity` 이전 합니다. 방향 변경에서 구현할 수 있습니다 다시 실행 해야 하는 모든 코드는 `OnConfigurationChanged` 메서드.


## <a name="maintaining-state-during-orientation-changes"></a>방향 변경 하는 동안 상태를 유지합니다.

선언적으로 또는 프로그래밍 방식으로 회전 처리 여부를 모든 Android 응용 프로그램에는 장치 방향 변경 될 때 상태를 관리 하기 위한 동일한 기술을 구현 해야 합니다. Android 장치를 회전할 때 시스템이 실행 중인 작업을 다시 시작 때문에 상태를 관리 하는 것이 중요 합니다. Android는 레이아웃 등 특정 방향에 맞게 설계 된 드로어 블 대체 리소스를 로드할 수 있도록 하려면이 작업을 수행 합니다. 다시 시작 될 때 로컬 클래스 변수에 저장 한 있습니다 일시적인 상태 활동에 손실 됩니다. 따라서 의존 하는 상태 활동을 사용 하는 경우 응용 프로그램 수준 상태를 유지 해야 것입니다. 응용 프로그램 처리 해야 하는 경우 저장 및 방향 변경 내용 전체를 유지 하고자 하는 응용 프로그램 상태를 복원 합니다.

Android에서 지속 상태에 대 한 자세한 내용은 참조는 [작업 수명 주기](~/android/app-fundamentals/activity-lifecycle/index.md) 가이드입니다.


## <a name="summary"></a>요약

이 문서에서는 Android의 기본 제공 기능을 사용 하 여 회전을 사용 하는 방법을 설명 합니다. 먼저 Android 리소스 시스템을 사용 하 여 방향 인식 응용 프로그램을 만드는 방법을 설명 했습니다. 다음 화면 방향이 변경을 수동으로 처리 하는 방법 뿐만 아니라 코드에서 컨트롤을 추가 하는 방법을 제공 합니다.



## <a name="related-links"></a>관련 링크

- [회전 데모 (샘플)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/RotationDemo/)
- [작업 수명 주기](~/android/app-fundamentals/activity-lifecycle/index.md)
- [런타임 변경 내용 처리](http://developer.android.com/guide/topics/resources/runtime-changes.html)
- [빠른 화면 방향 변경](http://android-developers.blogspot.com/2009/02/faster-screen-orientation-change.html)
