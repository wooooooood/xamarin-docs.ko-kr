---
title: Android에서 앱 연결
description: 이 가이드에서는 Android 6.0이 앱 연결을 지원하는 방법을 설명합니다. 앱 연결은 모바일 앱에서 웹 사이트의 URL에 응답할 수 있도록 하는 기술입니다. 앱 연결에 대한 정보, Android 6.0 애플리케이션에서 앱 연결을 구현하는 방법 및 도메인의 모바일 앱에 권한을 부여하도록 웹 사이트를 구성하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 48174E39-19FD-43BC-B54C-9AF11D4B1F91
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: af90c286d2bb960a9f78547dd15c3d98a69529ae
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "75487830"
---
# <a name="app-linking-in-android"></a>Android에서 앱 연결

_이 가이드에서는 Android 6.0이 앱 연결을 지원하는 방법을 설명합니다. 앱 연결은 모바일 앱에서 웹 사이트의 URL에 응답할 수 있도록 하는 기술입니다. 앱 연결에 대한 정보, Android 6.0 애플리케이션에서 앱 연결을 구현하는 방법 및 도메인의 모바일 앱에 권한을 부여하도록 웹 사이트를 구성하는 방법을 설명합니다._

## <a name="app-linking-overview"></a>앱 연결 개요

모바일 애플리케이션은 더 이상 사일로 &ndash;에 존재하지 않으며 대부분의 경우 해당 웹 사이트와 함께 비즈니스의 중요한 구성 요소입니다. 비즈니스를 수행하기 위해서는 모바일 애플리케이션을 시작하고 모바일 앱에 관련 콘텐츠를 표시하는 웹 사이트의 링크와 함께 웹 서비스 및 모바일 애플리케이션을 원활하게 연결하는 것이 좋습니다. *앱 연결*(*딥 링크*라고도 함)은 모바일 디바이스가 URI에 응답하고 해당 URI에 대응하는 모바일 애플리케이션을 시작할 수 있도록 하는 기술 중 하나입니다.

Android는 *의도 시스템*을 통해 앱 링크를 처리합니다. 사용자가 모바일 브라우저에서 링크를 클릭하면 Android가 등록된 애플리케이션에 위임하는 의도를 모바일 브라우저가 디스패치합니다. 예를 들어, 요리 웹 사이트의 링크를 클릭하면 해당 웹 사이트와 연결된 모바일 앱이 열리고 사용자에게 특정 조리법이 표시됩니다. 해당 의도를 처리하기 위해 등록된 애플리케이션이 두 개 이상 있는 경우, Android는 의도를 처리할 애플리케이션을 선택하기 위해 해당 애플리케이션을 사용자에게 확인하는 *명확성 대화 상자*를 발생시킵니다. 예를 들면 다음과 같습니다.

![명확성 대화 상자의 예제 스크린샷](app-linking-images/01-disambiguation-dialog.png)

Android 6.0은 자동 연결 처리를 통해 이를 개선합니다. Android에서 URI에 대한 기본 처리기로 애플리케이션을 자동으로 등록할 수 있습니다. 그러면 앱이 자동으로 시작되고 관련 작업으로 직접 이동합니다. Android 6.0에서 URI 클릭을 처리하는 방법은 다음 조건에 따라 달라집니다.

1. **기존 앱이 이미 URI와 연결되어 있는지 여부** &ndash; 사용자가 이미 기존 앱을 URI와 연결했을 수 있습니다. 이 경우 Android는 해당 애플리케이션을 계속 사용합니다.
2. **URI와 연결된 기존 앱은 없지만 지원되는 앱이 설치된 경우** &ndash; 이 시나리오에서는 사용자가 기존 앱을 지정하지 않았기 때문에 Android는 이미 설치된 지원 애플리케이션을 사용하여 요청을 처리합니다.
3. **URI와 연결된 기존 앱은 없지만 많은 지원 앱이 설치된 경우** &ndash; URI를 지원하는 애플리케이션이 여러 개 있으므로 명확성 대화 상자가 표시되고 사용자는 URI를 처리할 앱을 선택해야 합니다.

사용자에게 URI를 지원하는 앱이 설치되어 있지 않은 경우 나중에 설치하면 Android가 URI와 연결된 웹 사이트와의 연결을 확인한 후 URI에 대한 기본 처리기로 해당 애플리케이션을 설정합니다.

이 가이드에서는 Android 6.0 애플리케이션을 구성하는 방법과 Android 6.0에서 앱 연결을 지원하기 위해 디지털 자산 링크 파일을 만들어 게시하는 방법을 설명합니다.

## <a name="requirements"></a>요구 사항

이 가이드를 사용하기 위해서는 Android 6.0(API 수준 23) 이상을 대상으로 하는 Xamarin.Android 6.1 및 애플리케이션이 필요합니다.

