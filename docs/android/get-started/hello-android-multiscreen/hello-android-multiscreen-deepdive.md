---
title: "Hello, Android 멀티스크린: 심층 분석"
description: "두 부분으로 구성된 이 가이드에서는 Hello, Android 가이드에서 만든 기본 Phoneword 응용 프로그램을 확장하여 두 번째 화면을 처리합니다. 이 과정에서 기본 Android 응용 프로그램 구성 요소를 소개합니다. Android 아키텍처에 대한 심층 분석이 포함되어 Android 응용 프로그램 구조체 및 기능을 보다 잘 이해할 수 있습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: E4150036-7760-4023-BD33-B7BDE7B7AF5B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: acced081daa9416c5c8dcf90f769aaacd584ec9a
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="hello-android-multiscreen-deep-dive"></a>Hello, Android 멀티스크린: 심층 분석

_두 부분으로 구성된 이 가이드에서는 Hello, Android 가이드에서 만든 기본 Phoneword 응용 프로그램을 확장하여 두 번째 화면을 처리합니다. 이 과정에서 기본 Android 응용 프로그램 구성 요소를 소개합니다. Android 아키텍처에 대한 심층 분석이 포함되어 Android 응용 프로그램 구조체 및 기능을 보다 잘 이해할 수 있습니다._

## <a name="hello-android-multiscreen-deep-dive"></a>Hello, Android 멀티스크린 심층 분석

[Hello, Android 멀티스크린 빠른 시작](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-quickstart.md)에서 첫 번째 멀티스크린 Xamarin.Android 응용 프로그램을 빌드하고 실행했습니다.
이제 더 복잡한 프로그램을 빌드할 수 있도록 Android 탐색 및 아키텍쳐에 대해 심층적으로 알아보겠습니다.

이 가이드에서는 Android *응용 프로그램 구성 요소*에서 소개한 대로 더 많은 고급 Android 아키텍처를 탐색합니다. *의도*를 사용하는 Android 탐색을 설명하고 Android 하드웨어 탐색 옵션을 탐색합니다. 운영 체제 및 기타 응용 프로그램을 사용하여 응용 프로그램 관계의 전체적인 관점을 개발하는 경우 Phoneword 앱에 새로운 추가 기능을 설명합니다.


## <a name="android-architecture-basics"></a>Android 아키텍처 기본 사항

[Hello, Android 심층 분석](~/android/get-started/hello-android/hello-android-deepdive.md)에서 단일 액세스 지점이 부족하기 때문에 Android 응용 프로그램이 고유한 프로그램임을 알아보았습니다. 대신, 운영 체제(또는 다른 응용 프로그램)는 응용 프로그램의 등록된 작업 중 하나를 시작합니다. 그러면 응용 프로그램에 대한 프로세스를 다시 시작합니다. Android 아키텍처에 대한 심층 분석은 Android 응용 프로그램 구성 요소와 해당 기능을 도입하여 Android 응용 프로그램을 구축하는 방법에 대한 이해로 확장됩니다.


### <a name="android-application-blocks"></a>Android 응용 프로그램 블록

Android 응용 프로그램은 개수에 관계 없이 이미지, 테마, 도우미 클래스 등의 앱 리소스와 함께 번들된 *응용 프로그램 블록*이라는 특별한 Android 클래스 컬렉션으로 이루어집니다. &ndash; 이러한 항목은 *Android 매니페스트*라는 XML 파일에 의해 조정됩니다.

일반적으로 일반 클래스에서 수행할 수 없는 작업을 수행할 수 있기 때문에 응용 프로그램 블록은 Android 응용 프로그램의 핵심을 형성합니다. 가장 중요한 두 가지는 _작업_ 및 _서비스_입니다.

-   **작업** &ndash; 작업은 사용자 인터페이스를 포함하는 화면에 해당하고 웹 응용 프로그램의 웹 페이지와 개념적으로 유사합니다. 예를 들어 뉴스 피드 응용 프로그램에서 로그인 화면은 첫 번째 작업이고, 뉴스 항목의 스크롤 가능한 목록이 두 번째 작업이고, 각 항목에 대한 세부 정보 페이지는 세 번째 작업입니다. [작업 수명 주기](~/android/app-fundamentals/activity-lifecycle/index.md) 가이드에서 작업에 대해 자세히 알아볼 수 있습니다.

