---
title: Android에서 앱 연결
description: 이 가이드에서는 Android 6.0이 앱 연결을 지 원하는 방법에 대해 설명 합니다. 모바일 앱이 웹 사이트의 Url에 응답할 수 있는 기술입니다. 앱 링크의 정의, Android 6.0 응용 프로그램에서 앱 연결을 구현 하는 방법 및 도메인에 대 한 모바일 앱에 권한을 부여 하도록 웹 사이트를 구성 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 48174E39-19FD-43BC-B54C-9AF11D4B1F91
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: d1a96c81da8d71d92e3ce5acd9928b293f3cf3dd
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69524711"
---
# <a name="app-linking-in-android"></a>Android에서 앱 연결

_이 가이드에서는 Android 6.0이 앱 연결을 지 원하는 방법에 대해 설명 합니다. 모바일 앱이 웹 사이트의 Url에 응답할 수 있는 기술입니다. 앱 링크의 정의, Android 6.0 응용 프로그램에서 앱 연결을 구현 하는 방법 및 도메인에 대 한 모바일 앱에 권한을 부여 하도록 웹 사이트를 구성 하는 방법을 설명 합니다._

## <a name="app-linking-overview"></a>앱 연결 개요

모바일 응용 프로그램은 더 이상 사일로 &ndash; 에 살고 있지 않습니다. 대부분의 경우 해당 웹 사이트와 함께 비즈니스의 중요 한 구성 요소입니다. 비즈니스는 모바일 응용 프로그램을 시작 하 고 모바일 앱에 관련 콘텐츠를 표시 하는 웹 사이트의 링크와 함께 웹 상태 및 모바일 응용 프로그램을 원활 하 게 연결 하는 것이 좋습니다. *앱 링크* ( *딥 링크*라고도 함)는 모바일 장치가 uri에 응답 하 고 해당 uri에 해당 하는 모바일 응용 프로그램을 시작할 수 있도록 하는 한 가지 방법입니다.

Android는 사용자가 모바일 브라우저에서 링크를 클릭할 때 *의도 시스템* &ndash; 을 통해 앱 링크를 처리 합니다. 모바일 브라우저는 Android가 등록 된 응용 프로그램에 위임할 의도를 디스패치합니다. 예를 들어, 요리 웹 사이트의 링크를 클릭 하면 해당 웹 사이트와 연결 된 모바일 앱이 열리고 사용자에 게 특정 조리법이 표시 됩니다. 해당 의도를 처리 하기 위해 등록 된 응용 프로그램이 두 개 이상 있는 경우 Android는 의도를 처리 해야 하는 응용 프로그램을 선택 하는 응용 프로그램을 사용자에 게 요청 하는 *명확성 대화 상자* 를 생성 합니다. 예를 들면 다음과 같습니다.

![명확성 대화 상자의 예제 스크린샷](app-linking-images/01-disambiguation-dialog.png)

Android 6.0는 자동 링크 처리를 사용 하 여이를 개선 합니다. Android는 응용 프로그램을 자동으로 시작 되는 URI &ndash; 의 기본 처리기로 자동으로 등록 하 고 관련 작업으로 직접 이동할 수 있습니다. Android 6.0에서 URI 클릭을 처리 하는 방법은 다음 조건에 따라 달라 집니다.

1. **기존 앱이 URI와 이미 연결 되어 있습니다** . &ndash; 사용자가 기존 앱과 URI를 이미 연결 했을 수 있습니다. 이 경우 Android는 해당 응용 프로그램을 계속 사용 합니다.
2. **URI와 연결 된 기존 앱이 없지만 지원 앱이 설치 되어 있습니다** . &ndash; 이 시나리오에서는 사용자가 기존 앱을 지정 하지 않았기 때문에 Android에서 설치 된 지원 응용 프로그램을 사용 하 여 요청을 처리 합니다.
3. **URI와 연결 된 기존 앱이 없지만 많은 지원 앱이 설치 되어 있습니다** . &ndash; Uri를 지 원하는 여러 응용 프로그램이 있으므로 명확성 대화 상자가 표시 되 고 사용자는 uri를 처리할 앱을 선택 해야 합니다.

사용자에 게 URI를 지 원하는 앱이 설치 되어 있지 않고 나중에 설치 되는 경우, Android는 URI와 연결 된 웹 사이트와의 연결을 확인 한 후 URI에 대 한 기본 처리기로 해당 응용 프로그램을 설정 합니다.

이 가이드에서는 android 6.0 응용 프로그램을 구성 하는 방법과 Android 6.0에서 앱 연결을 지원 하기 위해 디지털 자산 링크 파일을 만들고 게시 하는 방법을 설명 합니다.

## <a name="requirements"></a>요구 사항

