---
title: Hello, 마모
description: 첫 번째 Android Wear 앱을 만들고 Wear 에뮬레이터 또는 장치에서 실행 합니다. 이 연습에서는 단추 클릭을 처리 하 고 Wear 장치에서 클릭 카운터를 표시 하는 작은 Android Wear 프로젝트를 만들기 위한 단계별 지침을 제공 합니다. Wear 에뮬레이터 또는 Android 휴대폰에 Bluetooth를 통해 연결 된 Wear 장치를 사용 하 여 앱을 디버그 하는 방법을 설명 합니다. 또한 Android Wear 용 디버깅 팁의 집합을 제공합니다.
ms.prod: xamarin
ms.assetid: 86BCD0E7-E9DC-40F1-9B44-887BC51BB48D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/10/2018
ms.openlocfilehash: a8e27063040ff91f72a1cbf932b1b277a5dee63d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104923"
---
# <a name="hello-wear"></a>Hello, 마모

_첫 번째 Android Wear 앱을 만들고 Wear 에뮬레이터 또는 장치에서 실행 합니다. 이 연습에서는 단추 클릭을 처리 하 고 Wear 장치에서 클릭 카운터를 표시 하는 작은 Android Wear 프로젝트를 만들기 위한 단계별 지침을 제공 합니다. Wear 에뮬레이터 또는 Android 휴대폰에 Bluetooth를 통해 연결 된 Wear 장치를 사용 하 여 앱을 디버그 하는 방법을 설명 합니다. 또한 Android Wear 용 디버깅 팁의 집합을 제공합니다._

![이 자습서에 완료할 수 있도록 Wear 앱의 스크린샷](hello-wear-images/example.png)

## <a name="your-first-wear-app"></a>첫 번째 Wear 앱

첫 번째 Xamarin.Android Wear 앱을 만드는이 단계를 수행 합니다.

### <a name="1-create-a-new-android-project"></a>1. 새 Android 프로젝트 만들기

