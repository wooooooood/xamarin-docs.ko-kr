---
title: 서비스 만들기
ms.prod: xamarin
ms.assetid: A78A55E7-FB5C-4C42-8E3E-939B5E98F9EB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/03/2018
ms.openlocfilehash: 63f815cc974315735220a99fd4cce2af408a8c2f
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70119054"
---
# <a name="creating-a-service"></a>서비스 만들기

Xamarin Android 서비스는 Android 서비스의 두 가지 inviolable 규칙을 준수 해야 합니다.

- 를 [`Android.App.Service`](xref:Android.App.Service)확장 해야 합니다.
- 로 데코 레이트 되어야 합니다 [`Android.App.ServiceAttribute`](xref:Android.App.ServiceAttribute).

Android 서비스의 또 다른 요구 사항은 **Androidmanifest** 에 등록 하 고 고유한 이름을 지정 해야 한다는 것입니다. Xamarin.ios는 빌드 시 필요한 XML 특성을 사용 하 여 매니페스트에 서비스를 자동으로 등록 합니다.

이 코드 조각은 이러한 두 요구 사항을 충족 하는 Xamarin Android에서 서비스를 만드는 가장 간단한 예제입니다.  

```csharp
[Service]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

컴파일 시 Xamarin.Android는 다음 XML 요소를 **Androidmanifest**에 삽입하여 서비스를 등록합니다(Xamarin.Android에서 서비스에 대해 무작위로 이름을 생성했음을 알 수 있습니다).

```xml
<service android:name="md5a0cbbf8da641ae5a4c781aaf35e00a86.DemoService" />
```

다른 Android 응용 프로그램을 _내보내_ 서비스를 공유할 수 있습니다. 이렇게 하려면 `Exported` `ServiceAttribute`에서 속성을 설정 합니다. 서비스를 내보낼 때 서비스에 `ServiceAttribute.Name` 대 한 의미 있는 공개 이름을 제공 하도록 속성을 설정 해야 합니다. 이 코드 조각에서는 서비스를 내보내고 이름을 설명 하는 방법을 보여 줍니다.

```csharp
[Service(Exported=true, Name="com.xamarin.example.DemoService")]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

이 서비스에 대 한 **Androidmanifest .xml** 요소는 다음과 같습니다.

```xml
<service android:exported="true" android:name="com.xamarin.example.DemoService" />
```

서비스는 서비스를 만들 때 호출 되는 콜백 메서드를 사용 하 여 자체 수명 주기를 가집니다. 호출 되는 메서드는 서비스 유형에 따라 달라 집니다. 시작 된 서비스는 바인딩된 서비스와 다른 수명 주기 메서드를 구현 해야 하는 반면, 하이브리드 서비스는 시작 된 서비스와 바인딩된 서비스 모두에 대 한 콜백 메서드를 구현 해야 합니다. 이러한 메서드는 모두 `Service` 클래스의 멤버입니다. 서비스가 시작 되는 방법에 따라 호출 될 수명 주기 메서드가 결정 됩니다. 이러한 수명 주기 메서드는 나중에 자세히 설명 합니다.

기본적으로 서비스는 Android 응용 프로그램과 동일한 프로세스에서 시작 됩니다. 속성을 `ServiceAttribute.IsolatedProcess` true로 설정 하 여 자체 프로세스에서 서비스를 시작할 수 있습니다.

```csharp
[Service(IsolatedProcess=true)]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things, in it's own process!
}
```

다음 단계는 서비스를 시작 하는 방법을 검토 한 후로 이동 하 여 세 가지 유형의 서비스를 구현 하는 방법을 검사 하는 것입니다.

> [!NOTE]
> 서비스는 UI 스레드에서 실행 되므로 UI를 차단 하는 작업이 수행 되는 경우 서비스에서 스레드를 사용 하 여 작업을 수행 해야 합니다.

## <a name="starting-a-service"></a>서비스 시작

Android에서 서비스를 시작 하는 가장 기본적인 방법은 시작할 서비스를 식별 `Intent` 하는 데 도움이 되는 메타 데이터를 포함 하는를 디스패치 하는 것입니다. 서비스를 시작 하는 데 사용할 수 있는 두 가지 다른 스타일의 의도가 있습니다.

