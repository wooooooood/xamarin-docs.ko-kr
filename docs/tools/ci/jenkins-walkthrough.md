---
title: Xamarin에서 Jenkins 사용
description: 이 문서에서는 Xamarin 응용 프로그램을 사용 하 여 연속 통합을 위한 Jenkins를 사용 하는 방법을 설명 합니다. 설치, 구성 및 Jenkins를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 1E6825DF-1254-4FCB-B94D-ADD33D1B5309
author: lobrien
ms.author: laobri
ms.date: 03/23/2017
ms.openlocfilehash: 2e6a75fa3c4c63e8dea402c6761f8ef753908540
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61047868"
---
# <a name="using-jenkins-with-xamarin"></a>Xamarin에서 Jenkins 사용

_이 가이드에서는 Jenkins 연속 통합 서버로 설정 및 Xamarin을 사용 하 여 만든 모바일 응용 프로그램을 컴파일 자동화 하는 방법을 보여 줍니다. OS X에서 Jenkins를 설치, 구성 및 변경 내용이 소스 코드 관리 시스템에 커밋될 때 Xamarin.iOS 및 Xamarin.Android 응용 프로그램을 컴파일하는 데 작업을 설정 하는 방법을 설명 합니다._

[Xamarin 사용 하 여 연속 통합 소개](~/tools/ci/intro-to-ci.md) 중단 되었거나 호환 되지 않는 코드의 조기 경고를 제공 하는 유용한 소프트웨어 개발 방식으로 연속 통합을 소개 합니다. CI에서는 배포에 대 한 적절 한 상태에서 소프트웨어를 유지 하 고, 발생할 개발자가 문제를 해결 하 고 문제를 허용 합니다. 이 연습에서는 두 문서에서 콘텐츠를 함께 사용 하는 방법에 설명 합니다.

이 가이드에는 OS X를 실행 하는 전용된 컴퓨터에 Jenkins를 설치 하 고 컴퓨터를 시작할 때 자동으로 실행 되도록 구성 하는 방법을 보여 줍니다. Jenkins 설치 되 면 MS 빌드를 지원 하기 위해 추가 플러그 인을 설치 하겠습니다. Jenkins는 기본적으로 Git를 지원합니다. 소스 코드 제어를 위한 TFS 사용 중인 경우에 추가 플러그 인 및 명령줄 유틸리티를 설치 합니다.

Jenkins가 구성 하 고 필요한 플러그 인 설치 된 Xamarin.iOS 및 Xamarin.Android 프로젝트를 컴파일하는 데 하나 이상의 작업을 만듭니다. 작업은 단계 및 일부 작업을 수행 하는 데 필요한 메타 데이터 컬렉션입니다. 작업 중 일반적으로 구성 됩니다.

- **소스 코드 관리 (SCM)** – Jenkins 구성 파일에서 소스 코드 제어에 연결 하는 방법 및 검색할 파일 내용에 대 한 정보를 포함 하는 메타 데이터 항목입니다.
- **트리거** – 트리거는 개발자 변경 내용을 소스 코드 리포지토리로 커밋합니다 하는 경우와 같은 특정 작업을 기반으로 하는 작업을 시작 하는 데 사용 됩니다.
- **빌드 지침** – 플러그 인 또는 소스 코드를 컴파일 및 모바일 장치에 설치할 수 있는 이진 파일을 생성 하는 스크립트입니다.
- **선택적 빌드 작업** -단위 테스트 실행, 코드 서명 하는 코드에 대 한 정적 분석 수행 포함 될 수 또는 관련된 작업을 빌드 다른 수행 하는 다른 작업을 시작 합니다.
- **알림** – 작업을 몇 가지 종류의 빌드 상태에 대 한 알림 보낼 수 있습니다.
- **보안** -선택적으로 Jenkins 보안 기능을 사용도 것이 좋습니다.

이 가이드에서는 이러한 각 지점 다루는 Jenkins 서버를 설정 하는 방법을 안내 합니다. 엔드에서 설정 IPA를 만들고 Xamarin 모바일 프로젝트에 대 한 APK의 Jenkins를 구성 하는 방법 이해가 있어야 합니다.

## <a name="requirements"></a>요구 사항

이상적인 빌드 서버를 구축 하 고 가능한 경우 응용 프로그램을 테스트 하는 용도로 전용 독립 실행형 컴퓨터. 전용된 컴퓨터 아티팩트 (예: 웹 서버는) 다른 역할에 대해 필요할 수 있는 빌드를 묻지 않도록 보장 합니다. 예를 들어, 빌드 서버도 역할을 하는 웹 서버, 웹 서버에는 충돌 하는 버전의 일부 공용 라이브러리 필요할 수 있습니다. 이 충돌로 인해 웹 서버 수 제대로 작동 하지 또는 Jenkins 사용자에 게 배포 하는 경우 작동 하지 않는 빌드를 만들 수 있습니다.

Xamarin 모바일 앱에 대 한 빌드 서버는 개발자의 워크스테이션와 매우 비슷하게 설정 됩니다. Mac 용 Visual Studio는 Jenkins 사용자 계정에 및 Xamarin.iOS 및 Xamarin.Android 설치 됩니다. 모든 코드 서명 인증서, 프로필 및 키 저장소를 프로 비전도 설치 되어야 합니다. *일반적으로 빌드 서버의 사용자 계정이 개발자 계정에서 별도-모든 소프트웨어, 키 및 빌드 서버 사용자 계정으로 로그인 하는 동안 인증서 설치 및 구성 해야 합니다.*

