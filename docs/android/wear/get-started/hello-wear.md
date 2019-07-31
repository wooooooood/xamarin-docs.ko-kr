---
title: Hello, Wear
description: 첫 번째 Android 앱을 만들고, 마모 된 에뮬레이터 또는 장치에서 실행 합니다. 이 연습에서는 단추 클릭을 처리 하 고 마모 된 장치에서 클릭 카운터를 표시 하는 작은 Android 마모 프로젝트를 만드는 방법에 대 한 단계별 지침을 제공 합니다. Android 휴대폰에 Bluetooth를 통해 연결 되는 마모 된 장치나 장치를 사용 하 여 앱을 디버그 하는 방법을 설명 합니다. 또한 Android 마모를 위한 디버깅 팁 집합을 제공 합니다.
ms.prod: xamarin
ms.assetid: 86BCD0E7-E9DC-40F1-9B44-887BC51BB48D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/10/2018
ms.openlocfilehash: 056ab7a9fe4bcb7f07a9a7cd7c841a3d9f7574b6
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68648021"
---
# <a name="hello-wear"></a>Hello, Wear

_첫 번째 Android 앱을 만들고, 마모 된 에뮬레이터 또는 장치에서 실행 합니다. 이 연습에서는 단추 클릭을 처리 하 고 마모 된 장치에서 클릭 카운터를 표시 하는 작은 Android 마모 프로젝트를 만드는 방법에 대 한 단계별 지침을 제공 합니다. Android 휴대폰에 Bluetooth를 통해 연결 되는 마모 된 장치나 장치를 사용 하 여 앱을 디버그 하는 방법을 설명 합니다. 또한 Android 마모를 위한 디버깅 팁 집합을 제공 합니다._

![이 자습서에서 완료할 마모 된 앱의 스크린샷](hello-wear-images/example.png)

## <a name="your-first-wear-app"></a>첫 번째 마모 앱

첫 번째 Xamarin Android 앱을 만들려면 다음 단계를 따르세요.

### <a name="1-create-a-new-android-project"></a>1. 새 Android 프로젝트 만들기

