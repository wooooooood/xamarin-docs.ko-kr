---
title: Xamarin에서 Jenkins 사용
description: 이 문서에서는 Xamarin 응용 프로그램과의 연속 통합을 위해 Jenkins를 사용 하는 방법을 설명 합니다. Jenkins를 설치, 구성 및 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 1E6825DF-1254-4FCB-B94D-ADD33D1B5309
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 4f91e683b826657a9740de7e0b98137858130042
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725379"
---
# <a name="using-jenkins-with-xamarin"></a>Xamarin에서 Jenkins 사용

_이 가이드에서는 Jenkins를 연속 통합 서버로 설정 하 고 Xamarin을 사용 하 여 만든 모바일 응용 프로그램의 컴파일을 자동화 하는 방법을 보여 줍니다. OS X에 Jenkins을 설치 하 고, 구성 하 고, 변경 내용이 소스 코드 관리 시스템에 커밋될 때 Xamarin.ios 및 Xamarin Android 응용 프로그램을 컴파일하도록 작업을 설정 하는 방법을 설명 합니다._

[Xamarin과의 연속 통합 소개에](~/tools/ci/intro-to-ci.md) 는 중단 되거나 호환 되지 않는 코드의 초기 경고를 제공 하는 유용한 소프트웨어 개발 사례로 연속 통합이 도입 되었습니다. 개발자는 CI를 사용 하 여 발생 하는 문제 및 문제를 해결 하 고 소프트웨어를 배포에 적합 한 상태로 유지할 수 있습니다. 이 연습에서는 두 문서의 콘텐츠를 함께 사용 하는 방법을 설명 합니다.

이 가이드에서는 OS X를 실행 하는 전용 컴퓨터에 Jenkins를 설치 하 고 컴퓨터를 시작할 때 자동으로 실행 되도록 구성 하는 방법을 보여 줍니다. Jenkins가 설치 되 면 MS Build를 지원 하기 위해 추가 플러그 인을 설치 합니다. Jenkins는 즉시 Git를 지원 합니다. 소스 코드 제어에 TFS를 사용 하는 경우 추가 플러그 인 및 명령줄 유틸리티도 설치 해야 합니다.

Jenkins가 구성 되 고 필요한 모든 플러그 인이 설치 되 면 하나 이상의 작업을 만들어 Xamarin Android 및 Xamarin.ios 프로젝트를 컴파일합니다. 작업은 작업을 수행 하는 데 필요한 단계 및 메타 데이터의 컬렉션입니다. 작업은 일반적으로 다음과 같이 구성 됩니다.

- **SCM (소스 코드 관리)** -소스 코드 제어에 연결 하는 방법 및 검색할 파일에 대 한 정보를 포함 하는 Jenkins 구성 파일의 메타 데이터 항목입니다.
- **트리거** – 트리거는 개발자가 소스 코드 리포지토리에 대 한 변경 내용을 커밋하는 경우와 같은 특정 작업을 기반으로 작업을 시작 하는 데 사용 됩니다.
- **빌드 지침** – 소스 코드를 컴파일하고 모바일 장치에 설치할 수 있는 이진 파일을 생성 하는 플러그 인 또는 스크립트입니다.
- **선택적 빌드 작업** – 단위 테스트 실행, 코드에 대 한 정적 분석 수행, 코드 서명 또는 다른 작업을 시작 하 여 다른 빌드 관련 작업을 수행 하는 작업을 포함할 수 있습니다.
- **알림** – 작업은 빌드 상태에 대 한 일종의 알림을 보낼 수 있습니다.
- **보안** – 선택적 이지만 Jenkins 보안 기능을 사용 하도록 설정 하는 것이 좋습니다.

이 가이드에서는 이러한 각 사항에 대해 Jenkins 서버를 설정 하는 방법을 안내 합니다. 이 과정을 마치면 Xamarin 모바일 프로젝트용 IPA 및 APK를 만들도록 Jenkins를 설정 하 고 구성 하는 방법을 잘 알고 있어야 합니다.

## <a name="requirements"></a>요구 사항

이상적인 빌드 서버는 응용 프로그램을 빌드하고 테스트할 수 있는 유일한 목적을 전담 하는 독립 실행형 컴퓨터입니다. 전용 컴퓨터를 사용 하면 웹 서버와 같은 다른 역할에 필요할 수 있는 아티팩트가 빌드를 오염 하지 않습니다. 예를 들어 빌드 서버도 웹 서버 역할을 하는 경우 웹 서버에는 충돌 하는 버전의 공용 라이브러리가 필요할 수 있습니다. 이 충돌 때문에 웹 서버가 제대로 작동 하지 않거나 Jenkins 사용자에 게 배포 될 때 작동 하지 않는 빌드를 만들 수 있습니다.

Xamarin mobile apps 용 빌드 서버는 개발자의 워크스테이션과 매우 비슷하게 설정 됩니다. Jenkins, Mac용 Visual Studio 및 Xamarin.ios와 Xamarin.ios가 설치 되는 사용자 계정이 있습니다. 모든 코드 서명 인증서, 프로 비전 프로필 및 키 저장소도 설치 해야 합니다. *일반적으로 빌드 서버의 사용자 계정은 개발자 계정과 별개 이며, 빌드 서버 사용자 계정으로 로그인 하는 동안 모든 소프트웨어, 키 및 인증서를 설치 하 고 구성 해야 합니다.*

