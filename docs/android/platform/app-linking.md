---
title: Android에서 앱 연결
description: 이 가이드에서는 Android 6.0 지원 앱 링크, 모바일 앱 웹 사이트에서 Url에 응답할 수 있는 기술 하는 방법을 설명 합니다. 어떤 앱 연결, Android 6.0 응용 프로그램에서 앱 연결을 구현 하는 방법 및 도메인에 대 한 모바일 앱에 권한을 부여 하 여 웹 사이트를 구성 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 48174E39-19FD-43BC-B54C-9AF11D4B1F91
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: dd4ba236df8e5993c7f7ed86393eb66ce01db595
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111267"
---
# <a name="app-linking-in-android"></a>Android에서 앱 연결

_이 가이드에서는 Android 6.0 지원 앱 링크, 모바일 앱 웹 사이트에서 Url에 응답할 수 있는 기술 하는 방법을 설명 합니다. 어떤 앱 연결, Android 6.0 응용 프로그램에서 앱 연결을 구현 하는 방법 및 도메인에 대 한 모바일 앱에 권한을 부여 하 여 웹 사이트를 구성 하는 방법을 설명 합니다._

## <a name="app-linking-overview"></a>앱 연결 개요

모바일 응용 프로그램을 사일로에 둘 이상 라이브 &ndash; 자신의 웹 사이트와 함께 해당 비즈니스는 중요 한 구성 요소는 대부분의 경우에서. 기업용 모바일 응용 프로그램을 시작 하 고 모바일 앱에서 관련 콘텐츠를 표시 하는 웹 사이트에 대 한 링크를 사용 하 여 해당 웹 서비스 및 모바일 응용 프로그램을 원활 하 게 연결 하는 것이 좋습니다. *앱 연결* (라고도 *딥 링크*) URI에 응답 하 고 해당 URI에 해당 하는 모바일 응용 프로그램을 시작 하려면 모바일 장치를 허용 하는 한 가지 방법은 됩니다.

Android 앱이 연결을 통해 처리 합니다 *의도 시스템* &ndash; 모바일 브라우저는 Android 등록 된 응용 프로그램에 위임 하는 의도 전달 하는 사용자가 모바일 브라우저에서 링크를 클릭 합니다. 예를 들어, 요리 웹 사이트에서 링크를 클릭 하는 해당 웹 사이트와 연결 된 모바일 앱을 열고 사용자에 게 특정 작성법을 표시 합니다. 있는 경우 해당 의도 처리 하도록 둘 이상의 응용 프로그램 등록 후 Android 라고 시킵니다를 *명확성 대화* 하 게 사용자 응용 프로그램에 대 한 의도 처리 해야 하는 응용 프로그램 선택 예:

![명확성 대화 상자의 스크린샷 예제](app-linking-images/01-disambiguation-dialog.png)

Android 6.0 자동 링크 처리를 사용 하 여이 향상 됩니다. URI에 대 한 기본 처리기로 응용 프로그램을 자동으로 등록 하는 Android 용 있기 &ndash; 앱을 자동으로 시작 하 고 관련 활동으로 직접 이동 합니다. Android 6.0 URI 클릭을 처리 하도록 결정 하는 방법 다음 조건에 따라 달라 집니다.

1. **기존 앱 연결 URI와 사용 하 여 이미** &ndash; 사용자 연결 된 가능성이 이미 기존 앱 URI와 사용 하 여 합니다. 이 경우 해당 응용 프로그램을 사용 하려면 Android 계속 됩니다.
2. **기존 앱 없음 URI에 연결 되어 있지만 지원 앱을 설치** &ndash; 이 시나리오에서는 사용자 지정 하지 기존 앱을 Android 지원 응용 프로그램을 사용 하 여 요청을 처리할 수 있도록 합니다.
3. **기존 앱 없음 URI에 연결 되어 있지만 많은 지원 앱을 설치할지** &ndash; URI를 지 원하는 여러 응용 프로그램 이기 때문에 명확성 대화 상자 표시 되 고 사용자는 어떤 앱을 선택 해야 합니다 URI를 처리 합니다.

사용자가 없는 URI를 지 원하는 앱 설치 하 고 이후에 설치 되어 있는 경우 Android는 설정한 해당 응용 프로그램 기본 처리기로 URI에 대 한 URI와 사용 하 여 연결 된 웹 사이트를 사용 하 여 연결을 확인 한 후입니다.

이 가이드에서는 Android 6.0 응용 프로그램을 구성 하는 방법과 만들고 Android 6.0에 앱 연결을 지원 하기 위해 디지털 자산 링크 파일을 게시 하는 방법을 설명 합니다.

## <a name="requirements"></a>요구 사항

