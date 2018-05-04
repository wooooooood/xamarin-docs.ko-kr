---
title: Android에서 앱 연결
description: 이 가이드 앱 링크, 모바일 앱 웹 사이트 Url에 응답할 수 있도록 허용 하는 기술을 Android 6.0을 지 원하는 방법에 대해 설명 합니다. 어떤 앱 연결, Android 6.0 응용 프로그램에서 응용 프로그램 연결을 구현 하는 방법 및 도메인에 대 한 모바일 앱에 사용 권한을 부여 하 여 웹 사이트를 구성 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 48174E39-19FD-43BC-B54C-9AF11D4B1F91
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 84185bb616597ee62a35c1acacc5e3664f500c21
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/03/2018
---
# <a name="app-linking-in-android"></a>Android에서 앱 연결

_이 가이드 앱 링크, 모바일 앱 웹 사이트 Url에 응답할 수 있도록 허용 하는 기술을 Android 6.0을 지 원하는 방법에 대해 설명 합니다. 어떤 앱 연결, Android 6.0 응용 프로그램에서 응용 프로그램 연결을 구현 하는 방법 및 도메인에 대 한 모바일 앱에 사용 권한을 부여 하 여 웹 사이트를 구성 하는 방법을 설명 합니다._

## <a name="app-linking-overview"></a>응용 프로그램 연결 개요

모바일 응용 프로그램은 더 이상 사일로에 살고 &ndash; 많은 경우에는 중요 한 구성 요소를 자신의 웹 사이트와 함께 해당 비즈니스의 됩니다. 기업 원활 하 게 모바일 응용 프로그램을 시작 하 고 모바일 앱 관련 콘텐츠를 표시 하는 웹 사이트에 대 한 링크가 있는 자신의 웹 사이트 및 모바일 응용 프로그램을 연결 하는 것이 좋습니다. *응용 프로그램 연결* (라고도 *딥 링크*)는 모바일 장치를 URI에 응답 하 고 해당 URI에 해당 하는 모바일 응용 프로그램을 시작 하는 하나의 방법입니다.

Android 앱 연결을 통해 처리는 *의도 시스템* &ndash; 모바일 브라우저 Android 등록된 된 응용 프로그램에 위임 하는 프로그램 의도 전달 하는 사용자가 모바일 브라우저의 링크를 클릭 합니다. 예를 들어 요리 웹 사이트에서 링크를 클릭 하면 해당 웹 사이트와 연결 된 모바일 앱을 열고 고 게 특정 레시피 사용자에 게 표시 됩니다. 있는 경우 해당 의도 처리 하는 둘 이상의 응용 프로그램 등록 후 라고 Android에서 발생 한 *명확성 대화* 하 게 사용자 할 응용 프로그램에 대 한 의미를 가지는 처리 해야 하는 응용 프로그램을 선택 예:

![명확성 대화 상자의 예제 스크린 샷](app-linking-images/01-disambiguation-dialog.png)

Android 6.0 자동 링크 처리를 사용 하 여이 향상 합니다. Android를 자동으로 URI에 대 한 기본 처리기로 응용 프로그램을 등록할 수 &ndash; 응용 프로그램에서는 자동으로 시작 하 고 관련 활동으로 직접 이동 합니다. Android 6.0 URI 클릭을 처리 하도록 결정 하는 방법 다음 조건에 따라 달라 집니다.

1. **기존 응용 프로그램 URI와 연결 되어** &ndash; 사용자 연결 된 가능성이 이미 기존 응용 프로그램 URI와 합니다. 이 경우 해당 응용 프로그램을 사용 하 여 Android 계속 됩니다.
2. **기존 앱의 URI에 연결 되어 있지만 지원 앱이 설치 된** &ndash; 이 시나리오에서는 사용자 지정 하지는 기존 응용 프로그램 Android 지원 설치 된 응용 프로그램을 사용 하 여 요청을 처리할 수 있도록 합니다.
3. **기존 앱의 URI에 연결 되어 있지만 많은 지원 앱이 설치 되어** &ndash; 있기 때문에 여러 응용 프로그램을 지 원하는 URI, 명확성 대화 상자 표시 되 고 사용자는 어떤 앱을 선택 해야 합니다 URI를 처리 합니다.

사용자에 게 앱이 없습니다 설치를 지 원하는 URI, 하나는 이후에 설치 하는 경우 다음 Android 됩니다 설정 해당 응용 프로그램 기본 처리기로 URI에 대 한 URI와 연결 된 웹 사이트와 연결을 확인 한 후 합니다.

이 가이드는 Android 6.0 응용 프로그램을 구성 하는 방법과 만들고 Android 6.0에서 응용 프로그램 연결을 지원 하기 위해 디지털 자산 링크 파일을 게시 하는 방법을 설명 합니다.