이 가이드에는 Android 6.0 (API 레벨 23) 이상을 대상으로 하는 Xamarin Android 6.1 및 응용 프로그램이 필요 합니다.

앱 연결은 Xamarin 구성 요소 저장소에서 [Rivets NuGet 패키지](https://www.nuget.org/packages/Rivets/) 를 사용 하 여 이전 버전의 Android에서 가능 합니다. Rivets 패키지는 Android 6.0의 앱 연결과 호환 되지 않습니다. Android 6.0 앱 연결은 지원 하지 않습니다.

## <a name="configuring-app-linking-in-android-60"></a>Android 6.0에서 앱 연결 구성

Android 6.0에서 앱 링크를 설정 하려면 다음 두 가지 주요 단계를 수행 해야 합니다.

1. **웹 사이트 URI에 대해 하나 이상의 의도 필터 추가** &ndash; 의도 필터 가이드 Android는 모바일 브라우저에서 URL을 처리 하는 방법을 참조 하세요.
2. **웹 사이트**  &ndash; 에 디지털 자산 링크 JSON 파일 게시 웹 사이트에 업로드 되 고 Android에서 모바일 앱과 웹 사이트 도메인 간의 관계를 확인 하는 데 사용 되는 파일입니다. 이를 사용 하지 않으면 Android에서 앱을 URI의 기본 핸들로 설치할 수 없습니다. 사용자는 수동으로이 작업을 수행 해야 합니다.

<a name="configure-intent-filter" />

### <a name="configuring-the-intent-filter"></a>의도 필터 구성

웹 사이트에서 Android 응용 프로그램의 작업에 URI (또는 Uri 집합 가능)를 매핑하는 의도 필터를 구성 해야 합니다. Xamarin.ios에서이 관계는 [Intentfilterattribute](xref:Android.App.IntentFilterAttribute)를 사용 하 여 활동을 표시할 하 여 설정 합니다. 의도 필터는 다음 정보를 선언 해야 합니다.

* **`Intent.ActionView`** &ndash; 이렇게 하면 정보를 보기 위해 요청에 응답 하는 의도 필터를 등록 합니다.
* **`Categories`** 의도 필터는 두 **[의도](xref:Android.Content.Intent.CategoryBrowsable)** 를 모두 등록 해야 합니다. categorybrowsable 가능 하 고 의도 된 웹 URI를 올바르게 처리할 수 있습니다 **[.](xref:Android.Content.Intent.CategoryDefault)** &ndash;
* **`DataScheme`** 의도 필터는 and/ `http` or `https`를 선언 해야 합니다. &ndash; 이러한 두 가지 유효한 스키마만 있습니다.
* **`DataHost`** &ndash; Uri가 생성 되는 도메인입니다.
* **`DataPathPrefix`** &ndash; 웹 사이트의 리소스에 대 한 선택적 경로입니다.
* **`AutoVerify`** 특성은 Android에 응용 프로그램과 웹 사이트 간의 관계를 확인 하도록 지시 합니다 `autoVerify`. &ndash; 이에 대해서는 아래에서 자세히 설명 합니다.

다음 예제에서는 [intentfilterattribute](xref:Android.App.IntentFilterAttribute) 를 사용 하 여에서 `https://www.recipe-app.com/recipes` `http://www.recipe-app.com/recipes`링크를 처리 하는 방법을 보여 줍니다.

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

Android는 응용 프로그램을 URI에 대 한 기본 처리기로 등록 하기 전에 웹 사이트의 디지털 자산 파일에 대해 의도 필터로 식별 된 모든 호스트를 확인 합니다. Android에서 앱을 기본 처리기로 설정 하기 전에 모든 의도 필터에서 확인을 통과 해야 합니다.

### <a name="creating-the-digital-assets-link-file"></a>디지털 자산 링크 파일 만들기

Android 6.0 앱을 연결 하려면 응용 프로그램을 URI에 대 한 기본 처리기로 설정 하기 전에 Android에서 응용 프로그램과 웹 사이트 간의 연결을 확인 해야 합니다. 이 확인은 응용 프로그램을 처음 설치할 때 발생 합니다. *디지털 자산 링크* 파일은 관련 webdomain에서 호스팅하는 JSON 파일입니다.

> [!NOTE]
> 특성 `android:autoVerify` 은 의도 필터 &ndash; 에 의해 설정 되어야 합니다. 그렇지 않으면 Android에서 확인을 수행 하지 않습니다.

파일은 해당 위치 **https://domain/.well-known/assetlinks.json** 에 있는 도메인의 웹 마스터에 의해 배치 됩니다.

디지털 자산 파일에는 Android에서 연결을 확인 하는 데 필요한 메타 데이터가 포함 되어 있습니다. **Assetlinks. json** 파일의 키-값 쌍은 다음과 같습니다.

* `namespace`&ndash; Android 응용 프로그램의 네임 스페이스입니다.
* `package_name`&ndash; 응용 프로그램 매니페스트에 선언 된 Android 응용 프로그램의 패키지 이름입니다.
* `sha256_cert_fingerprints`&ndash; 서명 된 응용 프로그램의 SHA256 지문입니다. 응용 프로그램의 SHA1 지문을 가져오는 방법에 대 한 자세한 내용은 [키 저장소의 MD5 또는 Sha1 서명 찾기](~/android/deploy-test/signing/keystore-signature.md) 가이드를 참조 하세요.

다음 코드 조각은 단일 응용 프로그램이 나열 된 **assetlinks. json** 의 예입니다.

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

응용 프로그램의 다양 한 버전이 나 빌드를 지원 하기 위해 두 개 이상의 SHA256 지문을 등록할 수 있습니다. 다음 **assetlinks. json** 파일은 여러 응용 프로그램을 등록 하는 예제입니다.

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

[Google Digital Asset Links 웹 사이트](https://developers.google.com/digital-asset-links/tools/generator) 에는 디지털 자산 파일을 만들고 테스트 하는 데 도움이 될 수 있는 온라인 도구가 있습니다.

### <a name="testing-app-links"></a>앱 테스트 링크

앱 링크를 구현한 후에는 다양 한 부분을 테스트 하 여 예상 대로 작동 하는지 확인 해야 합니다.

다음 예제와 같이 Google의 디지털 자산 링크 API를 사용 하 여 디지털 자산 파일의 형식이 올바르게 지정 되 고 호스트 되는지 확인할 수 있습니다.

```html
https://digitalassetlinks.googleapis.com/v1/statements:list?source.web.site=
  https://<WEB SITE ADDRESS>:&relation=delegate_permission/common.handle_all_urls
```

의도 필터가 제대로 구성 되어 있고 앱이 URI에 대 한 기본 처리기로 설정 되었는지 확인 하기 위해 수행할 수 있는 두 가지 테스트가 있습니다.

1. 디지털 자산 파일은 위에서 설명한 대로 제대로 호스트 됩니다. 첫 번째 테스트는 Android가 모바일 응용 프로그램으로 리디렉션하는 의도를 디스패치합니다. Android 응용 프로그램을 시작 하 고 URL에 대해 등록 된 활동을 표시 합니다. 명령 프롬프트에서 다음을 입력 합니다.

    ```shell
    $ adb shell am start -a android.intent.action.VIEW \
        -c android.intent.category.BROWSABLE \
        -d "http://<domain1>/recipe/scalloped-potato"
    ```

2. 지정 된 장치에 설치 된 응용 프로그램에 대 한 기존 링크 처리 정책을 표시 합니다. 다음 명령을 사용 하 여 장치의 각 사용자에 대 한 링크 정책 목록을 다음 정보와 함께 덤프 합니다. 명령 프롬프트에 다음 명령을 입력합니다.

    ```shell
    $ adb shell dumpsys package domain-preferred-apps
    ```

    * **`Package`** &ndash; 응용 프로그램의 패키지 이름입니다.
    * **`Domain`** &ndash; 응용 프로그램에서 웹 링크를 처리 하는 도메인 (공백으로 구분)입니다.
    * **`Status`** &ndash; 앱에 대 한 현재 링크 처리 상태입니다. 값은 **항상** 응용 프로그램이 `android:autoVerify=true` 를 선언 하 고 시스템 확인을 통과 했음을 의미 합니다. 그 다음에는 기본 설정의 Android 시스템 레코드를 나타내는 16 진수가 나옵니다.

    예를 들어:

    ```shell
    $ adb shell dumpsys package domain-preferred-apps

    App linkages for user 0:
    Package: com.android.vending
    Domains: play.google.com market.android.com
    Status: always : 200000002
    ```

## <a name="summary"></a>요약

이 가이드에서는 Android 6.0에서 앱 연결이 작동 하는 방식에 대해 설명 했습니다. 그런 다음 앱 링크를 지원 하 고 응답 하도록 Android 6.0 응용 프로그램을 구성 하는 방법을 살펴보았습니다. Android 응용 프로그램에서 앱 링크를 테스트 하는 방법에 대해서도 설명 합니다.


## <a name="related-links"></a>관련 링크

- [키 저장소의 MD5 또는 SHA1 서명 찾기](~/android/deploy-test/signing/keystore-signature.md)
- [활동 및 의도](https://university.xamarin.com/classes#4)
- [AppLinks](http://applinks.org/)
- [Google Digital 자산 링크](https://developers.google.com/digital-asset-links/)
- [문 목록 생성기 및 테스터](https://developers.google.com/digital-asset-links/tools/generator)