-   **서비스** &ndash; Android 서비스는 장기 실행 작업을 넘겨 받고 백그라운드에서 실행하여 작업을 지원합니다. 서비스에는 사용자 인터페이스가 없고 화면에 연결되지 않은 작업을 처리하는 데 사용됩니다. &ndash; 예를 들어, 백그라운드에서 음악을 재생하거나 서버에 사진을 업로드하는 작업입니다. 서비스에 대한 자세한 내용은 [서비스 만들기](~/android/app-fundamentals/services/index.md) 및 [Android 서비스](~/android/app-fundamentals/services/index.md) 가이드를 참조하세요.


Android 응용 프로그램은 모든 형식의 블록이 아니라 한 가지 형식의 여러 블록을 사용하는 경우가 많습니다. 예를 들어 [Hello, Android 빠른 시작](~/android/get-started/hello-android/hello-android-quickstart.md)에서 Phoneword 응용 프로그램은 한 가지 작업(화면) 및 일부 리소스 파일로 구성되었습니다. 간단한 음악 플레이어 앱에는 백그라운드에서 작동될 때 음악을 재생하는 여러 작업 및 서비스가 있을 수 있습니다.

### <a name="intents"></a>의도

Android 응용 프로그램의 다른 기본 개념은 *의도*입니다.
Android는 *최소 권한의 원칙*에 맞게 디자인되었습니다. &ndash; 응용 프로그램에는 작동하는 데 필요한 블록에 대한 액세스 권한이 있고 운영 체제 또는 기타 응용 프로그램을 구성하는 블록에 대한 액세스 권한은 제한됩니다. 마찬가지로 블록은 *느슨하게 결합*됩니다. &ndash; 다른 블록에 대한 적은 기술 및 제한된 액세스이 포함되도록 디자인되었습니다(동일한 응용 프로그램의 일부인 블록인 경우에도).

통신을 위해 응용 프로그램 블록은 *의도*라고 하는 비동기 메시지를 서로 전달합니다. 의도는 수신하는 블록 및 경우에 따라 일부 데이터에 대한 정보를 포함합니다. 하나의 앱 구성 요소에서 보낸 의도는 다른 앱 구성 요소에 어떤 항목을 트리거하여 두 가지 앱 구성 요소를 바인딩하고 통신할 수 있도록 합니다. 의도를 서로 전송하여 블록에서 카메라 앱을 시작하거나, 위치 정보를 수집하거나, 화면 간에 이동하는 등 복잡한 작업을 조정할 수 있습니다.


### <a name="androidmanifestxml"></a>AndroidManifest.XML

응용 프로그램에 블록을 추가하는 경우 **Android 매니페스트**라는 특별한 XML 파일로 등록합니다. 매니페스트는 응용 프로그램의 모든 응용 프로그램 블록뿐만 아니라 버전 요구 사항, 사용 권한 및 연결된 라이브러리를 추적합니다. &ndash; 즉, 응용 프로그램을 실행하기 위해 운영 체제에서 알고 있어야 하는 모든 것입니다. **Android 매니페스트**는 지정된 작업에 적절한 조치를 제어하는 작업 및 의도도 사용합니다. Android 매니페스트의 이러한 고급 기능에 대해서는 [Android 매니페스트 사용](~/android/platform/android-manifest.md) 가이드에서 다룹니다.

단일 화면 버전의 Phoneword 응용 프로그램에서 아이콘과 같은 추가 리소스와 함께 한 가지 작업, 한 가지 의도 및 `AndroidManifest.xml`을 사용합니다. 다중 화면 버전의 Phoneword에서 추가 작업이 추가되었습니다. 의도를 사용하여 첫 번째 작업에서 시작되었습니다. 다음 섹션에서는 Android 응용 프로그램에서 탐색을 만드는 데 의도가 도움이 되는 방법을 탐색합니다.

## <a name="android-navigation"></a>Android 탐색

화면 간의 탐색에 의도가 사용되었습니다. 의도를 사용하여 Android 탐색 창에서 해당 역할을 이해하는 방법을 확인하기 위해 이 코드를 자세히 알아보겠습니다.


### <a name="launching-a-second-activity-with-an-intent"></a>의도를 사용하여 두 번째 작업 시작