Xamarin 구성 요소 저장소의 [Rivets NuGet 패키지](https://www.nuget.org/packages/Rivets/)를 사용하면 이전 버전의 Android에서 앱 연결을 사용할 수 있습니다. Rivets 패키지는 Android 6.0의 앱 연결과 호환되지 않으며 Android 6.0 앱 연결을 지원하지 않습니다.

## <a name="configuring-app-linking-in-android-60"></a>Android 6.0에서 앱 연결 구성

Android 6.0에서 앱 연결을 설정하려면 다음 두 가지 주요 단계를 수행해야 합니다.

1. **웹 사이트 URI에 대한 하나 이상의 의도 필터 추가** &ndash; 의도 필터는 모바일 브라우저에서 URL 클릭을 처리하는 방법을 Android에게 안내합니다.
2. **웹 사이트에 *디지털 자산 연결 JSON* 파일 게시** &ndash; 이 파일은 웹 사이트에 업로드되어 Android에서 모바일 앱과 웹 사이트의 도메인 간 관계를 확인하는 데 사용됩니다. 이 파일이 없으면 Android에서 앱을 URI의 기본 처리기로 설치할 수 없으므로 사용자가 수동으로 설치해야 합니다.

<a name="configure-intent-filter" />

### <a name="configuring-the-intent-filter"></a>의도 필터 구성

웹 사이트의 URI(또는 사용 가능한 URI 세트)를 Android 애플리케이션의 작업에 매핑하는 의도 필터를 구성해야 합니다. Xamarin.Android에서 이 관계는 [IntentFilterAttribute](xref:Android.App.IntentFilterAttribute)로 작업을 표시하여 설정됩니다. 의도 필터는 다음 정보를 선언해야 합니다.

- **`Intent.ActionView`** &ndash; 정보를 보기 위한 요청에 응답하는 의도 필터를 등록합니다.
- **`Categories`** &ndash; 의도 필터는 웹 URI를 적절히 처리할 수 있도록 **[Intent.CategoryBrowsable](xref:Android.Content.Intent.CategoryBrowsable)** 및 **[Intent.CategoryDefault](xref:Android.Content.Intent.CategoryDefault)** 를 모두 등록해야 합니다.
- **`DataScheme`** &ndash; 의도 필터는 `http` 및/또는 `https`를 선언해야 합니다. 유일하게 유효한 두 가지 스키마입니다.
- **`DataHost`** &ndash; URI가 생성되는 도메인입니다.
- **`DataPathPrefix`** &ndash; 웹 사이트의 리소스에 대한 선택적 경로입니다.
- **`AutoVerify`** &ndash; `autoVerify` 특성은 Android에 애플리케이션과 웹 사이트 간의 관계를 확인하도록 지시합니다. 여기에 대해서는 아래에서 자세히 설명합니다.

다음 예제에서는 [IntentFilterAttribute](xref:Android.App.IntentFilterAttribute)를 사용하여 `https://www.recipe-app.com/recipes` 및 `http://www.recipe-app.com/recipes`의 링크를 처리하는 방법을 보여 줍니다.

```csharp
[IntentFilter(new [] { Intent.ActionView },
              Categories = new[] { Intent.CategoryBrowsable, Intent.CategoryDefault },
              DataScheme = "http",
              DataHost = "recipe-app.com",
              DataPathPrefix = "/recipe",
              AutoVerify=true)]
public class RecipeActivity : Activity
{
    // Code for the activity omitted
}
```

Android는 애플리케이션을 URI에 대한 기본 처리기로 등록하기 전에 웹 사이트의 디지털 자산 파일에 대해 의도 필터로 식별된 모든 호스트를 확인합니다. Android에서 앱을 기본 처리기로 설정하려면 먼저 모든 의도 필터가 확인 과정을 통과해야 합니다.

### <a name="creating-the-digital-assets-link-file"></a>디지털 자산 링크 파일 만들기

Android 6.0 앱 연결을 사용하려면 애플리케이션을 URI에 대한 기본 처리기로 설정하기 전에 Android에서 애플리케이션과 웹 사이트 간 연결을 확인해야 합니다. 이 확인 과정은 애플리케이션을 처음 설치할 때 수행됩니다. *디지털 자산 링크* 파일은 해당 웹 도메인에서 호스트되는 JSON 파일입니다.

> [!NOTE]
> `android:autoVerify` 특성은 의도 필터 &ndash;에서 설정해야 합니다. 아니면 Android에서 확인을 수행하지 않습니다.

이 파일은 **https://domain/.well-known/assetlinks.json** 위치에 도메인의 웹 마스터가 배치합니다.

디지털 자산 파일에는 Android에서 연결을 확인하는 데 필요한 메타데이터가 포함되어 있습니다. **assetlinks.json** 파일에는 다음과 같은 키-값 쌍이 있습니다.

- `namespace` &ndash; Android 애플리케이션의 네임스페이스.
- `package_name` &ndash; 애플리케이션 매니페스트에 선언된 Android 애플리케이션의 패키지 이름.
- `sha256_cert_fingerprints` &ndash; 서명된 애플리케이션의 SHA256 지문. 애플리케이션의 SHA1 지문을 가져오는 방법에 대한 자세한 내용은 [키 저장소의 MD5 또는 SHA1 서명 찾기](~/android/deploy-test/signing/keystore-signature.md) 가이드를 참조하세요.

다음 코드 조각은 단일 애플리케이션이 있는 **assetlinks.json**의 예입니다.

```json
[
   {
      "relation":[
         "delegate_permission/common.handle_all_urls"
      ],
      "target":{
         "namespace":"android_app",
         "package_name":"com.example",
         "sha256_cert_fingerprints":[
            "14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"
         ]
      }
   }
]
```

애플리케이션의 여러 버전 또는 빌드를 지원하는 SHA256 지문을 둘 이상 등록할 수 있습니다. 다음 **assetlinks.json** 파일은 여러 애플리케이션을 등록하는 예제입니다.

```json
[
   {
      "relation":[
         "delegate_permission/common.handle_all_urls"
      ],
      "target":{
         "namespace":"android_app",
         "package_name":"example.com.puppies.app",
         "sha256_cert_fingerprints":[
            "14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"
         ]
      }
   },
   {
      "relation":[
         "delegate_permission/common.handle_all_urls"
      ],
      "target":{
         "namespace":"android_app",
         "package_name":"example.com.monkeys.app",
         "sha256_cert_fingerprints":[
            "14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"
         ]
      }
   }
]
```

[Google Digital Asset Links 웹 사이트](https://developers.google.com/digital-asset-links/tools/generator)에서는 디지털 자산 파일을 만들고 테스트하는 데 도움이 될 수 있는 온라인 도구를 제공합니다.

### <a name="testing-app-links"></a>앱 연결 테스트

앱 연결을 구현한 후 다양한 부분을 테스트하여 예상 대로 작동하는지 확인해야 합니다.

다음 예제와 같이 Google의 디지털 자산 링크 API를 사용하여 디지털 자산 파일이 올바르게 형식 지정되고 호스트되는지 확인할 수 있습니다.

```html
https://digitalassetlinks.googleapis.com/v1/statements:list?source.web.site=
  https://<WEB SITE ADDRESS>:&relation=delegate_permission/common.handle_all_urls
```

의도 필터가 제대로 구성되어 있고 앱이 URI에 대한 기본 처리기로 설정되었는지 확인하려면 두 가지 테스트를 수행할 수 있습니다.

1. 디지털 자산 파일이 위에서 설명한 대로 제대로 호스트됩니다. 첫 번째 테스트는 Android가 모바일 애플리케이션으로 리디렉션하는 의도를 디스패치합니다. Android 애플리케이션이 시작되고 URL에 대해 등록된 작업을 표시합니다. 명령 프롬프트에서 다음을 입력합니다.

    ```shell
    $ adb shell am start -a android.intent.action.VIEW \
        -c android.intent.category.BROWSABLE \
        -d "http://<domain1>/recipe/scalloped-potato"
    ```

2. 지정된 디바이스에 설치된 애플리케이션에 대한 정책을 처리하는 기존 링크를 표시합니다. 다음 명령은 다음 정보를 사용하여 디바이스의 각 사용자에 대한 링크 정책 목록을 덤프합니다. 명령 프롬프트에 다음 명령을 입력합니다.

    ```shell
    $ adb shell dumpsys package domain-preferred-apps
    ```

    - **`Package`** &ndash; 애플리케이션의 패키지 이름입니다.
    - **`Domain`** &ndash; 애플리케이션에서 웹 연결을 처리하는 도메인(공백으로 구분)입니다.
    - **`Status`** &ndash; 앱에 대한 현재 링크 처리 상태입니다. **always** 값은 애플리케이션이 `android:autoVerify=true`를 선언하고 시스템 확인을 통과했음을 의미합니다. 그 다음에는 Android 시스템의 기본 설정 레코드를 나타내는 16진수가 나옵니다.

    예를 들어:

    ```shell
    $ adb shell dumpsys package domain-preferred-apps

    App linkages for user 0:
    Package: com.android.vending
    Domains: play.google.com market.android.com
    Status: always : 200000002
    ```

## <a name="summary"></a>요약

이 가이드에서는 Android 6.0에서 앱 연결이 작동하는 방식에 대해 설명했습니다. 그런 다음, 앱 연결을 지원하고 응답하도록 Android 6.0 애플리케이션을 구성하는 방법을 살펴보았습니다. Android 애플리케이션에서 앱 연결을 테스트하는 방법에 대해서도 설명합니다.

## <a name="related-links"></a>관련 링크

- [키 저장소의 MD5 또는 SHA1 서명 찾기](~/android/deploy-test/signing/keystore-signature.md)
- [앱 연결](https://developers.facebook.com/docs/applinks)
- [Google Digital Assets Links](https://developers.google.com/digital-asset-links/)
- [문 목록 생성기 및 테스터](https://developers.google.com/digital-asset-links/tools/generator)
