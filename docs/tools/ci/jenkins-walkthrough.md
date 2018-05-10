---
title: Xamarin을 사용한 Jenkins를 사용 하 여
description: 이 가이드에는 Jenkins 연속 통합 서버를 설정 하 고 컴파일 Xamarin을 사용 하 여 만든 모바일 응용 프로그램을 자동화 하는 방법을 보여 줍니다. Jenkins OS x 설치, 구성, 및 변경 내용이 소스 코드 관리 시스템에 커밋될 때 Xamarin.iOS 및 Xamarin.Android 응용 프로그램을 컴파일할 작업을 설정 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 1E6825DF-1254-4FCB-B94D-ADD33D1B5309
author: topgenorth
ms.author: toopge
ms.date: 03/23/2017
ms.openlocfilehash: 1052507bfbf06e264f9e9da89be1e0f35fa70ce1
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="using-jenkins-with-xamarin"></a>Xamarin을 사용한 Jenkins를 사용 하 여

_이 가이드에는 Jenkins 연속 통합 서버를 설정 하 고 컴파일 Xamarin을 사용 하 여 만든 모바일 응용 프로그램을 자동화 하는 방법을 보여 줍니다. Jenkins OS x 설치, 구성, 및 변경 내용이 소스 코드 관리 시스템에 커밋될 때 Xamarin.iOS 및 Xamarin.Android 응용 프로그램을 컴파일할 작업을 설정 하는 방법을 설명 합니다._

[Xamarin 사용한 연속 통합 소개](~/tools/ci/intro-to-ci.md) 으로 유용한 소프트웨어 개발 방식으로 손상 되었거나 호환 되지 않는 코드의 조기 경고를 제공 하는 연속 통합을 소개 합니다. 소프트웨어 배포에 대 한 적합 한 상태가 유지 고, 발생할 CI 하면 개발자가 문제를 해결 하 고 문제가 있습니다. 이 연습에서는 두 문서에서 콘텐츠를 함께 사용 하는 방법을 설명 합니다.

이 가이드에는 Jenkins OS X를 실행 하는 전용된 컴퓨터에 설치 하 고 컴퓨터를 시작할 때 자동으로 실행 되도록 구성 하는 방법을 보여 줍니다. Jenkins가 설치 되 면 하기 위해 Msbuild를 지원 하기 위해 추가 플러그 인을 설치 하겠습니다. Jenkins 즉시 Git를 지원 합니다. 소스 코드 제어를 위한 TFS 사용 중인 경우에 추가 플러그 인 및 명령줄 유틸리티 설치 합니다.

Jenkins 구성 하 고 필요한 모든 플러그 인 설치 된을 Xamarin.iOS 및 Xamarin.Android 프로젝트를 컴파일하려면 하나 이상의 작업을 만들겠습니다. 작업은 단계 및 작업을 수행 하는 데 필요한 메타 데이터의 컬렉션입니다. 일반적으로 작업을 다음으로 구성 됩니다.

-  **소스 코드 관리 (SCM)** -검색할 파일 및 소스 코드 제어에 연결 하는 방법에 대 한 정보를 포함 하는 Jenkins 구성 파일에서 메타 데이터 항목입니다.
-  **트리거** – 트리거는 경우 개발자 변경 사항을 커밋합니다 소스 코드 리포지토리 등의 특정 동작에 따라 작업을 시작 하는 데 사용 됩니다.
-  **빌드 지침** – 플러그 인 또는 소스 코드를 컴파일하고 모바일 장치에 설치할 수 있는 이진 파일을 생성 하는 스크립트입니다.
-  **작성 하는 선택적 작업** -여기에 포함 코드 서명 하는 코드에서 정적 분석을 수행 하는 단위 테스트를 실행 또는 관련된 작업을 빌드 다른 수행 하는 다른 작업을 시작 합니다.
-  **알림** – 작업을 몇 가지 종류의 빌드 상태에 대 한 알림 보낼 수 있습니다.
-  **보안** – 있지만 선택적 보안 기능을 Jenkins를 활성화도 가장 좋습니다.


이 가이드에서는 이러한 각 지점 다루는 Jenkins 서버를 설정 하는 방법을 단계별로 안내 합니다. 끝 부분에서 우리는 설정 및 Xamarin 모바일 프로젝트 만들기 IPA 및 APK의 Jenkins를 구성 하는 방법 잘 알고가 있어야 합니다.

## <a name="requirements"></a>요구 사항

이상적인 빌드 서버가 독립 실행형 컴퓨터 전용 구축 하 고 가능한 응용 프로그램을 테스트 목적으로 사용 합니다. 전용된 컴퓨터 아티팩트 (예: 웹 서버는) 다른 역할에 대 한 필요할 수 있는 빌드를 묻지 않도록 보장 합니다. 예를 들어, 빌드 서버는 웹 서버 역할도, 웹 서버에는 충돌 하는 버전의 일부 공용 라이브러리 필요할 수 있습니다. 이 충돌 때문에 웹 서버가 올바르게 작동 하지 않습니다 또는 Jenkins 사용자에 게 배포 하는 경우 작동 하지 않는 빌드를 만들 수 있습니다.

Xamarin 모바일 앱 용 빌드 서버로 매우 유사 개발자의 워크스테이션 설정 됩니다. 사용자 계정, Mac 용 Visual Studio는 Jenkins에이 고 Xamarin.iOS 및 Xamarin.Android 설치 됩니다. 코드 서명 인증서, 프로비저닝 프로필 및 키 저장소의 모든도 설치 해야 합니다. *일반적으로 빌드 서버의 사용자 계정 개발자 계정을와 별개인-설치 및 소프트웨어, 키 및 빌드 서버 사용자 계정으로 로그인 하는 동안 인증서를 구성 해야 합니다.*

