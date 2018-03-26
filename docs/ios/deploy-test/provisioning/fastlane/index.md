---
title: iOS에 대한 Fastlane 소개
description: 이 가이드에서는 iOS 응용 프로그램 서명을 코드하는데 사용할 수 있는 다양한 fastlane 도구를 소개합니다.
ms.topic: article
ms.prod: xamarin
ms.assetid: 8202C57D-22FF-4224-A5B1-AAEF12B7C106
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 4bba92180e77accaa42b70843fb5dbf12c94d632
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="introduction-to-fastlane-for-ios"></a>iOS에 대한 Fastlane 소개

_이 가이드에서는 iOS 응용 프로그램 서명을 코드하는데 사용할 수 있는 다양한 fastlane 도구를 소개합니다._

fastlane는 iOS 및 Android 앱을 릴리스하는 복잡하고 종종 번거로운 프로세스를 간소화하기 위해 만든 오픈 소스 프로젝트입니다. 여러 유틸리티로 구성되며 각 유틸리티는 앱 릴리스의 다음과 같은 특정 측면을 처리합니다.

- [deliver](https://github.com/fastlane/fastlane/tree/master/deliver#readme) - 스크린샷, 메타 데이터 및 응용 프로그램 번들을 관리하고 iTunes Connect로 업로드합니다.
- [produce](https://github.com/fastlane/fastlane/tree/master/produce#readme) – iTunes Connect와 개발자 포털(AppID 라고도 함)에서 앱을 만듭니다. 또한 앱 그룹 및 응용 프로그램 서비스에 대한 지원이 포함됩니다.
- [pem](https://github.com/fastlane/fastlane/tree/master/pem#readme) – 푸시 알림 프로 비전 프로필을 생성하고 관리합니다.
- [gym](https://github.com/fastlane/fastlane/tree/master/gym#readme) – iOS 응용 프로그램을 빌드하고 서명 하는데 사용할 수 있습니다. (Xamarin 앱은 MSBuild를 사용하여 앱을 빌드, 서명 및 보관합니다)
- [cert](https://github.com/fastlane/fastlane/tree/master/cert#readme) – 인증서를 서명하는 코드를 생성하고 관리합니다. 
- [sigh](https://github.com/fastlane/fastlane/tree/master/sigh#readme) – 프로비저닝을 생성하고 관리합니다.
- [match](https://github.com/fastlane/fastlane/tree/master/match#readme) – 인증서와 프로필을 생성 및 유지 관리하고 git 리포지토리에 저장하여 개발 팀 전반에 동기화될 수 있도록 합니다.

fastlane은 다양한 방식으로 사용할 수 있습니다: 그러한 방식으로는 터미널 명령을 통한, 파일 기반 수단을 통한 또는 연속 통합 빌드에 대한 환경 변수를 사용한 방식 등이 있습니다. 

이 가이드는 iOS 앱 개발을 위한 장치 설정에 대해 구체적으로 다루며, **cert**, **sigh** 및 **match** 유틸리티에 중점을 둡니다. 

제공된 콘텐츠는 연속 통합 서버에서 프로세스를 완전히 자동화하는 것을 포함하여 앱 배포를 지원하기 위한 springboard로 사용할 수 있습니다. 그러나 fastlane은 Xcode 프로젝트를 지원하는 도구를 만드는 타사 제품이라 `fastlane init`와 같은 일부 도구나 명령이 csproj 파일과는 예상대로 작동하지 않을 수도 있다는 점이 중요합니다. fastlane 사용, 추가 도구 또는 fastlane을 사용하여 Android 출시에 대한 자세한 내용은 [https://fastlane.tools/](https://fastlane.tools/)를 참조하세요.

<a name="Installation" />

## <a name="installation"></a>설치

1. Xcode 명령줄 도구가 macOS 컴퓨터에 설치되어 있는지 확인합니다. 도구를 설치하려면 터미널에서 `xcode-select --install` 명령을 사용합니다. 이미 설치되어 있다면 다음과 같은 오류가 표시됩니다.

    ```bash
    error: command line tools are already installed, use "Software Update" to install updates
    ```

2. [https://download.fastlane.tools](https://download.fastlane.tools)에서 fastlane 도구를 다운로드합니다. 

    > [!NOTE]
    > `brew cask install fastlane`을 사용하여 Homebrew에서 또는 `sudo gem install fastlane –NV`를 사용하여 Rubygems(2.0 이상)를 통해 fastlane 도구를 설치할 수 있습니다. 그러나 설치 관리자를 사용하면 올바른 종속성을 사용할 수 있도록 합니다. 

3. 파일의 압축을 풀어 fastlane를 설치하고 `install` 실행 파일을 두 번 클릭합니다. 파일이 "정체불명의 개발자로부터 왔기 때문에 열 수 없다"고 통지하는 오류가 발생하면 확인을 누르고 다음 작업을 수행합니다.
    - `install` 실행 파일에서 컨트롤 +를 클릭합니다. 아래의 대화 상자를 표시합니다.

      ![](images/fastlane-image12.png "설치 대화 상자")
    
    - Fastlane 도구 설치를 시작하려면 [확인]을 누릅니다.

4. 터미널은 아래 그림과 같이 대화 상자로 묻는 메시지를 표시합니다. `y` 누르기:

  ![](images/fastlane-image13.png "터미널 프롬프트")
 
4. 처음으로 fastlane을 사용하기 전에.`which fastlane`을 실행합니다. 경로는 다음과 같습니다. 

    ```bash
    /Users/[user]/.fastlane/bin
    ```

5. 경로가 위와 일치하면 시작할 준비가 되었습니다.

     아닐 경우 다음 작업을 수행합니다: macOS에서 다음 명령을 사용하여 홈 디렉터리에 있는 숨겨진 일반 텍스트 파일인 `.bash_profile`을 엽니다.

    ```bash
    open ~/.bash_profile
    ```

6. 다음 PATH 환경 변수를 추가하고 저장합니다. 

    ```bash
    export PATH="$HOME/.fastlane/bin:$PATH"
    ```

7.  `which fastlane`을 다시 실행하여 `/Users/[user]/.fastlane/bin`과 같이 보이는 경로를 확인합니다.


## <a name="updating-fastlane"></a>fastlane 업데이트

fastlane는 정기적으로 새 릴리스를 푸시하는 매우 활성화된 오픈 소스 프로젝트입니다. 새 버전의 fastlane이 사용 가능하게 된다면 fastlane 명령을 실행할 때 알림을 받을 것입니다.

[![](images/fastlane-image0.png "패스트 레인 업데이트 프롬프트")](images/fastlane-image0.png#lightbox)


Fastlane의 새 버전으로 업데이트하려면 [여기](https://download.fastlane.tools)서 최신 패키지를 다운로드하여 설치 패키지를 두 번 클릭하여 실행합니다.

[![](images/fastlane-image0a.png "설치 패키지 실행")](images/fastlane-image0a.png#lightbox)


## <a name="contents"></a>목차

이 가이드 시리즈는 개발 또는 배포의 준비로 iOS 앱을 코드 서명하기 위해 사용하는 도구 일부를 제공합니다. 현재 다뤄진 도구는 다음과 같습니다.

- [cert](~/ios/deploy-test/provisioning/fastlane/cert.md)
- [sigh](~/ios/deploy-test/provisioning/fastlane/sigh.md)
- [match](~/ios/deploy-test/provisioning/fastlane/match.md)

cert와 sigh는 서명 인증서를 만들고 관리하며 로컬 컴퓨터에 프로필을 프로비전하는 것을 다룹니다. match는 이 프로세스 한층 더 깊이 다룹니다. 인증서 및 프로비저닝 프로필을 만들어 관리하며, git 리포지토리에 저장하여 개발 팀의 모두가 액세스할 수 있게 합니다. 작동하는 방법 및 사용할 수 있는 방법을 알아보려면 각 섹션을 자세히 읽습니다.

## <a name="using-fastlane-tools-with-xamarin"></a>Xamarin과 함께 fastlane 도구 사용하기

fastlane을 사용하여 서명 ID와 프로비전 프로필을 만들었다면 Mac용 Visual Studio에서 번들 서명 옵션을 설정하는 일은 인증서와 개인 키가 macOS 키 집합에 있고 프로비전 프로필이 폴더 `~/Library/MobileDevice/Provisioning Profiles`에 있는 경우 간단합니다.

Xamarin.iOS 응용 프로그램에 대한 코드 서명 옵션을 설정하려면 프로젝트 이름을 마우스 오른쪽 단추로 클릭하여 **프로젝트 옵션 > 빌드 > iOS 번들 서명**을 선택하고 아래에 설명했듯이 서명 ID와 프로비전 프로필을 명시적으로 설정합니다.

[![](images/fastlane-image11.png "서명 ID와 프로비전 프로필을 명시적으로 설정")](images/fastlane-image11.png#lightbox)

## <a name="related-links"></a>관련 링크

- [fastlane Docs](https://fastlane.tools/)
- [fastlane 코드 서명 Docs](https://docs.fastlane.tools/codesigning/getting-started/)
- [코드 서명 가이드](https://codesigning.guide/)