## <a name="requirements"></a>요구 사항

이 가이드는 Xamarin.Android 6.1 및 Android 6.0 (API 수준 23)을 대상으로 하는 응용 프로그램 필요 이상.

사용 하 여 이전 Android 버전에 비해는 응용 프로그램 연결에서 [Rivets NuGet 패키지](https://www.nuget.org/packages/Rivets/) Xamarin 구성 요소 저장소에서 합니다. 대 갈 못 패키지 호환 되지 않습니다. Android 6.0;에서 응용 프로그램 링크 Android 6.0 앱 링크는 지원 하지 않습니다.

## <a name="configuring-app-linking-in-android-60"></a>Android 6.0에서 응용 프로그램 연결 구성

Android 6.0의 응용 프로그램 링크 설정 두 가지 주요 단계가 포함 됩니다.

1. **하나 이상의 의도 필터링 하 여 웹 사이트 URI에 대 한 추가** &ndash; 의도 필터 Android 모바일 브라우저에서 URL 클릭을 처리 하는 방법을 안내 합니다.
2. **게시는 *디지털 자산 링크 JSON* 웹 사이트에서 파일** &ndash; 웹 사이트에 업로드 하 고 Android에서 모바일 응용 프로그램 및 웹 사이트의 도메인 간의 관계를 확인 하는 데 사용 되는 파일입니다. 이렇게 하지 않으면 Android 수 없는 앱을 설치 기본 핸들로 URI;에 대 한 사용자 하므로 수동으로 수행 해야 합니다.

<a name="configure-intent-filter" />

### <a name="configuring-the-intent-filter"></a>의도 필터 구성

Android 응용 프로그램에서 활동에는 웹 사이트에서 URI (또는 가능한 Uri 집합을)를 매핑하는 의도 필터를 구성 하는 데 필요한 경우 Xamarin.Android,이 관계와 관련 된 활동을 표시 하 여 설정 되 고 [IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)합니다. 의도 필터에서 다음 정보를 선언 해야 합니다.

* **`Intent.ActionView`** &ndash; 이 정보를 보려면 요청에 응답 하도록 의도 필터를 등록 합니다.
* **`Categories`** &ndash;  의도 필터 둘 모두를 등록 해야 **[Intent.CategoryBrowsable](https://developer.xamarin.com/api/field/Android.Content.Intent.CategoryBrowsable/)** 및 **[Intent.CategoryDefault](https://developer.xamarin.com/api/field/Android.Content.Intent.CategoryDefault/)** 제대로 할 수 웹 URI를 처리 합니다.
* **`DataScheme`** &ndash; 선언 해야 합니다. 의도 필터 `http` 및/또는 `https`합니다. 다음은 두 개의 유효한 계획입니다.
* **`DataHost`** &ndash; 이의 도메인에 있는 Uri에서 생성 됩니다.
* **`DataPathPrefix`** &ndash; 웹 사이트에 있는 리소스에는 선택적 경로입니다.
* **`AutoVerify`** &ndash; `autoVerify` 특성은 응용 프로그램 및 웹 사이트 간의 관계를 확인 하는 Android 지시 합니다. 아래 더 논의 될 것입니다.

사용 하는 방법을 보여 주는 다음 예제는 [IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) 에서 링크를 처리 하도록 `https://www.recipe-app.com/recipes` 들어오고 `http://www.recipe-app.com/recipes`:

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

Android에서는 URI에 대 한 기본 처리기로 응용 프로그램을 등록 하기 전에 웹 사이트에서 디지털 자산 파일에 대 한 의도 된 필터에 의해 식별 되는 모든 호스트를 확인 합니다. Android 기본 처리기로 앱을 설정할 수 있으려면 모든 의도 필터 확인을 통과 해야 합니다.

### <a name="creating-the-digital-assets-link-file"></a>디지털 자산 링크 파일 만들기

Android 6.0 응용 프로그램 연결 Android URI에 대 한 기본 처리기로 응용 프로그램을 설정 하기 전에 웹 사이트와 응용 프로그램 간의 연결을 확인 하도록 해야 합니다. 이 확인은 응용 프로그램이 처음 설치할 때 발생 합니다. *디지털 자산 링크* 파일이 관련 webdomain(s)에서 호스팅하는 JSON 파일입니다.

> [!NOTE]
> `android:autoVerify` 의도 필터에서 특성을 설정 해야 &ndash; 그렇지 않은 경우 Android 확인 수행 하지 것입니다.

파일 위치에 있는 도메인의 웹 마스터 배치할 **https://domain/.well-known/assetlinks.json**합니다.

디지털 자산 파일 연결을 확인 하는 Android에 대 한 메타 데이터가 필요한 포함 되어 있습니다. **assetlinks.json** 파일에 다음 키-값 쌍:

* `namespace` &ndash; Android 응용 프로그램의 네임 스페이스입니다.
* `package_name` &ndash; Android 응용 프로그램 (응용 프로그램 매니페스트에 선언 됨)의 패키지 이름입니다.
* `sha256_cert_fingerprints` &ndash; 서명 된 응용 프로그램의 경우 SHA256 지문 합니다. 이 가이드를 참조 하십시오 [키 저장소의 MD5 또는 SHA1 서명 찾기](~/android/deploy-test/signing/keystore-signature.md) 응용 프로그램의 SHA1 지문을 구하는 방법에 대 한 자세한 내용은 합니다.

다음 코드 조각은의 한 예로 **assetlinks.json** 단일 응용 프로그램으로 나열:

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

인스턴스가 서로 다른 버전을 지원 하기 위해 둘 이상의 SHA256 지문을 등록할 수 있거나 응용 프로그램의 작성 합니다. 다음이 단계 **assetlinks.json** 파일은 여러 응용 프로그램 등록의 예:

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

[디지털 자산 링크 Google 웹 사이트](https://developers.google.com/digital-asset-links/tools/generator) 가 온라인을 작성 하 고 디지털 자산 파일을 테스트 하는 데 유용한 도구입니다.

### <a name="testing-app-links"></a>테스트 응용 프로그램 링크

응용 프로그램 링크를 구현한 후 예상 대로 작동 하는지 확인 해야 다양 한 부분을 테스트 해야 합니다.

디지털 자산 파일 올바르게 포맷 및이 예제에 나와 있는 것 처럼 Google의 디지털 자산 링크 API를 사용 하 여 호스트를 확인 하는 것이 불가능 합니다.

```html
https://digitalassetlinks.googleapis.com/v1/statements:list?source.web.site=
  https://<WEB SITE ADDRESS>:&relation=delegate_permission/common.handle_all_urls
```

의도 필터 제대로 구성 된 응용 프로그램 URI에 대 한 기본 처리기로 설정 되어 있는지 확인을 수행할 수 있는 두 가지 테스트 가지가 있습니다.

1.  위에서 설명한 대로 올바르게 디지털 자산 파일이 호스팅됩니다. Android 모바일 응용 프로그램 리디렉션되어야 하는 의도 전달 하는 첫 번째 테스트 합니다. Android 응용 프로그램을 시작 하 고 URL에 대 한 등록 작업을 표시 해야 합니다. 명령 프롬프트에서:

    ```shell
    $ adb shell am start -a android.intent.action.VIEW \
        -c android.intent.category.BROWSABLE \
        -d "http://<domain1>/recipe/scalloped-potato"
    ```

2.  제공된 된 장치에 설치 된 응용 프로그램에 대 한 정책을 처리 하 고 기존 연결을 표시 합니다. 다음 명령은 각 사용자는 다음 정보를 사용 하 여 장치에 대 한 링크 정책 목록을 덤프 합니다. 명령 프롬프트에 다음 명령을 입력합니다.

    ```shell
    $ adb shell dumpsys package domain-preferred-apps
    ```

    * **`Package`** &ndash; 응용 프로그램의 패키지 이름입니다.
    * **`Domain`** &ndash; 웹 링크가 응용 프로그램에서 처리 되는 (공백으로 구분 된) 도메인
    * **`Status`** &ndash; 응용 프로그램에 대 한 현재 링크 처리 상태입니다. 값이 **항상** 응용 프로그램에 있다는 것을 의미 `android:autoVerify=true` 선언 하 고 시스템 확인을 통과 했습니다. 환경 설정의 Android 시스템의 레코드를 나타내는 16 진수 숫자 나옵니다.

    예를 들어:

    ```shell
    $ adb shell dumpsys package domain-preferred-apps

    App linkages for user 0:
    Package: com.android.vending
    Domains: play.google.com market.android.com
    Status: always : 200000002
    ```

## <a name="summary"></a>요약

이 가이드는 Android 6.0의 작동 방법을 응용 프로그램 연결을 설명합니다. 다음 Android 6.0 응용 프로그램을 지원 하 고 응용 프로그램 링크에 응답을 구성 하는 방법을 설명 합니다. 또한 Android 응용 프로그램에서 응용 프로그램 연결을 테스트 하는 방법에 대해서도 설명 합니다.


## <a name="related-links"></a>관련 링크

- [키 저장소의 MD5 또는 SHA1 서명 찾기](~/android/deploy-test/signing/keystore-signature.md)
- [활동 및 의도](https://university.xamarin.com/classes#4)
- [AppLinks](http://applinks.org/)
- [Google 디지털 자산 링크](https://developers.google.com/digital-asset-links/)
- [문 목록 생성기 및 테스터](https://developers.google.com/digital-asset-links/tools/generator)