다음 다이어그램에서는 일반 Jenkins 빌드 서버에서 이러한 요소를 모두 수행 합니다.

 [![](jenkins-walkthrough-images/image1.png "이 다이어그램에서는 모든 일반 Jenkins 빌드 서버에서 이러한 요소")](jenkins-walkthrough-images/image1.png#lightbox)

iOS 응용 프로그램 빌드 및 Mac OS X를 실행 하는 컴퓨터에 서명 된만 수 있습니다. Mac Mini 필요한 저렴 한 비용 옵션을 적당 한 건 OS X 10.10 (Yosemite)를 실행할 수 있는 모든 컴퓨터 또는 이상이 충분 합니다.

설치 하려는 경우 TFS 소스 코드 제어에 대 한 사용량을 [Team Explorer Everywhere](http://www.microsoft.com/en-ca/download/details.aspx?id=40785), Microsoft에서 제공 합니다. Team Explorer Everywhere OS X에서 터미널 TFS에 플랫폼 간 액세스를 제공합니다.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="installing-jenkins"></a>Jenkins 설치

첫 번째 작업 Jenkins를 사용 하 여 설치 하는 것입니다. OS x:에 Jenkins를 실행 하는 방법은 세 가지가

-  백그라운드에서 실행 중인 디먼 합니다.
-  Tomcat, Jetty, JBoss 등 servlet 컨테이너입니다.
-  프로세스로 일반 사용자 계정으로 실행 합니다.


디먼으로 대부분의 일반적인 연속 통합 응용 프로그램은 백그라운드에서 실행 (OS x 또는 \*nix) 또는 Windows에는 서비스입니다. 이 GUI 조작은 필요 하지 않습니다 하 고 설치 하는 빌드 환경 쉽게 수행할 수 있습니다 시나리오에 적합 합니다. 모바일 앱은 키 저장소 및 Jenkins는 디먼으로 실행 될 때 액세스 문제를 일으킬 수 있는 인증서를 서명 해야 합니다. 이러한 문제 때문에이 문서는 사용자 계정 Jenkins 빌드 서버에서 실행-세 번째 시나리오에 중점을 둡니다.

Jenkins.App는 Jenkins를 설치 하는 편리한 방법입니다. 시작을 간소화 하는 AppleScript 래퍼와 Jenkins 서버를 중지 하는 중입니다. Bash 셸의 실행 하는 대신 Jenkins 앱으로 실행 한 Dock에 아이콘과 함께 다음 스크린샷에 표시 된 것 처럼:

 [![](jenkins-walkthrough-images/image2.png "Bash 셸의 실행 하는 대신 Jenkins 앱으로 실행 한 Dock에 아이콘과 함께이 스크린 샷에 표시 된 것 처럼")](jenkins-walkthrough-images/image2.png#lightbox)

시작 또는 중지 Jenkins은 시작 또는 중지 Jenkins.App 하기만 합니다.

Jenkins.App를 설치 하려면 아래 스크린샷에 표시 되는 프로젝트의 다운로드 페이지에서 최신 버전을 다운로드 합니다.

 [![](jenkins-walkthrough-images/image3.png "응용 프로그램에는 프로젝트에서 최신 버전 다운로드 페이지에서이 스크린샷에 나온 다운로드")](jenkins-walkthrough-images/image3.png#lightbox)

Zip 파일을 추출는 `/Applications` 폴더에 빌드 서버와 마찬가지로 다른 모든 OS X 응용 프로그램 시작 합니다.
처음 Jenkins.App를 시작 하면 Jenkins 다운로드 알리는 대화 상자가 제공 합니다.

 [![](jenkins-walkthrough-images/image4.png "앱을 Jenkins 다운로드 알리는 대화 상자가 표시를 제공 합니다.")](jenkins-walkthrough-images/image4.png#lightbox)

Jenkins.App 다운로드 완료 되 면 것인지 Jenkins 시작 사용자 지정 하려면 다음 스크린샷에 표시 된 대로 라는 또 다른 대화 상자를 표시 됩니다.

 [![](jenkins-walkthrough-images/image5.png "응용 프로그램 다운로드를 완료 하는 경우 원하는 Jenkins 시작 사용자 지정 하려면이 스크린 샷에 표시 된 대로 라는 또 다른 대화 상자를 표시 됩니다, 그리고")](jenkins-walkthrough-images/image5.png#lightbox)

Jenkins 사용자 지정 하는 선택적 이며 대부분의 경우에 Jenkins 작동할지에 대 한 응용 프로그램 시작 될 때마다-기본 설정을 수행할 필요가 없습니다.

Jenkins 사용자 지정 해야 하는 경우 클릭 하 여는 **기본값 변경** 단추입니다. 이 두 개의 연속 된 대화 상자가 제공 합니다: Java 명령줄 매개 변수를 요청 하 고 Jenkins 명령줄 매개 변수를 요청 하는 다른 합니다. 다음 두 가지 스크린샷입니다 이러한 두 대화 상자를 보여 줍니다.

 [![](jenkins-walkthrough-images/image6.png "이 스크린샷은 다음 대화 상자를 보여 줍니다.")](jenkins-walkthrough-images/image6.png#lightbox)

 [![](jenkins-walkthrough-images/image7.png "이 스크린샷은 다음 대화 상자를 보여 줍니다.")](jenkins-walkthrough-images/image7.png#lightbox)

Jenkins 실행 되 면 시작 될 때마다 컴퓨터에 사용자 로그인에 있도록 로그인 항목으로 설정 하는 것이 좋습니다. 도킹 스테이션에서 Jenkins 아이콘을 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 이렇게 하려면 **옵션... 로그인 할 때 열린**다음 스크린샷에 표시 된 것 처럼:

 [![](jenkins-walkthrough-images/image8.png "이 스크린 샷에 표시 된 것 처럼 도킹 스테이션에서 Jenkins 아이콘을 마우스 오른쪽 단추로 클릭 하 고 로그인 시 OptionsOpen를 선택 하 여 이렇게 하려면 있습니다.")](jenkins-walkthrough-images/image8.png#lightbox)

이렇게 하면 자동으로 실행 될 때마다 Jenkins.App 하지 때 컴퓨터 부팅 시 하지만 사용자가 로그인 합니다. 부팅 시 자동으로 로그인 하기 위해 OS X가 사용할 사용자 계정을 지정 하는 것이 불가능 합니다. 열기는 **시스템 환경 설정**, 선택는 **사용자 및 그룹** 이 스크린 샷에 표시 된 것 처럼 아이콘:

 [![](jenkins-walkthrough-images/image9.png "시스템 환경 설정을 열고이 스크린 샷에 표시 된 것 처럼 사용자 그룹 아이콘을 선택 합니다.")](jenkins-walkthrough-images/image9.png#lightbox)

클릭는 **로그인 옵션** 단추를 선택한 다음 부팅 시 로그인에 대 한 OS X 사용 하는 계정을 선택 합니다.

이 시점에서 Jenkins가 설치 되었습니다. 그러나, Xamarin 모바일 응용 프로그램을 구축 하려고 하는 경우 일부 플러그 인을 설치 해야 합니다.


### <a name="installing-plugins"></a>플러그 인 설치

Jenkins.App 설치 관리자가 완료 되 면 Jenkins를 시작 하 고 URL로 웹 브라우저를 시작할 http://localhost:8080아래 스크린샷에 표시 된 것 처럼:

 [![](jenkins-walkthrough-images/image10.png "이 스크린 샷에 표시 된 것 처럼 8080")](jenkins-walkthrough-images/image10.png#lightbox)

이 페이지에서 선택 **Jenkins > Jenkins 관리 > 플러그 인 관리** 아래 스크린샷에 표시 된 것 처럼 왼쪽 위 모서리에 있는 메뉴에서:

 [![](jenkins-walkthrough-images/image11.png "이 페이지에서 왼쪽 위 모서리에 있는 메뉴에서 Jenkins Jenkins 관리 플러그 인 관리 선택")](jenkins-walkthrough-images/image11.png#lightbox)

이 표시 됩니다는 **Jenkins 플러그 인 관리자** 페이지. 사용 가능한 탭을 클릭 하면 다운로드 및 설치 하는 600 개가 넘는 플러그 인 목록이 표시 됩니다. 이 작업은 아래 스크린샷에 표시:

 [![](jenkins-walkthrough-images/image12.png "사용 가능한 탭을 클릭 다운로드 및 설치 하는 600 개가 넘는 플러그 인 목록이 표시 됩니다.")](jenkins-walkthrough-images/image12.png#lightbox)

몇 가지는 것을 찾을 수 플러그 인 및 오류 발생 가능성에 대 한 모든 600를 통해 스크롤입니다. Jenkins는 인터페이스의 오른쪽 상단 모서리에 필터 검색 필드를 제공합니다. 이 필터 필드를 사용 하 여 검색 하려면 다음 플러그 인 중 찾기 및 설치 된 하나 또는 전부가 간소화 됩니다.

-  **Jenkins MSBuild 플러그 인** –이 플러그 인을 사용 하면 Mac 솔루션 (.sln) 및 프로젝트 (.csproj)에 대 한 Visual Studio 및 Visual Studio를 빌드할 수 있습니다.
-  **환경 인젝터 플러그 인** – 수준 하에서 작업 환경 변수를 설정할 수 있도록 하는 선택 사항 이지만 유용한 플러그 인입니다. 또한 응용 프로그램에 서명 코드에 사용 되는 암호와 같은 변수에 대 한 보다 효과적인 보호를 제공 합니다. 로 간략히 표시 되기도 *EnvInject 플러그 인* 합니다.
-  **Team Foundation Server 플러그 인** – 에게만 허용 되는 선택적 플러그 인이는 Team Foundation Server 또는 Team Foundation 서비스 소스 코드 제어를 사용 중인 경우 필요 합니다.

Jenkins 없이 모든 추가 플러그 인 Git를 지원합니다.

모든는 플러그 인을 설치한 후을 Jenkins를 다시 시작 하 고 각 플러그 인에 대 한 전역 설정을 구성 합니다. 플러그 인에 대 한 전역 설정을 선택 하 여 있습니다 **Jenkins > Jenkins 관리 > 시스템 구성** 아래 스크린샷에 표시 된 것 처럼 왼쪽 위 모서리에서:

 [![](jenkins-walkthrough-images/image13.png "Jenkins를 선택 하 여 플러그 인에 대 한 전역 설정이 있습니다 / Jenkins 관리 / 모퉁이 전달 하는 왼쪽 상단에서 시스템 구성")](jenkins-walkthrough-images/image13.png#lightbox)

이 메뉴 옵션을 선택 하면 돌아갑니다는 **시스템 구성 [Jenkins]** 페이지. 이 페이지에는 섹션을 Jenkins 자체를 구성 하 고 일부는 전역 플러그 인 값을 설정할 수 있습니다.  아래 스크린샷에서이 페이지의 예를 보여 줍니다.

 [![](jenkins-walkthrough-images/image14.png "이 스크린샷은이 페이지의 예를 보여 줍니다.")](jenkins-walkthrough-images/image14.png#lightbox)


#### <a name="configuring-the-msbuild-plugin"></a>MSBuild 플러그 인 구성

MSBuild 플러그 인을 사용 하도록 구성 해야 **/Library/Frameworks/Mono.framework/Commands/xbuild** Mac 솔루션 및 프로젝트 파일에 대 한 Visual Studio를 컴파일하는 데 있습니다. 아래로 스크롤하여는 **시스템 구성 [Jenkins]** 될 때까지 페이지는 **추가 MSBuild** 아래 스크린샷에 표시 된 것 처럼 단추가 나타납니다.

 [![](jenkins-walkthrough-images/image15.png "추가 MSBuild 단추가 나타날 때까지 시스템 Jenkins 구성 페이지 아래로 스크롤하여")](jenkins-walkthrough-images/image15.png#lightbox)

이 단추를 클릭 하 고 채울는 **이름** 및 **경로** 를 **MSBuild** 표시 되는 양식에서 필드입니다. 이름에 **MSBuild** 설치를 사용 해야 하는 동안 의미 있는 **MSBuild에 대 한 경로** 에 경로가 지정 되어야 `xbuild`, 일반적으로 **/Library/프레임 워크 / Mono.framework/Commands/xbuild**합니다. 변경 내용을 저장 또는 페이지의 맨 아래에 있는 [적용] 단추를 클릭 하 여, 저장 한 후 Jenkins에서 사용할 수 있게 됩니다. `xbuild` 솔루션을 컴파일할 수 있습니다.

#### <a name="configuring-the-tfs-plugin"></a>TFS 플러그 인 구성

이 섹션은 소스 코드 제어에 대 한 TFS를 사용 하려는 경우에 필수입니다.

TFS 서버와 상호 작용 하는 OS X 워크스테이션에 대 한 순서로 Team Explorer Everywhere 워크스테이션에 설치 되어야 합니다. Team Explorer Everywhere은 TFS를 액세스 하기 위한 크로스 플랫폼 명령줄 클라이언트를 포함 하는 microsoft 도구 집합입니다. Team Explorer everywhere만 Microsoft에서 다운로드 한 후 설치할 수 있습니다 3 단계에 따라:

1. 사용자 계정에 액세스할 수 있는 디렉터리에 보관 파일을 압축을 풉니다. 파일을 압축을 풀 수는 예를 들어 **~/tee**합니다.
2. 위의 1 단계에서 압축 되지 않은 파일이 있는 폴더를 포함 하도록 셸 또는 시스템 경로 구성 합니다. 예를 들어 개체에 적용된

        echo export PATH~/tee/:$PATH' >> ~/.bash_profile

3. Team Explorer everywhere만 설치 되어 있는지를 확인 하려면 터미널 세션을 열고 실행의 `tf` 명령입니다. Tf가 올바르게 구성 하는 경우 터미널 세션에서 다음과 같은 출력이 나타납니다.

        $ tf
        Team Explorer Everywhere Command Line Client (version 11.0.0.201306181526)

        Available commands and their options:

TFS에 대 한 명령줄 클라이언트 설치 되 면 Jenkins의 전체 경로를 구성 해야는 `tf` 명령줄 클라이언트입니다. 아래로 스크롤하여는 **시스템 구성 [Jenkins]** 다음 스크린샷에 표시 된 것 처럼 Team Foundation Server 섹션을 찾을 때까지 페이지:

 [![](jenkins-walkthrough-images/image17.png "Team Foundation Server 섹션을 찾을 때까지 시스템 Jenkins 구성 페이지 아래로 스크롤하여")](jenkins-walkthrough-images/image17.png#lightbox)

전체 경로를 입력는 `tf` 명령을 실행 하 고 클릭는 **저장** 단추입니다.

### <a name="configure-jenkins-security"></a>Jenkins 보안 구성

를 처음 설치할 때 모든 사용자를 설정 하 고 익명으로 모든 종류의 작업을 실행 하는 Jenkins 보안이 비활성화 됨에 있습니다. 이 섹션에서는 인증 및 권한 부여를 구성 하는 Jenkins 사용자 데이터베이스를 사용 하 여 보안을 구성 하는 방법에 설명 합니다.

보안 설정을 선택 하 여 있습니다 **Jenkins > Jenkins 관리 > 글로벌 보안 구성**이 스크린 샷에 표시 된 것 처럼:

 [![](jenkins-walkthrough-images/image18.png "보안 설정은 Jenkins를 선택 하 여 있습니다 / Jenkins 관리 / 글로벌 보안 구성")](jenkins-walkthrough-images/image18.png#lightbox)

에 **글로벌 보안 구성** 확인 페이지는 **보안 사용** 확인란 및 **액세스 제어** 폼 나타나야 다음 스크린샷과 유사 하 게:

 [![](jenkins-walkthrough-images/image19.png "전역 보안 구성 페이지에서 보안 사용 확인 확인란을 선택 하 고 액세스 제어 형식와 비슷하게 나타납니다이 스크린 샷")](jenkins-walkthrough-images/image19.png#lightbox)

라디오 단추를 설정/해제 **Jenkins' 사용자 데이터베이스** 에 **보안 영역 섹션**에 및 **사용자가 등록할 수 있도록** 확인란도 선택 하면에 설명 된 대로 다음 스크린 샷:

 [![](jenkins-walkthrough-images/image20.png "보안 영역 섹션의 Jenkins 사용자 자체 데이터베이스에 대 한 라디오 단추를 설정/해제 하 고 사용자가 등록 하도록 허용도 선택 되어 있는지 확인 하십시오.")](jenkins-walkthrough-images/image20.png#lightbox)

마지막으로 Jenkins를 다시 시작 하 고 새 계정을 만듭니다. 만든 첫 번째 계정은 루트 계정이 고이 계정 관리자에 게 자동으로 승격 됩니다. 로 돌아가 **글로벌 보안 구성** 페이지 및 확인는 **매트릭스 기반 보안** 라디오 단추입니다. 루트 계정의 모든 액세스 권한을 부여 해야 하며, 익명 계정 주어져야 읽기 전용 액세스 다음 스크린샷에 표시 된 것 처럼:

 [![](jenkins-walkthrough-images/image21.png "루트 계정의 모든 액세스 권한을 부여 해야 하며 익명 계정 읽기 전용 액세스를 제공")](jenkins-walkthrough-images/image21.png#lightbox)

이러한 설정을 저장할 Jenkins를 다시 시작 되 면 보안 켜 집니다.


#### <a name="disabling-security"></a>보안을 사용 하지 않도록 설정

된 잊어버린된 암호 또는 Jenkins 수준의 잠금 다음이 단계를 수행 하 여 보안을 사용 하지 않도록 설정할 수 있습니다.

1. Jenkins를 중지 합니다. Jenkins.app를 사용 하는 경우, 도킹 스테이션에서 Jenkins.App 아이콘을 마우스 오른쪽 단추로 클릭 하 고 팝업 메뉴에서 종료를 선택 하 여이 수행할 수 있습니다.

    ![](jenkins-walkthrough-images/image19.png "Dock, 및 팝업 메뉴에서 종료를 선택 하면 응용 프로그램 아이콘")
2. 파일을 열고 **~/.jenkins/config.xml** 텍스트 편집기에서.
3. 값을 변경는 `<usesecurity></usesecurity>` 요소 `true` 를 `false`합니다.
4. 삭제는 `<authorizationstrategy></authorizationstrategy>` 및 `<securityrealm></securityrealm>` 는 파일의에서 요소입니다.
5. Jenkins를 다시 시작 합니다.

## <a name="setting-up-a-job"></a>작업 설정

최상위 수준에서 Jenkins 소프트웨어를 빌드하는 데 필요한 다양 한 작업에서 모든 구성는 *작업*합니다. 작업에 연결 된 소스 코드, 빌드를 실행할 빈도, 빌딩에 필요한 모든 특별 한 변수를 가져오는 방법으로 빌드가 실패 한 경우 개발자가 알리는 방법 등 빌드에 대 한 정보를 제공 하는 메타 데이터를 있습니다.

작업을 선택 하 여 만들어집니다 **Jenkins > 새 작업** 다음 스크린샷에 표시 된 것 처럼 오른쪽 위 모서리에 있는 메뉴에서:


![](jenkins-walkthrough-images/image22.png "작업의 오른쪽 상단 메뉴에서 Jenkins 새 작업을 선택 하 여 생성 됩니다.")

이 표시 됩니다는 **새 작업 [Jenkins]** 페이지. 작업에 대 한 이름을 입력 하 고 선택 된 **자유 스타일 소프트웨어 프로젝트를 빌드하** 라디오 단추입니다. 다음 스크린 샷에서 이러한 예제를 보여 줍니다.

![](jenkins-walkthrough-images/image23.png "작업에 대 한 이름을 입력 하 고 빌드 자유 스타일 소프트웨어 프로젝트 라디오 단추 선택")

클릭 하 고 **확인** 단추는 작업에 대 한 구성 페이지를 표시 합니다. 이 작업은 다음 스크린 샷에서 유사 합니다.

![](jenkins-walkthrough-images/image24.png "이것은 유사이 스크린 샷")

다음 경로에 있는 하드 디스크에 있는 디렉터리에 있는 작업을 구성 하는 Jenkins: **~/.jenkins/jobs/[JOB 이름]**

이 폴더는 모든 파일 및 로그, 구성 파일 및 소스 코드를 컴파일해야 할 같은 작업에 고유한 아티팩트를 포함 합니다.

초기 작업을 만든 후 다음 중 하나 이상을 구성 해야 합니다.

 - 소스 코드 관리 시스템을 지정 해야 합니다.
 - 하나 이상의 *빌드 작업* 프로젝트에 추가 해야 합니다. 이들은 응용 프로그램을 빌드하는 데 필요한 작업 또는 단계입니다.
 - 하나는 작업을 할당 되어야 합니다 *빌드 트리거* – Jenkins 얼마나 자주 하는 코드를 검색 하 고 최종 프로젝트 빌드 알리는 명령 집합입니다.

### <a name="configuring-source-code-control"></a>소스 코드 제어를 구성합니다.

Jenkins는 첫 번째 작업 소스 코드 관리 시스템에서 소스 코드를 검색 됩니다. Jenkins 인기 있는 소스 코드 관리 시스템 현재 사용할 수 있는 다양 한 지원합니다. 이 섹션에서는 두 인기 있는 시스템 Git 및 Team Foundation Server에 설명 합니다. 이러한 각 소스 코드 관리 시스템 아래 섹션에서 자세히 설명 합니다.

#### <a name="using-git-for-source-code-control"></a>소스 코드 제어에 Git를 사용 하 여

소스 코드 제어에 대 한 TFS를 사용 하는 경우 [건너뛸](#Using_TFS_for_Source_Code_Management) 이 섹션 및 TFS를 사용 하 여 다음 섹션으로 이동 합니다.

Jenkins Git 즉시 지원 – 추가 플러그 인을 찾지는 필요 합니다. Git를 사용 하려면 클릭는 **Git** 라디오 단추를 선택한 다음 스크린샷에 표시 된 대로 Git 리포지토리에 대 한 URL을 입력 합니다.

![](jenkins-walkthrough-images/image25.png "Git를 사용 하려면 Git 라디오 단추를 클릭 하 고 Git 리포지토리에 대 한 URL을 입력 합니다.")

변경 내용이 저장 되 면 Git 구성이 완료 되었습니다.

#### <a name="using-tfs-for-source-code-management"></a>TFS를 사용 하 여 소스 코드 관리에 대 한

이 섹션은 TFS 사용자에만 적용 됩니다.

클릭는 **Team Foundation Server** 라디오 단추 및 TFS 구성 섹션와 비슷하게 나타납니다 다음 스크린 샷에서에 포함 된 내용:


![](jenkins-walkthrough-images/image26.png "Team Foundation Server 라디오 단추를 클릭 하 고 TFS 구성 섹션 같아야 합니다.")

TFS에 대 한 필요한 정보를 제공 합니다. 다음 스크린 샷에서 완성 된 폼의 예를 보여 줍니다.

![](jenkins-walkthrough-images/image27.png "이 스크린 샷 완성 된 폼의 예를 보여 줍니다.")

#### <a name="testing-the-source-code-control-configuration"></a>테스트 소스 코드 제어 구성

적절 한 소스 코드 제어를 구성한 후 클릭 **저장** 여 변경 내용을 저장 합니다. 다음 스크린 샷에서 비슷합니다는 작업에 대 한 홈 페이지로 돌아갑니다.

![](jenkins-walkthrough-images/image28.png "이 돌아갑니다이 스크린샷과 유사 합니다 되는 작업에 대 한 홈 페이지")

소스 코드 제어 올바르게 구성 되어 있는지 확인 하는 가장 간단한 방법은 지정 된 빌드 작업이 없는 경우에 빌드를 트리거할 수동으로 됩니다. 작업의 홈 페이지에는 빌드를 수동으로 시작 하는 **이제 빌드** 아래 스크린샷에 표시 된 것 처럼 왼쪽의 항목에 연결 합니다.

![](jenkins-walkthrough-images/image29.png "왼쪽에 있는 메뉴에 작업의 홈 페이지에 이제 빌드 링크 빌드를 수동으로 시작 하려면")

빌드 시작 되 면 빌드 기록 대화 상자에는 깜박이 파란색 원, 진행률 표시줄, 빌드 번호 및 빌드 시작, 다음 스크린샷과 유사 하 게 된 시간을 표시 합니다.

![](jenkins-walkthrough-images/image30.png "빌드 기록 대화 상자 깜박임 파란색 원, 진행률 표시줄, 빌드 번호 및 빌드 시작 된 시간을 표시 하는 빌드 시작 되 면")

작업이 성공 하면 파란색 원을 표시 됩니다. 작업이 실패 하면 빨간색 원이 표시 됩니다.

빌드의 일부로 발생할 수 있는 문제 해결에 도움이 되도록 작업에 대 한 콘솔 출력의 모든 Jenkins 캡처됩니다. 콘솔 출력을 보려면 작업의 클릭 **빌드 기록**를 선택한 다음는 **콘솔 출력** 왼쪽 메뉴에서 링크 합니다. 다음 스크린 샷에 표시 된 **콘솔 출력** 링크를 뿐만 아니라 일부 성공적인 작업에서 출력:

![](jenkins-walkthrough-images/image31.png "이 스크린 샷 콘솔 출력 링크도 성공적인 작업에서 출력의 일부를 보여 줍니다.")

#### <a name="location-of-build-artifacts"></a>빌드 아티팩트의 위치

Jenkins 라는 특수 폴더에는 전체 소스 코드를 검색 합니다는 *작업 영역*합니다. 다음 위치에 폴더에이 디렉터리를 찾을 수 있습니다.

    ~/.jenkins/jobs/[JOB NAME]/workspace

작업 영역에 대 한 경로 이라는 환경 변수에 저장 됩니다 `$WORKSPACE`합니다.

작업에 대 한 방문 페이지도 이동한 다음에서 클릭 하 여 Jenkins에서 작업 영역 폴더를 찾아볼 수는 **작업 영역** 왼쪽 메뉴에 링크 합니다. 다음 스크린 샷에서 라는 작업에 대 한 작업 영역의 예를 보여 줍니다. **HelloWorld**:

![](jenkins-walkthrough-images/image32.png "이 스크린 샷 HelloWorld 라는 작업에 대 한 작업 영역의 예를 보여 줍니다.")

### <a name="build-triggers"></a>빌드 트리거

Jenkins에서 빌드를 시작 하기 위한 일부의 전략-라고 *빌드 트리거*합니다. 빌드 트리거 Jenkins 작업을 시작 하 고 프로젝트를 작성 하는 시기를 결정할 수 있습니다. 일반적인 빌드 트리거 중 두 가지는:

- **빌드를 정기적으로** –이 트리거에 의해 지정 된 간격으로 2 시간 마다 등 또는 평일에 자정에 작업을 시작 하는 Jenkins 합니다. 빌드 여부에 관계 없이 시작 됩니다 소스 코드 리포지토리에 모든 변경 내용이 있습니다.
- **설문 조사 SCM** –이 트리거를 소스 코드 제어를 정기적으로 폴링합니다. 변경 내용을 소스 코드 리포지토리에 커밋된을 Jenkins 새 빌드를 시작 됩니다.

SCM 폴링 되므로 인기 있는 트리거 개발자 빌드가 중단 하도록 하는 변경 내용이 커밋될 때에 빠른 피드백을 제공 합니다. 일부 최근에 커밋된 코드 문제를 일으키는 되 고는 개발자는 변경 내용이 떠올리면 유의 해야 하는 동안 문제를 해결할 수 있습니다 팀 경고 기능이 유용 합니다.

정기적인 빌드는 테스터에 배포할 수 있는 응용 프로그램의 버전을 만들려면 자주 사용 됩니다. 예를 들어, QA 팀의 멤버가 이전 주 작업을 테스트할 수 있도록 정기적인 빌드 금요일 저녁에 대 한 예약 될 수 있습니다.


### <a name="compiling-a-xamarinios-applications"></a>Xamarin.iOS 응용 프로그램 컴파일
Xamarin.iOS 프로젝트를 사용 하 여 명령줄에서 컴파일할 수 `xbuild` 또는 `msbuild`합니다. 셸 명령은 Jenkins를 실행 하는 사용자 계정의 컨텍스트에서 실행 됩니다. 사용자 계정에 프로비저닝 프로필에 대 한 액세스 배포에 대 한 응용 프로그램을 제대로 패키지할 수 있도록 유용 합니다. 작업 구성 페이지에서이 셸 명령을 추가 하는 것이 불가능 합니다.

으로 아래로 스크롤하여는 **빌드** 섹션. 클릭는 **추가 빌드 단계** 선택한 단추 **셸 실행**다음 스크린샷에 표시 된 것 처럼,:

![](jenkins-walkthrough-images/image33.png "빌드 단계 추가 단추를 클릭 하 고 Execute 셸 선택")


[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

### <a name="building-a-xamarinandroid-project"></a>Xamarin.Android 프로젝트 빌드

Xamarin.Android 프로젝트를 작성 하는 것은 매우 유사 Xamarin.iOS 프로젝트 빌드입니다. APK Xamarin.Android 프로젝트에서를 만들려면 다음 두 단계를 수행 하려면 Jenkins는 구성 해야 합니다.

 - 프로젝트를 컴파일하는 MSBuild 플러그 인을 사용 하 여
 - 기호 및 zip 유효한 릴리스가 키 저장소와 APK를 맞춥니다.

이러한 두 단계는 다음 두 섹션에서 자세히 설명 합니다.

### <a name="creating-the-apk"></a>APK 만들기

클릭는 **추가 빌드 단계** 단추를 선택 하 고 **에서 Visual Studio 프로젝트 또는 MSBuild를 사용 하 여 솔루션을 만들**아래 스크린샷에 표시 된 것 처럼:

![](jenkins-walkthrough-images/image36.png "Visual Studio 프로젝트 또는 솔루션 MSBuild를 사용 하 여 추가 빌드 단계 단추 및 빌드 선택에서 APK 클릭 만들기")

빌드 단계는 프로젝트에 추가 되 면 표시 되는 양식 필드를 입력 합니다. 다음 스크린 샷에서 완성 된 폼의 한 예:

![](jenkins-walkthrough-images/image37.png "빌드 단계는 프로젝트에 추가 되 면 표시 되는 폼 필드 입력")


이 빌드 단계가 실행 될 `xbuild` 에 **$WORKSPACE** 폴더입니다. MSBuild 작성 파일으로 설정 되어는 **Xamarin.Android.csproj** 파일입니다. **명령줄 인수** 대상의 릴리스 빌드를 지정 **PackageForAndroid**합니다. 이 단계의 제품은 APK 됩니다 하는 다음 위치에서:

    $WORKSPACE/[PROJECT NAME]/bin/Release

다음 스크린샷은이 APK의 예를 보여 줍니다.

![](jenkins-walkthrough-images/image38.png "이 스크린샷에서이 APK 예를 보여 줍니다.")

이 APK zip 정렬 해야 하 고 개인 키 저장소와 서명 되지 않은 배포에 대 한 준비 되지 않았습니다.

#### <a name="signing-and-zipaligning-the-apk-for-release"></a>서명 및 Zipaligning 릴리스 APK

서명 및 zipaligning APK는 기술적으로 두 가지 별도 작업이 Android SDK에서 두 개의 별도 명령줄 도구에 의해 수행 됩니다. 그러나이 하나 이상의 빌드 작업에서 수행 하 여 편리 합니다. 서명 및 zipaligning는 APK 하는 방법에 대 한 자세한 내용은 릴리스에 대 한 Android 응용 프로그램 준비 Xamarin의 설명서를 참조 합니다.

두이 명령은 모두 프로젝트 마다 다를 수 있는 명령줄 매개 변수가 필요 합니다. 또한 이러한 명령줄 매개 변수 중 일부는 빌드가 실행 될 때 콘솔 출력에 표시 되지 않도록 하는 암호입니다. 이러한 명령줄 매개 변수 중 일부 환경 변수에 저장 합니다. 서명 및/또는 zip 정렬 하는 데 필요한 환경 변수는 아래 표에 설명 되어 있습니다.

|환경 변수|설명|
|--- |--- |
|KEYSTORE_FILE|APK 서명에 키 저장소에 대 한 경로입니다.|
|KEYSTORE_ALIAS|APK 서명에 사용할 키 저장소의 키입니다.|
|INPUT_APK|에 의해 만들어진 APK `xbuild`합니다.|
|SIGNED_APK|생성 되는 서명 된 APK `jarsigner`합니다.|
|FINAL_APK|이것은 zip APK에 의해 생성 되는 정렬 `zipalign`합니다.|
|STORE_PASS|이것이 노래 파일에 대 한 키 저장소의 내용에 액세스 하는 데 사용 되는 암호입니다.|

요구 사항 섹션에서에서 설명한 대로 EnvInject 플러그 인을 사용 하 여 빌드하는 동안 이러한 환경 변수를 설정할 수 있습니다. 작업에는 새 빌드를 있어야 단계 넣기 환경 변수를 기반으로 다음 스크린샷에 표시 된 대로 추가:

![](jenkins-walkthrough-images/image39.png "작업에는 새 빌드를 있어야 넣기 환경 변수를 기반으로 하는 단계 추가")

에 **속성 콘텐츠** 다음 형식으로 표시 되는 필드, 환경 변수 추가, 줄 당 하나씩 1 개를 형성 합니다.

    ENVIRONMENT_VARIABLE_NAME = value

다음 스크린 샷에서 APK 서명에 필요한 환경 변수를 보여 줍니다.

![](jenkins-walkthrough-images/image40.png "이 스크린샷은 APK 서명에 필요한 환경 변수를 보여 줍니다.")

APK 파일에 대 한 환경 변수 중 일부 작성 된 것을 알는 `WORKSPACE` 환경 변수입니다.

최종 환경 변수는 키 저장소의 내용에 액세스 하려면 암호: `STORE_PASS`합니다. 암호는 중요 한 값을 가려진 또는 로그 파일에서 생략 해야 합니다. 로그에 표시 되지 않으면 이러한 값을 보호 하기 위해 EnvInject 플러그 인을 구성할 수 있습니다.

바로 앞의 **빌드** 작업 구성 섹션은는 **빌드 환경** 섹션. 경우는 **암호 삽입** 확인란의 설정/해제, 일종 필드 표시를 합니다. 이러한 양식 필드 이름 및 환경 변수 값을 캡처하는 데 사용 됩니다. 다음 스크린 샷에서 추가의 예는 `STORE_PASS` 환경 변수:

![](jenkins-walkthrough-images/image41.png "이 스크린 샷은 STOREPASS 환경 변수를 추가 하는 예")

환경 변수는 초기화 될 다음 단계는 것에 서명 하는 것에 대 한 빌드 단계를 추가 및 APK 맞춤 zip입니다. 환경 변수를 삽입 하는 빌드 단계 다른 됩니다 직후 **셸 실행** 에서 실행할 명령 빌드 `jarsigner` 및 `zipalign`합니다. 다음 코드 조각에 나와 있는 것 처럼 각 명령은 한 줄 위로 소요 됩니다.


    jarsigner -verbose -sigalg MD5withRSA -digestalg SHA1 -keystore $KEYSTORE_FILE -storepass $STORE_PASS -signedjar $SIGNED_APK $INPUT_APK $KEYSTORE_ALIAS
    zipalign -f -v 4 $SIGNED_APK $FINAL_APK

다음 스크린샷은 입력 하는 방법의 예는 `jarsigner` 및 `zipalign` 명령 하는 단계에:

![](jenkins-walkthrough-images/image42.png "이 스크린샷에서 단계에 jarsigner 및 zipalign 명령을 입력 하는 방법의 예를 보여 줍니다.")

모든 빌드 작업을 저장 한 후 것이 모든 기능이 작동을 확인 하려면 수동 빌드를 트리거할 좋습니다. 빌드가 실패 한 경우는 **콘솔 출력** 빌드 실패의 원인에 대 한 내용은 검토 해야 합니다.

### <a name="submitting-tests-to-test-cloud"></a>테스트 테스트 클라우드를 제출합니다.

자동화 된 테스트 셸 명령을 사용 하 여 테스트 클라우드로 제출할 수 있습니다. Xamarin 테스트 클라우드에서 테스트 실행 설정에 대 한 자세한 내용은 사용 하기 위한 가이드 있는 [Xamarin.UITest](https://developer.xamarin.com/guides/testcloud/uitest/working-with/submitting-tests-to-xamarin-test-cloud/) 또는 [Calabash](https://developer.xamarin.com/guides/testcloud/calabash/working-with/submitting-tests-to-xamarin-test-cloud/)합니다.


## <a name="summary"></a>요약

이 가이드에서는에서 Mac OS X, 빌드 서버 Jenkins를 도입 하 고를 컴파일하고 Xamarin 릴리스에 대 한 모바일 응용 프로그램을 준비 하려면이 구성 합니다. 여러 플러그 인 빌드 프로세스를 지원 하기 위해 함께 Mac OS X 컴퓨터에서 Jenkins를 설치 했습니다. 생성 하 고, Git 또는 TFS에서 코드를 끌어오도록 되며 다음 릴리스 준비 응용 프로그램에 해당 코드를 컴파일 하는 작업을 구성 합니다. 작업이 실행 되어야 하는 시점을 예약 하는 두 가지 방법으로 살펴본 합니다.

## <a name="related-links"></a>관련 링크

- [연속 통합](https://developer.xamarin.com~/testcloud/ci/)
- [Xamarin 테스트 클라우드로 테스트를 제출합니다.](https://developer.xamarin.com~/testcloud/submitting/)
- [이유 Jenkins에서 지원 하지 않는 Xamarin?](~/cross-platform/troubleshooting/questions/xamarin-jenkins.md)
