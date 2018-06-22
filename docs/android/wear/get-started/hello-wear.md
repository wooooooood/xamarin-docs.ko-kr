---
title: Hello, 마모
description: 첫 Android 착용 응용 프로그램을 만들고 마모 에뮬레이터 또는 장치에서 실행 합니다. 이 연습에서는 단추 클릭을 처리 하 고 마모 장치에 클릭 하 여 카운터를 표시 하는 작은 쓰는 유형 Android 프로젝트를 만들기 위한 단계별 지침을 제공 합니다. 마모 에뮬레이터 또는 Android 휴대폰에 Bluetooth를 통해 연결 되어 있는 마모 장치를 사용 하 여 응용 프로그램을 디버깅 하는 방법을 설명 합니다. 또한 Android 착용에 대 한 디버깅 팁의 집합을 제공합니다.
ms.prod: xamarin
ms.assetid: 86BCD0E7-E9DC-40F1-9B44-887BC51BB48D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/10/2018
ms.openlocfilehash: 17c12c4ec818c21d6697932315874ea4f63e6109
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33798416"
---
# <a name="hello-wear"></a>Hello, 마모

_첫 Android 착용 응용 프로그램을 만들고 마모 에뮬레이터 또는 장치에서 실행 합니다. 이 연습에서는 단추 클릭을 처리 하 고 마모 장치에 클릭 하 여 카운터를 표시 하는 작은 쓰는 유형 Android 프로젝트를 만들기 위한 단계별 지침을 제공 합니다. 마모 에뮬레이터 또는 Android 휴대폰에 Bluetooth를 통해 연결 되어 있는 마모 장치를 사용 하 여 응용 프로그램을 디버깅 하는 방법을 설명 합니다. 또한 Android 착용에 대 한 디버깅 팁의 집합을 제공합니다._

![이 자습서의 완료에 마모 응용 프로그램의 스크린 샷](hello-wear-images/example.png)

## <a name="your-first-wear-app"></a>첫 번째 마모 앱

첫 번째 Xamarin.Android 마모 앱을 만들려면 다음이 단계를 수행 합니다.

### <a name="1-create-a-new-android-project"></a>1. 새 Android 프로젝트 만들기

