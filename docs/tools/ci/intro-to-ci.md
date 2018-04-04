---
title: Xamarin 사용한 연속 통합 소개
description: 연속 통합은 자동화 된 빌드 컴파일 및 코드를 추가 하거나 프로젝트의 버전 제어 리포지토리에 개발자가 변경한 경우 필요에 따라 앱을 테스트 하는 소프트웨어 엔지니어링 좋습니다. 이 문서는 연속 통합의 일반적인 개념 및 옵션 중 일부는 사용할 수 있는 연속 통합에 대 한 Xamarin 프로젝트와 함께 설명 합니다.
ms.prod: xamarin
ms.assetid: C034200E-2947-4309-9DDD-80DAC505C43F
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 07/19/2017
ms.openlocfilehash: 017691ece68f979eea1627c0442f49018d5742fb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-continuous-integration-with-xamarin"></a>Xamarin 사용한 연속 통합 소개

_연속 통합은 자동화 된 빌드 컴파일 및 코드를 추가 하거나 프로젝트의 버전 제어 리포지토리에 개발자가 변경한 경우 필요에 따라 앱을 테스트 하는 소프트웨어 엔지니어링 좋습니다. 이 문서는 연속 통합의 일반적인 개념 및 옵션 중 일부는 사용할 수 있는 연속 통합에 대 한 Xamarin 프로젝트와 함께 설명 합니다._

동시에 작동 하 고 개발자가 소프트웨어 프로젝트에서 경우가 많습니다. 어느 시점 부터는 이러한 항목 모두 통합 하는 데 필요한이를 하나의 작업의 병렬 스트림을 최종 제품을 구성 하는 코드 베이스 확인 합니다. 소프트웨어 개발의 초기가이 통합 어렵고 위험한 프로세스 하 던 프로젝트의 끝에 수행한입니다.

연속 통합 (CI)를 지속적으로 공용 코드 베이스에 모든 개발자의 변경 내용을 병합 하 여 이러한 복잡성을 피하기 위해, 모든 개발자가 체크 인할 때마다 일반적으로 프로젝트의 변경 코드 저장소를 공유 합니다. 각 체크 인에 자동화 된 빌드 트리거하며 새로 도입 된 코드가 기존 코드를 중단 하지 않은 있는지 확인 하는 자동화 된 테스트를 실행 합니다.  이러한 방식으로 CI 오류 및 문제를 즉시 표시 하 고 모든 팀 멤버가 유지 작업에 서로 관련 최신 되도록 합니다. 이 인해 결속력과 안정적인 코드 베이스에 있습니다.

연속 통합 시스템이 두 부분으로 구성 합니다.

-  **버전 제어** – 버전 제어 (VC), 소스 제어 또는 소스 코드 관리를 단일 공유 저장소로 통합 하는 모든 프로젝트의 코드 및 모든 파일에 모든 변경 내용의 전체 기록을 유지 합니다. 이 저장소 라 불리는 *지원은* 또는 *마스터* 분기, 궁극적으로 프로덕션 빌드 또는 릴리스 버전 앱의 소스 코드를 포함 합니다. 여러 오픈 소스 및이 작업에 대 한 일반적으로 팀 이나 개인을 광범위 하 게 변경 하거나 마스터 분기에 대 한 위험 없이 실험을 수행할 수 있는 보조 분기로 코드의 복사본을 분기 수 있는 상용 제품 가지가 있습니다. 보조 분기의 변경 내용을 유효성이 확인 되 면 될 수 있습니다 다음 모두 함께 마스터 분기에 다시 병합.
-  **연속 통합 서버** –의 지속적인 통합 서버는 모든 프로젝트의 아티팩트 (예: 소스 코드, 이미지, 비디오, 데이터베이스, 자동화 된 테스트 등)를 수집, 앱을 컴파일 및 자동화 된 테스트 실행을 담당 합니다. 마찬가지로 여러 오픈 소스 및 고급 CI 서버 도구 됩니다.