Phoneword 응용 프로그램에서 의도를 사용하여 두 번째 화면(작업)을 시작합니다. 의도를 만들어 시작하고, 현재 *컨텍스트*(`this`, 현재 **컨텍스트**라고 함) 및 원하는 응용 프로그램 블록의 형식(`TranslationHistoryActivity`)을 전달합니다.

```csharp
Intent intent = new Intent(this, typeof(TranslationHistoryActivity));
```

**컨텍스트**는 응용 프로그램 환경에 대한 전역 정보의 인터페이스입니다. &ndash; 이를 통해 새로 만든 개체가 응용 프로그램의 진행 상황을 알 수 있습니다. 의도를 메시지로 가정하는 경우 메시지 받는 사람의 이름(`TranslationHistoryActivity`) 및 받는 사람 주소(`Context`)을 제공합니다.

Android에서는 간단한 데이터를 의도에 연결하는 옵션을 제공합니다(복잡한 데이터를 다르게 처리). Phoneword 예제에서 `PutStringArrayExtra`는 전화 번호 목록을 의도에 연결하는 데 사용되고 `StartActivity`는 의도의 받는 사람에 대해 호출됩니다. 완성된 코드는 다음과 같습니다.

```csharp
translationHistoryButton.Click += (sender, e) =>
{
    var intent = new Intent(this, typeof(TranslationHistoryActivity));
    intent.PutStringArrayListExtra("phone_numbers", _phoneNumbers);
    StartActivity(intent);
};
```


## <a name="additional-concepts-introduced-in-phoneword"></a>Phoneword에 도입된 추가 개념

Phoneword 응용 프로그램에는 이 가이드에서 다루지 않은 몇 가지 개념이 도입되었습니다. 이러한 개념은 다음과 같습니다.

**문자열 리소스** &ndash; Phoneword 응용 프로그램에서 `"@string/translationHistory"`의 텍스트는 `TranslationHistoryButton`로 설정되었습니다. `@string` 구문은 문자열의 값이 _문자열 리소스 파일_인 **Strings.xml**에 저장되었음을 의미합니다. `translationHistory` 문자열에 대한 값은 **Strings.xml**에 추가되었습니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="translationHistory">Call History</string>
</resources>
```

문자열 리소스 및 다른 Android 리소스에 대한 자세한 내용은 [Android 리소스 가이드](~/android/app-fundamentals/resources-in-android/index.md)를 참조하세요.

**ListView 및 ArrayAdapter** &ndash; _ListView_는 행의 스크롤 목록을 표시하는 간단한 방법을 제공하는 UI 구성 요소입니다. `ListView` 인스턴스에는 행 보기에 포함된 데이터를 사용하여 피드하는 _어댑터_가 필요합니다. 다음 코드 줄은 `TranslationHistoryActivity`의 사용자 인터페이스를 채우는 데 사용되었습니다.

```csharp
this.ListAdapter = new ArrayAdapter<string>(this, Android.Resource.Layout.SimpleListItem1, phoneNumbers);
```

Listview 및 어댑터는 이 문서의 범위를 벗어나지만 매우 포괄적인 [Listview 및 어댑터](~/android/user-interface/layouts/list-view/index.md) 가이드에서 다룹니다.
[데이터로 ListView 채우기](~/android/user-interface/layouts/list-view/populating.md)에서는 Phoneword 예제에서 수행된 것과 마찬가지로 기본 제공 `ListActivity` 및 `ArrayAdapter` 클래스를 사용하여 사용자 지정 레이아웃을 정의하지 않고 `ListView`를 만들고 채우는 방법을 다룹니다.


## <a name="summary"></a>요약

첫 번째 멀티스크린 Android 응용 프로그램을 완성한 것을 축하합니다! 이 가이드에서는 *Android 응용 프로그램 구성 요소* 및 *의도*를 소개하고 다중스크린 Android 응용 프로그램을 빌드하는 데 사용했습니다. 이제 고유한 Xamarin.Android 응용 프로그램 개발을 시작할 견고한 기반이 마련되었습니다.

다음으로 [플랫폼 간 응용 프로그램 빌드 가이드](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)에서 Xamarin을 사용하여 플랫폼 간 응용 프로그램을 빌드하는 방법을 알아봅니다.
