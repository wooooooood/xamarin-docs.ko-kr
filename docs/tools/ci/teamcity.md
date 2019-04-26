---
title: Xamarin을 사용한 팀 도시를 사용 하 여
description: 이 가이드에서는 모바일 응용 프로그램을 컴파일 및 Xamarin Test Cloud에 제출할 수 TeamCity 사용과 관련 된 단계를 설명 합니다.
ms.prod: xamarin
ms.assetid: AC2626CB-28A7-4808-B2A9-789D67899546
author: lobrien
ms.author: laobri
ms.date: 03/23/2017
ms.openlocfilehash: a3cb79f33b64d933aa8ab4d3555479cc16238992
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61218948"
---
# <a name="using-team-city-with-xamarin"></a>Xamarin을 사용한 팀 도시를 사용 하 여

_이 가이드에서는 모바일 응용 프로그램을 컴파일 및 Xamarin Test Cloud에 제출할 수 TeamCity 사용과 관련 된 단계를 설명 합니다._

에 설명 된 대로 합니다 [연속 통합 소개](~/tools/ci/intro-to-ci.md) 가이드, CI (지속적인 통합)은 유용한 고품질의 모바일 응용 프로그램을 개발 하는 경우. 연속 통합 서버 소프트웨어에 대 한 실행 가능한 옵션이 여러 가지 이 가이드에서 중점적 [TeamCity](http://www.jetbrains.com/teamcity/) JetBrains에서.

TeamCity 설치의 다른 순열 여러 가지가 있습니다. 다음은 그 중 일부의 목록입니다.

- **Windows 서비스** –이 시나리오에서는 TeamCity 시작 되는 Windows 서비스로 Windows 부팅 하는 경우. IOS 응용 프로그램을 컴파일하려면 Mac 빌드 호스트를 사용 하 여 쌍으로 연결할 수 있어야 합니다.

- **OS X에서 디먼 시작** – 개념적으로이 이전 단계에서 설명한 Windows 서비스로 실행 매우 비슷합니다. 기본적으로 빌드를 루트 계정으로 실행 됩니다.

- **OS X에서 사용자 계정을** – TeamCity 시작 될 때마다 사용자가 로그인 하는 사용자 계정으로 실행 하는 것이 가능 합니다.

간단 하 고 설치 하는 가장 쉬운 방법은 이전 시나리오의은 OS X에서 TeamCity 사용자 계정으로 실행 합니다.

TeamCity 설정 연관 가지 몇 가지 단계가 있습니다.

- **TeamCity 설치** – TeamCity 설치가이 가이드에서는 다루지 않습니다. 이 가이드 TeamCity는 설치 하 고 사용자 계정으로 실행 하는 것으로 가정 합니다. 에 대 한 지침 [TeamCity 설치](http://confluence.jetbrains.com/display/TCD8/Installation) 에서 찾을 수 있습니다 합니다 [TeamCity 8 설명서](http://confluence.jetbrains.com/display/TCD8/TeamCity+Documentation) jetbrains 합니다.

- **빌드 서버를 준비 하 고** –이 단계에서는 필요한 소프트웨어, 도구, 설치 하는 데 필요한 인증서 및 모바일 응용 프로그램을 빌드하고 배포 하도록 준비 합니다.

- **만들기, 빌드 스크립트는** –이 단계가 꼭 필요 하지 않습니다. 하지만 빌드 스크립트는 무인된 응용 프로그램 구축에 유용한 지원 합니다. 빌드 스크립트를 사용 하 여 발생할 수 있습니다 하 고 지속적인 통합은 경험 하지 하는 경우에 일관 되 고 반복 가능한 배포에 대 한 이진 파일을 만드는 방법을 제공 하는 빌드 문제를 해결 하는 데 도움이 됩니다.

- **TeamCity 프로젝트 만들기** – 모든 메타 데이터가 있는 TeamCity 프로젝트를 만들어야 이전의 세 단계를 완료 되 면 소스 코드를 가져올 프로젝트를 컴파일하고 테스트 Xamarin Test Cloud를 제출 하는 데 필요한 합니다.

## <a name="requirements"></a>요구 사항

경험이 [Xamarin Test Cloud](https://developer.xamarin.com/guides/testcloud) 필요 합니다.

TeamCity 8.1 지식이 필요합니다. TeamCity 설치가 문서의 범위를 벗어납니다. TeamCity OS X Mavericks에 설치 하 고 일반 사용자 계정으로 루트 계정이 아닌 실행 중인 간주 됩니다.

빌드 서버에는 연속 통합을 전용 OS X를 실행 하는 독립 실행형 컴퓨터에 있어야 합니다. 이상적으로 빌드 서버는 데이터베이스 서버, 웹 서버 또는 개발자 워크스테이션 등의 다른 역할을 담당 되지 않습니다.

> [!IMPORTANT]
> 이 가이드에서는 Xamarin 설치 하는 "헤드리스" 다루지 않습니다.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="preparing-the-build-server"></a>빌드 서버 준비

빌드 서버 구성에서 중요 한 단계 모든 필요한 도구, 소프트웨어 및 모바일 응용 프로그램을 빌드하는 인증서를 설치 하는 것입니다. 빌드 서버에 모바일 솔루션을 컴파일 및 테스트를 실행 하는 일을 할 수는 반드시 합니다. 구성 문제를 최소화 하려면 소프트웨어 및 도구를 TeamCity를 호스트 하는 동일한 사용자 계정에 설치 되어야 합니다. 다음은 필요한 것의 목록입니다.

1. **Mac 용 visual Studio** – Xamarin.iOS 및 Xamarin.Android 포함 됩니다.
2. **Xamarin Component Store에 로그인** –이 선택적 단계 이며만 Xamarin 구성 요소 저장소에서 응용 프로그램 구성 요소를 사용 하는 경우 필요 합니다. 사전 로그인 구성 요소 저장소가 시점에서 TeamCity 빌드 응용 프로그램을 컴파일 하려고 할 때 문제가 되지 것입니다.
3. **Xcode** – Xcode는 iOS 응용 프로그램을 컴파일 및 로그인 해야 합니다.
4. **Xcode 명령줄 도구** –이의 설치 섹션의 1 단계에서 설명 합니다 [rbenv 사용 하 여 Ruby 업데이트](https://developer.xamarin.com/guides/testcloud/calabash/updating-ruby-using-rbenv/) 가이드입니다.
5. **서명 Id 및 프로 비전 프로필** – 인증서를 가져오고 XCode 통해 프로필을 프로 비전 합니다. Apple 가이드를 참조 하세요 [내보내기 서명 Id 및 프로 비전 프로필](https://developer.apple.com/library/ios/recipes/xcode_help-accounts_preferences/articles/export_signing_assets.html) 대 한 자세한 내용은 합니다.
6. **Android 키 저장소** – 필요한 Android 키 저장소 TeamCity 사용자가 액세스할 수, 즉 디렉터리로 복사 `~/Documents/keystores/MyAndroidApp1`합니다.
7. **Calabash** – 응용 프로그램에 테스트 Calabash를 사용 하 여 작성 하는 경우이 단계는 선택 사항입니다. 참조 하세요 합니다 [OS X Mavericks에서 Calabash 설치](https://developer.xamarin.com/guides/testcloud/calabash/osx-installation/) 가이드 및 [rbenv 사용 하 여 Ruby 업데이트](https://developer.xamarin.com/guides/testcloud/calabash/updating-ruby-using-rbenv/) 자세한 가이드입니다.

다음 다이어그램에서는 이러한 모든 구성이 요소를 보여 줍니다.

![](teamcity-images/image1.png "이 다이어그램에 이러한 모든 구성이 요소")

모든 소프트웨어를 설치한 후, 사용자 계정에 로그인 하 고이 제대로 설치 되어 있는 모든 소프트웨어 및 작동을 확인 합니다. 이 솔루션을 컴파일 및 Test Cloud에 응용 프로그램을 제출 포함 해야 합니다. 이 상당히 간단해 집니다 빌드 스크립트를 실행 하 여 다음 섹션에 설명 된 대로 합니다.

## <a name="create-a-build-script"></a>빌드 스크립트 만들기

컴파일 및 Test Cloud에 대 한 모바일 응용 프로그램을 자체적으로 제출의 모든 측면을 처리 하는 TeamCity도 가능 이지만 것이 좋습니다는 빌드 스크립트를 만듭니다. 빌드 스크립트는 다음과 같은 이점이 있습니다.

1. **설명서** – 소프트웨어 빌드 방법에 대 한 설명서의 형태로 빌드 스크립트를 제공 합니다. 이 기능에 집중할 수 있게 해 주는 연결 된 응용 프로그램을 배포 하는 "매직" 중 일부를 제거 합니다.
1. **반복성** – 빌드 스크립트를 선택 하면 작업을 수행 또는 사용자에 관계 없이 정확히 동일한 방법으로 수행이 될 때마다 응용 프로그램 컴파일 및 배포 합니다. 이 반복 가능한 일관성 문제 또는 잘못 실행된 된 빌드 때문에 중요 한 점은 웹 수 있는 오류 또는 인적 오류를 제거 합니다.
1. **버전 관리** -소스 제어 시스템에 빌드 스크립트를 포함할 수 있습니다. 즉, 수 빌드 스크립트의 변경 내용은 추적, 모니터링 및 오류 나 부정확 한 내용이 없으면 수정 될 수 있습니다.
1. **환경 준비** – 빌드 스크립트를 모든 필요한 타사 종속성을 설치 하는 논리를 포함할 수 있습니다. 이렇게 하면는 응용 프로그램은 적절 한 구성 요소를 사용 하 여 빌드됩니다.

빌드 스크립트 (Windows)에 Powershell 파일 또는 OS X) (에서 bash 스크립트 처럼 간단할 수 있습니다. 빌드 스크립트를 만들 때 가지 스크립팅 언어에 대 한 몇 가지 방법이 있습니다.

- [**Rake** ](https://github.com/jimweirich/rake) -Ruby를 기반으로 프로젝트를 빌드하기 위한 도메인 특정 언어 (DSL)입니다. Rake는 인기도 활용 및 라이브러리의 풍부한 에코 시스템에 있습니다.

- [**psake** ](https://github.com/psake/psake) – 소프트웨어 구축을 위한 Windows Powershell 라이브러리

- [**모조** ](http://fsharp.github.io/FAKE/) –이 DSL을 기반으로 F# 필요한 경우 기존.NET 라이브러리를 활용할 수 있습니다.

스크립팅 언어를 사용 하는 기본 설정 및 요구 사항에 따라 달라 집니다. 합니다 [TaskyPro Calabash](https://github.com/xamarin/test-cloud-samples/tree/master/TaskyPro/TaskyPro-Calabash) 으로 Rake를 사용 하 여 예제를 포함 하는 예제는 [빌드 스크립트](https://github.com/xamarin/test-cloud-samples/blob/master/TaskyPro/TaskyPro-Calabash/Rakefile)합니다.

> [!NOTE]
> 과 소프트웨어를 구축 하는 데 전용 되는 DSL의 유지 관리 용이성은 MSBuild 또는 NAnt, 있지만 이러한 부족 같은 XML 기반 빌드 시스템을 사용 하는 것이 가능 합니다.

### <a name="parameterizing-the-build-script"></a>빌드 스크립트를 매개 변수화

빌드 및 테스트 소프트웨어의 프로세스에는 비밀 유지 해야 하는 정보가 필요 합니다. 특히 APK를 만드는 키 저장소 및/또는 키 저장소의 키 별칭에 대 한 암호가 필요할 수 있습니다. 마찬가지로, Test Cloud에 개발자에 게 고유한 API 키가 필요 합니다. 이러한 유형의 값 아니어야 하드 빌드 스크립트에 코딩 합니다. 대신이 빌드 스크립트 변수로 전달 되어야 합니다.

덜 중요 한은 실행 되는 iOS 장치 ID 또는 Test Cloud에 대 한 사용 해야 하는 장치에서 테스트 하는 항목을 식별 하는 Android 장치 ID와 같은 값입니다. 이러한 보호 해야 하는 값 되지 않지만 빌드 간에 변경 될 수 있습니다.

이러한 유형의 빌드 스크립트는 외부 변수를 저장 합니다. 또한 쉽게 예를 들어 개발자와 조직 내 빌드 스크립트를 공유 합니다. 개발자가 똑같은 스크립트를 빌드 서버로 사용할 수 있지만 자신의 키 저장소 및 API 키를 사용할 수 있습니다.

이러한 중요 한 값을 저장 하기 위한 두 가지 가능한 옵션에는

- **구성 파일을** – 버전 제어에이 값을 검사 해야 테스트 클라우드 API 키를 보호 합니다. 각 컴퓨터에 대 한 파일을 만들 수 있습니다. 이 파일에서 값을 읽은 방식을 사용 하는 스크립팅 언어에 따라 달라 집니다.

- **환경 변수** – 컴퓨터 별로 기본 스크립트 언어에 관계 없이 공유에서 쉽게 설정할 수 있습니다.

장점과 단점을 이러한 각 선택 있습니다. TeamCity는 빌드 스크립트를 만들 때이 가이드는이 기법을 권장 하므로 환경 변수를 사용 하 여 원활 하 게 합니다.

### <a name="build-steps"></a>빌드 단계

빌드 스크립트는 다음 단계를 수행할 수 있어야 합니다.

- **응용 프로그램을 컴파일** – 여기에 올바른 프로 비전 프로필을 사용 하 여 응용 프로그램에 서명 합니다.

- **Xamarin Test Cloud 응용 프로그램 제출** – 서명 및 적절 한 키 저장소를 사용 하 여 APK에 맞게 조정 하는 zip이 포함 되어 있습니다.

이러한 두 단계는 아래에서 자세히 설명 되어 있습니다.

#### <a name="compiling-a-xamarinios-application"></a>Xamarin.iOS 응용 프로그램 컴파일

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

#### <a name="compiling-a-xamarinandroid-application"></a>Xamarin.Android 응용 프로그램 컴파일

사용 하 여 Android 응용 프로그램을 컴파일하려면 **xbuild** (또는 **msbuild** Windows에서):

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:SignAndroidPackage /p:Configuration=Release /path/to/android.csproj
```

Xamarin Android 응용 프로그램을 컴파일할 수 있음을 **xbuild** 하 고 프로젝트를 사용 하 여 iOS 응용 프로그램을 빌드하려면 **xbuild** 솔루션이 필요 합니다.

#### <a name="submitting-xamarinuitests-to-test-cloud"></a>Test Cloud에 Xamarin.uitest를 제출합니다.

Uitest를 사용 하 여 제출 되는 `test-cloud.exe` 다음 코드 조각에 표시 된 것과 같이 응용 프로그램:

```bash
test-cloud.exe <path-to-apk-or-ipa-file> <test-cloud-team-api-key> --devices <device-selection-id> --assembly-dir <path-to-tests-containing-test-assemblies> --nunit-xml report.xml --user <email>
```

테스트를 실행 하면 테스트 결과 라는 NUnit 스타일 XML 파일의 형태로 반환 됩니다 **report.xml**합니다. TeamCity는 빌드 로그에 정보를 표시 됩니다.

Test Cloud에 대 한 Uitest 제출 하는 방법에 대 한 자세한 내용은이 가이드에 참조 [업로드에 대 한 준비 Xamarin.uitest](/appcenter/test-cloud/preparing-for-upload/uitest/)합니다.

#### <a name="submitting-calabash-tests-to-test-cloud"></a>Calabash 테스트 클라우드 테스트를 제출 합니다.

Calabash 테스트를 사용 하 여 제출 되는 `test-cloud` gem을, 다음 코드 조각에 나와 있는 것 처럼:

```bash
test-cloud submit /path/to/APK-or-IPA <test-cloud-team-api-key> --devices <device-id> --user <email>
```
Test Cloud에 Android 응용 프로그램을 제출 하려면 먼저 calabash-android를 사용 하 여 APK 테스트 서버를 다시 작성 해야 하는:

```bash
$ calabash-android build </path/to/signed/APK>
$ test-cloud submit /path/to/APK <test-cloud-team-api-key> --devices <ANDROID_DEVICE_ID> --profile=android --config=config/cucumber.yml --pretty
```
Calabash 테스트 등록에 대 한 자세한 내용은 설명서를 참조 하십시오 Xamarin의 온 [Calabash 테스트를 Test Cloud에 제출](https://developer.xamarin.com/guides/testcloud/calabash/working-with/submitting-tests-to-xamarin-test-cloud/)합니다.

## <a name="creating-a-teamcity-project"></a>TeamCity 프로젝트 만들기

TeamCity는 설치 된 Mac 용 Visual Studio 프로젝트를 빌드할 수 있습니다 하 고 프로젝트를 만들려면 프로젝트를 빌드 및 Test Cloud에 제출 하려면 TeamCity에서 차례입니다.

1. 웹 브라우저를 통해 TeamCity에 로그인 하 여 시작 합니다. 루트 프로젝트를 이동 합니다.

    ![루트 프로젝트 이동](teamcity-images/image2.png "루트 프로젝트 이동") 루트 프로젝트 아래에 있는 새 하위 프로젝트를 만듭니다.

    ![루트 프로젝트 아래는 루트 프로젝트 이동한 후에 새 하위 프로젝트를 만듭니다](teamcity-images/image3.png "탐색 루트 프로젝트 아래는 루트 프로젝트에 새 하위 프로젝트 만들기")
2. 하위 프로젝트를 만든 후 새 빌드 구성을 추가 합니다.

    ![하위 프로젝트를 만든 후 새 빌드 구성을 추가](teamcity-images/image5.png "하위 프로젝트를 만든 후 새 빌드 구성을 추가")
3. VC 프로젝트 빌드 구성에 연결 합니다. 버전 제어 설정 화면을 통해 이루어집니다.

    ![버전 제어 설정 화면을 통해 이렇게](teamcity-images/image6.png "버전 제어 설정 화면을 통해 이렇게")

    만든 없습니다 VC 프로젝트의 경우 아래 표시 되는 새 VC 루트 페이지에서 만들 수가 있습니다.

    ![새 VC 루트 페이지에서 만들 수 있는 생성 VC 프로젝트가 있으면](teamcity-images/image7.png "새 VC 루트 페이지에서 만들 수 있는 생성 VC 프로젝트가 있으면")

    VCS 루트에 연결 되 면 자동으로 시도 TeamCity는 프로젝트를 체크 아웃 빌드 단계를 검색 합니다. TeamCity에 익숙한 경우 선택할 수 있습니다 검색된 빌드 단계 중 하나입니다. 이제 검색 된 빌드 단계를 무시 해도 됩니다.

4. 다음으로 빌드 트리거를 구성 합니다. 이 값은 사용자 코드 리포지토리로 커밋합니다 하는 경우와 같은 특정 조건이 충족 될 때 빌드를 대기 됩니다. 다음 스크린 샷에서 빌드 트리거를 추가 하는 방법을 보여 줍니다.

    ![이 스크린샷은 빌드 트리거를 추가 하는 방법을 보여 줍니다](teamcity-images/image8.png "이 스크린샷 빌드 트리거를 추가 하는 방법을 보여 줍니다") 빌드 트리거를 구성 하는 예로 다음 스크린샷에서 볼 수 있습니다.

    ![이 스크린샷에서 빌드 트리거를 구성 하는 예제를 볼 수 있습니다](teamcity-images/image9.png "이 스크린샷에 빌드 트리거를 구성 하는 예제를 볼 수 있습니다")

5. 빌드 스크립트를 매개 변수화의 이전 섹션에서 일부 값을 환경 변수로 저장 것이 좋습니다. 이러한 변수는 매개 변수 화면을 통해 빌드 구성에 추가할 수 있습니다. 테스트 클라우드 API 키, iOS 장치 ID 및 Android 장치 ID 아래 스크린샷에 표시 된 것 처럼 변수를 추가 합니다.

    ![테스트 클라우드 API 키, iOS 장치 ID 및 Android 장치 ID에 대 한 변수를 추가](teamcity-images/image11.png "테스트 클라우드 API 키, iOS 장치 ID 및 Android 장치 ID에 대 한 변수를 추가 합니다.")

6. 응용 프로그램 및 큐에 넣기 Test Cloud에 응용 프로그램을 컴파일하려면 빌드 스크립트를 호출 하는 빌드 단계 추가 하는 최종 단계가입니다. 다음 스크린샷에서 응용 프로그램을 빌드하는 Rakefile를 사용 하는 빌드 단계 예가 같습니다.

    ![이 스크린샷에서 응용 프로그램을 빌드하는 Rakefile를 사용 하는 빌드 단계의 예로](teamcity-images/image12.png "이 스크린샷에서 응용 프로그램을 빌드하는 Rakefile를 사용 하는 빌드 단계의 예로")

7. 이 시점에서 빌드 구성을 완료 되었습니다. 프로젝트를 올바르게 구성 되어 있는지 확인 하려면 빌드를 트리거하는 것이 좋습니다. 이렇게 하려면 작은, 불필요 한 변경 내용을 리포지토리로 커밋합니다을 것이 좋습니다. TeamCity는 커밋을 감지 하 고 빌드를 시작 해야 합니다.

8. 빌드가 완료 되 면 빌드 로그를 검사 하 고 모든 문제 또는 주의 해야 하는 빌드를 사용 하 여 경고가 있는지 확인 합니다.

## <a name="summary"></a>요약

이 가이드는 TeamCity Xamarin 모바일 응용 프로그램을 빌드 및 Test Cloud에 제출할 수를 사용 하는 방법을 설명 합니다. 빌드 프로세스를 자동화 하는 빌드 스크립트를 만드는 것을 설명 합니다. 빌드 스크립트는 응용 프로그램의 컴파일 Test Cloud에 제출 하 고 결과 기다리는 담당 합니다.

그런 다음 개발자는 코드를 커밋하면 때마다 빌드를 큐는 빌드 스크립트를 호출 하는 TeamCity에서 프로젝트를 만드는 방법에 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [Xamarin.uitest fpr 업로드 준비](/appcenter/test-cloud/preparing-for-upload/uitest/)
- [설치 및 구성 TeamCity](http://confluence.jetbrains.com/display/TCD8/Installing+and+Configuring+the+TeamCity+Server)
