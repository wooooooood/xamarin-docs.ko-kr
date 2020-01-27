---
title: Xamarin과 함께 팀 도시 사용
description: 이 가이드에서는 TeamCity를 사용 하 여 모바일 응용 프로그램을 컴파일한 다음 Xamarin Test Cloud에 제출 하는 단계에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: AC2626CB-28A7-4808-B2A9-789D67899546
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 94bc775366d832e0994b8d3c74a45123ff56c13b
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725307"
---
# <a name="using-team-city-with-xamarin"></a>Xamarin과 함께 팀 도시 사용

_이 가이드에서는 TeamCity를 사용 하 여 모바일 응용 프로그램을 컴파일한 다음 Xamarin Test Cloud에 제출 하는 단계에 대해 설명 합니다._

[지속적인 통합 가이드 소개](~/tools/ci/intro-to-ci.md) 에 설명 된 대로 CI (지속적인 통합)는 품질 모바일 응용 프로그램을 개발할 때 유용한 방법입니다. 연속 통합 서버 소프트웨어에 대해 다양 한 옵션을 사용할 수 있습니다. 이 가이드는 JetBrains에서 [Teamcity](https://www.jetbrains.com/teamcity/) 에 집중 합니다.

TeamCity 설치의 여러 다른 순열이 있습니다. 다음은 일부 목록입니다.

- **Windows 서비스** –이 시나리오에서 teamcity는 windows 서비스로 부팅 될 때 시작 됩니다. IOS 응용 프로그램을 컴파일하려면 Mac 빌드 호스트와 쌍을 이루어야 합니다.

- **OS X에서 디먼 시작** – 개념적으로이는 이전 단계에서 설명한 Windows 서비스로 실행 하는 것과 매우 비슷합니다. 기본적으로 빌드는 루트 계정으로 실행 됩니다.

- **OS X의 사용자 계정** – 사용자가 로그인 할 때마다 시작 되는 사용자 계정에서 teamcity를 실행할 수 있습니다.

이전 시나리오의 경우 OS X의 사용자 계정에서 TeamCity를 실행 하는 것이 가장 간단 하 고 설정 하는 것이 가장 간단 합니다.

TeamCity를 설정 하는 데는 몇 가지 단계가 있습니다.

- **TeamCity 설치** –이 가이드에서 teamcity 설치는 다루지 않습니다. 이 가이드에서는 사용자 계정으로 TeamCity가 설치 되어 실행 되 고 있다고 가정 합니다. [Teamcity 설치](https://confluence.jetbrains.com/display/TCD8/Installation) 지침은 JetBrains에서 [teamcity 8 설명서](https://confluence.jetbrains.com/display/TCD8/TeamCity+Documentation) 에 나와 있습니다.

- **빌드 서버 준비** -이 단계는 모바일 응용 프로그램을 빌드하는 데 필요한 소프트웨어, 도구 및 인증서를 설치 하 고 배포를 위해 준비 하는 데 포함 됩니다.

- **빌드 스크립트 만들기** –이 단계는 반드시 필요한 것은 아니지만 빌드 스크립트는 응용 프로그램을 무인 방식으로 빌드하는 데 도움이 되는 유용한 도구입니다. 빌드 스크립트를 사용 하면 발생할 수 있는 빌드 문제를 해결 하는 데 도움이 되며 지속적인 통합을 사용 하지 않는 경우에도 배포를 위한 이진 파일을 만드는 일관 되 고 반복 가능한 방법을 제공 합니다.

- **TeamCity 프로젝트 만들기** – 위의 세 단계를 완료 한 후에는 소스 코드를 검색 하 고, 프로젝트를 컴파일하고, 테스트를 Xamarin Test Cloud 하는 데 필요한 모든 메타 데이터가 포함 될 teamcity 프로젝트를 만들어야 합니다.

## <a name="requirements"></a>요구 사항

[App Center 테스트](https://docs.microsoft.com/appcenter/test-cloud/) 에 대 한 경험이 필요 합니다.

TeamCity 8.1에 대 한 지식이 필요 합니다. TeamCity의 설치는이 문서의 범위를 벗어났습니다. TeamCity가 OS X Mavericks에 설치 되어 있고 루트 계정이 아닌 일반 사용자 계정으로 실행 되 고 있다고 가정 합니다.

빌드 서버는 연속 통합 전용 OS X를 실행 하는 독립 실행형 컴퓨터 여야 합니다. 이상적으로는 빌드 서버는 데이터베이스 서버, 웹 서버 또는 개발자 워크스테이션과 같은 다른 역할을 담당 하지 않습니다.

> [!IMPORTANT]
> 이 가이드에서는 Xamarin의 "헤드리스" 설치에 대해 다루지 않습니다.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="preparing-the-build-server"></a>빌드 서버 준비

빌드 서버를 구성 하는 중요 한 단계는 모바일 응용 프로그램을 빌드하기 위해 필요한 모든 도구, 소프트웨어 및 인증서를 설치 하는 것입니다. 빌드 서버에서 모바일 솔루션을 컴파일하고 모든 테스트를 실행할 수 있어야 합니다. 구성 문제를 최소화 하려면 TeamCity를 호스트 하는 동일한 사용자 계정에 소프트웨어 및 도구를 설치 해야 합니다. 다음은 필요한 항목의 목록입니다.

1. **Mac용 Visual Studio** – Xamarin.ios 및 xamarin.ios가 포함 됩니다.
2. **Xamarin 구성 요소 저장소에 로그인** –이는 선택적 단계 이며 응용 프로그램에서 Xamarin 구성 요소 저장소의 구성 요소를 사용 하는 경우에만 필요 합니다. 이 시점에서 구성 요소 저장소에 사전 로그인 하면 TeamCity 빌드에서 응용 프로그램을 컴파일하려고 할 때 문제가 발생 하지 않습니다.
3. **Xcode** – Xcode은 iOS 응용 프로그램을 컴파일하고 서명 하는 데 필요 합니다.
4. **Xcode 명령줄 도구** -이에 대 한 설명은 [r단계별 v를 사용 하 여 Ruby 업데이트](https://github.com/calabash/calabash-ios/wiki) 가이드의 설치 섹션 1 단계에 설명 되어 있습니다.
5. **서명 id & 프로 비전 프로필** – XCode를 통해 인증서 및 프로 비전 프로필을 가져옵니다. 자세한 내용은 Apple의 [서명 id 및 프로 비전 프로필 내보내기](https://developer.apple.com/library/ios/recipes/xcode_help-accounts_preferences/articles/export_signing_assets.html) 에 대 한 지침을 참조 하세요.
6. **Android keystores** – teamcity 사용자가 액세스할 수 있는 디렉터리 (예: `~/Documents/keystores/MyAndroidApp1`)에 필요한 android keystores를 복사 합니다.
7. **Calabash** – 응용 프로그램에 calabash를 사용 하 여 작성 된 테스트가 있는 경우 선택적 단계입니다. 자세한 내용은 [OS X Mavericks에서 Calabash 설치](https://github.com/calabash/calabash-ios/wiki) 가이드 및 [rguide v를 사용 하 여 Ruby 업데이트](https://github.com/calabash/calabash-ios/wiki) 가이드를 참조 하세요.

다음 다이어그램에서는 이러한 구성 요소를 모두 보여 줍니다.

![](teamcity-images/image1.png "This diagram illustrates all of these components")

모든 소프트웨어가 설치 되 면 사용자 계정에 로그인 하 여 모든 소프트웨어가 올바르게 설치 되어 작동 하는지 확인 합니다. 이렇게 하려면 솔루션을 컴파일하고 응용 프로그램을 Test Cloud에 제출 해야 합니다. 다음 섹션에 설명 된 대로 빌드 스크립트를 실행 하 여이를 크게 간소화할 수 있습니다.

## <a name="create-a-build-script"></a>빌드 스크립트 만들기

TeamCity는 모바일 응용 Test Cloud 프로그램을 컴파일 및 제출 하는 모든 측면을 처리 하는 것은 완전히 가능 하지만, 빌드 스크립트를 만드는 것이 좋습니다. 빌드 스크립트는 다음과 같은 이점을 제공 합니다.

1. **설명서** – 빌드 스크립트는 소프트웨어를 빌드하는 방법에 대 한 설명서 형태의 역할을 합니다. 이렇게 하면 응용 프로그램 배포와 관련 된 일부 "매직"이 제거 되며 개발자가 기능에 집중할 수 있습니다.
1. **반복성** – 빌드 스크립트는 응용 프로그램이 컴파일되고 배포 될 때마다 작업을 수행 하는 사람 또는 작업에 관계 없이 정확히 동일한 방법으로 수행 되도록 합니다. 이 반복 가능한 일관성은 잘못 실행 된 빌드 또는 인간 오류로 인해 증가가 발생할 수 있는 모든 문제 또는 오류를 제거 합니다.
1. **버전 관리** – 빌드 스크립트를 원본 제어 시스템에 포함할 수 있습니다. 즉, 오류 또는 잘못 된 항목이 발견 되 면 빌드 스크립트에 대 한 변경 내용을 추적, 모니터링 및 수정할 수 있습니다.
1. **환경 준비** – 빌드 스크립트는 필요한 타사 종속성을 설치 하는 논리를 포함할 수 있습니다. 그러면 적절 한 구성 요소를 사용 하 여 응용 프로그램을 빌드할 수 있습니다.

빌드 스크립트는 Powershell 파일 (Windows) 또는 bash 스크립트 (OS X의 경우) 처럼 간단할 수 있습니다. 빌드 스크립트를 만들 때 스크립팅 언어에 대해 다음과 같은 몇 가지 옵션을 선택할 수 있습니다.

- [**Rake**](https://github.com/jimweirich/rake) – Ruby를 기준으로 프로젝트를 빌드하기 위한 DSL (도메인별 언어)입니다. Rake에는 인기도와 다양 한 라이브러리 에코 시스템의 이점이 있습니다.

- [**psake**](https://github.com/psake/psake) – 소프트웨어를 빌드하기 위한 Windows Powershell 라이브러리입니다.

- [**가짜**](https://fsharp.github.io/FAKE/) – 필요에 F# 따라 기존 .net 라이브러리를 활용할 수 있도록 하는 DSL입니다.

사용 되는 스크립팅 언어는 기본 설정 및 요구 사항에 따라 달라 집니다.

> [!NOTE]
> MSBuild 또는 NAnt와 같은 XML 기반 빌드 시스템을 사용할 수 있지만 이러한 시스템에는 소프트웨어 빌드 전용 DSL의 표현을 및 유지 관리 기능이 없습니다.

### <a name="parameterizing-the-build-script"></a>빌드 스크립트 매개 변수화

소프트웨어를 구축 하 고 테스트 하는 프로세스에는 비밀을 유지 해야 하는 정보가 필요 합니다. 특히 APK를 만들려면 키 저장소의 키 저장소 및/또는 키 별칭에 대해 암호를 입력 해야 할 수 있습니다. 마찬가지로 Test Cloud에는 개발자에 게 고유한 API 키가 필요 합니다. 이러한 유형의 값은 빌드 스크립트에서 하드 코드 되어서는 안 됩니다. 대신, 빌드 스크립트에 변수로 전달 되어야 합니다.

보다 중요 한 것은 테스트 실행에 사용 해야 하 Test Cloud 장치를 식별 하는 iOS 장치 ID 또는 Android 장치 ID와 같은 값입니다. 이러한 값은 보호 해야 하는 값이 아니지만 빌드에서 빌드로 변경 될 수 있습니다.

이러한 형식의 변수를 빌드 스크립트 외부에 저장 하면 개발자와 같은 조직 내에서 빌드 스크립트를 쉽게 공유할 수 있습니다. 개발자는 빌드 서버와 정확히 동일한 스크립트를 사용할 수 있지만 고유한 keystores 및 API 키를 사용할 수 있습니다.

이러한 중요 한 값을 저장 하는 두 가지 가능한 옵션은 다음과 같습니다.

- **구성 파일** -Test Cloud API 키를 보호 하기 위해이 값은 버전 제어에 체크 인 되지 않아야 합니다. 각 컴퓨터에 대 한 파일을 만들 수 있습니다. 이 파일에서 값을 읽는 방법은 사용 된 스크립트 언어에 따라 달라 집니다.

- **환경 변수** – 컴퓨터 단위로 쉽게 설정 하 고 기본 스크립팅 언어와 독립적으로 ared 수 있습니다.

이러한 각 선택에는 장단점이 있습니다. TeamCity는 환경 변수를 사용 하 여 원활 하 게 작동 하므로이 가이드는 빌드 스크립트를 만들 때이 기술을 권장 합니다.

### <a name="build-steps"></a>빌드 단계

빌드 스크립트는 다음 단계를 수행할 수 있어야 합니다.

- **응용 프로그램 컴파일** – 올바른 프로 비전 프로필을 사용 하 여 응용 프로그램에 서명 하는 작업이 포함 됩니다.

- **응용 프로그램을 Xamarin Test Cloud에 제출** 합니다. 여기에는 적절 한 키 저장소으로 apk를 정렬 하는 서명 및 zip이 포함 됩니다.

이러한 두 단계에 대해서는 아래에서 자세히 설명 합니다.

#### <a name="compiling-a-xamarinios-application"></a>Xamarin.ios 응용 프로그램 컴파일

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

#### <a name="compiling-a-xamarinandroid-application"></a>Xamarin Android 응용 프로그램 컴파일

Android 응용 프로그램을 컴파일하려면 **xbuild** 를 사용 하거나 Windows에서 **msbuild** 를 사용 합니다.

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:SignAndroidPackage /p:Configuration=Release /path/to/android.csproj
```

Xamarin Android 응용 프로그램을 컴파일하려면 **xbuild** 에서 프로젝트를 사용 하 고 iOS 응용 프로그램 **xbuild** 를 빌드하기 위해 솔루션이 필요 합니다.

#### <a name="submitting-xamarinuitests-to-test-cloud"></a>Test Cloud에 Uitest을 제출 하는 중

Uitest는 다음 코드 조각과 같이 `test-cloud.exe` 응용 프로그램을 사용 하 여 제출 됩니다.

```bash
test-cloud.exe <path-to-apk-or-ipa-file> <test-cloud-team-api-key> --devices <device-selection-id> --assembly-dir <path-to-tests-containing-test-assemblies> --nunit-xml report.xml --user <email>
```

테스트를 실행 하면 테스트 결과가 NUnit 스타일 XML 파일 ( **report .xml**)의 형태로 반환 됩니다. TeamCity는 빌드 로그에 정보를 표시 합니다.

Test Cloud에 Uitest를 제출 하는 방법에 대 한 자세한 내용은 [Xamarin Android 앱 준비](/appcenter/test-cloud/preparing-for-upload/xamarin-android-uitest) 또는 [xamarin.ios 앱 준비](/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest)를 참조 하세요.

#### <a name="submitting-calabash-tests-to-test-cloud"></a>Test Cloud에 Calabash 테스트 제출

Calabash 테스트는 다음 코드 조각과 같이 `test-cloud` 보석을 사용 하 여 제출 됩니다.

```bash
test-cloud submit /path/to/APK-or-IPA <test-cloud-team-api-key> --devices <device-id> --user <email>
```

Android 응용 프로그램을 Test Cloud에 제출 하려면 먼저 calabash-Android를 사용 하 여 APK 테스트 서버를 다시 작성 해야 합니다.

```bash
$ calabash-android build </path/to/signed/APK>
$ test-cloud submit /path/to/APK <test-cloud-team-api-key> --devices <ANDROID_DEVICE_ID> --profile=android --config=config/cucumber.yml --pretty
```

Calabash 테스트를 제출 하는 방법에 대 한 자세한 내용은 [Test Cloud에 Calabash 테스트 제출](https://github.com/calabash/calabash-ios/wiki)에 대 한 Xamarin 가이드를 참조 하세요.

## <a name="creating-a-teamcity-project"></a>TeamCity 프로젝트 만들기

TeamCity가 설치 되어 Mac용 Visual Studio 프로젝트를 빌드할 수 있게 되 면 TeamCity에 프로젝트를 만들어 프로젝트를 빌드하고 Test Cloud에 제출 해야 합니다.

1. 웹 브라우저를 통해 TeamCity에 로그인 하 여 시작 합니다. 루트 프로젝트로 이동 합니다.

    ![루트 프로젝트로 이동](teamcity-images/image2.png "루트 프로젝트로 이동") 루트 프로젝트 아래에서 새 하위 프로젝트를 만듭니다.

    ![루트 프로젝트 아래의 루트 프로젝트로 이동 하 여 새 하위 프로젝트를 만듭니다.](teamcity-images/image3.png "루트 프로젝트 아래의 루트 프로젝트로 이동 하 여 새 하위 프로젝트를 만듭니다.")
2. 하위 프로젝트를 만든 후 새 빌드 구성을 추가 합니다.

    ![하위 프로젝트를 만든 후 새 빌드 구성을 추가 합니다.](teamcity-images/image5.png "하위 프로젝트를 만든 후 새 빌드 구성을 추가 합니다.")
3. 빌드 구성에 VCS 프로젝트를 연결 합니다. 이 작업은 버전 제어 설정 화면을 통해 수행 됩니다.

    ![이 작업은 버전 제어 설정 화면을 통해 수행 됩니다.](teamcity-images/image6.png "이 작업은 버전 제어 설정 화면을 통해 수행 됩니다.")

    생성 된 VCS 프로젝트가 없는 경우 아래에 표시 된 새 VCS 루트 페이지에서 만들 수 있습니다.

    ![생성 된 VCS 프로젝트가 없는 경우 새 VCS 루트 페이지에서 만들 수 있는 옵션이 있습니다.](teamcity-images/image7.png "생성 된 VCS 프로젝트가 없는 경우 새 VCS 루트 페이지에서 만들 수 있는 옵션이 있습니다.")

    VCS 루트가 연결 되 면 TeamCity는 프로젝트를 체크 아웃 하 고 빌드 단계를 자동 검색 합니다. TeamCity에 익숙한 경우 검색 된 빌드 단계 중 하나를 선택할 수 있습니다. 지금은 검색 된 빌드 단계를 무시 해도 됩니다.

4. 다음으로 빌드 트리거를 구성 합니다. 이렇게 하면 사용자가 리포지토리에 코드를 커밋하는 경우와 같이 특정 조건이 충족 될 때 빌드를 큐에 대기 합니다. 다음 스크린샷은 빌드 트리거를 추가 하는 방법을 보여 줍니다.

    ![이 스크린샷에서는 빌드 트리거를 추가 하는 방법을 보여 줍니다](teamcity-images/image8.png "이 스크린샷에서는 빌드 트리거를 추가 하는 방법을 보여 줍니다.") . 빌드 트리거를 구성 하는 예는 다음 스크린샷에서 볼 수 있습니다.

    ![이 스크린샷에서 빌드 트리거를 구성 하는 예제를 볼 수 있습니다.](teamcity-images/image9.png "이 스크린샷에서 빌드 트리거를 구성 하는 예제를 볼 수 있습니다.")

5. 이전 섹션인 빌드 스크립트를 매개 변수화 하 여 일부 값을 환경 변수로 저장 하는 것이 좋습니다. 이러한 변수는 매개 변수 화면을 통해 빌드 구성에 추가할 수 있습니다. 아래 스크린샷에 표시 된 것 처럼 Test Cloud API 키, iOS 장치 ID 및 Android 장치 ID에 대 한 변수를 추가 합니다.

    ![Test Cloud API 키, iOS 장치 ID 및 Android 장치 ID에 대 한 변수를 추가 합니다.](teamcity-images/image11.png "Test Cloud API 키, iOS 장치 ID 및 Android 장치 ID에 대 한 변수를 추가 합니다.")

6. 최종 단계는 빌드 스크립트를 호출 하 여 응용 프로그램을 컴파일하고 응용 프로그램을 Test Cloud에 큐에 추가 하는 빌드 단계를 추가 하는 것입니다. 다음 스크린샷은 Rakefile을 사용 하 여 응용 프로그램을 빌드하는 빌드 단계의 예입니다.

    ![이 스크린샷은 Rakefile을 사용 하 여 응용 프로그램을 빌드하는 빌드 단계의 예입니다.](teamcity-images/image12.png "이 스크린샷은 Rakefile을 사용 하 여 응용 프로그램을 빌드하는 빌드 단계의 예입니다.")

7. 이때 빌드 구성이 완료 됩니다. 빌드를 트리거하여 프로젝트가 제대로 구성 되어 있는지 확인 하는 것이 좋습니다. 이 작업을 수행 하는 좋은 방법은 리포지토리에 작은 사소한 변경을 커밋하는 것입니다. TeamCity는 커밋을 검색 하 고 빌드를 시작 해야 합니다.

8. 빌드가 완료 되 면 빌드 로그를 검사 하 고 주의가 필요한 빌드와 관련 된 문제나 경고가 있는지 확인 합니다.

## <a name="summary"></a>요약

이 가이드에서는 TeamCity를 사용 하 여 Xamarin 모바일 응용 프로그램을 빌드한 다음 Test Cloud에 제출 하는 방법에 대해 설명 했습니다. 빌드 프로세스를 자동화 하는 빌드 스크립트를 만드는 방법을 설명 했습니다. 빌드 스크립트는 응용 프로그램을 컴파일하고 Test Cloud 전송 하 고 결과를 기다리는 작업을 담당 합니다.

그런 다음 개발자가 코드를 커밋하고 빌드 스크립트를 호출할 때마다 빌드를 큐에 대기 시키는 프로젝트를 TeamCity에 만드는 방법을 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [Xamarin Android 앱 준비](/appcenter/test-cloud/preparing-for-upload/xamarin-android-uitest)
- [Xamarin.ios 앱 준비](/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest)
- [TeamCity 설치 및 구성](https://confluence.jetbrains.com/display/TCD8/Installing+and+Configuring+the+TeamCity+Server)
