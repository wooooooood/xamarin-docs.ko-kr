---
title: Xamarin을 사용한 팀 도시를 사용 하 여
description: 이 가이드를 모바일 응용 프로그램을 컴파일하고 다음에 전송 하려면 Xamarin 테스트 클라우드 TeamCity 사용과 관련 된 단계를 설명 합니다.
ms.prod: xamarin
ms.assetid: AC2626CB-28A7-4808-B2A9-789D67899546
author: topgenorth
ms.author: toopge
ms.date: 03/23/2017
ms.openlocfilehash: e7279c03c730e95f211b555e5b832942c19ea8aa
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935417"
---
# <a name="using-team-city-with-xamarin"></a>Xamarin을 사용한 팀 도시를 사용 하 여

_이 가이드를 모바일 응용 프로그램을 컴파일하고 다음에 전송 하려면 Xamarin 테스트 클라우드 TeamCity 사용과 관련 된 단계를 설명 합니다._

에 설명 된 대로 [연속 통합 소개](~/tools/ci/intro-to-ci.md) 가이드 CI (연속 통합)은 유용한 기능 품질 모바일 응용 프로그램을 개발 하는 경우. 연속 통합 서버 소프트웨어에 대 한 많은 실행 가능한 옵션이 없습니다 이 가이드는 중점 [TeamCity](http://www.jetbrains.com/teamcity/) JetBrains에서 합니다.

TeamCity 설치의 다른 순열 여러 가지가 있습니다. 다음은이 중 일부의 목록입니다.

- **Windows 서비스** –이 시나리오에서는 TeamCity 시작 되는 Windows 서비스로 Windows 부팅 하는 경우. IOS 응용 프로그램을 컴파일하는 데 Mac 빌드 호스트와 이루어야 합니다.

- **OS X에는 디먼을 시작** – 개념적으로이 이전 단계에서 설명 하는 Windows 서비스로 실행 되 고 매우 비슷합니다. 기본적으로 빌드를 루트 계정으로 실행 됩니다.

- **OS X에 사용자 계정이** – TeamCity 시작 될 때마다 사용자가 로그인 하는 사용자 계정으로 실행할 수 있습니다.

간단 하 고 설치 프로그램에 가장 쉬운 이전 시나리오의는 OS X에서 TeamCity 사용자 계정으로 실행 합니다.

TeamCity 설정와 관련 된 여러 단계가 있습니다.

- **TeamCity 설치** – TeamCity 설치가이 가이드에서 다루지 않습니다. 이 가이드 TeamCity 설치 및 사용자 계정으로 실행 되어 있다고 가정 합니다. 에 대 한 지침 [TeamCity 설치](http://confluence.jetbrains.com/display/TCD8/Installation) 에서 찾을 수는 [TeamCity 8 설명서](http://confluence.jetbrains.com/display/TCD8/TeamCity+Documentation) JetBrains 여 합니다.

- **빌드 서버 준비** –이 단계에서는 필요한 소프트웨어 도구, 설치 하는 데 필요한 인증서 및 모바일 응용 프로그램을 빌드하고 배포 준비 합니다.

- **빌드 스크립트를 만드는** –이 단계가 반드시 필요 하지 않습니다. 하지만 빌드 스크립트는 무인된 응용 프로그램을 구축 하는 데 유용한 지원 합니다. 빌드 스크립트를 사용 하 여 발생할 수 하 고 연속 통합은 실행 되지 하는 경우에 배포에 대 한 이진 파일을 만들를 일관 되 고 반복 가능한 방식으로 제공 하는 빌드 문제를 해결 하는 데 도움이 됩니다.

- **A TeamCity 프로젝트 만들기** -메타 데이터를 모두 포함 될 TeamCity 프로젝트를 만들어야 앞의 세 단계 완료 되 면 소스 코드를 가져올 프로젝트를 컴파일하고 테스트 Xamarin 테스트 클라우드를 제출 하는 데 필요한 합니다.

## <a name="requirements"></a>요구 사항

경험이 [Xamarin 테스트 클라우드](https://developer.xamarin.com/guides/testcloud) 가 필요 합니다.

TeamCity 8.1 친근 함이 필요 합니다. TeamCity 설치가 문서의 범위를 벗어납니다. TeamCity OS X Mavericks에 설치 되어 있고 일반 사용자 계정으로 루트 계정이 아니라 실행 중인 것으로 가정 됩니다.

빌드 서버에는 연속 통합 하도록 지정 된 OS X를 실행 하는 독립 실행형 컴퓨터 여야 합니다. 이상적으로 빌드 서버 데이터베이스 서버, 웹 서버 또는 개발자 워크스테이션 등의 다른 역할을 담당 되지 않습니다.

> [!IMPORTANT]
> 이 가이드에서는 Xamarin 설치 하는 "헤드리스" 다루지 않습니다.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="preparing-the-build-server"></a>빌드 서버 준비

빌드 서버 구성에서 중요 한 단계가 모두 필요한 도구, 소프트웨어 및 모바일 응용 프로그램을 빌드하는 인증서를 설치 하는 것입니다. 빌드 서버를 모바일 솔루션을 컴파일하고 모든 테스트를 실행할 수 유용 합니다. 구성 문제를 최소화 하려면 소프트웨어 및 도구 TeamCity 호스팅하는 동일한 사용자 계정에 설치 되어야 합니다. 다음은 필요한 것의 목록입니다.

1. **Mac 용 visual Studio** – Xamarin.iOS 및 Xamarin.Android 포함 됩니다.
2. **Xamarin 구성 요소 저장소에 대 한 로그인** –이 단계는 선택적 단계 이며만 응용 프로그램 Xamarin 구성 요소 저장소에서 구성 요소를 사용 하는 경우에 필요 합니다. 사전에 로그인 할 구성 요소 저장소가 시점에서 TeamCity 빌드 응용 프로그램을 컴파일할 하려고 할 때 문제가 되지 것입니다.
3. **Xcode** – Xcode는 iOS 응용 프로그램을 컴파일 및 서명 하는 데 필요 합니다.
4. **Xcode 명령줄 도구** –의 설치 섹션의 1 단계에서이 내용이 설명 되어는 [rbenv와 업데이트 Ruby](https://developer.xamarin.com/guides/testcloud/calabash/updating-ruby-using-rbenv/) 가이드입니다.
5. **Id 및 프로비저닝 프로필 서명** – 인증서를 가져오고 XCode 통해 프로필을 프로 비전 합니다. Apple의 가이드를 참조 하십시오 [내보내기 서명 Id 및 프로비저닝 프로필](https://developer.apple.com/library/ios/recipes/xcode_help-accounts_preferences/articles/export_signing_assets.html) 자세한 세부 정보에 대 한 합니다.
6. **Android 키 저장소** – 필요한 Android 키 저장소 TeamCity 사용자에 게 있는지에 대 한 액세스, 즉 디렉터리로 복사 `~/Documents/keystores/MyAndroidApp1`합니다.
7. **Calabash** – 응용 프로그램에 테스트 Calabash를 사용 하 여 작성 하는 경우이 단계는 선택 사항입니다. 참조 하십시오는 [OS X Mavericks에서 Calabash 설치](https://developer.xamarin.com/guides/testcloud/calabash/osx-installation/) 가이드 및 [rbenv와 업데이트 Ruby](https://developer.xamarin.com/guides/testcloud/calabash/updating-ruby-using-rbenv/) 대 한 자세한 내용은 가이드입니다.

다음 다이어그램에서는 이러한 구성 요소를 모두 수행 합니다.

![](teamcity-images/image1.png "이 다이어그램에서는 이러한 모든 구성이 요소")

모든 소프트웨어를 설치한 후에 사용자 계정에 로그인 하 고 모든 소프트웨어가 제대로 설치 되어 작동 하 고 확인 합니다. 솔루션을 컴파일하고 제출 하는 테스트 클라우드로 응용 프로그램을 참여 시켜야 합니다. 이 수 수 크게 간소화 빌드 스크립트를 실행 하 여 다음 섹션에 설명 된 대로 합니다.

## <a name="create-a-build-script"></a>빌드 스크립트 만들기

TeamCity 컴파일 및 테스트 클라우드로 모바일 응용 프로그램 자체로 제출의 모든 측면을 처리 하기에 대 한 완전히 수는 있지만 빌드 스크립트를 작성 하는 것이 좋습니다. 빌드 스크립트에는 다음과 같은 이점을 제공합니다.

1. **설명서** – 소프트웨어를 빌드하는 방식에 대 한 설명서는 형태의로 빌드 스크립트를 사용 합니다. 이 "매직" 기능에 집중할 수 있게 해 주는 응용 프로그램 배포와 연결 된 하는 중 일부를 제거 합니다.
1. **반복성** – 빌드 스크립트를 선택 하면 누구 인지 또는 작업의 용도 관계 없이 정확히 동일한 방법으로 수행 것 될 때마다 응용 프로그램을 컴파일 및 배포 합니다. 이 반복 가능한 일관성 문제 또는 잘못 실행 된 빌드 때문에 영향이 미칠 수 있는 오류 또는 실수로 제거 합니다.
1. **버전 관리** -소스 제어 시스템에는 빌드 스크립트를 포함할 수 있습니다. 즉, 빌드 스크립트에 대 한 변경 수 추적, 모니터링 하 고을 오류 또는 잘못 된 발견 되 면 수정 했습니다.
1. **환경 준비** – 빌드 스크립트에서 모든 필수 타사 종속성을 설치 하는 논리를 포함할 수 있습니다. 이렇게 하면 응용 프로그램은 적절 한 구성 요소와 함께 작성 됩니다.

빌드 스크립트는 Windows에 Powershell 파일 또는 (OS X)에 대 한 bash 스크립트 처럼 간단할 수 있습니다. 빌드 스크립트를 만들 때 가지 스크립팅 언어에 대 한 몇 가지 방법이 있습니다.

- [**Rake** ](https://github.com/jimweirich/rake) – Ruby 기반 프로젝트를 빌드하기 위한는 도메인 특정 언어 DSL ()입니다. Rake는 라이브러리의 풍부한 생태계와 인기도 활용할에 있습니다.

- [**psake** ](https://github.com/psake/psake) –이 소프트웨어를 작성 하기 위한 Windows Powershell 라이브러리

- [**가짜** ](http://fsharp.github.io/FAKE/) – 필요한 경우에 기존.NET 라이브러리를 활용 하 여 수 있는 F #에서 기반으로 하는 DSL입니다.

스크립팅 언어에 사용 되는 기본 설정 및 요구 사항에 따라 달라 집니다. [TaskyPro Calabash](https://github.com/xamarin/test-cloud-samples/tree/master/TaskyPro/TaskyPro-Calabash) Rake로 사용 하는 예제를 포함 하는 예제는 [빌드 스크립트](https://github.com/xamarin/test-cloud-samples/blob/master/TaskyPro/TaskyPro-Calabash/Rakefile)합니다.

> [!NOTE]
> 표현성 및 소프트웨어를 작성 하도록 지정 된 DSL의 유지 관리 용이성 MSBuild 또는 코 버 넌 트, 하지만 이러한 부족 같은 XML 기반 빌드 시스템을 사용 하는 것이 불가능 합니다.

### <a name="parameterizing-the-build-script"></a>빌드 스크립트를 매개 변수화

빌드 및 소프트웨어를 테스트 하는 과정에는 기밀로 유지 해야 하는 정보가 필요 합니다. 특히는 APK 만드는 키 저장소 및/또는 키 저장소에 키 별칭에 대 한 암호가 필요할 수 있습니다. 마찬가지로, 테스트 클라우드를 개발자에 게 고유한 API 키가 필요 합니다. 이러한 유형의 값 되지 않아야 하드 빌드 스크립트에 코딩 합니다. 대신이 빌드 스크립트 변수로 전달 되어야 합니다.

비교적 중요 하지 않은 실행 되는 iOS 장치 ID 또는 클라우드 테스트에 사용 해야 하는 장치에서 테스트 하는 기능을 식별 하는 Android 장치 ID와 같은 값입니다. 보호 되어야 하는 값 이들은 있지만 빌드에 변경할 수 있습니다.

이러한 유형의 외부 빌드 스크립트의 변수를 저장 하면 쉽게 예를 들어 개발자와 조직 내에서 빌드 스크립트를 공유할 수 있습니다. 개발자 빌드 서버 똑같은 스크립트를 사용할 수 있습니다 하지만 자신의 키 저장소 및 API 키를 사용할 수 있습니다.

이러한 중요 한 값을 저장 하기 위한 두 가지 가능한 옵션에는

- **구성 파일** –이 값을 버전 제어에 체크 인할 수 해야 테스트 클라우드 API 키를 보호 합니다. 각 컴퓨터에 대 한 파일을 만들 수 있습니다. 값이이 파일에서 읽은 방법을 사용 하는 스크립팅 언어에 따라 달라 집니다.

- **환경 변수** – 컴퓨터 별로 및 기본 스크립트 언어의 ared 독립적인에서 쉽게 설정할 수 있습니다 이러한.

이 선택한 각 단점이 있습니다. TeamCity이이 가이드는 빌드 스크립트를 만들 때이 기술을 추천 하도록 환경 변수에서 원활 하 게 작동 합니다.

### <a name="build-steps"></a>빌드 단계

빌드 스크립트에서 다음 단계를 수행할 수 있어야 합니다.

- **응용 프로그램을 컴파일할** -여기에 응용 프로그램에서 올바른 프로비저닝 프로필을 서명 합니다.

- **Xamarin 테스트 클라우드로 응용 프로그램을 제출** –에 서명 하 고 적절 한 키 저장소와 APK 맞춤 zip이 포함 됩니다.

이러한 두 단계는 아래에서 자세히 설명 되어 있습니다.

#### <a name="compiling-a-xamarinios-application"></a>Xamarin.iOS 응용 프로그램 컴파일

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

#### <a name="compiling-a-xamarinandroid-application"></a>Xamarin.Android 응용 프로그램 컴파일

Android 응용 프로그램을 컴파일하려면를 사용 하 여 **xbuild** (또는 **msbuild** Windows에서):

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:SignAndroidPackage /p:Configuration=Release /path/to/android.csproj
```

Xamarin Android 응용 프로그램을 컴파일하려면 다음에 유의 **xbuild** iOS 응용 프로그램을 빌드하는 프로젝트를 사용 하 여 **xbuild** 는 솔루션이 필요 합니다.

#### <a name="submitting-xamarinuitests-to-test-cloud"></a>Xamarin.UITests 테스트 클라우드로 전송

사용 하 여 UITests 제출 되는 `test-cloud.exe` 다음 코드 조각에 나와 있는 것 처럼 응용 프로그램:

```bash
test-cloud.exe <path-to-apk-or-ipa-file> <test-cloud-team-api-key> --devices <device-selection-id> --assembly-dir <path-to-tests-containing-test-assemblies> --nunit-xml report.xml --user <email>
```

테스트 결과 테스트를 실행 하는 경우 라는 NUnit 스타일 XML 파일의 형태로 반환 됩니다 **아래에 있는 report.xml**합니다. TeamCity 빌드 로그에 정보를 표시 됩니다.

UITests 테스트 클라우드를 제출 하는 방법에 대 한 자세한 내용은이 가이드를 참조에 [업로드에 대 한 준비 Xamarin.UITests](/appcenter/test-cloud/preparing-for-upload/uitest/)합니다.

#### <a name="submitting-calabash-tests-to-test-cloud"></a>Calabash 테스트 클라우드 테스트를 제출 합니다.

Calabash 테스트를 사용 하 여 제출 되는 `test-cloud` 보석, 다음 코드 조각에 나와 있는 것 처럼:

```bash
test-cloud submit /path/to/APK-or-IPA <test-cloud-team-api-key> --devices <device-id> --user <email>
```
테스트 클라우드로 Android 응용 프로그램을 제출 하려면 먼저 calabash android를 사용 하 여 APK 테스트 서버를 다시 작성 해야 해야 합니다.

```bash
$ calabash-android build </path/to/signed/APK>
$ test-cloud submit /path/to/APK <test-cloud-team-api-key> --devices <ANDROID_DEVICE_ID> --profile=android --config=config/cucumber.yml --pretty
```
Calabash 테스트 등록에 대 한 자세한 내용은 설명서를 참조 하십시오 Xamarin에서 [Calabash 테스트를 테스트 클라우드를 제출](https://developer.xamarin.com/guides/testcloud/calabash/working-with/submitting-tests-to-xamarin-test-cloud/)합니다.

## <a name="creating-a-teamcity-project"></a>TeamCity 프로젝트 만들기

TeamCity가 설치 하 고 Mac 용 Visual Studio 프로젝트를 빌드할 수, 속도가 프로젝트를 빌드 및 테스트 클라우드로 제출 하려면 TeamCity에서 프로젝트를 만듭니다.

1. 웹 브라우저를 통해 TeamCity에 로그인 하 여 시작 합니다. 루트 프로젝트로 이동 합니다.

    ![루트 프로젝트로 이동](teamcity-images/image2.png "Root 프로젝트를 탐색") 루트 프로젝트 아래 새 하위 프로젝트를 만듭니다.

    ![루트 프로젝트 아래에서 루트 프로젝트를 탐색, 새 하위 프로젝트를 만들](teamcity-images/image3.png "Navigate 루트 프로젝트 아래에서 루트 프로젝트에 새 하위 프로젝트 만들기")
2. 하위 프로젝트를 만든 후에 새 빌드 구성을 추가 합니다.

    ![하위 프로젝트를 만든 후 새 빌드 구성을 추가](teamcity-images/image5.png "하위 프로젝트를 만든 후 새 빌드 구성을 추가")
3. VC 프로젝트 빌드 구성에 연결 합니다. 이 작업은 버전 제어 설정을 화면을 통해 수행 됩니다.

    ![버전 제어 설정을 화면을 통해 이렇게](teamcity-images/image6.png "버전 제어 설정을 화면을 통해 이렇게")

    만든 VCS 프로젝트가 없습니다, 있는지를 아래 표시 된 새 VCS 루트 페이지에서 하나를 만드는 옵션:

    ![새 VCS 루트 페이지에서 하나를 만들 수 있는 옵션이 있는 만든 VCS 프로젝트가 없으면](teamcity-images/image7.png "새 VCS 루트 페이지에서 하나를 만들 수 있는 옵션이 있는 VCS 프로젝트가 만든 경우")

    VCS 루트, 한번 자동으로 시도 TeamCity가 체크 아웃 프로젝트 빌드 단계를 검색 합니다. TeamCity에 익숙한 경우 선택할 수 있습니다는 검색 된 빌드 단계 중 하나입니다. 지금은 검색 된 빌드 단계에는 무시 해도 됩니다.

4. 그런 다음 빌드 트리거를 구성 합니다. 이 값은 사용자 저장소에 코드를 커밋합니다 하는 경우 등 특정 조건이 충족 되는 경우 빌드를 대기 됩니다. 다음 스크린 샷에서 빌드 트리거를 추가 하는 방법을 보여 줍니다.

    ![이 스크린샷은 빌드 트리거를 추가 하는 방법을 보여 줍니다](teamcity-images/image8.png "이 스크린샷은 빌드 트리거를 추가 하는 방법을 보여 줍니다") 빌드 트리거 구성의 예는 다음 스크린 샷에서에서 볼 수 있습니다.

    ![이 스크린 샷에서 빌드 트리거 구성의 예를 볼 수](teamcity-images/image9.png "빌드 트리거 구성의 예는이 스크린샷에서에서 볼 수 있습니다")

5. 빌드 스크립트를 매개 변수화의 이전 섹션에서 제안 환경 변수로 일부 값을 저장 합니다. 이러한 변수는 매개 변수 화면을 통해 빌드 구성에 추가할 수 있습니다. 테스트 클라우드 API 키, iOS 장치 ID 및 아래 스크린샷에 표시 된 대로 Android 장치 ID에 대 한 변수를 추가 합니다.

    ![테스트 클라우드 API 키, iOS 장치 ID 및 Android 장치 ID에 대 한 변수를 추가](teamcity-images/image11.png "테스트 클라우드 API 키, iOS 장치 ID 및 Android 장치 ID에 대 한 변수를 추가 합니다.")

6. 마지막 단계는 응용 프로그램 및 테스트 클라우드로 응용 프로그램 큐에 삽입 하는 빌드 스크립트를 호출 하는 빌드 단계를 추가 하는 것입니다. 다음 스크린 샷에서 응용 프로그램을 빌드하는 Rakefile를 사용 하는 빌드 단계의 예시:

    ![이 스크린샷에서 응용 프로그램을 빌드하는 Rakefile를 사용 하는 빌드 단계의 예로](teamcity-images/image12.png "이 스크린샷은 응용 프로그램을 빌드하는 Rakefile를 사용 하는 빌드 단계의 예")

7. 이 시점에서 빌드 구성이 완료 되었습니다. 프로젝트가 올바르게 구성 되어 있는지 확인 하려면 빌드를 트리거 하는 것이 좋습니다. 이렇게 하려면 저장소에 작은, 불필요 한 변경 내용을 커밋하는 것이 좋습니다. TeamCity는 커밋이 감지 하 고 빌드를 시작 해야 합니다.

8. 빌드가 완료 되 면 빌드 로그를 검사 한 문제 또는 주의가 필요한 빌드와 경고 확인 하십시오.

## <a name="summary"></a>요약

이 가이드는 TeamCity Xamarin 모바일 응용 프로그램을 구축 하 고 다음에 전송 하려면 테스트 클라우드를 사용 하는 방법을 설명 합니다. 빌드 프로세스를 자동화 하는 빌드 스크립트를 만드는 것을 설명 합니다. 빌드 스크립트 응용 프로그램의 컴파일, 테스트 클라우드로 전송 하 고 결과 기다리는는 담당 합니다.

다음에서는에서 TeamCity 합니다 빌드를 큐 대기는 개발자는 코드를 커밋합니다 될 때마다 합니다 빌드 스크립트를 호출 하는 프로젝트를 만드는 방법을 다룹니다.

## <a name="related-links"></a>관련 링크

- [Xamarin.UITests fpr 업로드 준비](/appcenter/test-cloud/preparing-for-upload/uitest/)
- [설치 및 구성 TeamCity](http://confluence.jetbrains.com/display/TCD8/Installing+and+Configuring+the+TeamCity+Server)