새 **Android 마모 응용 프로그램**을 만듭니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![새 프로젝트 대화 상자에서 새 Android 마모 응용 프로그램 만들기](hello-wear-images/vs/new-solution-sml.w157.png)](hello-wear-images/vs/new-solution.w157.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![새 솔루션 대화 상자에서 새 Android 마모 응용 프로그램 만들기](hello-wear-images/xs/new-solution-sml.png)](hello-wear-images/xs/new-solution.png#lightbox)

-----


이 템플릿에는 **Xamarin Android Wearable Library** NuGet (및 종속성)이 자동으로 포함 되므로, 사용자는 마모 된 위젯에 액세스할 수 있습니다. 마모 된 템플릿이 표시 되지 않으면 [설치 및 설정](~/android/wear/get-started/installation.md) 가이드를 검토 하 여 지원 되는 Android SDK를 설치 했는지 확인 합니다. 

### <a name="2-choose-the-correct-target-framework"></a>2. 올바른 **대상 프레임 워크** 선택

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

**최소 android 대상** 이 **Android 5.0 (롤리팝)** 이상으로 설정 되어 있는지 확인 합니다. 

[![Visual Studio에서 대상 프레임 워크를 Android 5.0로 설정](hello-wear-images/vs/target-framework-sml.png)](hello-wear-images/vs/target-framework.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

대상 프레임 워크가 **Android 5.0 (롤리팝)** 이상으로 설정 되어 있는지 확인 합니다.

[![Mac용 Visual Studio에서 대상 프레임 워크를 Android 5.0로 설정](hello-wear-images/xs/target-framework-sml.png)](hello-wear-images/xs/target-framework.png#lightbox)

-----

대상 프레임 워크를 설정 하는 방법에 대 한 자세한 내용은 [ANDROID API 수준 이해](~/android/app-fundamentals/android-api-levels.md)를 참조 하세요.


### <a name="3-edit-the-mainaxml-layout"></a>3. **주. axml** 레이아웃 편집

샘플 `TextView` `Button` 에 대 한 및를 포함 하도록 레이아웃을 구성 합니다. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:layout_width="match_parent"
android:layout_height="match_parent">
  <ScrollView
    android:id="@+id/scroll"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:background="#000000"
    android:fillViewport="true">
    <LinearLayout
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:orientation="vertical">
      <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="2dp"
        android:text="Main Activity"
        android:textSize="36sp"
        android:textColor="#006600" />
      <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="2dp"
        android:textColor="#cccccc"
        android:id="@+id/result" />
      <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:onClick="showNotification"
        android:text="Click Me!"
        android:id="@+id/click_button" />
    </LinearLayout>
  </ScrollView>
</FrameLayout>
```

### <a name="4-edit-the-mainactivitycs-source"></a>4. **MainActivity.cs** 원본 편집

카운터를 증가 시키고 단추를 클릭할 때마다 표시 하는 코드를 추가 합니다. 

```csharp
[Activity (Label = "WearTest", MainLauncher = true, Icon = "@drawable/icon")]
public class MainActivity : Activity
{
  int count = 1;

  protected override void OnCreate (Bundle bundle)
  {
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);

    Button button = FindViewById<Button> (Resource.Id.click_button);
    TextView text = FindViewById<TextView> (Resource.Id.result);

    button.Click += delegate {
      text.Text = string.Format ("{0} clicks!", count++);
    };
  }
}
```

### <a name="5-setup-an-emulator-or-device"></a>5. 에뮬레이터 또는 장치 설정

다음 단계는 앱을 배포 하 고 실행 하기 위한 에뮬레이터 또는 장치를 설정 하는 것입니다. 일반적으로 Xamarin Android 앱을 배포 하 고 실행 하는 프로세스에 익숙하지 않은 경우 [Hello, Android 빠른](~/android/get-started/hello-android/hello-android-quickstart.md)시작을 참조 하세요.

Android 마모 된 Smartwatch 같은 Android 장치를 사용할 수 없는 경우 에뮬레이터에서 앱을 실행할 수 있습니다. 에뮬레이터에서 마모 된 앱을 디버깅 하는 방법에 대 한 자세한 내용은 [에뮬레이터에서 Android 마모 디버그](~/android/wear/deploy-test/debug-on-emulator.md)를 참조 하세요.

Android 마모 Smartwatch 같은 Android 장치를 사용 하는 경우 에뮬레이터를 사용 하는 대신 장치에서 앱을 실행할 수 있습니다. 마모 된 장치에서 디버깅 하는 방법에 대 한 자세한 내용은 [마모 된 장치에서 디버그](~/android/wear/deploy-test/debug-on-device.md)를 참조 하세요.


### <a name="6-run-the-android-wear-app"></a>6. Android 앱 실행

Android 마모 장치가 장치 풀 다운 메뉴에 표시 됩니다. 디버깅을 시작 하기 전에 올바른 Android 마모 장치 또는 AVD를 선택 해야 합니다. 장치를 선택한 후 재생 단추를 클릭 하 여 에뮬레이터 또는 장치에 앱을 배포 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Visual Studio 장치 메뉴에서 마모 된 AVD 선택](hello-wear-images/vs/choose-wear-sim.png)](hello-wear-images/vs/choose-wear-sim.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![Mac용 Visual Studio 장치 메뉴에서 마모 된 AVD 선택](hello-wear-images/xs/choose-wear-sim.png)](hello-wear-images/xs/choose-wear-sim.png#lightbox)

-----

처음에는 **1 분 ...** 메시지 (또는 다른 중간 화면)가 표시 될 수 있습니다. 

![보기 에뮬레이터에는 1 분만 표시 됩니다.](hello-wear-images/please-wait.png)

Watch 에뮬레이터를 사용 하는 경우 앱을 시작 하는 데 시간이 걸릴 수 있습니다. Bluetooth를 사용 하는 경우 USB를 사용 하는 것 보다 앱을 배포 하는 데 더 많은 시간이 걸립니다. 예를 들어이 앱을 LG G Watch에 배포 하는 데 약 5 분이 걸립니다 .이는 Nexus 5 phone에 연결 된 것입니다.

앱이 성공적으로 배포 되 면 마모 된 장치의 화면에 다음과 같은 화면이 표시 됩니다.

[![마모 된 앱의 초기 화면](hello-wear-images/mainactivity-screen.png)](hello-wear-images/mainactivity-screen.png#lightbox)

클릭 하세요 **.** 단추를 클릭 하 고 각 탭에서 카운트 증분을 확인 합니다.

[![3 번 클릭 후 착용 앱의 스크린샷](hello-wear-images/mainactivity-counts.png)](hello-wear-images/mainactivity-counts.png#lightbox)


## <a name="next-steps"></a>다음 단계

Android 용 앱을 포함 한 [마모 된 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Android+wear) 을 확인 하세요.

앱을 배포할 준비가 되 면 [패키징을 사용한 작업](~/android/wear/deploy-test/packaging.md)을 참조 하세요.


## <a name="related-links"></a>관련 링크

- [Me 앱 (샘플)을 클릭 합니다.](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-weartest)
