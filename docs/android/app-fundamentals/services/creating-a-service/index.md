---
title: 서비스 만들기
ms.prod: xamarin
ms.assetid: A78A55E7-FB5C-4C42-8E3E-939B5E98F9EB
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 05/03/2018
ms.openlocfilehash: 00785ad161f5f05fd70b059bb0a3f1c8d6c31f97
ms.sourcegitcommit: daa089d41cfe1ed0456d6de2f8134cf96ae072b1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33850836"
---
# <a name="creating-a-service"></a>서비스 만들기

Xamarin.Android 서비스 Android 서비스의 두 가지 불가침의 규칙을 준수 해야 합니다.

* 확장 해야 합니다는 [ `Android.App.Service` ](https://developer.xamarin.com/api/type/Android.App.Service/)합니다.
* 로 데코 레이트 되어야 합니다는 [ `Android.App.ServiceAttribute` ](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/)합니다.

Android 서비스의 또 다른 요구 사항에 등록 해야 하는 **AndroidManifest.xml** 에 고유한 이름을 지정 하 고 있습니다. Xamarin.Android 자동으로 등록 하므로 서비스 매니페스트에 필요한 XML 특성으로 빌드 시.

이 코드 조각은 이러한 두 요구 사항을 충족 하는 Xamarin.Android에 서비스를 만드는 가장 간단한 예입니다.  

```csharp
[Service]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

컴파일 타임에 Xamarin.Android은 등록 서비스에 다음 XML 요소를 삽입 하 여 **AndroidManifest.xml** (Xamarin.Android 서비스에 대 한 임의의 이름을 생성 하 고 이때 됨):

```xml
<service android:name="md5a0cbbf8da641ae5a4c781aaf35e00a86.DemoService" />
```

다른 Android 응용 프로그램에서 서비스를 공유할 수는 _내보내기_ 것입니다. 설정 하 여 이렇게는 `Exported` 속성에는 `ServiceAttribute`합니다. 서비스를 내보낼 때의 `ServiceAttribute.Name` 속성 서비스에 대 한 공용 의미 있는 이름을 제공 하기 위해 설정 해야 합니다. 이 조각은 내보내기는 서비스 이름을 지정 하는 방법을 보여 줍니다.

```csharp
[Service(Exported=true, Name="com.xamarin.example.DemoService")]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

**AndroidManifest.xml** 이 서비스에 대 한 요소는 다음 유사 합니다.

```xml
<service android:exported="true" android:name="com.xamarin.example.DemoService" />
```

서비스는 서비스를 만들 때 호출 되는 콜백 메서드와 자신의 기간이 있습니다. 어떤 메서드가 호출 되는 정확 하 게 서비스의 유형에 따라 달라 집니다. 하이브리드 서비스는 시작된 서비스와 바인딩된 서비스에 대 한 콜백 메서드를 구현 해야 하는 동안 시작된 되는 서비스에서 바인딩된 서비스 이외의 다른 수명 주기 메서드를 구현 해야 합니다. 이러한 메서드는의 모든 멤버는 `Service` 클래스; 서비스를 시작 하는 방법을 어떤 수명 주기 메서드 호출 될 결정 됩니다. 이러한 수명 주기 메서드 나중에 자세히 설명 합니다.

기본적으로 서비스는 Android 응용 프로그램과 동일한 프로세스에서 시작 됩니다. 설정 하 여 자체 프로세스에서 서비스를 시작할 수는 `ServiceAttribute.IsolatedProcess` 속성을 true로:

```csharp
[Service(IsolatedProcess=true)]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things, in it's own process!
}
```

다음 단계는 서비스를 시작한 다음 세 가지 유형의 서비스를 구현 하는 방법을 검사로 이동 하는 방법을 확인 하는 것입니다.

> [!NOTE]
> UI 스레드에서 실행 되는 서비스 하므로 모든 작업이 수행 되도록 UI를 차단 하는 경우 서비스 작업을 수행 하 스레드를 사용 해야 합니다.

## <a name="starting-a-service"></a>서비스 시작

Android에서 서비스를 시작 하려면 가장 기본적인 방법은 디스패치 하는 것는 `Intent` 되는 서비스를 시작 해야 함 확인할 메타 데이터가 들어 있는입니다. 두 가지 다른 스타일의 의도 하는 서비스를 시작 하는 데 사용할 수 있습니다.

-   **명시적 의도** &ndash; 는 _명시적 의도_ 어떤 서비스에 지정된 된 동작을 완료 하려면 사용할 정확 하 게 식별 됩니다. 특정 주소; 문자로 명시적 의도 생각할 수 있습니다. Android 의도를 명시적으로 식별 되는 서비스에 라우팅합니다. 이 조각은 라는 서비스를 시작 하려면 명시적 의도 사용 하 여 한 가지 예는 `DownloadService`:

    ```csharp
    // Example of creating an explicit Intent in an Android Activity
    Intent downloadIntent = new Intent(this, typeof(DownloadService));
    downloadIntent.data = Uri.Parse(fileToDownload);
    ```

-   **암시적 의도** &ndash; 느슨하게이 이와 같은 의도 식별 하는 작업을 수행 하려면 사용자가 있지만 해당 작업을 완료 하려면 서비스를 알 수 없습니다. 다음은 문자는 "To Whom It 년 5 월 문제가..." 배달의 암시적 의도 생각할 수 있습니다.
    Android 의미를 가지는의 내용을 검사 되며 의도 일치 하는 기존 서비스 인지 확인 합니다.

    _의도 필터_ 등록된 서비스와 함께 암시적 의도 충족할 수 있도록 하는 데 사용 됩니다. 의도 한 필터에 추가 하는 XML 요소는 **AndroidManifest.xml** 암시적 의도 가진 서비스를 충족할 수 있도록 하는 데 필요한 메타 데이터가 들어 있는입니다.

    ```csharp
    Intent sendIntent = new Intent("common.xamarin.DemoService");
    sendIntent.Data = Uri.Parse(fileToDownload);
    ```

Android에 암시적 의도 대 한 가능한 일치 항목을 여러 개 있는 경우 사용자가 작업을 처리 하는 구성 요소를 선택할 수을 요청할 수 있습니다.

![명확성 대화 상자의 스크린 샷](images/creating-a-service-01.png "명확성 대화 상자의 스크린 샷")

> [!IMPORTANT]
> Android 5.0 (AP 수준 21) 암시적 의도 서비스를 시작 하려면 사용할 수 없습니다.

가능한 경우, 응용 프로그램 서비스를 시작 하려면 명시적 의도 사용 해야 합니다. 암시적 의도 특정 서비스를 시작 하도록 요청 하지 않으며 &ndash; 중인 요청을 처리 하는 장치에 설치 된 일부 서비스에 대 한 요청입니다. 이 모호한 요청 잘못 된 서비스 요청 또는 다른 응용 프로그램 불필요 하 게 시작 (장치에서 리소스에 대 한 증가)는 처리 될 수 있습니다.

어떻게 의도 서비스의 형식에 따라 달라 집니다 디스패치되고에서 각 유형의 서비스에 특정 가이드의 뒷부분에서 자세히 설명 합니다.


### <a name="creating-an-intent-filter-for-implicit-intents"></a>암시적 의도 대 한 의도 필터 만들기

서비스를 암시적 의도를 연결 하려면 Android 앱 서비스의 기능을 식별 일부 메타 데이터를 제공 해야 합니다. 이 메타 데이터에서 제공 _의도 필터_합니다. 의도 필터 동작 또는 서비스를 시작할 의도에 존재 해야 하는 데이터의 형식 같은 몇 가지 정보를 포함 합니다. Xamarin.Android, 의도 필터에 등록 되어 **AndroidManifest.xml** 사용 하 여 서비스를 데코레이팅하는 [ `IntentFilterAttribute` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)합니다. 다음 코드의 동작과 연결된 된 의도 필터를 추가 하는 예를 들어 `com.xamarin.DemoService`:

```csharp
[Service]
[IntentFilter(new String[]{"com.xamarin.DemoService"})]
public class DemoService : Service
{
}
```

항목에 포함 되 고 그 결과 **AndroidManifest.xml** 파일 &ndash; 다음 예와 유사한 방식으로 응용 프로그램과 함께 패키지 하는 항목:

```xml
<service android:name="demoservice.DemoService">
    <intent-filter>
        <action android:name="com.xamarin.DemoService" />
    </intent-filter>
</service>
```

방해가 Xamarin.Android 서비스의 기본 기능을 검토해 보겠습니다는 자세히 서비스의 서로 다른 하위 형식입니다.


## <a name="related-links"></a>관련 링크

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/)
- [Android.App.Intent](https://developer.xamarin.com/api/type/Android.Content.Intent/)
- [Android.App.IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)
