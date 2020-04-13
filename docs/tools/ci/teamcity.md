---
title: 자마린과 함께 팀 시티 사용
description: 이 가이드에서는 TeamCity를 사용하여 모바일 응용 프로그램을 컴파일한 다음 앱 센터 테스트에 제출하는 방법에 대해 설명합니다.
ms.prod: xamarin
ms.assetid: AC2626CB-28A7-4808-B2A9-789D67899546
author: davidortinau
ms.author: daortin
ms.date: 04/01/2020
ms.openlocfilehash: 6ecd453180e8c392ba7d7527778617eb40950a9e
ms.sourcegitcommit: 6f3281a32017cfcebadde8a2d6e10651a277828f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80587459"
---
# <a name="using-team-city-with-xamarin"></a>자마린과 함께 팀 시티 사용

_이 가이드에서는 TeamCity를 사용하여 모바일 응용 프로그램을 컴파일한 다음 앱 센터 테스트에 제출하는 방법에 대해 설명합니다._

[연속 통합 소개](~/tools/ci/intro-to-ci.md) 가이드에서 설명한 것처럼 연속 통합(CI)은 고품질 모바일 응용 프로그램을 개발할 때 유용한 방법입니다. 연속 통합 서버 소프트웨어에는 여러 가지 실행 가능한 옵션이 있습니다. 이 가이드는 제트브레인의 [팀시티에](https://www.jetbrains.com/teamcity/) 초점을 맞춥니다.

TeamCity 설치에는 여러 가지 순열이 있습니다. 다음 목록에서는 이러한 순열 중 일부를 설명합니다.

- **Windows 서비스** - 이 시나리오에서 TeamCity 는 Windows가 Windows 서비스로 부팅될 때 시작됩니다. iOS 응용 프로그램을 컴파일하려면 Mac 빌드 호스트와 페어링해야 합니다.

- **OS X에서 데몬을 실행** – 개념적으로, 이것은 이전 단계에서 설명한 Windows 서비스로 실행 비슷합니다. 기본적으로 빌드는 루트 계정에서 실행됩니다.

- **OS X의 사용자 계정** - 사용자가 로그인할 때마다 시작되는 사용자 계정에서 TeamCity를 실행할 수 있습니다.

이전 시나리오 중 OS X에서 사용자 계정으로 TeamCity를 실행하는 것이 가장 간단하고 쉽게 설정할 수 있습니다.

TeamCity 설정과 관련된 몇 가지 단계가 있습니다.

- **TeamCity 설치** – TeamCity 설치는 이 가이드에서 다루지 않습니다. 이 가이드는 TeamCity가 사용자 계정으로 설치되고 실행되고 있다고 가정합니다. [팀시티 설치에](https://confluence.jetbrains.com/display/TCD8/Installation) 대한 지침은 JetBrains의 [TeamCity 8 설명서에서](https://confluence.jetbrains.com/display/TCD8/TeamCity+Documentation) 찾을 수 있습니다.

- **빌드 서버 준비** - 이 단계에서는 모바일 응용 프로그램을 빌드하고 배포를 준비하는 데 필요한 소프트웨어, 도구 및 인증서를 설치해야 합니다.

- **빌드 스크립트 만들기** - 이 단계는 반드시 필요한 것은 아니지만 빌드 스크립트는 응용 프로그램을 무인으로 빌드하는 데 유용합니다. 빌드 스크립트를 사용하면 발생할 수 있는 빌드 문제를 해결하는 데 도움이 되며 연속 통합이 실행되지 않더라도 배포를 위한 바이너리를 만드는 일관되고 반복 가능한 방법을 제공합니다.

- **TeamCity 프로젝트 만들기** - 이전 세 단계가 완료되면 소스 코드를 검색하고 프로젝트를 컴파일하고 테스트를 앱 센터 테스트에 제출하는 데 필요한 모든 메타 데이터가 포함된 TeamCity 프로젝트를 만들어야 합니다.

## <a name="requirements"></a>요구 사항

앱 [센터 테스트](https://docs.microsoft.com/appcenter/test-cloud/) 경험이 필요합니다.

TeamCity 8.1에 대한 친숙함이 필요합니다. TeamCity의 설치는 이 문서의 범위를 벗어납니다. TeamCity는 OS X 매버릭스에 설치되어 있으며 루트 계정이 아닌 일반 사용자 계정으로 실행되고 있다고 가정합니다.

빌드 서버는 지속적인 통합전용 OS X를 실행하는 독립 실행형 컴퓨터여야 합니다. 이상적으로 빌드 서버는 데이터베이스 서버, 웹 서버 또는 개발자 워크스테이션과 같은 다른 역할에 대해 책임을 지지 않습니다.

> [!IMPORTANT]
> 이 가이드는 Xamarin의 "헤드리스" 설치를 다루지 않습니다.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="preparing-the-build-server"></a>빌드 서버 준비

빌드 서버를 구성하는 데 중요한 단계는 모바일 응용 프로그램을 빌드하는 데 필요한 모든 도구, 소프트웨어 및 인증서를 설치하는 것입니다. 빌드 서버가 모바일 솔루션을 컴파일하고 테스트를 실행할 수 있는 것이 중요합니다. 구성 문제를 최소화하려면 TeamCity를 호스팅하는 동일한 사용자 계정에 소프트웨어 및 도구를 설치해야 합니다. 다음 목록에서는 필요한 사항을 자세히 설명합니다.

1. **맥에 대 한 비주얼 스튜디오** - 이 Xamarin.iOS와 Xamarin.Android를 포함한다.
2. **Xamarin 구성 요소 저장소에 로그인** - 이 단계는 선택 사항이며 응용 프로그램에서 Xamarin 구성 요소 저장소의 구성 요소를 사용하는 경우에만 필요합니다. 이 시점에서 구성 요소 저장소에 미리 로그인하면 TeamCity 빌드에서 응용 프로그램을 컴파일하려고 할 때 문제가 발생하지 않습니다.
3. **Xcode** - iOS 응용 프로그램을 컴파일하고 서명하려면 Xcode가 필요합니다.
4. **Xcode 명령줄 도구** - 이것은 [rbenv 가이드와 루비 업데이트의](https://github.com/calabash/calabash-ios/wiki) 설치 섹션의 1 단계에서 설명되어 있습니다.
5. **ID & 프로비저닝 프로필 서명** – XCode를 통해 인증서 및 프로비저닝 프로필을 가져옵니다. 자세한 내용은 서명 [ID 및 프로비저닝 프로필 내보내기에](https://developer.apple.com/library/ios/recipes/xcode_help-accounts_preferences/articles/export_signing_assets.html) 대한 Apple의 가이드를 참조하십시오.
6. **안드로이드 키스토어** - TeamCity 사용자가 액세스 할 수있는 디렉토리에 필요한 안드로이드 `~/Documents/keystores/MyAndroidApp1`키 스토어를 복사, 즉 .
7. **칼라바시 ( Calabash)** - 응용 프로그램에 Calabash를 사용하여 작성된 테스트가 있는 경우 선택적 단계입니다. 자세한 내용은 OS X 매버릭스 가이드와 [rbenv 가이드와 함께 업데이트 루비에](https://github.com/calabash/calabash-ios/wiki) [칼라바시 설치를](https://github.com/calabash/calabash-ios/wiki) 참조하십시오.

다음 다이어그램은 이러한 모든 구성 요소를 보여 줍니다.

![](teamcity-images/image1.png "This diagram illustrates all of these components")

모든 소프트웨어가 설치되면 사용자 계정에 로그인하여 모든 소프트웨어가 제대로 설치되고 작동하는지 확인합니다. 여기에는 솔루션을 컴파일하고 응용 프로그램 센터 테스트에 응용 프로그램을 제출하는 것이 포함됩니다. 다음 섹션에서 설명한 대로 빌드 스크립트를 실행 하여 단순화할 수 있습니다.

## <a name="create-a-build-script"></a>빌드 스크립트 만들기

TeamCity가 모바일 응용 프로그램을 컴파일하고 앱 센터 테스트에 제출하는 모든 측면을 자체적으로 처리할 수 있지만; 빌드 스크립트를 만드는 것이 좋습니다. 빌드 스크립트는 다음과 같은 이점을 제공합니다.

1. **문서** - 빌드 스크립트는 소프트웨어 빌드 방법에 대한 문서의 한 형태로 제공됩니다. 이렇게 하면 응용 프로그램 배포와 관련된 "마법"이 제거되고 개발자가 기능에 집중할 수 있습니다.
1. **반복성** – 빌드 스크립트는 응용 프로그램을 컴파일하고 배포할 때마다 작업을 수행하는 사람 또는 수행 내용에 관계없이 동일한 방식으로 수행되도록 합니다. 이 반복 가능한 일관성은 잘못 실행된 빌드 또는 인적 오류로 인해 나타날 수 있는 문제 나 오류를 제거합니다.
1. **버전 관리** - 빌드 스크립트를 소스 제어 시스템에 포함할 수 있습니다. 즉, 오류 나 부정확성이 발견되면 빌드 스크립트의 변경 내용을 추적, 모니터링 및 수정할 수 있습니다.
1. **환경 준비** - 빌드 스크립트에는 필요한 제3자 종속성을 설치하는 논리가 포함될 수 있습니다. 이렇게 하면 응용 프로그램이 적절한 구성 요소로 빌드됩니다.

빌드 스크립트는 PowerShell 파일(Windows) 또는 BASH 스크립트(OS X)만큼 간단할 수 있습니다. 빌드 스크립트를 만들 때 언어를 스크립팅하기 위한 몇 가지 선택 사항이 있습니다.

- [**Rake**](https://github.com/jimweirich/rake) - 루비를 기반으로 프로젝트를 구축하기 위한 도메인 별 언어(DSL)입니다. 레이크는 인기와 라이브러리의 풍부한 생태계의 장점을 가지고있다.

- [**psake**](https://github.com/psake/psake) - 이것은 소프트웨어를 구축하기위한 Windows PowerShell 라이브러리입니다.

- [**가짜**](https://fsharp.github.io/FAKE/) - 필요한 경우 기존 .NET 라이브러리를 사용할 수 있도록 F #에 기반한 DSL입니다.

사용되는 스크립팅 언어는 기본 설정 및 요구 사항에 따라 다릅니다.

> [!NOTE]
> MSBuild 또는 NAnt와 같은 XML 기반 빌드 시스템을 사용할 수 있지만 소프트웨어 구축전용 DSL의 표현력과 유지 관리성이 부족합니다.

### <a name="parameterizing-the-build-script"></a>빌드 스크립트 매개 변수화

소프트웨어를 구축하고 테스트하는 과정에는 비밀로 유지해야 하는 정보가 필요합니다. APK를 만들려면 키 저장소 및/또는 키 저장소의 키 별칭에 대한 암호가 필요할 수 있습니다. 마찬가지로 앱 센터 테스트에는 개발자에게 고유한 [API 키가](/appcenter/api-docs/) 필요합니다. 이러한 유형의 값은 빌드 스크립트에서 하드 코딩해서는 안 됩니다. 대신 빌드 스크립트에 변수로 전달되어야 합니다.

덜 민감도는 iOS 장치 ID 또는 앱 센터에서 테스트 실행에 사용해야 하는 장치를 식별하는 Android 장치 ID와 같은 값입니다. 이러한 값은 보호해야 하는 값은 아니지만 빌드에서 빌드로 변경될 수 있습니다.

이러한 유형의 변수를 빌드 스크립트 외부에 저장하면 예를 들어 개발자와 조직 내에서 빌드 스크립트를 더 쉽게 공유할 수 있습니다. 개발자는 빌드 서버와 정확히 동일한 스크립트를 사용할 수 있지만 자체 키 저장소 및 [API 키를](/appcenter/api-docs/)사용할 수 있습니다.

이러한 중요한 값을 저장하는 데는 두 가지 옵션이 있습니다.

- **구성 파일** - [API 키를](/appcenter/api-docs/) 보호하려면 이 값을 버전 제어에 체크 인해서는 안 됩니다. 각 컴퓨터에 대해 파일을 만들 수 있습니다. 이 파일에서 값을 읽는 방법은 사용되는 스크립팅 언어에 따라 다릅니다.

- 환경 변수 - 이러한 **변수는** 시스템 단위로 쉽게 설정할 수 있으며 기본 스크립팅 언어와 는 별개입니다.

이러한 각 선택 의 장점과 단점이 있습니다. TeamCity는 환경 변수와 잘 작동하므로 빌드 스크립트를 만들 때 이 방법을 권장합니다.

### <a name="build-steps"></a>빌드 단계

빌드 스크립트는 다음 단계를 수행해야 합니다.

- **응용 프로그램 컴파일** - 올바른 프로비저닝 프로필로 응용 프로그램에 서명하는 것이 포함됩니다.

- **Xamarin 테스트 클라우드에 응용 프로그램 제출** - 여기에는 APK를 적절한 키 저장소에 정렬하는 서명 및 압축이 포함됩니다.

이 두 단계는 아래에서 자세히 설명합니다.

#### <a name="compiling-a-xamarinios-application"></a>Xamarin.iOS 응용 프로그램 컴파일

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

#### <a name="compiling-a-xamarinandroid-application"></a>자마린.안드로이드 응용 프로그램을 컴파일

Android 응용 프로그램을 컴파일하려면 **xbuild(또는** Windows에서 **msbuild)를** 사용합니다.

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:SignAndroidPackage /p:Configuration=Release /path/to/android.csproj
```
Android 응용 프로그램 **xbuild를** 컴파일하면 프로젝트를 사용하고 iOS 응용 프로그램 **xbuild는** 솔루션을 사용합니다.

#### <a name="submitting-xamarinuitests-to-app-center"></a>앱 센터에 Xamarin.UI테스트 제출

UITest는 다음 코드 조각에 표시된 대로 [앱 센터 CLI를](https://github.com/microsoft/appcenter-cli)사용하여 제출됩니다.

```bash
appcenter test run uitest --app <TEAM-NAME/APP-NAME> --devices <DEVICE_SET> --token <API_KEY> --app-path <appname.APK-or-appname.IPA> --merge-nunit-xml report.xml --build-dir pathToUITestBuildDir
```

테스트가 실행되면 테스트 결과가 **report.xml이라는**NUnit 스타일 XML 파일의 형태로 반환됩니다. TeamCity는 빌드 로그에 정보를 표시합니다.

앱 센터에 UITest를 제출하는 방법에 대한 자세한 내용은 [Xamarin.Android 앱 준비](/appcenter/test-cloud/uitest/preparing-for-upload-android) 또는 [Xamarin.iOS 앱 준비를](/appcenter/test-cloud/uitest/preparing-for-upload-ios)참조하십시오.

#### <a name="submitting-calabash-tests-to-app-center"></a>앱 센터에 칼라바시 테스트 제출

칼라바시 테스트는 다음 스니펫과 같이 [앱 센터 CLI를](https://github.com/microsoft/appcenter-cli)사용하여 제출됩니다.

```bash
appcenter test run calabash --app <TEAM-NAME/APP-NAME> --devices <DEVICE_SET> --token <API_KEY> --app-path <appname.APK-or-appname.IPA> --project-dir pathToProjectDir
```

앱 센터 테스트에 Android 응용 프로그램을 제출하려면 먼저 calabash-android를 사용하여 APK 테스트 서버를 다시 빌드해야합니다.

```bash
$ calabash-android build </path/to/signed/APK>
$ appcenter test run calabash --app <TEAM-NAME/APP-NAME> --devices <DEVICE_SET> --token <API_KEY> --app-path <appname.APK> --project-dir pathToProjectDir
```

칼라바시 시험 제출에 대한 자세한 내용은 테스트 [클라우드에 칼라바시 테스트 제출에](https://github.com/calabash/calabash-ios/wiki)대한 Xamarin의 가이드를 참조하십시오.

## <a name="creating-a-teamcity-project"></a>팀시티 프로젝트 만들기

TeamCity가 설치되고 Mac용 Visual Studio가 프로젝트를 빌드할 수 있으면 TeamCity에서 프로젝트를 만들어 프로젝트를 빌드하고 앱 센터에 제출해야 합니다.

1. 웹 브라우저를 통해 TeamCity에 로그인하여 시작했습니다. 루트 프로젝트로 이동:

    ![루트 프로젝트로 이동](teamcity-images/image2.png "루트 프로젝트로 이동") 루트 프로젝트 아래에 새 하위 프로젝트를 만듭니다.

    ![루트 프로젝트 아래의 루트 프로젝트로 이동하여 새 하위 프로젝트를 만듭니다.](teamcity-images/image3.png "루트 프로젝트 아래의 루트 프로젝트로 이동하여 새 하위 프로젝트를 만듭니다.")
2. 하위 프로젝트가 만들어지면 새 빌드 구성을 추가합니다.

    ![하위 프로젝트가 만들어지면 새 빌드 구성을 추가합니다.](teamcity-images/image5.png "하위 프로젝트가 만들어지면 새 빌드 구성을 추가합니다.")
3. VCS 프로젝트를 빌드 구성에 연결합니다. 이 작업은 버전 제어 설정 화면을 통해 수행됩니다.

    ![이 작업은 버전 제어 설정 화면을 통해 수행됩니다.](teamcity-images/image6.png "이 작업은 버전 제어 설정 화면을 통해 수행됩니다.")

    생성된 VCS 프로젝트가 없는 경우 아래 표시된 새 VCS 루트 페이지에서 프로젝트를 만들 수 있습니다.

    ![생성된 VCS 프로젝트가 없는 경우 새 VCS 루트 페이지에서 프로젝트를 만들 수 있습니다.](teamcity-images/image7.png "생성된 VCS 프로젝트가 없는 경우 새 VCS 루트 페이지에서 프로젝트를 만들 수 있습니다.")

    VCS 루트가 연결되면 TeamCity는 프로젝트를 체크 아웃하고 빌드 단계를 자동으로 감지하려고 시도합니다. TeamCity에 익숙한 경우 검색된 빌드 단계 중 하나를 선택할 수 있습니다. 지금은 검색된 빌드 단계를 무시해도 안전합니다.

4. 다음으로 빌드 트리거를 구성합니다. 이렇게 하면 사용자가 리포지토리에 코드를 커밋하는 경우와 같이 특정 조건이 충족되면 빌드가 큐에 대기합니다. 다음 스크린샷은 빌드 트리거를 추가하는 방법을 보여 주며 다음과 같은 방법을 보여 주며 다음과 같은 방법을 보여 주며 다음과 같은 방법을 보여 주며 다음과 같은 방법을 보여 주

    ![이 스크린샷은 빌드 트리거를 추가하는 방법을 보여 주며](teamcity-images/image8.png "이 스크린샷은 빌드 트리거를 추가하는 방법을 보여 주며") 빌드 트리거 구성의 예는 다음 스크린샷에서 확인할 수 있습니다.

    ![이 스크린샷에서 빌드 트리거 를 구성하는 예제를 볼 수 있습니다.](teamcity-images/image9.png "이 스크린샷에서 빌드 트리거 를 구성하는 예제를 볼 수 있습니다.")

5. 이전 섹션에서는 빌드 스크립트의 매개 변수화를 통해 일부 값을 환경 변수로 저장하는 것이 좋습니다. 이러한 변수는 매개 변수 화면을 통해 빌드 구성에 추가할 수 있습니다. 아래 스크린샷과 같이 앱 센터 [API 키,](/appcenter/api-docs/)iOS 장치 ID 및 Android 장치 ID에 대한 변수를 추가합니다.

    ![앱 센터 테스트 API 키, iOS 장치 ID 및 Android 장치 ID에 대한 변수 추가](teamcity-images/image11.png "테스트 클라우드 API 키, iOS 장치 ID 및 Android 장치 ID에 대한 변수 추가")

6. 마지막 단계는 빌드 스크립트를 호출하여 응용 프로그램을 컴파일하고 응용 프로그램을 App Center Test로 큐에 넣은 빌드 단계를 추가하는 것입니다. 다음 스크린샷은 Rakefile을 사용하여 응용 프로그램을 빌드하는 빌드 단계의 예입니다.

    ![이 스크린샷은 Rakefile을 사용하여 응용 프로그램을 빌드하는 빌드 단계의 예입니다.](teamcity-images/image12.png "이 스크린샷은 Rakefile을 사용하여 응용 프로그램을 빌드하는 빌드 단계의 예입니다.")

7. 이 시점에서 빌드 구성이 완료됩니다. 프로젝트가 제대로 구성되어 있는지 확인하기 위해 빌드를 트리거하는 것이 좋습니다. 이 작업을 수행하는 좋은 방법은 저장소에 작고 사소한 변경을 커밋하는 것입니다. TeamCity는 커밋을 감지하고 빌드를 시작해야 합니다.

8. 빌드가 완료되면 빌드 로그를 검사하고 주의가 필요한 빌드에 문제가 있거나 경고가 있는지 확인합니다.

## <a name="summary"></a>요약

이 가이드에서는 TeamCity를 사용하여 Xamarin Mobile 응용 프로그램을 빌드한 다음 앱 센터 테스트에 제출하는 방법을 다루었습니다. 빌드 프로세스를 자동화하기 위한 빌드 스크립트를 만드는 방법을 설명했습니다. 빌드 스크립트는 응용 프로그램을 컴파일하고, App Center Test에 제출하고, 결과를 기다리는 것을 처리합니다.

그런 다음 개발자가 코드를 커밋하고 빌드 스크립트를 호출할 때마다 빌드를 큐에 대기하는 TeamCity에서 프로젝트를 만드는 방법을 다루었습니다.

## <a name="related-links"></a>관련 링크

- [자마린.안드로이드 애플 리케이션 준비](/appcenter/test-cloud/uitest/preparing-for-upload-android)
- [Xamarin.iOS 앱 준비](/appcenter/test-cloud/uitest/preparing-for-upload-ios)
- [TeamCity 설치 및 구성](https://confluence.jetbrains.com/display/TCD8/Installing+and+Configuring+the+TeamCity+Server)