새 **Android Wear 응용 프로그램**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![새 프로젝트 대화 상자에서 새 Android Wear 응용 프로그램 만들기](hello-wear-images/vs/new-solution-sml.w157.png)](hello-wear-images/vs/new-solution.w157.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![새 솔루션 대화 상자에서 새 Android Wear 응용 프로그램 만들기](hello-wear-images/xs/new-solution-sml.png)](hello-wear-images/xs/new-solution.png#lightbox)

-----


이 템플릿은 자동으로 포함 합니다 **Xamarin Android 착용 식 라이브러리** NuGet (및 종속성) Wear 별 위젯에 액세스할 수 있도록 합니다. Wear 템플릿이 보이지 않으면 검토 합니다 [설치 및 설정](~/android/wear/get-started/installation.md) 지원 되는 Android SDK를 설치 했는지 다시 확인 하는 가이드입니다. 

### <a name="2-choose-the-correct-target-framework"></a>2. 올바른 선택 **대상 프레임 워크**

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

했는지 **최소 Android 대상** 로 설정 된 **Android 5.0 (Lollipop)** 이상: 

[![Visual Studio에서 Android 5.0에 대상 프레임 워크를 설정합니다.](hello-wear-images/vs/target-framework-sml.png)](hello-wear-images/vs/target-framework.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

대상 프레임 워크 설정 되어 있는지 확인 **Android 5.0 (Lollipop)** 이상:

[![Mac 용 Visual Studio에서 Android 5.0에 대상 프레임 워크 설정](hello-wear-images/xs/target-framework-sml.png)](hello-wear-images/xs/target-framework.png#lightbox)

-----

대상 프레임 워크를 설정 하는 방법은 참조 하세요 [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)합니다.


### <a name="3-edit-the-mainaxml-layout"></a>3. 편집 된 **Main.axml** 레이아웃

포함할 레이아웃을 구성 된 `TextView` 및 `Button` 샘플: 

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

### <a name="4-edit-the-mainactivitycs-source"></a>4. 편집 된 **MainActivity.cs** 원본

카운터를 증가 하 고 단추를 클릭할 때마다이 표시 하는 코드를 추가 합니다. 

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

### <a name="5-setup-an-emulator-or-device"></a>5. 에뮬레이터 또는 장치를 설정 합니다.

다음 단계는 에뮬레이터 또는 장치를 배포 하 여 앱을 실행 설정 됩니다. 배포 및 실행 프로세스에 익숙해 Xamarin.Android 앱에 일반적으로 볼 수 아직 없는 경우는 [Hello, Android 빠른 시작](~/android/get-started/hello-android/hello-android-quickstart.md)합니다.

Android Wear 장치를 Android Wear Smartwatch는 같은 없는 경우에 에뮬레이터에서 앱을 실행할 수 있습니다. 에뮬레이터에서 Wear 앱을 디버깅 하는 방법에 대 한 내용은 [에뮬레이터에서 Android Wear 디버그](~/android/wear/deploy-test/debug-on-emulator.md)합니다.

Android Wear Smartwatch 같은 Android Wear 장치를 사용 하는 경우에 에뮬레이터를 사용 하는 대신 장치에서 앱을 실행할 수 있습니다. Wear 장치에서 디버깅 하는 방법에 대 한 자세한 내용은 참조 하세요. [Wear 장치에서 디버그](~/android/wear/deploy-test/debug-on-device.md)합니다.


### <a name="6-run-the-android-wear-app"></a>6. Android Wear 앱 실행

Android Wear 장치는 장치 풀 다운 메뉴에 표시 됩니다. 디버깅을 시작 하기 전에 올바른 Android Wear 장치 또는 AVD를 선택 해야 합니다. 장치를 선택 하면 에뮬레이터 또는 장치에 앱을 배포 하려면 Play 단추를 클릭 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Wear AVD를 Visual Studio 장치 메뉴에서 선택](hello-wear-images/vs/choose-wear-sim.png)](hello-wear-images/vs/choose-wear-sim.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![Mac 장치 메뉴에 대 한 Visual Studio에서 Wear AVD를 선택](hello-wear-images/xs/choose-wear-sim.png)](hello-wear-images/xs/choose-wear-sim.png#lightbox)

-----

표시 될 수 있습니다는 **잠시...**  처음 메시지 (또는 일부 다른 중간 화면): 

![에뮬레이터에만 표시 됩니다. 보기...](hello-wear-images/please-wait.png)

조사식 에뮬레이터를 사용 하는 경우 앱을 시작 하는 데 걸릴 수 있습니다. Bluetooth를 사용할 때와 USB를 통해 앱을 배포 하는 데 시간이 더 걸립니다. (예를 들어, 약 5 분이 걸립니다 Nexus 5 전화기로 Bluetooth 연결 되는 LG G 조사식에이 앱을 배포 합니다.)

앱을 성공적으로 배포 Wear 장치 화면에는 다음과 같은 화면이 표시 되어야 합니다.

[![Wear 앱의 초기 화면](hello-wear-images/mainactivity-screen.png)](hello-wear-images/mainactivity-screen.png#lightbox)

탭의 **CLICK ME!** Wear 장치 및 각 탭을 사용 하 여 수 증가 참조 되는 단추:

[![3 번의 클릭 한 후 앱 Wear 스크린 샷](hello-wear-images/mainactivity-counts.png)](hello-wear-images/mainactivity-counts.png#lightbox)


## <a name="next-steps"></a>다음 단계

체크 아웃 합니다 [샘플 Wear](https://developer.xamarin.com/samples/android/Android%20Wear/) 도우미 전화 앱을 사용 하 여 Android Wear 앱을 포함 하 여 합니다.

앱을 배포할 준비가 되었을 때 참조 [패키징 작업](~/android/wear/deploy-test/packaging.md)합니다.


## <a name="related-links"></a>관련 링크

- [내 응용 프로그램 (샘플)를 클릭 합니다.](https://developer.xamarin.com/samples/monodroid/wear/WearTest/)