다음 다이어그램에서는 모든 일반적인 Jenkins 빌드 서버에서 이러한 요소를 보여 줍니다.

[![](jenkins-walkthrough-images/image1.png "이 다이어그램에 모든 일반적인 Jenkins 빌드 서버에서 이러한 요소")](jenkins-walkthrough-images/image1.png#lightbox)

만 iOS 응용 프로그램을 빌드 및 macOS를 실행 하는 컴퓨터에 로그온 수 있습니다. Mac 미니는 적절 한 저렴 한 비용 옵션, OS X 10.10 (Yosemite)를 실행할 수 있는 모든 컴퓨터 이상이 충분 합니다.

설치 하려는 경우 소스 코드 제어를 위한 TFS 사용 중인 [Team Explorer Everywhere](https://docs.microsoft.com/azure/devops/java/download-eclipse-plug-in/)합니다. Team Explorer Everywhere macOS에서 터미널에서 TFS에 플랫폼 간 액세스를 제공합니다.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="installing-jenkins"></a>Jenkins를 설치합니다.

Jenkins를 사용 하 여 첫 번째 태스크는 설치 하는 것입니다. OS X에서 Jenkins를 실행 하는 방법은 세 가지가 있습니다.

- 디먼, 백그라운드에서 실행 합니다.
- 내 Tomcat, Jetty, JBoss 등 서블릿 컨테이너입니다.
- 프로세스로 일반 사용자 계정으로 실행 합니다.

대부분의 기존 연속 통합 응용 프로그램은 백그라운드에서 디먼으로 실행 (OS x 또는 \*nix) 또는 as a service (Windows). 이 GUI 상호 작용이 필요 없는 위치 및 빌드 환경 설치 하는 쉽게 수행할 수 있는 시나리오에 적합 합니다. 모바일 앱은 키 저장소 및 Jenkins가 디먼으로 실행 하는 경우에 액세스 하려면 문제를 일으킬 수 있는 인증서를 서명 해야 합니다. 이러한 문제 때문에이 문서 빌드 서버의 Jenkins 사용자 계정으로 실행-세 번째 시나리오에 중점을 둡니다.

Jenkins.App는 Jenkins를 설치 하는 편리한 방법입니다. 이 시작을 간소화 하는 AppleScript 래퍼 및 Jenkins 서버의 중지 합니다. Bash 셸에서 실행 하는 대신 Jenkins 실행 앱으로 도크를 아이콘으로 다음 스크린샷에 표시 된 대로 합니다.

[![](jenkins-walkthrough-images/image2.png "Bash 셸에서 실행 하는 대신 Jenkins 실행 앱으로 아이콘에 도킹을 사용 하 여이 스크린샷에 표시 된 대로")](jenkins-walkthrough-images/image2.png#lightbox)

시작 또는 중지 Jenkins 간단 하 게 시작 또는 Jenkins.App를 중지 합니다.

Jenkins.App를 설치 하려면 아래 스크린샷에 나온 프로젝트의 다운로드 페이지에서 최신 버전을 다운로드 합니다.

[![](jenkins-walkthrough-images/image3.png "앱 프로젝트에서 최신 버전 다운로드 페이지에서이 스크린샷에 나온 다운로드")](jenkins-walkthrough-images/image3.png#lightbox)

Zip 파일을 추출 합니다 `/Applications` 폴더에 빌드 서버와 마찬가지로 모든 다른 OS X 응용 프로그램 시작 합니다.
Jenkins.App를 시작 하면 처음으로 Jenkins 다운로드를 알리는 대화 상자가 표시 됩니다.

[![](jenkins-walkthrough-images/image4.png "Jenkins는 다운로드를 알리는 대화 상자가 제공 앱")](jenkins-walkthrough-images/image4.png#lightbox)

Jenkins.App 다운로드 완료 되 면 다른 대화 묻는 경우 원하는 Jenkins을 시작 하는 사용자 지정 하려면 다음 스크린샷에 표시 된 것으로 표시 됩니다.

[![](jenkins-walkthrough-images/image5.png "앱 다운로드를 완료 하려는 경우 Jenkins을 시작 하는 사용자 지정 하려면이 스크린샷에 표시 된 것 처럼 요청 하는 다른 대화 상자가 표시 됩니다 것,")](jenkins-walkthrough-images/image5.png#lightbox)

Jenkins를 사용자 지정은 선택적 이며 대부분의 경우에 Jenkins 작동에 대 한 앱이 시작할 때마다 – 기본 설정을 수행할 필요가 없습니다.

Jenkins를 사용자 지정 하는 데 필요한 경우 클릭 합니다 **기본값을 변경할** 단추입니다. 이 두 개의 연속 된 대화 상자가 표시 됩니다: Java 명령줄 매개 변수를 요청 하는 하나 및 다른 Jenkins 명령줄 매개 변수를 요청 합니다. 다음 두 스크린샷에서 이러한 두 대화 상자를 보여 줍니다.

[![](jenkins-walkthrough-images/image6.png "대화 상자를 보여 주는이 스크린샷")](jenkins-walkthrough-images/image6.png#lightbox)

[![](jenkins-walkthrough-images/image7.png "대화 상자를 보여 주는이 스크린샷")](jenkins-walkthrough-images/image7.png#lightbox)

Jenkins가 실행 되 면 컴퓨터에 사용자 로그인 될 때마다 시작 됩니다 있도록 로그인 항목으로 설정 하는 것이 좋습니다. 도킹 스테이션에서 Jenkins 아이콘을 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 이렇게 **옵션... > 로그인 라도**다음 스크린샷에 표시 된 것 처럼:

[![](jenkins-walkthrough-images/image8.png "이 스크린샷에 표시 된 대로 도킹 스테이션에서 Jenkins 아이콘을 마우스 오른쪽 단추로 클릭 하 고 로그인 시 OptionsOpen를 선택 하 여 이렇게 수 있습니다.")](jenkins-walkthrough-images/image8.png#lightbox)

이렇게 하면 자동으로 실행 될 때마다 Jenkins.App 사용자가 로그인 하지만 컴퓨터가 부팅 될 때 되지 않습니다. OS X가 부팅 시 자동으로 로그인 하는 데 사용할 사용자 계정을 지정 하는 것이 가능 합니다. 열기는 **시스템 기본 설정**, 선택한 합니다 **사용자 및 그룹** 이 스크린샷에 표시 된 대로 아이콘:

[![](jenkins-walkthrough-images/image9.png "시스템 기본 설정을 열고이 스크린샷에 표시 된 대로 사용자 그룹 아이콘을 선택 합니다.")](jenkins-walkthrough-images/image9.png#lightbox)

클릭 합니다 **로그인 옵션** 단추를 선택한 다음 OS X에서 부팅 시 로그인에 사용할 계정을 선택 합니다.

이 시점에서 Jenkins가 설치 되었습니다. 그러나 Xamarin 모바일 응용 프로그램을 구축 하고자 하는 경우 일부 플러그 인을 설치 해야 합니다.

### <a name="installing-plugins"></a>플러그 인 설치

Jenkins.App 설치 관리자 완료 되 면 Jenkins를 시작 하 고 URL 사용 하 여 웹 브라우저를 시작할 http://localhost:8080아래 스크린샷에 표시 된 것 처럼:

[![](jenkins-walkthrough-images/image10.png "이 스크린샷에 표시 된 대로 8080")](jenkins-walkthrough-images/image10.png#lightbox)

이 페이지에서 선택 **Jenkins > Jenkins 관리 > 플러그 인 관리** 아래 스크린샷에 표시 된 것 처럼 왼쪽 위 모퉁이에 있는 메뉴에서:

[![](jenkins-walkthrough-images/image11.png "이 페이지에서 왼쪽 위 모서리의 메뉴에서 Jenkins 관리 Jenkins 플러그 인 관리 선택")](jenkins-walkthrough-images/image11.png#lightbox)

이 표시 됩니다는 **Jenkins 플러그 인 관리자** 페이지입니다. 사용 가능한 탭을 클릭 하면 다운로드 및 설치할 수는 600 개가 넘는 플러그 인 목록이 표시 됩니다. 이 옵션은 아래 스크린샷에 표시 되어:

[![](jenkins-walkthrough-images/image12.png "다운로드 및 설치할 수는 600 개가 넘는 플러그인 목록을 표시는 사용할 수 있는 탭을 클릭 하면")](jenkins-walkthrough-images/image12.png#lightbox)

스크롤 모든 600 플러그 인 몇 가지 작업은 지루할 수 찾기 오류가 발생 하기 쉽습니다. Jenkins는 인터페이스의 오른쪽 위 모서리에서 필터 검색 필드를 제공합니다. 이 필터 필드를 사용 하 여 검색 찾기 및 설치 된 하나 또는 모든 플러그 인을 간소화 됩니다.

- **Jenkins MSBuild 플러그 인** –이 플러그 인을 사용 하면 Mac 솔루션 (.sln) 및 프로젝트 (.csproj)에 대 한 Visual Studio 및 Visual Studio를 빌드할 수 있습니다.
- **환경 인젝터 플러그 인** – 작업에서 환경 변수를 설정 하 고 빌드 수준 수 있도록 하는 선택 사항 이지만 유용한 플러그 인입니다. 또한 응용 프로그램에 서명할 코드에 사용 되는 암호와 같은 변수에 대 한 추가 보호를 제공 합니다. 으로 약어 되기도 합니다 *EnvInject 플러그 인* 합니다.
- **Team Foundation Server 플러그 인** –만 되는 선택적 플러그 인이는 Team Foundation Server 또는 소스 코드 제어에 대 한 Team Foundation Services를 사용 중인 경우 필요 합니다.

Jenkins는 플러그 인 추가 하지 않고 Git를 지원합니다.

모든 플러그 인을 설치한 후 Jenkins를 다시 시작 하 여 각 플러그 인에 대 한 전역 설정을 구성 합니다. 플러그 인에 대 한 전역 설정을 선택 하 여 찾을 수 있습니다 **Jenkins > Jenkins 관리 > Configure System** 아래 스크린샷에 표시 된 것 처럼 왼쪽 위 모퉁이에서.

[![](jenkins-walkthrough-images/image13.png "Jenkins를 선택 하 여 플러그 인에 대 한 전역 설정을 찾을 수 있습니다 / Manage Jenkins 구성 시스템에서 왼쪽 위 모퉁이 전달 하는 /")](jenkins-walkthrough-images/image13.png#lightbox)

이 메뉴 옵션을 선택 하면 돌아갑니다 합니다 **시스템 구성 [Jenkins]** 페이지입니다. 이 페이지에 자체 Jenkins를 구성 하 고 일부 전역 플러그 인 값을 설정 하는 섹션이 있습니다.  다음 스크린샷은이 페이지의 예제를 보여 줍니다.

[![](jenkins-walkthrough-images/image14.png "이 스크린샷에서이 페이지의 예제를 보여 줍니다.")](jenkins-walkthrough-images/image14.png#lightbox)

#### <a name="configuring-the-msbuild-plugin"></a>MSBuild 플러그 인 구성

MSBuild 플러그 인을 사용 하도록 구성 해야 합니다 **/Library/Frameworks/Mono.framework/Commands/xbuild** Mac 솔루션 및 프로젝트 파일에 대 한 Visual Studio를 컴파일할 수 있습니다. 아래로 스크롤하여는 **시스템 구성 [Jenkins]** 될 때까지 페이지를 **추가 MSBuild** 아래 스크린샷에 표시 된 것과 같이 단추가 나타납니다.

 [![](jenkins-walkthrough-images/image15.png "MSBuild 추가 단추를 나타날 때까지 시스템 Jenkins 구성 페이지를 아래로 스크롤하여")](jenkins-walkthrough-images/image15.png#lightbox)

이 단추를 클릭 하 고 채울 합니다 **이름을** 및 **경로** 에 **MSBuild** 나타나는 양식의 필드입니다. 이름에 **MSBuild** 의미 하는 동안 설치 해야 합니다 **MSBuild에 대 한 경로** 에 대 한 경로 여야 합니다 `xbuild`, 일반적으로 **/Library 프레임 워크 / Mono.framework/Commands/xbuild**합니다. 변경 내용을 저장 또는 페이지의 맨 아래에서 [적용] 단추를 클릭 하 여 저장, Jenkins 사용할 수 있게 됩니다 `xbuild` 솔루션을 컴파일할 수 있습니다.

#### <a name="configuring-the-tfs-plugin"></a>TFS 플러그 인 구성

이 섹션은 소스 코드 제어에 대 한 TFS를 사용 하려는 경우에 필수입니다.

TFS 서버와 상호 작용 하는 macOS 워크스테이션에 대 한 순서로 [Team Explorer Everywhere](https://docs.microsoft.com/azure/devops/java/download-eclipse-plug-in/) 워크스테이션에 설치 해야 합니다. Team Explorer Everywhere는 TFS에 액세스 하기 위한 플랫폼 간 명령줄 클라이언트가 포함 된 Microsoft의 도구 집합입니다. Team Explorer Everywhere 수 Microsoft에서 다운로드 되어 설치 3 단계에 따라:

1. 사용자 계정에 액세스할 수 있는 디렉터리에 보관 파일을 압축을 풉니다. 예를 들어, 있습니다 수 파일 압축을 풉니다 **~/tee**합니다.
2. 위의 1 단계에서 압축 되지 않은 파일이 들어 있는 폴더를 포함 하도록 셸 또는 시스템 경로 구성 합니다. 예를 들면 다음과 같습니다.

    ```
    echo export PATH~/tee/:$PATH' >> ~/.bash_profile
    ```

3. Team Explorer Everywhere 설치 되어 있는지를 확인 하려면 터미널 세션을 열고 실행 합니다 `tf` 명령입니다. Tf가 제대로 구성 하는 경우 터미널 세션에서 다음과 같은 출력이 나타납니다.

    ```
    $ tf
    Team Explorer Everywhere Command Line Client (version 11.0.0.201306181526)

    Available commands and their options:
    ```

TFS 용 명령줄 클라이언트를 설치한 후 Jenkins의 전체 경로를 구성 해야 합니다는 `tf` 명령줄 클라이언트입니다. 아래로 스크롤하여 합니다 **시스템 구성 [Jenkins]** 다음 스크린샷에 표시 된 것과 같이 Team Foundation Server 섹션을 찾을 때까지 페이지:

[![](jenkins-walkthrough-images/image17.png "Team Foundation Server 섹션을 찾을 때까지 시스템 Jenkins 구성 페이지를 아래로 스크롤하여")](jenkins-walkthrough-images/image17.png#lightbox)

전체 경로를 입력 합니다 `tf` 명령을 실행 하 고 클릭 합니다 **저장** 단추입니다.

### <a name="configure-jenkins-security"></a>Jenkins 보안 구성

를 처음 설치 하면 Jenkins에서는 모든 사용자를 설정 하 고 모든 종류의 작업을 익명으로 실행 되므로 보안이 비활성화 됨, 합니다. 이 섹션에서는 인증 및 권한 부여를 구성 하려면 Jenkins 사용자 데이터베이스를 사용 하 여 보안을 구성 하는 방법에 설명 합니다.

보안 설정을 선택 하 여 찾을 수 있습니다 **Jenkins > Jenkins 관리 > 전역 보안 구성**이 스크린샷에 표시 된 것 처럼:

[![](jenkins-walkthrough-images/image18.png "Jenkins를 선택 하 여 보안 설정을 찾을 수 있습니다 / Jenkins 관리 / 전역 보안 구성")](jenkins-walkthrough-images/image18.png#lightbox)

에 **전역 보안 구성** 페이지를 확인 합니다 **보안을 사용 하도록 설정** 확인란을 선택 및 **Access Control** 폼 표시 됩니다, 다음 스크린샷과 유사 하 게:

[![](jenkins-walkthrough-images/image19.png "전역 보안 구성 페이지에서 보안을 사용 하도록 설정 확인 확인란을 선택 하 고 액세스 제어 형식 유사,이 스크린 샷")](jenkins-walkthrough-images/image19.png#lightbox)

라디오 단추를 설정/해제 **Jenkins의 사용자 데이터베이스** 에 **보안 영역 섹션**, 확인 **사용자가 등록 하도록 허용** 확인란도 선택 하면에 설명 된 대로 다음 스크린 샷:

[![](jenkins-walkthrough-images/image20.png "보안 영역 섹션에서는 Jenkins 자체 사용자 데이터베이스에 대 한 라디오 단추를 설정/해제 하 고 사용자가 등록할 수 있음도 선택 되어 있는지 확인 합니다.")](jenkins-walkthrough-images/image20.png#lightbox)

마지막으로 Jenkins를 다시 시작 하 고 새 계정을 만듭니다. 루트 계정이 고, 만든 첫 번째 계정 및이 계정 관리자에 게 자동으로 승격 됩니다. 로 다시 이동 합니다 **전역 보안 구성** 페이지 및 확인 합니다 **행렬 기반 보안** 라디오 단추. 루트 계정 전체 액세스를 부여 해야 하며, 익명 계정 주어져야 읽기 전용 액세스는 다음 스크린샷에 표시 된 대로:

[![](jenkins-walkthrough-images/image21.png "루트 계정 전체 액세스를 부여 해야 하며, 익명 계정 읽기 전용 액세스를 제공 해야")](jenkins-walkthrough-images/image21.png#lightbox)

이러한 설정을 저장할 Jenkins를 다시 시작 하 고 보안으로 설정 됩니다.

#### <a name="disabling-security"></a>보안을 사용 하지 않도록 설정

잊어버린된 암호 또는 Jenkins 전체 잠금 발생할 경우 다음이 단계를 수행 하 여 보안을 사용 하지 않도록 설정 가능:

1. Jenkins를 중지 합니다. Jenkins.app를 사용 하는 경우에 도킹 Jenkins.App 아이콘을 마우스 오른쪽 단추로 클릭 하 고 팝업 메뉴에서 종료를 선택 하 여이 수행할 수 있습니다.

    ![도킹 및 팝업 메뉴에서 종료를 선택 하면 앱 아이콘](jenkins-walkthrough-images/image19.png)

2. 파일을 엽니다 **~/.jenkins/config.xml** 텍스트 편집기에서.
3. 값을 변경 합니다 `<usesecurity></usesecurity>` 요소를 `true` 에 `false`입니다.
4. 삭제 된 `<authorizationstrategy></authorizationstrategy>` 및 `<securityrealm></securityrealm>` 파일의 요소입니다.
5. Jenkins를 다시 시작 합니다.

## <a name="setting-up-a-job"></a>작업 설정

Jenkins 구성 소프트웨어를 빌드하는 데 필요한 다양 한 작업의 모든 최상위 수준에 *작업*합니다. 작업을 연관 된 메타 데이터를 소스 코드, 빌드를 실행할 빈도, 건물에 필요한 모든 특수 변수를 가져오는 방법 및 빌드에 실패 하면 개발자에 알리는 방법 등 빌드에 대 한 정보를 제공 하는 기능도 합니다.

작업을 선택 하 여 만들어집니다 **Jenkins > 새 작업** 다음 스크린샷에 표시 된 것 처럼 오른쪽 위 모퉁이에 있는 메뉴에서:

![](jenkins-walkthrough-images/image22.png "작업의 오른쪽 위 모서리의 메뉴에서 새 Jenkins 작업을 선택 하 여 만들어집니다.")

이 표시 됩니다는 **새 작업 [Jenkins]** 페이지입니다. 작업에 대 한 이름을 입력 하 고 선택 합니다 **자유롭게 소프트웨어 프로젝트를 빌드할** 라디오 단추입니다. 다음 스크린샷은이 예제를 보여 줍니다.

![](jenkins-walkthrough-images/image23.png "작업에 대 한 이름을 입력 하 고 빌드를 자유롭게 소프트웨어 프로젝트 라디오 단추 선택")

클릭 하 여 **확인** 단추 작업에 대 한 구성 페이지를 제공 합니다. 이 작업은 다음 스크린샷과 유사 합니다.

![](jenkins-walkthrough-images/image24.png "이이 스크린샷과 비슷해야 합니다.")

Jenkins 작업 디렉터리에 다음 경로에 있는 하드 디스크 구성: **~/.jenkins/jobs/[JOB 이름]**

이 폴더는 모든 파일 및 로그와 구성 파일을 컴파일해야 하는 소스 코드 작업에 특정 아티팩트를 포함 합니다.

초기 작업을 만든 후 다음 중 하나 이상을 사용 하 여 구성 해야 합니다.

- 소스 코드 관리 시스템을 지정 해야 합니다.
- 하나 이상의 *빌드 작업* 프로젝트에 추가 해야 합니다. 이들은, 단계 또는 응용 프로그램을 빌드하는 데 필요한 작업입니다.
- 하나는 작업을 할당 되어야 합니다 *빌드 트리거* – 코드 가져와 최종 프로젝트 빌드 빈도 Jenkins를 알리는 명령 집합입니다.

### <a name="configuring-source-code-control"></a>소스 코드 제어를 구성합니다.

Jenkins는 첫 번째 작업 소스 코드 관리 시스템에서 소스 코드를 검색 됩니다. Jenkins는 여러 현재 사용할 수 있는 인기 있는 소스 코드 관리 시스템을 지원 합니다. 이 섹션에서는 두 개의 인기 있는 시스템, Git 및 Team Foundation Server를 다룹니다. 이러한 소스 코드 관리 시스템의 각 작업은 아래 섹션에서 자세히 설명 되어 있습니다.

#### <a name="using-git-for-source-code-control"></a>소스 코드 제어에 Git를 사용 하 여

소스 코드 제어에 대 한 TFS를 사용 하는 경우 [건너뛸](#using-tfs-for-source-code-management) 이 섹션 및 TFS를 사용 하 여 다음 섹션으로 이동 합니다.

Jenkins Git 즉시 지원 – 추가 플러그 인을 찾지는 필요 합니다. Git을 사용 하려면 클릭 합니다 **Git** 라디오 단추 및 다음 스크린샷에 표시 된 대로 Git 리포지토리의 URL을 입력 합니다.

![](jenkins-walkthrough-images/image25.png "Git을 사용 하려면 Git 라디오 단추를 클릭 하 고 Git 리포지토리에 대 한 URL을 입력 합니다.")

변경 내용이 저장 되 면 Git 구성이 완료 되었습니다.

#### <a name="using-tfs-for-source-code-management"></a>TFS를 사용 하 여 소스 코드 관리

이 섹션에서는 TFS 사용자에만 적용 됩니다.

클릭 합니다 **Team Foundation Server** 라디오 단추 및 TFS 구성 섹션은 유사, 다음 스크린샷에 란:

![](jenkins-walkthrough-images/image26.png "Team Foundation Server 라디오 단추를 클릭 하 고 TFS 구성 섹션에 표시 됩니다.")

TFS에 필요한 정보를 제공 합니다. 다음 스크린샷은 완료 된 폼의 예를 보여 줍니다.

![](jenkins-walkthrough-images/image27.png "이 스크린샷은 완료 된 폼의 예를 보여 줍니다.")

#### <a name="testing-the-source-code-control-configuration"></a>테스트 소스 코드 제어 구성

적절 한 소스 코드 제어를 구성한 후 클릭 **저장할** 변경 내용을 저장 합니다. 다음 스크린샷에서 비슷합니다는 작업의 경우 홈 페이지로 돌아갑니다.

![](jenkins-walkthrough-images/image28.png "이 돌아갑니다이 스크린샷과 비슷해야 합니다는 작업에 대 한 홈 페이지")

지정 된 빌드 작업이 없는 경우에 소스 코드 제어 올바르게 구성 되어 있는지 유효성을 검사 하는 가장 간단한 방법은 수동으로 빌드를 트리거하려면 됩니다. 수동으로 빌드를 시작 하려면 작업의 홈 페이지에는 **Build Now** 아래 스크린샷에 표시 된 것 처럼 왼쪽에 있는 메뉴 링크:

![](jenkins-walkthrough-images/image29.png "작업의 홈 페이지의 왼쪽 메뉴에서 Build Now 링크가 있습니다 빌드를 수동으로 시작 하려면")

빌드에 시작 되 면 빌드 기록 대화 깜박이 파란색 원, 진행률 표시줄, 빌드 번호 및 빌드가 시작 된 다음 스크린샷과 유사 하 게에 표시 됩니다.

![](jenkins-walkthrough-images/image30.png "시작 된 다음 빌드에 빌드 기록 대화 상자 표시 깜박임 파란색 원, 진행률 표시줄, 빌드 번호 및 빌드 시작 시간")

작업이 성공한 경우 파란색 원을 표시 됩니다. 작업에 실패 하면 빨간색 원이 표시 됩니다.

빌드의 일부로 발생할 수 있는 문제 해결을 위해 모든 작업에 대 한 콘솔 출력 Jenkins 캡처됩니다. 콘솔 출력을 보려면의 작업을 클릭 **빌드 기록**를 선택한 다음는 **콘솔 출력** 왼쪽 메뉴에서 링크 합니다. 다음 스크린 샷에 표시 합니다 **콘솔 출력** 링크 뿐만 아니라 성공적인 작업에서 출력의 일부입니다.

![](jenkins-walkthrough-images/image31.png "콘솔 출력 링크 뿐만 아니라 일부 성공적인 작업의에서 출력을 보여 주는이 스크린샷")

#### <a name="location-of-build-artifacts"></a>빌드 아티팩트의 위치

Jenkins 라는 특수 폴더에는 전체 소스 코드를 검색 합니다는 *작업 영역*합니다. 다음 위치에 폴더 내에 있는이 디렉터리를 찾을 수 있습니다.

    ```
    ~/.jenkins/jobs/[JOB NAME]/workspace
    ```

작업 영역에 대 한 경로 라는 환경 변수에 저장 됩니다 `$WORKSPACE`합니다.

작업에 대 한 방문 페이지로 이동한 후을 클릭 하 여 Jenkins에서 작업 영역 폴더를 찾아볼 수는 **작업 영역** 왼쪽 메뉴에서 링크 합니다. 다음 스크린샷은 이라는 작업 영역의 예로 **HelloWorld**:

![](jenkins-walkthrough-images/image32.png "이 스크린샷에서 HelloWorld 라는 작업에 대 한 작업 영역의 예를 보여 줍니다.")

### <a name="build-triggers"></a>빌드 트리거

Jenkins에서 빌드를 시작 하기 위한 몇 가지 다른 전략이 –이 라고 *트리거를 빌드할*합니다. 빌드 트리거를 통해 Jenkins 프로젝트를 빌드하고 작업을 시작 하는 시기를 결정할 수 있습니다. 일반적인 빌드 트리거 중 다음과 같습니다.

- **정기적으로 빌드** -그러면 Jenkins 월요일부터 금요일 자정 또는 지정 된 간격으로 2 시간 마다 같은 작업을 시작 합니다. 빌드 여부에 관계 없이 시작 됩니다 소스 코드 리포지토리에 모든 변경 내용이 있습니다.
- **폴링 SCM** –이 트리거를 폴링하는 정기적으로 소스 코드 제어 합니다. 모든 변경 내용이 커밋되면 소스 코드 리포지토리에 새 빌드가 Jenkins 시작 됩니다.

SCM 폴링 되므로 인기 있는 트리거를 개발자 빌드를 중단 시키는 변경 내용이 커밋될 때에 빠른 피드백을 제공 합니다. 최근에 커밋된 코드도 문제를 일으키는지를 통해 변경 내용이 여전히 새로 염두에서 하는 동안 문제를 해결 하는 개발자 팀 경고에 대 한 유용 합니다.

정기적인 빌드 테스터에 게 배포할 수 있는 응용 프로그램의 버전을 만들려면 자주 사용 됩니다. 예를 들어, QA 팀의 멤버가 이전 주의 작업을 테스트할 수 있도록 정기적인 빌드 금요일 밤에 대 한 예약 될 수 있습니다.

### <a name="compiling-a-xamarinios-applications"></a>Xamarin.iOS 응용 프로그램을 컴파일
Xamarin.iOS 프로젝트를 사용 하 여 명령줄에서 컴파일할 수 있는 `xbuild` 또는 `msbuild`합니다. 셸 명령 Jenkins를 실행 하는 사용자 계정의 컨텍스트에서 실행 됩니다. 배포에 대 한 응용 프로그램을 제대로 패키지할 수 있도록 사용자 계정 프로 비전 프로필에 대 한 액세스에는 반드시 합니다. 이 셸 명령 작업 구성 페이지에 추가할 가능성이 있습니다.

아래로 스크롤하여 합니다 **빌드** 섹션입니다. 클릭 합니다 **빌드 단계 추가** 단추를 선택 **셸 실행**와 다음 스크린샷에 표시 된 것과 같이:

![](jenkins-walkthrough-images/image33.png "빌드 단계 추가 단추를 클릭 하 고 Execute shell 선택")

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

### <a name="building-a-xamarinandroid-project"></a>Xamarin.Android 프로젝트 빌드

Xamarin.Android 프로젝트를 빌드하는 매우 비슷합니다 개념에서 Xamarin.iOS 프로젝트를 빌드할 합니다. APK를 Xamarin.Android 프로젝트를 만들려면 다음 두 단계를 수행 하려면 Jenkins는 구성 해야 합니다.

- 프로젝트를 컴파일하는 MSBuild 플러그 인을 사용 하 여
- 로그인 및 zip 유효한 릴리스 키 저장소를 사용 하 여 APK를 맞춥니다.

이러한 두 단계는 다음 두 섹션에서 자세히 설명 합니다.

### <a name="creating-the-apk"></a>APK 만들기

클릭 합니다 **빌드 단계 추가** 단추를 선택한 **Visual Studio 프로젝트 또는 MSBuild를 사용 하 여 솔루션 빌드**아래 스크린샷에 표시 된 것 처럼:

![](jenkins-walkthrough-images/image36.png "Visual Studio 프로젝트 또는 MSBuild를 사용 하 여 솔루션 빌드 선택에 추가 빌드 단계 단추를 클릭 하 여 APK 만들기")

빌드 단계는 프로젝트에 추가 되 면 표시 되는 양식 필드에 입력 합니다. 다음 스크린샷에서 완성 된 폼의 한 예:

![](jenkins-walkthrough-images/image37.png "빌드 단계는 프로젝트에 추가 되 면 표시 되는 폼 필드 입력")

이 빌드 단계 실행 `xbuild` 에 **$WORKSPACE** 폴더입니다. MSBuild 빌드 파일을 설정 합니다 **Xamarin.Android.csproj** 파일입니다. 합니다 **명령줄 인수** 대상의 릴리스 빌드를 지정 **PackageForAndroid**합니다. APK 수를 곱한 값이 단계는 다음 위치에서:

    ```
    $WORKSPACE/[PROJECT NAME]/bin/Release
    ```

다음 스크린샷은이 APK의 예를 보여 줍니다.

![](jenkins-walkthrough-images/image38.png "이 스크린샷에서이 APK의 예를 보여 줍니다.")

이 APK zip 맞춰야 하 고 개인 키 저장소를 사용 하 여 서명 되지 않은 배포에 대 한 준비 되지 않았습니다.

#### <a name="signing-and-zipaligning-the-apk-for-release"></a>서명 및 Zipaligning 릴리스 APK

서명 및 zipaligning APK는 기술적으로 두 가지 별도 작업이 Android SDK에서 두 개의 별도 명령줄 도구에서 수행 됩니다. 그러나 빌드 작업에서을 수행 하는 것이 유용 합니다. 서명 및 zipaligning APK에 대 한 자세한 내용은 Android 응용 프로그램 릴리스 준비에 Xamarin 설명서를 참조 하세요.

두이 명령은 모두 프로젝트 마다 달라질 수 있는 명령줄 매개 변수가 필요 합니다. 또한 이러한 명령줄 매개 변수 중 일부는 빌드가 실행 될 때 콘솔 출력에 나타나지 않아야 하는 암호입니다. 이러한 명령줄 매개 변수 중 일부 환경 변수에 저장 됩니다. 서명 및/또는 zip 맞춤에 대 한 필요한 환경 변수는 아래 표에 설명 되어 있습니다.

|환경 변수|설명|
|--- |--- |
|KEYSTORE_FILE|이 APK를 서명 하는 것에 대 한 키 저장소에 대 한 경로|
|KEYSTORE_ALIAS|APK에 서명 하는 데 사용할 수 있는 키 저장소의 키입니다.|
|INPUT_APK|APK가 만든 `xbuild`합니다.|
|SIGNED_APK|서명된 된 APK가 개발한 `jarsigner`합니다.|
|FINAL_APK|이 zip에 의해 생성 된 APK를 정렬 `zipalign`합니다.|
|STORE_PASS|이 파일을 노래 하는 것에 대 한 키 저장소의 내용에 액세스 하는 데 사용 되는 암호입니다.|

요구 사항 섹션에서 설명한 대로, EnvInject 플러그 인을 사용 하 여 빌드하는 동안 이러한 환경 변수를 설정할 수 있습니다. 작업에 새 빌드가 있어야 합니다. 단계 넣기 환경 변수를 기반으로 다음 스크린샷에 표시 된 것과 같이 추가 합니다.

![](jenkins-walkthrough-images/image39.png "작업에 새 빌드가 있어야 넣기 환경 변수를 기반으로 하는 단계 추가")

에 **콘텐츠 속성** 다음 형식으로 표시 되는 필드, 환경 변수 추가, 줄 당 하나씩 1 개를 구성 합니다.

    ```
    ENVIRONMENT_VARIABLE_NAME = value
    ```

다음 스크린샷에서 APK를 서명 하는 데 필요한 환경 변수를 보여 줍니다.

![](jenkins-walkthrough-images/image40.png "다음이 스크린샷에서 APK를 서명 하는 데 필요한 환경 변수를 보여 줍니다.")

빌드된 APK 파일에 대 한 환경 변수 중 일부를 확인 합니다 `WORKSPACE` 환경 변수입니다.

최종 환경 변수는 키 저장소의 내용에 액세스 하려면 암호: `STORE_PASS`합니다. 암호는 가려지는지 또는 로그 파일에서 생략 해야 하는 중요 한 값입니다. 로그에 표시 되지 않습니다 있도록 이러한 값을 보호 하기 위해 EnvInject 플러그 인을 구성할 수 있습니다.

바로 앞의 **빌드** 작업 구성의 섹션을 **빌드 환경** 섹션. 경우는 **암호를 삽입** 확인란을 설정/해제, 어떤 형태로 표시 되도록 필드입니다. 이러한 양식 필드 이름 및 환경 변수의 값 캡처에 사용 됩니다. 다음 스크린샷에서 추가 하는 예로 `STORE_PASS` 환경 변수:

![](jenkins-walkthrough-images/image41.png "이 스크린샷은 STOREPASS 환경 변수를 추가 하는 예입니다.")

환경 변수는 초기화 될 되 면 서명 하는 것에 대 한 빌드 단계 추가 zip APK를 정렬 하는 다음 단계가입니다. 다른 환경 변수를 삽입 하는 빌드 단계 수는 직후 **셸 실행** 실행 하는 명령 빌드 `jarsigner` 및 `zipalign`합니다. 다음 코드 조각에 나와 있는 것 처럼를 한 줄 위로 각 명령을 걸립니다.

    ```
    jarsigner -verbose -sigalg MD5withRSA -digestalg SHA1 -keystore $KEYSTORE_FILE -storepass $STORE_PASS -signedjar $SIGNED_APK $INPUT_APK $KEYSTORE_ALIAS
    zipalign -f -v 4 $SIGNED_APK $FINAL_APK
    ```

다음 스크린 샷에서 입력 하는 방법의 예가 나와 합니다 `jarsigner` 및 `zipalign` 단계에 대 한 명령:

![](jenkins-walkthrough-images/image42.png "이 스크린샷에서 단계로 jarsigner 및 zipalign 명령을 입력 하는 방법의 예를 보여 줍니다.")

모든 빌드 작업이 진행에서 되 면 것이 좋습니다 모든 것이 작동을 확인 하려면 수동 빌드를 트리거하도록 합니다. 빌드에 실패 하는 경우는 **콘솔 출력** 빌드 실패의 원인에 대 한 정보를 검토 해야 합니다.

### <a name="submitting-tests-to-test-cloud"></a>Test Cloud에 대 한 테스트를 제출합니다.

자동화 된 테스트는 셸 명령을 사용 하 여 Test Cloud에 제출할 수 있습니다. Xamarin Test Cloud에서 테스트 실행 설정에 대 한 자세한 내용은이 가이드를 사용 하 여에 대 한 참조 [Xamarin.UITest](/appcenter/test-cloud/preparing-for-upload/uitest/)합니다.

## <a name="summary"></a>요약

이 가이드에서는 빌드 서버로 macOS에서 Jenkins를 도입 하 고 컴파일하고 릴리스에 대 한 Xamarin 모바일 응용 프로그램을 준비 하도록 구성 합니다. 여러 플러그 인 빌드 프로세스를 지원 하기 위해 함께 macOS 컴퓨터에서 Jenkins를 설치 했습니다. 생성 하 고 TFS 또는 Git에서 코드를 끌어오기 되며 다음 릴리스 준비 응용 프로그램에 해당 코드를 컴파일하는 작업을 구성 합니다. 작업을 실행 해야 할 시기를 예약 하는 두 가지 방법으로 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [연속 통합](~/tools/ci/index.md)
- [App Center 테스트](https://docs.microsoft.com/appcenter/test-cloud/)