- **명시적 의도** 명시적 의도는 지정 된 작업을 완료 하는 데 사용 해야 하는 서비스를 정확 하 게 식별 합니다. &ndash; 명시적 의도는 특정 주소가 있는 문자로 간주할 수 있습니다. Android는 명시적으로 식별 된 서비스로 의도를 라우팅합니다. 이 코드 조각은 명시적 의도를 사용 하 여 라는 `DownloadService`서비스를 시작 하는 한 가지 예입니다.

    ```csharp
    // Example of creating an explicit Intent in an Android Activity
    Intent downloadIntent = new Intent(this, typeof(DownloadService));
    downloadIntent.data = Uri.Parse(fileToDownload);
    ```

- **암시적 의도** &ndash; 이 유형의 의도는 사용자가 수행 하려는 동작의를 식별 하지만 해당 작업을 완료 하는 정확한 서비스를 알 수 없습니다. 암시적 의도는 "문제를 해결할 수 있습니다." 라고 하는 문자로 간주할 수 있습니다.
    Android는 의도 된 내용을 검사 하 고 해당 의도와 일치 하는 기존 서비스가 있는지 확인 합니다.

    _내재 된 필터_ 를 사용 하 여 등록 된 서비스와 암시적 의도를 일치 시킬 수 있습니다. 의도 필터는 서비스와 암시적 의도를 일치 시키는 데 도움이 되는 필요한 메타 데이터를 포함 하는 **Androidmanifest** 에 추가 되는 xml 요소입니다.

    ```csharp
    Intent sendIntent = new Intent("common.xamarin.DemoService");
    sendIntent.Data = Uri.Parse(fileToDownload);
    ```

Android가 암시적 의도에 대해 둘 이상의 가능한 일치 항목을 갖는 경우 사용자에 게 작업을 처리할 구성 요소를 선택 하도록 요청할 수 있습니다.

![명확성 대화 상자의 스크린샷](images/creating-a-service-01.png "명확성 대화 상자의 스크린샷")

> [!IMPORTANT]
> Android 5.0 (AP 수준 21)부터 암시적 의도를 사용 하 여 서비스를 시작할 수 없습니다.

가능 하면 응용 프로그램은 명시적 의도를 사용 하 여 서비스를 시작 해야 합니다. 암시적 의도는 특정 서비스를 시작 &ndash; 하도록 요청 하지 않습니다. 요청을 처리 하기 위해 장치에 설치 된 일부 서비스에 대 한 요청입니다. 이 모호한 요청으로 인해 요청을 처리 하는 잘못 된 서비스 또는 다른 앱을 불필요 하 게 시작할 수 있습니다 .이로 인해 장치에서 리소스의 압력을 높일 수 있습니다.

의도를 디스패치 하는 방법은 서비스 유형에 따라 다르며, 각 서비스 유형과 관련 된 가이드에서 나중에 자세히 설명 합니다.


### <a name="creating-an-intent-filter-for-implicit-intents"></a>암시적 의도에 대 한 의도 필터 만들기

서비스를 암시적 의도에 연결 하려면 Android 앱에서 서비스의 기능을 식별 하는 메타 데이터를 제공 해야 합니다. 이 메타 데이터는 _의도 필터_에 의해 제공 됩니다. 의도 필터에는 서비스를 시작 하기 위해 제공 해야 하는 작업 또는 데이터 유형과 같은 일부 정보가 포함 되어 있습니다. Xamarin Android에서 의도 필터는 서비스를로 [`IntentFilterAttribute`](xref:Android.App.IntentFilterAttribute)데코레이팅하는 방법으로 **androidmanifest** 에 등록 됩니다. 예를 들어 다음 코드는의 `com.xamarin.DemoService`연결 된 작업을 사용 하 여 의도 필터를 추가 합니다.

```csharp
[Service]
[IntentFilter(new String[]{"com.xamarin.DemoService"})]
public class DemoService : Service
{
}
```

그러면 다음 예제와 유사한 방식으로 응용 프로그램과 함께 패키지 된 항목이 **androidmanifest. xml** 파일 &ndash; 에 포함 됩니다.

```xml
<service android:name="demoservice.DemoService">
    <intent-filter>
        <action android:name="com.xamarin.DemoService" />
    </intent-filter>
</service>
```

Xamarin Android 서비스의 기본 사항을 사용 하 여 다양 한 서비스 하위 유형을 더 자세히 살펴보겠습니다.


## <a name="related-links"></a>관련 링크

- [Android.App.Service](xref:Android.App.Service)
- [Android.App.ServiceAttribute](xref:Android.App.ServiceAttribute)
- [Android.App.Intent](xref:Android.Content.Intent)
- [Android.App.IntentFilterAttribute](xref:Android.App.IntentFilterAttribute)