이 가이드에서는 Xamarin.Android 6.1과 Android 6.0 (API 레벨 23)를 대상으로 하는 응용 프로그램 이상.

사용 하 여 이전 버전의 Android는 앱 연결 합니다 [Rivets NuGet 패키지](https://www.nuget.org/packages/Rivets/) Xamarin Component store에서. 대 갈 못 패키지가 호환 되지 않습니다. Android 6.0; 앱 링크 Android 6.0 앱 링크는 지원 하지 않습니다.

## <a name="configuring-app-linking-in-android-60"></a>Android 6.0에서 응용 프로그램 연결 구성

Android 6.0에서 앱 링크 설정 두 가지 주요 단계가 포함 됩니다.

1. **하나 이상의 의도 필터 웹 사이트 URI에 대 한 추가** &ndash; 의도 필터 과정 Android 모바일 브라우저에서 URL 클릭을 처리 하는 방법을 안내 합니다.
2. **게시 된 *디지털 자산 링크 JSON* 웹 사이트에서 파일** &ndash; 웹 사이트에 업로드 되 고 Android에서 모바일 앱 및 웹 사이트의 도메인 간의 관계를 확인 하는 데 사용 되는 파일입니다. 이렇게 하지 않으면 Android 수 없습니다. 앱을 설치할 기본 핸들로 URI; 사용자 되므로 수동으로 수행 해야 합니다.

<a name="configure-intent-filter" />

### <a name="configuring-the-intent-filter"></a>의도 필터 구성

Android 응용 프로그램의 활동에 웹 사이트에서 URI (또는 가능한 Uri 집합을)를 매핑하는 의도 필터를 구성 하는 것이 반드시 합니다. Xamarin.android에서이 관계가 포함 된 활동을 표시 하 여 설정 합니다 [IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)합니다. 의도 필터에는 다음 정보를 선언 해야 합니다.

* **`Intent.ActionView`** &ndash; 이 의도 필터 정보를 보려면 요청에 응답 하도록 등록
* **`Categories`** &ndash;  의도 필터는 모두 등록 해야 **[Intent.CategoryBrowsable](https://developer.xamarin.com/api/field/Android.Content.Intent.CategoryBrowsable/)** 하 고 **[Intent.CategoryDefault](https://developer.xamarin.com/api/field/Android.Content.Intent.CategoryDefault/)** 제대로 할 수 웹 URI를 처리 합니다.
* **`DataScheme`** &ndash; 의도 필터 선언 해야 합니다 `http` 및/또는 `https`합니다. 이 두 개의 올바른 스키마입니다.
* **`DataHost`** &ndash; 이 Uri에서 발생 하는 도메인입니다.
* **`DataPathPrefix`** &ndash; 웹 사이트의 리소스에는 선택적 경로입니다.
* **`AutoVerify`** &ndash; `autoVerify` 특성은 응용 프로그램 및 웹 사이트 간의 관계를 확인 하는 Android에 알립니다. 이 아래 자세히 설명 되어 됩니다.

다음 예제에서는 사용 하는 방법을 보여 줍니다 합니다 [IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) 에서 링크를 처리 하기 `https://www.recipe-app.com/recipes` 들어오고 `http://www.recipe-app.com/recipes`:

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

Android는 URI에 대 한 기본 처리기로 응용 프로그램을 등록 하기 전에 웹 사이트에서 디지털 자산 파일에 대 한 의도 필터에 의해 식별 되는 모든 호스트를 확인 합니다. Android 기본 처리기로 앱을 설정 하기 전에 모든 의도 필터 확인을 전달 해야 합니다.

### <a name="creating-the-digital-assets-link-file"></a>디지털 자산 링크 파일 만들기

앱 연결 하는 android 6.0 URI에 대 한 기본 처리기로 응용 프로그램을 설정 하기 전에 응용 프로그램 및 웹 사이트 간의 연결을 확인 하는 Android 필요 합니다. 이 확인은 응용 프로그램을 처음 설치할 때 발생 합니다. 합니다 *디지털 자산 링크* 파일이 관련 webdomain(s)에서 호스트 되는 JSON 파일입니다.

> [!NOTE]
> 합니다 `android:autoVerify` 의도 필터에서 특성을 설정 해야 &ndash; 그렇지 않은 경우 Android 확인을 수행 하지 것입니다.

파일 위치에 있는 도메인의 웹 마스터에서 놓입니다 **https://domain/.well-known/assetlinks.json**합니다.

디지털 자산 파일에는 연결을 확인 하는 Android에 대 한 메타 데이터가 필요한 포함 되어 있습니다. **assetlinks.json** 파일에 다음 키-값 쌍:

* `namespace` &ndash; Android 응용 프로그램의 네임 스페이스입니다.
* `package_name` &ndash; 패키지 이름 (응용 프로그램 매니페스트에 선언 됨) Android 응용 프로그램입니다.
* `sha256_cert_fingerprints` &ndash; 서명 된 응용 프로그램의 SHA256 지문입니다. 이 가이드를 참조 하세요 [키 저장소의 MD5 또는 SHA1 서명 찾기](~/android/deploy-test/signing/keystore-signature.md) 응용 프로그램의 SHA1 지문을 가져오는 방법에 대 한 자세한 내용은 합니다.

다음 코드 조각은의 한 예로 **assetlinks.json** 단일 응용 프로그램을 사용 하 여 나열 합니다.

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

서로 다른 버전을 지원 하기 위해 둘 이상의 SHA256 지문 등록 수 또는 응용 프로그램의 작성 됩니다. 다음이 단계 **assetlinks.json** 파일이 여러 응용 프로그램을 등록 하는 예제:

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

합니다 [Google 디지털 자산 링크 웹 사이트](https://developers.google.com/digital-asset-links/tools/generator) 에 만들고 디지털 자산 파일을 테스트 하는 데 유용한 온라인 도구입니다.

### <a name="testing-app-links"></a>테스트 앱 링크

앱 링크를 구현한 후 예상 대로 작동 하는지 확인 하는 다양 한 부분을 테스트 해야 합니다.

디지털 자산 파일이 올바르게 형식이 지정 되 고이 예와 같이 Google의 디지털 자산 링크 API를 사용 하 여 호스트는 확인이 가능 합니다.

```html
https://digitalassetlinks.googleapis.com/v1/statements:list?source.web.site=
  https://<WEB SITE ADDRESS>:&relation=delegate_permission/common.handle_all_urls
```

의도 필터 제대로 구성 된 앱을 URI에 대 한 기본 처리기로 설정 되어 있는지 확인 하기 위해 수행할 수 있는 두 가지 테스트 가지가 있습니다.

1.  위에서 설명한 것 처럼 제대로 디지털 자산 파일이 호스팅됩니다. Android 모바일 응용 프로그램으로 리디렉션해야 하는 의도 전달 하는 첫 번째 테스트 합니다. Android 응용 프로그램을 시작 하 고 URL에 대 한 등록 작업을 표시 해야 합니다. 명령 프롬프트에서 입력:

    ```shell
    $ adb shell am start -a android.intent.action.VIEW \
        -c android.intent.category.BROWSABLE \
        -d "http://<domain1>/recipe/scalloped-potato"
    ```

2.  지정된 된 장치에 설치 된 응용 프로그램에 대 한 정책을 처리 하 고 기존 연결을 표시 합니다. 다음 명령을 각 사용자는 다음 정보를 사용 하 여 장치에 대 한 링크 정책의 목록을 덤프 합니다. 명령 프롬프트에 다음 명령을 입력합니다.

    ```shell
    $ adb shell dumpsys package domain-preferred-apps
    ```

    * **`Package`** &ndash; 응용 프로그램의 패키지 이름입니다.
    * **`Domain`** &ndash; 해당 웹 링크는 응용 프로그램에서 처리 되는 도메인 (공백으로 구분 됨)
    * **`Status`** &ndash; 앱에 대 한 현재 링크 처리 상태입니다. 값이 **항상** 응용 프로그램에 즉 `android:autoVerify=true` 선언 하 고 시스템 유효성 검증을 통과 했습니다. 뒤에 오지 환경 설정의 Android 시스템의 레코드를 나타내는 16 진수 숫자입니다.

    예를 들어:

    ```shell
    $ adb shell dumpsys package domain-preferred-apps

    App linkages for user 0:
    Package: com.android.vending
    Domains: play.google.com market.android.com
    Status: always : 200000002
    ```

## <a name="summary"></a>요약

이 가이드는 Android 6.0에서 작동 하는 방법을 앱 연결을 설명합니다. 다음을 지원 하 고 앱 링크 응답할 Android 6.0 응용 프로그램을 구성 하는 방법을 설명 합니다. 또한 Android 응용 프로그램에 앱 연결을 테스트 하는 방법에 대해서도 설명 합니다.


## <a name="related-links"></a>관련 링크

- [키 저장소의 MD5 또는 SHA1 서명 찾기](~/android/deploy-test/signing/keystore-signature.md)
- [작업 및 의도도](https://university.xamarin.com/classes#4)
- [AppLinks](http://applinks.org/)
- [Google 디지털 자산 링크](https://developers.google.com/digital-asset-links/)
- [문 목록 생성기 및 테스터](https://developers.google.com/digital-asset-links/tools/generator)
