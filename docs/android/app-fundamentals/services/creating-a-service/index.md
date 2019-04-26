---
title: 서비스 만들기
ms.prod: xamarin
ms.assetid: A78A55E7-FB5C-4C42-8E3E-939B5E98F9EB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/03/2018
ms.openlocfilehash: 8c2086025ccb5fe41b3ffddc9cd650c1e0c81fbc
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61013509"
---
# <a name="creating-a-service"></a>서비스 만들기

Xamarin.Android 서비스 Android 서비스의 두 가지 불가침의 규칙을 따라야 합니다.

* 확장 해야 합니다 [ `Android.App.Service` ](https://developer.xamarin.com/api/type/Android.App.Service/)합니다.
* 으로 데코레이팅 해야 합니다 [ `Android.App.ServiceAttribute` ](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/)합니다.

Android 서비스의 다른 요구 사항에 등록 해야 하는 합니다 **AndroidManifest.xml** 고유한 이름을 지정 합니다. Xamarin.Android는 자동으로 등록 서비스 매니페스트에서 필요한 XML 특성을 사용 하 여 빌드 시.

이 코드 조각은 다음과 같습니다. 이러한 두 요구 사항을 충족 하는 Xamarin.Android에서 서비스를 만드는 가장 간단한 예  

```csharp
[Service]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

컴파일 타임에 Xamarin.Android는 등록 서비스에 다음 XML 요소를 삽입 하 여 **AndroidManifest.xml** (이때 Xamarin.Android에는 서비스에 대 한 임의 이름을 생성 되도록).

```xml
<service android:name="md5a0cbbf8da641ae5a4c781aaf35e00a86.DemoService" />
```

다른 Android 응용 프로그램을 사용 하 여 서비스를 공유할 수 있기 _내보내기_ 것입니다. 설정 하 여 이렇게 합니다 `Exported` 속성에는 `ServiceAttribute`합니다. 서비스를 내보낼 때의 `ServiceAttribute.Name` 속성 서비스에 대 한 공용 의미 있는 이름을 제공 하기 위해 설정 해야 합니다. 이 코드 조각은 내보내기는 서비스 이름을 지정 하는 방법을 보여 줍니다.

```csharp
[Service(Exported=true, Name="com.xamarin.example.DemoService")]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

합니다 **AndroidManifest.xml** 이 서비스에 대 한 요소는 다음과 같은 모양이 됩니다.

```xml
<service android:exported="true" android:name="com.xamarin.example.DemoService" />
```

서비스는 서비스가 만들어질 때 호출 되는 콜백 메서드를 사용 하 여 자체 수명 주기를 있습니다. 메서드를 호출 하는 정확 하 게 서비스의 유형에 따라 달라 집니다. 시작된 서비스는 하이브리드 서비스를 시작된 하는 서비스 및 바인딩된 서비스 모두에 대 한 콜백 메서드를 구현 해야 하는 동안 바인딩된 서비스를 보다 다양 한 수명 주기 메서드를 구현 해야 합니다. 이러한 메서드는의 모든 멤버는 `Service` 클래스; 서비스를 시작 하는 방법을 결정 하는 어떤 수명 주기 메서드를 호출 됩니다. 나중에 이러한 수명 주기 메서드를 자세히 설명 합니다.

기본적으로 서비스는 Android 응용 프로그램으로 동일한 프로세스에서 시작 됩니다. 설정 하 여 자체 프로세스에서 서비스를 시작 하는 것이 불가능 합니다 `ServiceAttribute.IsolatedProcess` 속성을 true로 합니다.

```csharp
[Service(IsolatedProcess=true)]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things, in it's own process!
}
```

다음 단계는 서비스를 시작 하 고 다음 세 가지 유형의 서비스를 구현 하는 방법을 검사할 이동 하는 방법을 검토 하는 것입니다.

> [!NOTE]
> 서비스는 UI 스레드에서 실행 하므로 작업이 수행 되도록 UI를 차단 하는 경우 서비스 작업을 수행 하려면 스레드를 사용 해야 합니다.

## <a name="starting-a-service"></a>서비스 시작

Android에서 서비스를 시작 하는 가장 기본적인 방법은 디스패치 하는 것을 `Intent` 식별 하는 서비스를 시작 해야 하는 데 메타 데이터를 포함 하는 합니다. 두 가지 다른 스타일의 서비스를 시작할 수 있는 인 텐트 가지가 있습니다.

-   **명시적 의도** &ndash; 는 _내려졌습니다_ 지정 된 작업을 완료 하려면 서비스를 사용 해야는 정확 하 게 식별 됩니다. 특정 주소가; 포함 된 문자로 명시적 의도 생각할 수 있습니다. Android는 의도를 명시적으로 식별 되는 서비스에 라우팅합니다. 이 코드 조각은 명시적 의도 사용 하 여 호출 서비스를 시작 하는 한 가지 예는 `DownloadService`:

    ```csharp
    // Example of creating an explicit Intent in an Android Activity
    Intent downloadIntent = new Intent(this, typeof(DownloadService));
    downloadIntent.data = Uri.Parse(fileToDownload);
    ```

-   **암시적 Intent** &ndash; 이 이와 같은 의도 느슨하게 하 게 식별 하는 작업을 수행 하고자 하지만 해당 작업을 완료 하려면 서비스 알 수 없는. 암시적 의도 문자는 "To Whom It 월 문제..." 배달 생각할 수 있습니다.
    Android는 의도의 내용 검사 되며 의도 일치 하는 기존 서비스 인지 확인 합니다.

    _의도 필터_ 등록 된 서비스를 사용 하 여 암시적 intent를 일치 시키는 데 사용 됩니다. 의도 필터에 추가 되는 XML 요소는 **AndroidManifest.xml** 암시적 의도 사용 하 여 서비스와 일치 하는 데 필요한 메타 데이터가 들어 있는입니다.

    ```csharp
    Intent sendIntent = new Intent("common.xamarin.DemoService");
    sendIntent.Data = Uri.Parse(fileToDownload);
    ```

Android는 암시적 intent 둘 이상의 가능한 일치 항목에 있는 경우 사용자가 작업을 처리 하는 구성 요소를 선택할을 요청할 수 있습니다.

![명확성 대화 상자의 스크린 샷](images/creating-a-service-01.png "명확성 대화 상자의 스크린 샷")

> [!IMPORTANT]
> Android 5.0 (AP 수준 21)는 암시적 intent 서비스를 시작 하려면 사용할 수 없습니다.

가능한 경우 응용 프로그램 서비스를 시작 하도록 명시적 의도 사용 해야 합니다. 암시적 의도 특정 서비스를 시작 하도록 요청 하지 않으며 &ndash; 요청 장치에 설치 하는 일부 서비스에 대 한 요청을 처리 하는 것입니다. 이 모호한 요청 잘못 된 서비스 요청 또는 다른 응용 프로그램 불필요 하 게 시작 (장치의 리소스에 대 한 증가)는 처리 될 수 있습니다.

의도 디스패치 하는 방법 서비스의 각 형식에 특정 가이드의 뒷부분에서 자세히 다룰 및 서비스의 형식에 따라 다릅니다.


### <a name="creating-an-intent-filter-for-implicit-intents"></a>암시적 의도 대 한 의도 필터를 만들기

암시적 의도 사용 하 여 서비스에 연결 하려면 Android 앱 서비스의 기능을 식별 하려면 일부 메타 데이터를 제공 해야 합니다. 이 메타 데이터에서 제공 됩니다 _의도 필터_합니다. 의도 필터 작업 또는 서비스를 시작 하는 의도에 있어야 하는 데이터 형식 등 몇 가지 정보를 포함 합니다. Xamarin.android에서 의도 필터에 등록 되어 **AndroidManifest.xml** 사용 하 여 서비스를 데코레이팅하여 합니다 [ `IntentFilterAttribute` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)합니다. 다음 코드는 연결 된 작업을 통해 의도 필터를 추가 하는 예를 들어 `com.xamarin.DemoService`:

```csharp
[Service]
[IntentFilter(new String[]{"com.xamarin.DemoService"})]
public class DemoService : Service
{
}
```

이 인해 포함 되는 항목의 **AndroidManifest.xml** 파일 &ndash; 다음 예와 유사한 방식으로 응용 프로그램과 함께 패키지 된 진입점:

```xml
<service android:name="demoservice.DemoService">
    <intent-filter>
        <action android:name="com.xamarin.DemoService" />
    </intent-filter>
</service>
```

Xamarin.Android 서비스도의 기본 기능을 사용 하 여 자세히 서비스의 다른 하위 형식이 검토해 보겠습니다.


## <a name="related-links"></a>관련 링크

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/)
- [Android.App.Intent](https://developer.xamarin.com/api/type/Android.Content.Intent/)
- [Android.App.IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)