다음 다이어그램에서는 일반적인 Jenkins 빌드 서버에 있는 이러한 모든 요소를 보여 줍니다.

[![](jenkins-walkthrough-images/image1.png "This diagram illustrates all of these elements on a typical Jenkins build server")](jenkins-walkthrough-images/image1.png#lightbox)

iOS 응용 프로그램은 macOS를 실행 하는 컴퓨터 에서만 빌드하고 서명할 수 있습니다. Mac 미니는 적절 한 저렴 한 옵션 이지만 OS X 10.10 (Yosemite) 이상을 실행할 수 있는 모든 컴퓨터에는 충분 합니다.

소스 코드 제어에 TFS를 사용 하는 경우 [Team Explorer Everywhere](https://docs.microsoft.com/azure/devops/java/download-eclipse-plug-in/)를 설치 하는 것이 좋습니다. Team Explorer Everywhere은 macOS의 터미널에서 TFS에 대 한 플랫폼 간 액세스를 제공 합니다.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="installing-jenkins"></a>Jenkins 설치

Jenkins를 사용 하는 첫 번째 작업은이를 설치 하는 것입니다. OS X에서 Jenkins를 실행 하는 방법에는 세 가지가 있습니다.

- 디먼으로, 백그라운드로 실행 됩니다.
- Tomcat, Jetty 또는 JBoss와 같은 서블릿 컨테이너 내에서
- 사용자 계정으로 실행 되는 일반적인 프로세스입니다.

대부분의 일반적인 연속 통합 응용 프로그램은 디먼 (OS X 또는 \*nix) 또는 서비스 (Windows)로 백그라운드에서 실행 됩니다. 이는 GUI 조작이 필요 없고 빌드 환경의 설정을 쉽게 수행할 수 있는 시나리오에 적합 합니다. 또한 모바일 앱은 Jenkins가 디먼으로 실행 될 때 액세스에 문제가 될 수 있는 keystores 및 서명 인증서가 필요 합니다. 이러한 문제로 인해이 문서에서는 세 번째 시나리오, 즉 빌드 서버의 사용자 계정에서 Jenkins를 실행 하는 방법을 중점적으로 설명 합니다.

Jenkins는 Jenkins을 설치 하는 편리한 방법입니다. Jenkins 서버의 시작 및 중지를 간소화 하는 AppleScript 래퍼입니다. 다음 스크린샷에 표시 된 것 처럼 bash 셸에서 실행 하는 대신 Jenkins은 도크에서 아이콘이 있는 앱으로 실행 됩니다.

[![](jenkins-walkthrough-images/image2.png "Instead of running in a bash shell, Jenkins runs as an app with icon in the Dock, as shown in this screenshot")](jenkins-walkthrough-images/image2.png#lightbox)

Jenkins를 시작 하거나 중지 하는 것은 Jenkins를 시작 하거나 중지 하는 것 만큼 간단 합니다.

Jenkins를 설치 하려면 프로젝트의 다운로드 페이지에서 아래 스크린샷에 나온 최신 버전을 다운로드 합니다.

[![](jenkins-walkthrough-images/image3.png "App, download the latest version from the projects download page, pictured in this screenshot")](jenkins-walkthrough-images/image3.png#lightbox)

빌드 서버의 `/Applications` 폴더에 zip 파일을 추출 하 고 다른 OS X 응용 프로그램과 마찬가지로 시작 합니다.
Jenkins를 처음 시작 하는 경우 Jenkins를 다운로드할 것을 알리는 대화 상자가 표시 됩니다.

[![](jenkins-walkthrough-images/image4.png "App, it will present a dialog informing you that it will download Jenkins")](jenkins-walkthrough-images/image4.png#lightbox)

Jenkins 다운로드를 완료 한 후에는 다음 스크린샷에 표시 된 것 처럼 Jenkins 시작을 사용자 지정 하 라는 또 다른 대화 상자를 표시 합니다.

[![](jenkins-walkthrough-images/image5.png "App has finished its download, it will display another dialog asking you if you would like to customize the Jenkins startup, as seen in this screenshot")](jenkins-walkthrough-images/image5.png#lightbox)

Jenkins 사용자 지정은 선택 사항이 며 앱이 시작 될 때마다 수행할 필요가 없습니다. 대부분의 경우 Jenkins에 대 한 기본 설정이 적용 됩니다.

Jenkins를 사용자 지정 해야 하는 경우 **기본값 변경** 단추를 클릭 합니다. 그러면 두 개의 연속 된 대화 상자가 표시 됩니다. 하나는 Java 명령줄 매개 변수를 요청 하 고 다른 하나는 Jenkins 명령줄 매개 변수를 요구 합니다. 다음 두 스크린샷에는 이러한 두 개의 대화 상자가 표시 됩니다.

[![](jenkins-walkthrough-images/image6.png "This screenshot shows the dialogs")](jenkins-walkthrough-images/image6.png#lightbox)

[![](jenkins-walkthrough-images/image7.png "This screenshot shows the dialogs")](jenkins-walkthrough-images/image7.png#lightbox)

Jenkins이 실행 되 고 있으면 사용자가 컴퓨터에 로그인 할 때마다 시작 되도록 로그인 항목으로 설정할 수 있습니다. 도킹 및 선택 옵션에서 Jenkins 아이콘을 마우스 오른쪽 단추로 클릭 하 여이 작업을 수행할 수 있습니다.다음 스크린샷에 표시 된 것 처럼 로그인 시 열기를 > 합니다.

[![](jenkins-walkthrough-images/image8.png "You can do this by right-clicking on the Jenkins icon in the Dock and choosing OptionsOpen at Login, as shown in this screenshot")](jenkins-walkthrough-images/image8.png#lightbox)

이렇게 하면 사용자가 로그인 할 때마다 Jenkins이 자동으로 시작 되지만 컴퓨터가 부팅 되는 경우에는 자동으로 시작 되지 않습니다. OS X가 부팅 시에 자동으로 로그인 하는 데 사용할 사용자 계정을 지정할 수 있습니다. 다음 스크린샷에 표시 된 것 처럼 **시스템 기본 설정**을 열고 **사용자 & 그룹** 아이콘을 선택 합니다.

[![](jenkins-walkthrough-images/image9.png "Open the System Preferences, and select the User  Groups icon as shown in this screenshot")](jenkins-walkthrough-images/image9.png#lightbox)

**로그인 옵션** 단추를 클릭 한 다음 OS X가 부팅 시 로그인에 사용할 계정을 선택 합니다.

이제 Jenkins가 설치 되었습니다. 그러나 Xamarin 모바일 응용 프로그램을 빌드 하려는 경우에는 일부 플러그 인을 설치 해야 합니다.

### <a name="installing-plugins"></a>플러그 인 설치

Jenkins 설치 관리자가 완료 되 면 Jenkins를 시작 하 고 아래 스크린샷에 표시 된 것 처럼 URL http://localhost:8080 를 사용 하 여 웹 브라우저를 시작 합니다.

[![](jenkins-walkthrough-images/image10.png "8080, as shown in this screenshot")](jenkins-walkthrough-images/image10.png#lightbox)

아래 스크린샷에 표시 된 것 처럼이 페이지에서 왼쪽 위 모서리에 있는 메뉴에서 **Jenkins > 관리 Jenkins > 관리 플러그 인** 을 선택 합니다.

[![](jenkins-walkthrough-images/image11.png "From this page, select Jenkins  Manage Jenkins  Manage Plugins from the menu in the upper left hand corner")](jenkins-walkthrough-images/image11.png#lightbox)

그러면 **Jenkins 플러그 인 관리자** 페이지가 표시 됩니다. 사용 가능한 탭을 클릭 하면 다운로드 및 설치할 수 있는 600 플러그 인의 목록이 표시 됩니다. 이는 아래 스크린샷에 나와 있습니다.

[![](jenkins-walkthrough-images/image12.png "If you click on the Available tab, you will see a list of over 600 plugins that can be downloaded and installed")](jenkins-walkthrough-images/image12.png#lightbox)

모든 600 플러그 인을 통해 몇 가지를 검색 하면 지루한 오류가 발생할 수 있습니다. Jenkins는 인터페이스의 오른쪽 위 모서리에 필터 검색 필드를 제공 합니다. 검색에이 필터 필드를 사용 하면 다음 플러그 인의 하나 또는 전부를 찾고 설치 하는 작업이 간단해 집니다.

- **Jenkins MSBuild 플러그 인** –이 플러그 인을 사용 하면 Visual Studio 및 Mac용 Visual Studio 솔루션 (.sln) 및 프로젝트 (.csproj)를 빌드할 수 있습니다.
- **Environment 인젝터 plugin** – 작업 및 빌드 수준에서 환경 변수를 설정할 수 있도록 하는 선택적 이지만 유용한 플러그 인입니다. 또한 응용 프로그램에 코드 서명 하는 데 사용 되는 암호와 같은 변수에 대 한 추가 보호 기능을 제공 합니다. 때로는 *EnvInject 플러그 인* 으로 축약 됩니다.
- **Team Foundation Server 플러그 인** -소스 코드 제어에 Team Foundation Server 또는 Team Foundation Services를 사용 하는 경우에만 필요한 선택적 플러그 인입니다.

Jenkins는 추가 플러그 인 없이 Git를 지원 합니다.

플러그 인을 모두 설치한 후에는 Jenkins를 다시 시작 하 고 각 플러그 인에 대 한 전역 설정을 구성 해야 합니다. 플러그 인에 대 한 전역 설정은 아래 스크린샷에 표시 된 것 처럼 왼쪽 위 모서리에서 **Jenkins > 관리 Jenkins > 시스템 구성** 을 선택 하 여 찾을 수 있습니다.

[![](jenkins-walkthrough-images/image13.png "The global settings for a plugin can be found by selecting Jenkins / Manage Jenkins / Configure System from the upper left hand corner")](jenkins-walkthrough-images/image13.png#lightbox)

이 메뉴 옵션을 선택 하면 **시스템 구성 [Jenkins]** 페이지로 이동 됩니다. 이 페이지에는 Jenkins 자체를 구성 하 고 일부 전역 플러그 인 값을 설정 하는 섹션이 포함 되어 있습니다.  아래 스크린샷에서는이 페이지의 예를 보여 줍니다.

[![](jenkins-walkthrough-images/image14.png "This screenshot illustrates an example of this page")](jenkins-walkthrough-images/image14.png#lightbox)

#### <a name="configuring-the-msbuild-plugin"></a>MSBuild 플러그 인 구성

**/Library/Frameworks/Mono.framework/Commands/xbuild** 를 사용 하 여 Mac용 Visual Studio 솔루션 및 프로젝트 파일을 컴파일하려면 MSBuild 플러그 인을 구성 해야 합니다. 아래 스크린샷에서와 같이 **MSBuild 추가** 단추가 나타날 때까지 **시스템 구성 [Jenkins]** 페이지 아래로 스크롤합니다.

 [![](jenkins-walkthrough-images/image15.png "Scroll down the Configure System Jenkins page until the Add MSBuild button appears")](jenkins-walkthrough-images/image15.png#lightbox)

이 단추를 클릭 하 고 표시 되는 폼에서 **MSBuild** 필드의 **이름과** **경로** 를 채웁니다. **Msbuild 설치의** 이름은 의미가 있어야 하 고, **msbuild의 경로** 는 `xbuild`에 대 한 경로 (일반적으로 **/Library/Frameworks/Mono.framework/Commands/xbuild**) 여야 합니다. 페이지 맨 아래에 있는 저장 또는 적용 단추를 클릭 하 여 변경 내용을 저장 한 후에는 Jenkins를 `xbuild` 사용 하 여 솔루션을 컴파일할 수 있습니다.

#### <a name="configuring-the-tfs-plugin"></a>TFS 플러그 인 구성

소스 코드 제어에 TFS를 사용 하려는 경우이 섹션은 필수입니다.

MacOS 워크스테이션이 TFS 서버와 상호 작용 하도록 하려면 [Team Explorer Everywhere](https://docs.microsoft.com/azure/devops/java/download-eclipse-plug-in/) 워크스테이션에 설치 되어 있어야 합니다. Team Explorer Everywhere는 TFS 액세스를 위한 플랫폼 간 명령줄 클라이언트가 포함 된 Microsoft의 도구 집합입니다. Team Explorer Everywhere Microsoft에서 다운로드 하 여 세 단계로 설치할 수 있습니다.

1. 사용자 계정에 액세스할 수 있는 디렉터리에 보관 파일의 압축을 풉니다. 예를 들어 **~/tee**로 파일의 압축을 풀 수 있습니다.
2. 위의 1 단계에서 압축을 푼 파일을 보관 하는 폴더를 포함 하도록 셸 또는 시스템 경로를 구성 합니다. 예를 들어 입니다.

    ```
    echo export PATH~/tee/:$PATH' >> ~/.bash_profile
    ```

3. Team Explorer Everywhere 설치 되어 있는지 확인 하려면 터미널 세션을 열고 `tf` 명령을 실행 합니다. Tf가 올바르게 구성 된 경우 터미널 세션에 다음과 같은 출력이 표시 됩니다.

    ```
    $ tf
    Team Explorer Everywhere Command Line Client (version 11.0.0.201306181526)

    Available commands and their options:
    ```

TFS에 대 한 명령줄 클라이언트를 설치한 후에는 `tf` 명령줄 클라이언트의 전체 경로를 사용 하 여 Jenkins를 구성 해야 합니다. 다음 스크린샷에 표시 된 것 처럼 Team Foundation Server 섹션을 찾을 때까지 **시스템 구성 [Jenkins]** 페이지 아래로 스크롤합니다.

[![](jenkins-walkthrough-images/image17.png "Scroll down the Configure System Jenkins page until you find the Team Foundation Server section")](jenkins-walkthrough-images/image17.png#lightbox)

`tf` 명령의 전체 경로를 입력 하 고 **저장** 단추를 클릭 합니다.

### <a name="configure-jenkins-security"></a>Jenkins 보안 구성

처음 설치 하는 경우 Jenkins는 보안을 사용 하지 않도록 설정 되어 있으므로 모든 사용자가 익명으로 모든 종류의 작업을 설정 하 고 실행할 수 있습니다. 이 섹션에서는 Jenkins 사용자 데이터베이스를 사용 하 여 인증 및 권한 부여를 구성 하는 보안을 구성 하는 방법을 설명 합니다.

보안 설정은 다음 스크린샷에 표시 된 것 처럼 **Jenkins > Manage > Jenkins**를 선택 하 고 전역 보안 구성을 선택 하 여 찾을 수 있습니다.

[![](jenkins-walkthrough-images/image18.png "Security settings can be found by selecting Jenkins / Manage Jenkins / Configure Global Security")](jenkins-walkthrough-images/image18.png#lightbox)

**전역 보안 구성** 페이지에서 **보안 사용** 확인란을 선택 하면 다음 스크린샷 처럼 **Access Control** 양식이 표시 됩니다.

[![](jenkins-walkthrough-images/image19.png "On the Configure Global Security page, check the Enable Security checkbox and the Access Control form should appear, similar to this screenshot")](jenkins-walkthrough-images/image19.png#lightbox)

다음 스크린샷에 표시 된 것 처럼 **보안 영역 섹션**에서 **Jenkins ' 자체 사용자 데이터베이스 '** 의 라디오 단추를 설정/해제 하 고 **사용자가 등록 하도록 허용** 도 선택 되어 있는지 확인 합니다.

[![](jenkins-walkthrough-images/image20.png "Toggle the radio button for Jenkins own user database in the Security Realm Section, and ensure that Allow users to sign up is also checked")](jenkins-walkthrough-images/image20.png#lightbox)

마지막으로 Jenkins를 다시 시작 하 고 새 계정을 만듭니다. 생성 되는 첫 번째 계정은 루트 계정이 며이 계정은 관리자에 게 자동으로 승격 됩니다. **전역 보안 구성** 페이지로 다시 이동 하 여 **행렬 기반 보안** 라디오 단추를 선택 합니다. 다음 스크린샷에 표시 된 것 처럼 루트 계정에 모든 권한을 부여 하 고 익명 계정에 읽기 전용 액세스 권한을 부여 해야 합니다.

[![](jenkins-walkthrough-images/image21.png "The root account should be granted full access, and the anonymous account should be given read-only access")](jenkins-walkthrough-images/image21.png#lightbox)

이러한 설정을 저장 하 고 Jenkins를 다시 시작 하면 보안이 켜 집니다.

#### <a name="disabling-security"></a>보안 해제

잊어버린 암호나 Jenkins 잠금의 경우 다음 단계를 수행 하 여 보안을 해제할 수 있습니다.

1. Jenkins를 중지 합니다. Jenkins를 사용 하는 경우 Dock의 Jenkins 아이콘을 마우스 오른쪽 단추로 클릭 하 고 팝업 된 메뉴에서 끝내기를 선택 하 여이 작업을 수행할 수 있습니다.

    ![Dock의 앱 아이콘을 선택 하 고 팝업 메뉴에서 끝내기를 선택 합니다.](jenkins-walkthrough-images/image19.png)

2. 텍스트 편집기에서 **~/.jenkins/config.xml** 파일을 엽니다.
3. `<usesecurity></usesecurity>` 요소의 값을 `true`에서 `false`으로 변경 합니다.
4. 파일에서 `<authorizationstrategy></authorizationstrategy>` 및 `<securityrealm></securityrealm>` 요소를 삭제 합니다.
5. Jenkins를 다시 시작 합니다.

## <a name="setting-up-a-job"></a>작업 설정

최상위 수준에서 Jenkins는 소프트웨어를 *작업*에 빌드하는 데 필요한 다양 한 작업을 모두 구성 합니다. 또한 작업에는 연결 된 메타 데이터가 포함 되어 있습니다. 여기에는 소스 코드를 가져오는 방법, 빌드를 실행 하는 빈도, 빌드에 필요한 특수 변수, 빌드가 실패할 경우 개발자에 게 알리는 방법 등 빌드에 대 한 정보가 제공 됩니다.

다음 스크린샷에 표시 된 것 처럼 오른쪽 위 모퉁이의 메뉴에서 **Jenkins > 새 작업** 을 선택 하 여 작업을 만듭니다.

![](jenkins-walkthrough-images/image22.png "Jobs are created by selecting Jenkins  New Job from the menu in the upper right hand corner")

그러면 **새 작업 [Jenkins]** 페이지가 표시 됩니다. 작업 이름을 입력 하 고 **자유 스타일 소프트웨어 프로젝트 빌드** 라디오 단추를 선택 합니다. 다음 스크린샷에서는이에 대 한 예를 보여 줍니다.

![](jenkins-walkthrough-images/image23.png "Enter a name for the job, and select the Build a free-style software project radio button")

**확인** 단추를 클릭 하 여 작업에 대 한 구성 페이지를 표시 합니다. 이는 다음 스크린샷에와 비슷합니다.

![](jenkins-walkthrough-images/image24.png "This should resemble this screenshot")

Jenkins는 다음 경로에 있는 하드 디스크의 디렉터리에서 작업을 구성 합니다. **~/.jenkins/jobs/[작업 이름]**

이 폴더에는 로그, 구성 파일 및 컴파일해야 하는 소스 코드와 같이 작업과 관련 된 모든 파일 및 아티팩트가 포함 됩니다.

초기 작업을 만든 후에는 다음 중 하나 이상으로 구성 해야 합니다.

- 소스 코드 관리 시스템을 지정 해야 합니다.
- 하나 이상의 *빌드 작업* 을 프로젝트에 추가 해야 합니다. 응용 프로그램을 빌드하는 데 필요한 단계 또는 작업입니다.
- 작업에는 한 개의 *빌드 트리거* (코드를 검색 하 고 최종 프로젝트를 Jenkins 하는 빈도를 알려 주는 일련의 명령)가 할당 되어야 합니다.

### <a name="configuring-source-code-control"></a>소스 코드 제어 구성

첫 번째 작업 Jenkins는 소스 코드 관리 시스템에서 소스 코드를 검색 합니다. Jenkins는 현재 제공 되는 인기 있는 많은 소스 코드 관리 시스템을 지원 합니다. 이 섹션에서는 두 가지 인기 있는 시스템, Git 및 Team Foundation Server에 대해 설명 합니다. 이러한 각 소스 코드 관리 시스템에 대해서는 아래 섹션에서 자세히 설명 합니다.

#### <a name="using-git-for-source-code-control"></a>소스 코드 제어에 Git 사용

소스 코드 제어에 TFS를 사용 하는 경우이 섹션을 [건너뛰고](#using-tfs-for-source-code-management) tfs를 사용 하 여 다음 섹션으로 이동 합니다.

Jenkins는 즉시 Git를 지원 합니다. 추가 플러그 인은 필요 하지 않습니다. Git를 사용 하려면 **git** 라디오 단추를 클릭 하 고 다음 스크린샷에 표시 된 것 처럼 GIT 리포지토리의 URL을 입력 합니다.

![](jenkins-walkthrough-images/image25.png "To use Git, click on the Git radio button and enter the URL for the Git repository")

변경 내용이 저장 되 면 Git 구성이 완료 됩니다.

#### <a name="using-tfs-for-source-code-management"></a>소스 코드 관리에 TFS 사용

이 섹션은 TFS 사용자 에게만 적용 됩니다.

**Team Foundation Server** 라디오 단추를 클릭 하면 다음 스크린샷에 표시 된 것과 유사한 TFS 구성 섹션이 표시 됩니다.

![](jenkins-walkthrough-images/image26.png "Click on the Team Foundation Server radio button and the TFS configuration section should appear")

TFS에 필요한 정보를 제공 합니다. 다음 스크린샷에서는 완료 된 양식의 예를 보여 줍니다.

![](jenkins-walkthrough-images/image27.png "This screenshot shows an example of the completed form")

#### <a name="testing-the-source-code-control-configuration"></a>소스 코드 제어 구성 테스트

적절 한 소스 코드 컨트롤이 구성 되 면 **저장** 을 클릭 하 여 변경 내용을 저장 합니다. 이렇게 하면 작업의 홈 페이지로 돌아가 다음 스크린샷에 표시 됩니다.

![](jenkins-walkthrough-images/image28.png "This will return you to the home page for the job, which will resemble this screenshot")

소스 코드 컨트롤이 적절 하 게 구성 되었는지 확인 하는 가장 간단한 방법은 지정 된 빌드 작업이 없더라도 수동으로 빌드를 트리거하는 것입니다. 수동으로 빌드를 시작 하려면 아래 스크린샷에 표시 된 것 처럼 작업의 홈 페이지에 왼쪽의 메뉴에 있는 **지금 빌드** 링크가 있습니다.

![](jenkins-walkthrough-images/image29.png "To start a build manually, the home page of the job has a Build Now link in the menu on the left hand side")

빌드가 시작 되 면 빌드 기록 대화 상자에 다음 스크린샷에서 같이 깜박이는 파란색 원, 진행률 표시줄, 빌드 번호 및 빌드가 시작 된 시간이 표시 됩니다.

![](jenkins-walkthrough-images/image30.png "When a build has been started, the Build History dialog displays a flashing blue circle, a progress bar, the build number and the time that the build started")

작업이 성공 하면 파란색 원이 표시 됩니다. 작업이 실패 하면 빨간색 원이 표시 됩니다.

빌드의 일부로 발생할 수 있는 문제를 해결 하는 데 도움이 되도록 Jenkins은 작업에 대 한 모든 콘솔 출력을 캡처합니다. 콘솔 출력을 보려면 **빌드 기록**에서 작업을 클릭 한 다음 왼쪽 메뉴의 **콘솔 출력** 링크를 클릭 합니다. 다음 스크린샷에서는 성공한 작업의 출력 뿐만 아니라 **콘솔 출력** 링크를 보여 줍니다.

![](jenkins-walkthrough-images/image31.png "This screenshot shows the Console Output link, as well as some of the output from a successful job")

#### <a name="location-of-build-artifacts"></a>빌드 아티팩트의 위치

Jenkins는 전체 소스 코드를 *작업 영역*이라는 특수 폴더에 검색 합니다. 이 디렉터리는 다음 위치의 폴더 내에서 찾을 수 있습니다.

```
~/.jenkins/jobs/[JOB NAME]/workspace
```

작업 영역에 대 한 경로는 `$WORKSPACE`이라는 환경 변수에 저장 됩니다.

작업의 방문 페이지로 이동한 다음 왼쪽 메뉴의 **작업 영역** 링크를 클릭 하 여 Jenkins의 작업 영역 폴더를 찾아볼 수 있습니다. 다음 스크린샷은 **HelloWorld**이라는 작업의 작업 영역 예를 보여 줍니다.

![](jenkins-walkthrough-images/image32.png "This screenshot shows an example of the workspace for a job named HelloWorld")

### <a name="build-triggers"></a>빌드 트리거

Jenkins에서 빌드를 시작 하기 위한 여러 가지 전략이 있습니다. 이러한 전략을 *빌드 트리거*라고 합니다. 빌드 트리거를 사용 하면 Jenkins에서 작업을 시작 하 고 프로젝트를 빌드하는 시기를 결정할 수 있습니다. 보다 일반적인 빌드 트리거 중 두 가지는 다음과 같습니다.

- **정기적으로 빌드** –이 트리거는 Jenkins에서 2 시간 마다 또는 평일 자정에 지정 된 간격으로 작업을 시작 하도록 합니다. 소스 코드 리포지토리에 변경 내용이 있는지 여부에 관계 없이 빌드가 시작 됩니다.
- **SCM 폴링** –이 트리거는 소스 코드 제어를 정기적으로 폴링합니다. 변경 내용이 소스 코드 리포지토리에 커밋되면 Jenkins에서 새 빌드를 시작 합니다.

폴링 SCM은 개발자가 빌드를 중단 시키는 변경 내용을 커밋할 때 빠른 피드백을 제공 하므로 인기 있는 트리거입니다. 이는 최근에 커밋된 일부 코드에서 문제가 발생 하 고 있음을 알리는 경고 팀에 유용 하며, 변경 내용이 적용 되는 동안 개발자가 문제를 해결할 수 있도록 합니다.

정기적 빌드는 테스터에 게 배포할 수 있는 응용 프로그램의 버전을 만드는 데 주로 사용 됩니다. 예를 들어, QA 팀의 멤버가 이전 주 작업을 테스트할 수 있도록 금요일 저녁에 대해 주기적인 빌드를 예약할 수 있습니다.

### <a name="compiling-a-xamarinios-applications"></a>Xamarin.ios 응용 프로그램 컴파일
Xamarin.ios 프로젝트는 `xbuild` 또는 `msbuild`를 사용 하 여 명령줄에서 컴파일할 수 있습니다. Shell 명령은 Jenkins를 실행 하는 사용자 계정의 컨텍스트에서 실행 됩니다. 응용 프로그램을 배포용으로 적절 하 게 패키징할 수 있도록 사용자 계정에 프로 비전 프로필에 대 한 액세스 권한이 있어야 합니다. 이 셸 명령을 작업 구성 페이지에 추가할 수 있습니다.

**빌드** 섹션까지 아래로 스크롤합니다. 다음 스크린샷에 표시 된 것 처럼 **빌드 단계 추가** 단추를 클릭 하 고 **셸 실행**을 선택 합니다.

![](jenkins-walkthrough-images/image33.png "Click the Add build step button and select Execute shell")

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

### <a name="building-a-xamarinandroid-project"></a>Xamarin Android 프로젝트 빌드

Xamarin Android 프로젝트를 빌드하는 것은 Xamarin.ios 프로젝트를 빌드하기 위한 개념과 매우 비슷합니다. Xamarin Android 프로젝트에서 APK를 만들려면 다음 두 단계를 수행 하도록 Jenkins을 구성 해야 합니다.

- MSBuild 플러그 인을 사용 하 여 프로젝트 컴파일
- 서명 하 고 압축 하 여 APK를 유효한 릴리스 키 저장소으로 정렬 합니다.

이 두 단계는 다음 두 섹션에서 자세히 설명 합니다.

### <a name="creating-the-apk"></a>APK 만들기

아래 스크린샷에 표시 된 것 처럼 **빌드 단계 추가** 단추를 클릭 하 고 **MSBuild를 사용 하 여 Visual Studio 프로젝트 또는 솔루션 빌드**를 선택 합니다.

![](jenkins-walkthrough-images/image36.png "Creating the APK  Click on the Add build step button, and select Build a Visual Studio project or solution using MSBuild")

빌드 단계가 프로젝트에 추가 되 면 표시 되는 양식 필드를 채웁니다. 다음 스크린샷은 완료 된 양식의 한 예입니다.

![](jenkins-walkthrough-images/image37.png "Once the build step is added to the project, fill in the form fields that appear")

이 빌드 단계는 **$WORKSPACE** 폴더에 `xbuild`를 실행 합니다. MSBuild 빌드 파일은 **xamarin.ios .csproj** 파일로 설정 됩니다. **명령줄 인수** 는 대상 **packageforandroid**의 릴리스 빌드를 지정 합니다. 이 단계의 제품은 다음 위치에 있는 APK가 됩니다.

```
$WORKSPACE/[PROJECT NAME]/bin/Release
```

다음 스크린샷에서는이 APK의 예를 보여 줍니다.

![](jenkins-walkthrough-images/image38.png "This screenshot shows an example of this APK")

이 APK는 개인 키 저장소로 서명 되지 않은 경우에는 배포할 준비가 되지 않았으므로 zip으로 정렬 되어야 합니다.

#### <a name="signing-and-zipaligning-the-apk-for-release"></a>릴리스에 대 한 APK 서명 및 Zipalign

APK에 서명 하 고 zipalign는 Android SDK에서 별도의 두 명령줄 도구를 통해 수행 되는 별도의 두 작업입니다. 그러나 한 번의 빌드 작업으로이 작업을 수행 하는 것이 편리 합니다. APK에 서명 하 고 zipalign 하는 방법에 대 한 자세한 내용은 Android 응용 프로그램을 릴리스할 준비에 대 한 Xamarin 설명서를 참조 하세요.

이러한 명령에는 프로젝트와 프로젝트에 따라 다를 수 있는 명령줄 매개 변수가 필요 합니다. 또한 이러한 명령줄 매개 변수 중 일부는 빌드를 실행 하는 경우 콘솔 출력에 표시 되지 않아야 하는 암호입니다. 이러한 명령줄 매개 변수 중 일부는 환경 변수에 저장 합니다. 서명 및/또는 zip 정렬에 필요한 환경 변수는 아래 표에 설명 되어 있습니다.

|환경 변수|설명|
|--- |--- |
|KEYSTORE_FILE|APK에 서명 하기 위한 키 저장소 경로입니다.|
|KEYSTORE_ALIAS|APK에 서명 하는 데 사용 되는 키 저장소의 키입니다.|
|INPUT_APK|`xbuild`에서 만든 APK입니다.|
|SIGNED_APK|`jarsigner`에서 생성 한 서명 된 APK입니다.|
|FINAL_APK|`zipalign`에서 생성 되는 zip 정렬 된 APK입니다.|
|STORE_PASS|노래에 대 한 키 저장소의 콘텐츠에 액세스 하는 데 사용 되는 암호입니다.|

요구 사항 섹션에 설명 된 대로 EnvInject 플러그 인을 사용 하 여 빌드하는 동안 이러한 환경 변수를 설정할 수 있습니다. 다음 스크린샷에 표시 된 것 처럼 작업에는 삽입 환경 변수를 기반으로 하 여 새 빌드 단계가 추가 되어야 합니다.

![](jenkins-walkthrough-images/image39.png "The job should have a new build step added based on the Inject environment variables")

표시 되는 **속성 콘텐츠** 양식 필드에서 환경 변수는 줄 마다 하나씩 다음 형식으로 추가 됩니다.

```
ENVIRONMENT_VARIABLE_NAME = value
```

다음 스크린샷은 APK에 서명 하는 데 필요한 환경 변수를 보여 줍니다.

![](jenkins-walkthrough-images/image40.png "This screenshot shows the environment variables that are required for signing the APK")

APK 파일에 대 한 일부 환경 변수는 `WORKSPACE` 환경 변수를 기반으로 합니다.

최종 환경 변수는 키 저장소: `STORE_PASS`의 내용에 액세스 하는 데 사용 하는 암호입니다. 암호는 로그 파일에서 가려져 있거나 생략 해야 하는 중요 한 값입니다. EnvInject 플러그 인은 로그에 표시 되지 않도록 이러한 값을 보호 하도록 구성할 수 있습니다.

작업 구성의 **빌드** 섹션 바로 앞에는 **빌드 환경** 섹션이 있습니다. **암호 삽입** 확인란이 전환 되 면 일부 양식 필드가 표시 됩니다. 이러한 양식 필드는 환경 변수의 이름과 값을 캡처하는 데 사용 됩니다. 다음 스크린샷은 `STORE_PASS` 환경 변수를 추가 하는 예입니다.

![](jenkins-walkthrough-images/image41.png "This screenshot is an example of adding the STOREPASS environment variable")

환경 변수가 초기화 되 면 다음 단계는 APK에 서명 하 고 압축 하는 빌드 단계를 추가 하는 것입니다. 환경 변수를 삽입 하는 빌드 단계 바로 다음에는 `jarsigner` 및 `zipalign`를 실행 하는 또 다른 **실행 셸** 명령 빌드가 있습니다. 다음 코드 조각과 같이 각 명령은 한 줄을 사용 합니다.

```
jarsigner -verbose -sigalg MD5withRSA -digestalg SHA1 -keystore $KEYSTORE_FILE -storepass $STORE_PASS -signedjar $SIGNED_APK $INPUT_APK $KEYSTORE_ALIAS
zipalign -f -v 4 $SIGNED_APK $FINAL_APK
```

다음 스크린샷은 `jarsigner`를 입력 하 고 단계에 명령을 `zipalign` 하는 방법의 예를 보여 줍니다.

![](jenkins-walkthrough-images/image42.png "This screenshot shows an example of how to enter the jarsigner and zipalign commands into the step")

모든 빌드 작업이 준비 되 면 수동 빌드를 트리거하여 모든 것이 작동 하는지 확인 하는 것이 좋습니다. 빌드에 실패 하는 경우 빌드가 실패 한 원인에 대 한 정보를 보려면 **콘솔 출력** 을 검토 해야 합니다.

### <a name="submitting-tests-to-test-cloud"></a>Test Cloud에 테스트 제출

셸 명령을 사용 하 여 자동화 된 테스트를 Test Cloud에 제출할 수 있습니다. Xamarin Test Cloud에서 테스트 실행을 설정 하는 방법에 대 한 자세한 내용은 [Xamarin Android 앱 준비](/appcenter/test-cloud/preparing-for-upload/xamarin-android-uitest) 및 [xamarin.ios 앱 준비](/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest)를 참조 하세요.

## <a name="summary"></a>요약

이 가이드에서는 macOS에서 빌드 서버로 Jenkins를 도입 하 고, 릴리스를 위해 Xamarin 모바일 응용 프로그램을 컴파일하고 준비 하도록 구성 했습니다. 빌드 프로세스를 지원 하기 위해 여러 플러그인과 함께 macOS 컴퓨터에 Jenkins가 설치 되었습니다. TFS 또는 Git에서 코드를 끌어올 작업을 만들고 구성한 다음 릴리스 준비 응용 프로그램으로 해당 코드를 컴파일합니다. 작업 실행 시기를 예약 하는 두 가지 방법에 대해서도 살펴봅니다.

## <a name="related-links"></a>관련 링크

- [연속 통합](~/tools/ci/index.md)
- [App Center 테스트](https://docs.microsoft.com/appcenter/test-cloud/)