개발자는 일반적으로 작업이 처음 수행 되는 해당 워크스테이션에서 하나 이상의 분기의 작업 복사본을 갖습니다. 적절 한 작업 집합이 완료 되 면 변경 "에 체크 인" 되었거나 다른 개발자의 작업 복사본이 전파 하 여 적절 한 분기에는 "커밋"입니다. 이 팀 준수 하는 모두 동일한 코드에서 작동 하는 방식입니다.

다시, 연속 통합을의 변경 내용을 커밋하는 작업 CI를 하면 서버의 프로젝트를 빌드하고 소스 코드의 정확성을 확인할 수 있는 자동화 된 테스트 실행 합니다. CI 서버 책임 개발자에 게 알리는 오류 테스트 또는 빌드 오류가 없으면 (인스턴트 메시징, Twitter, 렁, 전자 메일을 통해 등)가 문제를 해결할 수 있도록 합니다. (CI 서버도 않이 있습니다 커밋은 "제어 된 체크 인" 라는 오류가 발생 하는 경우.)

다음 다이어그램에서는이 프로세스를 보여 줍니다.

[![](intro-to-ci-images/intro01-small.png "이 다이어그램에서는이 프로세스를 보여 줍니다.")](intro-to-ci-images/intro01.png#lightbox)

모바일 앱에는 연속 통합에 대 한 고유한 문제가 소개합니다. 앱만 실제 장치에서 사용할 수 있는 예: 카메라 또는 GPS 센서 필요할 수 있습니다. 또한 시뮬레이터, 에뮬레이터 하드웨어의 근사값만 하 고을 숨길 수도 있습니다를 려 서 문제. 되 결국에서 진정한 고객용 준비 인지 확신 하는 실제 하드웨어에서 모바일 앱을 테스트 해야 합니다.

[앱 센터 테스트](https://docs.microsoft.com/en-us/appcenter/test-cloud) 수백 대의 물리적 장치에서 직접 앱을 테스트 하 여이 특정 문제를 해결 합니다. 개발자는 강력한 UI 테스트를 허용 하는 자동화 된 통용 테스트를 작성 합니다. 이러한 테스트 앱 센터에 업로드 되 면 CI 서버 수 자동으로 실행할 CI 프로세스의 일부로 다음 다이어그램에 나와 있는 것 처럼:

[![](intro-to-ci-images/intro02-small.png "이러한 테스트 앱 센터에 업로드 되 면 CI 서버 수 자동으로 실행할 CI 프로세스의 일부로이 다이어그램에 나와 있는 것 처럼")](intro-to-ci-images/intro02.png#lightbox)

# <a name="components-of-continuous-integration"></a>연속 통합의 구성 요소

CI 지원 하도록 설계 상용 및 오픈 소스 도구는 광범위 한 에코 시스템을 있습니다. 이 섹션에서는 가장 일반적인 중 몇 가지를 설명 합니다.

## <a name="version-control"></a>버전 제어

### <a name="visual-studio-team-services-and-team-foundation-server"></a>Visual Studio Team Services 및 Team Foundation Server

[Visual Studio Team Services](https://www.visualstudio.com/products/visual-studio-team-services-vs) (VSTS) 및 [Team Foundation Server](http://msdn.microsoft.com/en-us/vstudio/ff637362.aspx) (TFS)는 연속 통합을 위한 Microsoft의 공동 작업 도구는 서비스, 작업 추적, agile 계획 및 보고 도구, 및 버전 빌드 컨트롤입니다. 버전 제어 VSTS 및 TFS (Team Foundation 버전 제어 또는 TFVC) 자체 시스템과 또는 GitHub에서 호스트 되는 프로젝트와 함께 작업할 수 있습니다.

 - Visual Studio Team Services를 통해 클라우드 서비스를 제공합니다. 주요 이점은 없거나 전용된 하드웨어 인프라 필요 하며에서 액세스할 수 있습니다 위치 하 고 있어 매력적인 지리적으로 분산 하는 팀에 대 한 Visual Studio와 같은 인기 있는 개발 도구를 통해 웹 브라우저를 통해 배포 됩니다. 인지 5 개발자 팀에 대 한 무료 간단한 후 증가 하 고 팀 수용 하기 위해 어떤 추가 라이선스를 구입할 수 있습니다.
 - TFS 온-프레미스 Windows 서버를 위한 것 이며 로컬 네트워크 또는 해당 네트워크에 VPN 연결을 통해 액세스 됩니다. 주요 이점은 완벽 하 게 빌드 서버의 구성을 제어 하는 필요한 모든 추가 소프트웨어 또는 서비스를 설치할 수입니다. TFS 소규모 팀에 대 한 초급 무료 Express 버전을 있습니다.

TFS 및 VSTS Visual Studio와 긴밀 하 게 통합 하 고 개발자가 여러 버전 제어 및 편안 하 게 하나의 IDE 내에서 CI 작업을 수행할 수 있게 합니다. Team Explorer Everywhere Eclipse 용 플러그 인 (아래 참조)도 제공 됩니다. Mac 용 visual Studio TFS 또는 VSTS에 대 한 모든 지원을 제공 하지 않습니다.

Visual Studio 팀 서비스의 빌드 시스템에 있는 대상 (Android, iOS 및 Windows)를 각 플랫폼에 대 한 빌드 정의 만들 되는 Xamarin 프로젝트에 대 한 직접 지원 합니다. 각 빌드 정의 대 한 적절 한 Xamarin 라이선스 필요 합니다. 로컬 연결 수 이기도, Xamarin 지원 TFS 빌드 서버에이 목적을 위해 Visual Studio Team Services. 이 설치 프로그램 VSTS를 대기 중인 빌드가 로컬 서버에 위임 합니다. 자세한 내용은 참조 [배포 빌드 서버를 구성 하 고](https://msdn.microsoft.com/en-us/library/ms181712.aspx)합니다. 또는 Jenkins 또는 팀 도시와 같은 다른 빌드 도구를 사용할 수 있습니다.

Visual Studio, Visual Studio Team Services 및 Team Foundation Server 참조의 모든 관리 ALM (Application Lifecycle) 기능을 완벽 하 게 요약 [애플리케이션 수명 주기 관리 및 Xamarin 앱](https://msdn.microsoft.com/en-us/library/mt162217(v=vs.140).aspx) msdn 합니다.


### <a name="team-explorer-everywhere"></a>Team Explorer Everywhere

[Team Explorer Everywhere](http://msdn.microsoft.com/en-us/library/gg413285.aspx) Visual Studio 외부의 개발 팀에 Team Foundation Server 및 Visual Studio Team Services의 기능을 제공 합니다. 개발자가 Osx 및 Linux 용 Eclipse 또는 크로스 플랫폼 명령줄 클라이언트에서 온-프레미스 또는 클라우드에서 팀 프로젝트에 연결할 수 있습니다. Team Explorer Everywhere 전체 제공 (Git 포함), 버전 제어에 작업 항목에 액세스 하 고 Windows 이외의 플랫폼에 대 한 기능을 빌드합니다.


### <a name="git"></a>Git

[Git](http://git-scm.com) 원래 Linux 커널에 대 한 소스 코드 관리를 위해 개발 된 인기 있는 오픈 소스 버전 제어 솔루션입니다. 그는 소프트웨어 프로젝트의 모든 규모의 인기 있는 매우 신속 하 고 유연한 시스템입니다. 쉽게를 전 세계에 걸쳐 있는 큰 팀 저하 인터넷에 연결 된 단일 개발자가 확장 됩니다. Git 분기에 위험을 최소화 하는 개발의 병렬 스트림을 요청할 수 있는 매우 쉽고 수도 있습니다.

Git 또는 웹 브라우저를 통해 완전히 작동할 수 [GUI 클라이언트](http://git-scm.com/downloads/guis) Linux, Mac OSX 및 Windows에서 실행 하는 합니다. 공용 저장소;에 대 한 무료입니다. 필요한 개인 저장소는 [계획 유료](https://github.com/pricing)합니다.

Visual Studio 2015 및 Visual Studio Mac에 대 한 Git;에 대 한 기본 지원을 제공합니다 Microsoft는 이전 버전의 경우 다음을 제공 합니다. 한 [Git에 대 한 다운로드 가능한 확장명](http://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c)합니다. 위에서 언급 한 대로 Visual Studio Team Services 및 TFS 대신 TFVC 버전 제어에 대 한 Git을 사용할 수 있습니다.


### <a name="subversion"></a>Subversion

[Subversion](http://subversion.apache.org) (SVN)는 2000 이후 사용에서 된 인기 있는 오픈 소스 버전 제어 시스템입니다. SVN 모든 최신 버전의 OS X, Windows, FreeBSD, Linux 및 Unix에서 실행 됩니다. Mac 용 visual Studio는 네이티브 SVN 지원 합니다. Visual Studio에 SVN 지원 가져오는 제 3 자 확장이 있습니다.


## <a name="continuous-integration-environments"></a>연속 통합 환경

연속 통합 환경 설정 버전 제어 시스템을 빌드 서비스로 결합을 의미 합니다.  후자의 경우는 두 개의 가장 일반적인 것은:

- [Team Foundation Build](https://msdn.microsoft.com/Library/vs/alm/Build/overview) 는 Visual Studio Team Services 및 TFS 빌드 시스템입니다. Visual studio에서 빌드를 트리거하는 개발자를 위한 편리한 것 있습니다에서 자동으로 테스트를 실행 하 고 결과 확인할 긴밀 하 게 통합 되어 있습니다.
- Jenkins는 오픈 소스 CI 서버 플러그 인 모든 종류의 소프트웨어 개발을 지원 하기 위해 다양 한 에코 시스템이 포함 하는 합니다. Windows에서 실행 되 고 Mac OS X. Jenkins 모든 특정 IDE와 통합 되지 않습니다. 대신, 구성 되 고 웹 인터페이스를 통해 관리 합니다. Jenkins CI 설치 하 고 소규모 팀에 게 유용 하므로 구성도 쉽습니다.

TFS/VSTS를 사용 하 여 단독으로 또는 Jenkins TFS/VSTS 또는 다음 섹션에 설명 된 대로 Git와 함께에서 사용할 수 있습니다.

### <a name="visual-studio-team-services-and-team-foundation-server"></a>Visual Studio Team Services 및 Team Foundation Server

앞에서 설명한 대로 Visual Studio Team Services 및 Team Foundation Server에는 두 버전 제어 하 고 서비스를 구축 합니다. 빌드 서비스는 항상 각 대상 플랫폼에 대 한 Xamarin 비즈니스 또는 엔터프라이즈 라이선스를 필요로 합니다.

Visual Studio Team Services를 통해 각 대상 플랫폼에 대 한 별도 빌드 정의 만들고 적절 한 라이선스를 있습니다를 입력 합니다. VSTS 빌드를 실행할 됩니다 구성 되 면 및 클라우드에서 테스트 합니다. 참조 [Team Foundation Build](https://msdn.microsoft.com/Library/vs/alm/Build/overview) 내용을 확인 합니다.

Team Foundation Server와 같이 특정 대상 플랫폼에 대 한 빌드 컴퓨터를 구성할 수 있습니다.

- **Android 및 Windows:** Visual Studio를 설치 및 Xamarin (Android 및 Windows 용 두) 도구 및 Xamarin 라이선스를 사용 하 여 구성 합니다. TFS 빌드 에이전트 서버의 공유 위치에 Android SDK를 찾을 수를 이동 하는 데 필요한 이기도 합니다. 자세한 내용은 참조 [구성 TFVC](https://docs.microsoft.com/vsts/tfvc/overview)합니다.
- **iOS 및 Xamarin:** Visual Studio를 설치 하 고 적절 한 라이선스와 함께 Windows server에서 Xamarin 도구입니다. 빌드 호스트로 사용할 때를 최종 응용 프로그램 패키지 (iOS, OS X 용 응용 프로그램에 대 한 IPA)를 만들 수는 네트워크에서 액세스할 수 있는 Mac OS X 컴퓨터에서 Mac 용 Visual Studio를 설치 합니다.

다음 다이어그램에서는이 지형을이 보여 줍니다.

[![](intro-to-ci-images/intro03-small.png "이 다이어그램에서는이 지형을이 보여 줍니다.")](intro-to-ci-images/intro03.png#lightbox)

VSTS 빌드는 로컬 서버에 위임 됩니다 있도록 Visual Studio Team Services 프로젝트에 대 한 로컬 TFS 서버 연결도 가능 합니다. 자세한 내용은 참조 [배포 빌드 서버를 구성 하 고](http://msdn.microsoft.com/en-us/library/ms181712.aspx) msdn 합니다.

### <a name="visual-studio-team-services-and-jenkins"></a>Visual Studio Team Services 및 Jenkins

앱을 빌드 해 Jenkins를 사용 하는 경우 Visual Studio Team Services 나 Team Foundation Server에 코드를 저장할 수 있으며 계속 Jenkins CI 빌드에 사용 합니다. 팀 프로젝트의 Git 리포지토리 또는 체크 인할 때는 코드 TFVC에 코드를 푸시할 때 Jenkins 빌드를 트리거할 수 있습니다. 자세한 내용은 참조 [Visual Studio Team Services를 통해 Jenkins](https://www.visualstudio.com/en-us/docs/marketplace/integrate/service-hooks/services/jenkins)합니다.

[![](intro-to-ci-images/intro04-small.png "앱을 빌드 해 Jenkins를 사용 하는 경우 Visual Studio Team Services 나 Team Foundation Server에 코드를 저장 하 고 계속 Jenkins CI 빌드에 대 한 사용 수 있습니다.")](intro-to-ci-images/intro04.png#lightbox)

### <a name="git-and-jenkins"></a>Git 및 Jenkins

다른 일반적인 CI 환경을 완전히 OS X 기반 수 있습니다. 이 시나리오에서는 소스 코드 제어 및 Jenkins 빌드 서버에 대 한 Git를 사용 합니다. 설치 된 Mac에 대 한 Visual Studio와 함께 단일 Mac OS X 컴퓨터에서 실행 중인 둘 다에 해당 합니다. 이 옵션은 Visual Studio Team Services + 이전 섹션에서 설명 하는 Jenkins 환경 매우 유사 합니다.

[![](intro-to-ci-images/intro05-small.png "이 Visual Studio Team Services + 이전 섹션에서 설명 하는 Jenkins 환경 매우 비슷합니다.")](intro-to-ci-images/intro05.png#lightbox)

> [!IMPORTANT]
> **참고: Jenkins는 [Xamarin에서 지원 되지 않습니다](~/cross-platform/troubleshooting/questions/xamarin-jenkins.md)합니다.**


# <a name="summary"></a>요약

이 문서에는 연속 통합의 개념 및 소프트웨어 개발 팀은 장점과 도입 되었습니다. 버전 제어의 중요성은 역할 및 책임의 빌드 서버와 함께 설명한 합니다. 문서는 다음 빌드 서버 및 소스 코드 제어에 사용할 수 있는 도구 중 일부에 대해 토론 하려면 들어 있습니다. 또한 개발자 품질 및 앱의 기능을 증명 되는 자동화 된 테스트를 실행 하 여 유용한 앱을 게시 하는 데 도움이 앱 센터 테스트 도입 되었습니다. 앱과 테스트 앱 센터를 찾을 수 있습니다 제출에 대 한 설명서 보다 자세한 [여기](https://docs.microsoft.com/en-us/appcenter/test-cloud)합니다. 마지막으로, 이해 하기 위해 연속 통합에 대 한 조직 설정 될 수 있습니다 하는 여러 다른 CI 환경을 설정한 모든 도구 및 구성 요소가 함께 포함 하는 방법입니다. Xamarin 프로젝트와 함께 Visual Studio Team Services 및 Team Foundation Server를 사용 하 여 자세한 내용은 참조 하십시오. [구성 TFVC](https://docs.microsoft.com/vsts/tfvc/overview) 이 [연속 통합 소개](https://docs.microsoft.com/en-us/vsts/build-release/actions/ci-cd-part-1)합니다. 마찬가지로, Jenkins를 사용 하 여 참조 [Xamarin 사용한 Jenkins를 사용 하 여](~/tools/ci/jenkins-walkthrough.md) 에 대 한 자세한 내용은 연속 통합을 설정 합니다.