새 **착용 Android 응용 프로그램**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![새 프로젝트 대화 상자에서 새 Android 착용 응용 프로그램 만들기](hello-wear-images/vs/new-solution-sml.w157.png)](hello-wear-images/vs/new-solution.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![새 솔루션 대화 상자에서 새 Android 착용 응용 프로그램 만들기](hello-wear-images/xs/new-solution-sml.png)](hello-wear-images/xs/new-solution.png#lightbox)

-----


이 서식 파일에서 자동으로 포함 됩니다는 **Xamarin Android 착용 식 라이브러리** NuGet (및 종속성) 마모 관련 위젯에 대 한 액세스를 해야 합니다. 마모 템플릿 보이지 않으면 검토는 [설치 및 설정](~/android/wear/get-started/installation.md) 가이드에는 지원 되는 Android SDK를 설치 했는지 확인 합니다. 

### <a name="2-choose-the-correct-target-framework"></a>2. 올바른 선택 **대상 프레임 워크**

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

되도록 **최소 Android 대상** 로 설정 된 **Android 5.0 (롤리팝)** 이상: 

[![Visual Studio에서 Android 5.0에 대상 프레임 워크를 설정합니다.](hello-wear-images/vs/target-framework-sml.png)](hello-wear-images/vs/target-framework.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

대상 프레임 워크로 설정 되어 **Android 5.0 (롤리팝)** 이상:

[![Mac 용 Visual Studio에서 Android 5.0에 대상 프레임 워크를 설정](hello-wear-images/xs/target-framework-sml.png)](hello-wear-images/xs/target-framework.png#lightbox)

-----

대상 프레임 워크 설정에 대 한 자세한 내용은 참조 하십시오. [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)합니다.


### <a name="3-edit-the-mainaxml-layout"></a>3. 편집 된 **Main.axml** 레이아웃

레이아웃을 포함 하도록 구성 된 `TextView` 및 `Button` 샘플: 

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

### <a name="4-edit-the-mainactivitycs-source"></a>4. 편집 된 **MainActivity.cs** 소스

카운터를 증가 하는 단추를 클릭할 때마다 표시 코드를 추가 합니다. 

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

다음 단계는 배포 하 고 응용 프로그램을 실행 에뮬레이터 또는 장치 설정 되어 있습니다. 실행 및 배포 프로세스에 잘 Xamarin.Android 앱 일반적으로 참조 아직 없는 경우는 [Hello, Android 퀵 스타트](~/android/get-started/hello-android/hello-android-quickstart.md)합니다.

Android 착용 Smartwatch 같은 쓰는 유형 Android 장치가 없는 경우 에뮬레이터에서 앱을 실행할 수 있습니다. 에뮬레이터에서 마모 앱을 디버깅 하는 방법에 대 한 정보를 참조 하십시오. [에뮬레이터 Android 착용 디버그](~/android/wear/deploy-test/debug-on-emulator.md)합니다.

Android 착용 Smartwatch 같은 쓰는 유형 Android 장치를 설정한 경우에 에뮬레이터를 사용 하는 대신 장치에 응용 프로그램을 실행할 수 있습니다. 마모 장치에서 디버깅 하는 방법에 대 한 자세한 내용은 참조 [착용 장치에서 디버깅](~/android/wear/deploy-test/debug-on-device.md)합니다.


### <a name="6-run-the-android-wear-app"></a>6. Android 마모 앱 실행

쓰는 유형 Android 장치는 장치 풀 다운 메뉴에 표시 됩니다. 디버깅을 시작 하기 전에 올바른 쓰는 유형 Android 장치 또는 AVD를 선택 해야 합니다. 장치를 선택한 후 에뮬레이터 또는 장치에 앱을 배포 하려면 [재생] 단추를 클릭 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Visual Studio 장치 메뉴에 쓰는 유형 AVD 선택](hello-wear-images/vs/choose-wear-sim.png)](hello-wear-images/vs/choose-wear-sim.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Mac 장치 메뉴에 대 한 Visual Studio에서 착용 AVD 선택](hello-wear-images/xs/choose-wear-sim.png)](hello-wear-images/xs/choose-wear-sim.png#lightbox)

-----

표시 될 수 있습니다는 **분만...**  처음에 메시지 (또는 다른 중간 화면): 

![에뮬레이터에 분만 표시 됩니다. 보기...](hello-wear-images/please-wait.png)

조사식 에뮬레이터에서 앱을 사용 하는 경우 앱을 시작 하려면 시간이 걸릴 수 있습니다. Bluetooth를 사용 하는 경우와 USB를 통해 앱을 배포 하는 데 더 많은 시간이 걸리는 합니다. (예를 들어 5 분 걸립니다 Nexus 5 전화에 Bluetooth 연결 되는 LG G 조사식에이 앱을 배포 하 합니다.)

응용 프로그램을 성공적으로 배포 후 마모 장치의 화면에는 다음과 같은 화면이 표시 되어야 합니다.

[![마모 응용 프로그램의 초기 화면](hello-wear-images/mainactivity-screen.png)](hello-wear-images/mainactivity-screen.png#lightbox)

탭의 **CLICK ME!** 마모 장치와 참조 각 탭이 있는 수 증가 되는 단추:

[![3 번 클릭 한 후 앱 착용 스크린 샷](hello-wear-images/mainactivity-counts.png)](hello-wear-images/mainactivity-counts.png#lightbox)


## <a name="next-steps"></a>다음 단계

체크 아웃는 [샘플 착용](https://developer.xamarin.com/samples/android/Android%20Wear/) 도우미 전화 앱을 사용 하 여 쓰는 유형 Android 앱을 포함 합니다.

참조를 응용 프로그램을 배포할 준비가 되 면 [패키징 작업](~/android/wear/deploy-test/packaging.md)합니다.


## <a name="related-links"></a>관련 링크

- [Me 응용 프로그램 (샘플)를 클릭 합니다.](https://developer.xamarin.com/samples/monodroid/wear/WearTest/)
