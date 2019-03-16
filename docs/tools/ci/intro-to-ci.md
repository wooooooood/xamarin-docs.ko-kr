---
title: Xamarin 사용 하 여 연속 통합 소개
description: 이 문서에서는 Xamarin 사용 하 여 연속 통합을 설명 합니다. 버전 제어 및 다양 한 연속 통합 환경에 설명 합니다.
ms.prod: xamarin
ms.assetid: C034200E-2947-4309-9DDD-80DAC505C43F
author: lobrien
ms.author: laobri
ms.date: 07/19/2017
ms.openlocfilehash: 35c5811d57ade1d320e56e292c1eeed094963a0d
ms.sourcegitcommit: 650458de1d362cd7de174cacef7838f0e74426f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2019
ms.locfileid: "58070919"
---
# <a name="introduction-to-continuous-integration-with-xamarin"></a>Xamarin 사용 하 여 연속 통합 소개

_연속 통합은 소프트웨어 엔지니어링 사례에 자동화 된 빌드를 컴파일하고 및 필요에 따라 코드를 추가 하거나 프로젝트의 버전 제어 리포지토리에 개발자가 변경할 때 앱을 테스트 합니다. 이 문서에서는 지속적인 통합의 일반적인 개념 및 옵션 중 일부를 사용할 수 있는 연속 통합에 대 한 Xamarin 프로젝트를 사용 하 여 설명 합니다._

병렬로 작동 하는 개발자를 위한 소프트웨어 프로젝트에서 일반적 이며 어느 시점에서 이러한 모든 통합 하는 데 필요한 것 최종 제품을 구성 하는 코드 베이스를 하나로 작업의 병렬 스트림으로. 소프트웨어 개발의 초창기를이 통합은 어렵고 위험한 프로세스는 프로젝트의 끝에 수행 되었습니다.

CI (지속적인 통합)를 지속적으로 공용 코드 베이스에 모든 개발자의 변경 내용을 병합 하 여 이러한 복잡성을 피하기 위해, 모든 개발자에 게 체크 인할 때마다 일반적으로 프로젝트의 변경 내용을 코드 리포지토리를 공유 합니다. 각 체크 인에 자동화 된 빌드를 트리거 하 고 새로 도입된 된 코드는 기존 코드를 중단 하지는 확인 하기 위해 자동화 된 테스트를 실행 합니다.  이러한 방식으로 CI 오류 및 문제를 즉시 표시 하 고는 모든 팀 멤버가 관련 최신 정보 얻기 각각 다른 작업을 확인 합니다. 이 인해 응집력 및 안정적인 코드 베이스입니다.

연속 통합 시스템에 두 주요 부분:

- **버전 제어** – 버전 제어 (VC), 소스 제어 또는 소스 코드 관리, 라고도를 단일 공유 리포지토리에 통합 하는 모든 프로젝트의 코드 및 모든 파일에 모든 변경 내용의 전체 기록을 유지 합니다. 이 리포지토리 라고도 합니다 *지원과* 또는 *마스터* 분기, 최종적으로 프로덕션 빌드 또는 릴리스 버전의 앱에 사용 될 소스 코드를 포함 합니다. 많은 오픈 소스 및 일반적으로 팀 이나 개인 광범위 하 게 변경 하거나 마스터 분기에는 위험 없이 실험을 수행할 수 있는 보조 분기로 코드의 복사본을 포크를 허용 하는이 태스크에 대 한 상용 제품 있습니다. 보조 분기의 변경 내용을 확인 되 면 되 면 다음 수 모두 마스터 분기에 병합 합니다.
- **연속 통합 서버** –의 연속 통합 서버는의 모든 프로젝트의 아티팩트 (예: 소스 코드, 이미지, 비디오, 데이터베이스, 자동화 된 테스트 등)를 수집, 앱을 컴파일 및 자동화 된 테스트 실행 하는 일을 담당 합니다. 마찬가지로 많은 오픈 소스 및 상업용 CI 서버 도구 있습니다.

개발자가 일반적으로 하나 이상의 분기의 작업 복사본을 자신의 워크스테이션에 있는 작업은 처음에 수행 됩니다. 적절 한 작업 집합이 완료 되 면 변경 "에 체크 인" 되었거나 다른 개발자의 작업 복사본에 전파할지는 적절 한 분기로 "커밋" 합니다. 팀 준수 하는 모두 동일한 코드에 대해 작동 하는 방식입니다.

마찬가지로 continuous integration과 함께 변경 내용을 커밋하는 작업에 CI 서버 프로젝트를 빌드 및 소스 코드의 정확성을 확인 하는 자동화 된 테스트를 실행 하면 됩니다. CI 서버 책임 개발자에 게 알리는 테스트 실패 또는 빌드 오류가 없으면 (전자 메일, IM, Twitter, 렁을 통해 등)가 문제를 해결할 수 있도록 합니다. (CI 서버 거부할 수 있습니다도 커밋 실패를 "제어 된 체크 인" 이라고 하는 경우입니다.)

다음 다이어그램에서는이 프로세스를 보여 줍니다.

[![](intro-to-ci-images/intro01-small.png "이 다이어그램에서는이 프로세스를 보여 줍니다.")](intro-to-ci-images/intro01.png#lightbox)

모바일 앱에는 연속 통합에 대 한 고유한 과제입니다. 앱에는 물리적 장치에서 이용할 수 있는 센서 GPS, 카메라 등 필요할 수 있습니다. 또한 시뮬레이터 또는 에뮬레이터는 하드웨어에 대 한 근사값만 숨길 수도 있고 문제를가 려 서. 마지막으로, 실제로 고객 맞춤형 것을 확신할 수를 실제 하드웨어에서 모바일 앱을 테스트 하는 데 필요한 됩니다.

합니다 [App Center 테스트](https://docs.microsoft.com/appcenter/test-cloud) 수백 대의 물리적 장치에서 직접 앱을 테스트 하 여이 특정 문제를 해결 합니다. 개발자는 강력한 UI 테스트에 대 한 허용 하는 자동화 된도 테스트를 작성 합니다. 이러한 테스트는 App Center에 업로드 되 면 CI 서버를 실행할 수 자동으로 CI 프로세스의 일부로 다음 다이어그램에 표시 된 대로:

[![](intro-to-ci-images/intro02-small.png "이러한 테스트는 App Center에 업로드 되 면 CI 서버를 실행할 수 자동으로 CI 프로세스의 일부로이 다이어그램에 표시 된 대로")](intro-to-ci-images/intro02.png#lightbox)

## <a name="version-control"></a>버전 제어

### <a name="azure-devops-and-team-foundation-server"></a>Azure DevOps 및 Team Foundation Server

[Azure DevOps](https://azure.microsoft.com/services/devops/) 하 고 [Team Foundation Server](https://visualstudio.microsoft.com/tfs/) (TFS)는 Microsoft의 지속적인 통합에 대 한 공동 작업 도구 빌드 서비스, 작업 추적, agile 계획 및 보고 도구 및 버전 제어 합니다. 버전 제어를 사용 하 여 Azure DevOps 및 TFS (Team Foundation 버전 제어 또는 TFVC) 자체 시스템을 사용 하 여 또는 GitHub에서 호스트 되는 프로젝트를 사용 하 여 작업할 수 있습니다.

- Visual Studio Team Services는 클라우드를 통해 서비스를 제공 합니다. 주요 이점은 전용된 하드웨어 나 인프라 없이 필요 하며에서 액세스할 수 있습니다 어디서 나 지리적으로 분산 된 팀을 위한 매력적인 만드는 Visual Studio와 같은 인기 있는 개발 도구 및 웹 브라우저를 통해 분산 합니다. 것이 5 명의 개발자의 팀을 위한 무료 이하로 후 팀이 성장 하에 맞게 추가 라이선스를 구입할 수 있습니다.
- TFS 온-프레미스 Windows 서버에 대 한 디자인 되 고 로컬 네트워크 또는 해당 네트워크에 VPN 연결을 통해 액세스 합니다. 기본 장점은 해당 완벽 하 게 빌드 서버는 구성을 제어 하는 모든 추가 소프트웨어 또는 서비스가 필요한 경우 설치할 수 있습니다. TFS는 소규모 팀에 대 한 초급 무료 Express 버전을 있습니다.

TFS와 Azure DevOps Visual Studio와 긴밀히 통합 되어 있고 개발자가 많은 버전 제어 및 편안 하 게 하나의 IDE 내에서 CI 작업을 수행할 수 있도록 합니다. Team Explorer Everywhere 용 플러그 인은 Eclipse (아래 참조)도 제공 됩니다. Mac 용 visual Studio에는 [사용 가능한 TFVC의 미리 보기](/visualstudio/mac/tf-version-control/)합니다.

[Azure DevOps 파이프라인](https://docs.microsoft.com/azure/devops/pipelines/languages/xamarin/) Xamarin 프로젝트의 경우 대상 (Android, iOS 및 Windows) 하려는 각 플랫폼에 대 한 빌드 정의 만들 수 있는 직접 지원 합니다. 각 빌드 정의 대 한 적절 한 Xamarin 라이선스가 필요 합니다. 로컬 연결도 가능, Xamarin 지원 TFS 빌드이 목적을 위해 Azure devops 서버. 이 설정을 사용 하면 로컬 서버에 Azure DevOps를 큐에 대기 중인 빌드를 위임 합니다. 자세한 내용은를 참조 [에이전트 빌드 및 릴리스](https://docs.microsoft.com/azure/devops/pipelines/agents/agents)합니다. 또는 Jenkins, 팀 도시와 같은 다른 빌드 도구를 사용할 수 있습니다.

Visual Studio, Azure DevOps 및 Team Foundation Server 참조의 모든 응용 프로그램 수명 주기 관리 (ALM) 기능의 전체 요약은 [Xamarin 앱을 사용 하 여 DevOps](https://docs.microsoft.com/visualstudio/cross-platform/application-lifecycle-management-alm-with-xamarin-apps)합니다.

### <a name="team-explorer-everywhere"></a>Team Explorer Everywhere

[Team Explorer Everywhere](https://docs.microsoft.com/azure/devops/java/download-eclipse-plug-in/) Visual Studio 외부에서 개발 팀에 Team Foundation Server 및 Visual Studio Team Services의 기능을 제공 합니다. 개발자가 OS X 및 Linux 용 Eclipse 또는 크로스 플랫폼 명령줄 클라이언트에서 온-프레미스 또는 클라우드의 팀 프로젝트에 연결할 수 있습니다. 전체를 제공 하는 Team Explorer Everywhere 작업 항목, 버전 제어 (Git 포함)에 액세스 하 고 Windows가 아닌 플랫폼용 기능을 빌드합니다.

### <a name="git"></a>Git

[Git](http://git-scm.com) 는 Linux 커널에 대 한 소스 코드를 관리 하려면 원래 개발한 인기 있는 오픈 소스 버전 제어 솔루션입니다. 이 모든 규모의 소프트웨어 프로젝트를 사용 하 여 널리 사용 되는 매우 빠르고 유연한 시스템입니다. 쉽게을 전 세계에 걸쳐 있는 대규모 팀으로 저하 인터넷 액세스를 사용 하 여 단일 개발자의 조정 합니다. Git 분기에서 위험을 최소화 하는 개발의 병렬 스트림으로 권장할 수 있는 매우 쉽게 하기도 합니다.

Git 또는 웹 브라우저를 통해 완전히 작동할 수 있습니다 [GUI 클라이언트](http://git-scm.com/downloads/guis) Linux, Mac OSX 및 Windows에서 실행 되는 합니다. 무료 공용 리포지토리입니다. 개인 리포지토리 필요는 [유료 플랜](https://github.com/pricing)합니다.

현재 버전의 Visual Studio에 대 한 Windows 및 Mac에 Git에 대 한 기본 지원을 제공합니다. Microsoft에서 제공 된 [Git에 대 한 다운로드 가능한 확장](http://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c) 이전 버전의 Visual Studio에 대 한 합니다. 위에서 설명한 대로 Visual Studio Team Services 및 TFS TFVC 대신 버전 제어에 Git을 사용할 수 있습니다.

### <a name="subversion"></a>Subversion

[Subversion](http://subversion.apache.org) (SVN)는 2000 년 이후로 사용 된 인기 있는 오픈 소스 버전 제어 시스템입니다. SVN 모든 최신 버전의 OS X, Windows, FreeBSD, Linux 및 Unix에서 실행 됩니다. Mac 용 visual Studio는 네이티브 SVN 지원 합니다. Visual Studio에 SVN 지원을 제공 하는 제 3 자 확장이 있습니다.

## <a name="continuous-integration-environments"></a>연속 통합 환경

연속 통합 환경 설정의 버전 제어 시스템을 빌드 서비스로 결합을 의미 합니다.  후자의 경우 두 가지 가장 일반적인 것 같습니다.

- [Azure 파이프라인](https://docs.microsoft.com/azure/devops/pipelines/) Azure DevOps 및 TFS의 빌드 시스템입니다. Visual studio에서 자동으로 테스트를 실행 및 결과 볼 수 있도록 빌드를 트리거하는 개발자를 위한 편리 하 게 해당 하는 긴밀 하 게 통합 됩니다.
- Jenkins는 오픈 소스 CI 서버 플러그 인 모든 종류의 소프트웨어 개발을 지 원하는 풍부한 에코 시스템을 사용 하 여 합니다. Windows에서 실행 하 고 Mac OS X. Jenkins 특정 IDE와 통합 되지 않습니다. 대신, 구성 및 웹 인터페이스를 통해 관리 됩니다. Jenkins CI를 설치 하 여 소규모 팀에 게 유용 하면 구성 하기도 쉽습니다.

TFS/Azure DevOps를 사용 하 여 자체적으로 또는 다음 섹션에 설명 된 대로 TFS/Azure DevOps 또는 Git와 함께에서 Jenkins를 사용할 수 있습니다.

### <a name="visual-studio-team-services-and-team-foundation-server"></a>Visual Studio Team Services 및 Team Foundation Server

Visual Studio Team Services 및 Team Foundation Server 버전 모두 제공에 설명한 대로 제어 하 고 서비스를 구축 합니다. 빌드 서비스는 항상 각 대상 플랫폼에 대 한 Xamarin Business 또는 Enterprise 라이선스를 요구합니다.

Visual Studio Team Services를 사용 하 여 각 대상 플랫폼에 대 한 별도 빌드 정의 만들고 적절 한 라이선스가 있습니다를 입력 합니다. 구성, Azure DevOps가 되 면 실행 빌드하고 클라우드에 테스트 합니다. 참조 [Azure 파이프라인](https://docs.microsoft.com/azure/devops/pipelines/) 대 한 자세한 내용은 합니다.

Team Foundation Server를 사용 하 여 다음과 같은 특정 대상 플랫폼에 대 한 빌드 컴퓨터를 구성할 수 있습니다.

- **Android 및 Windows:** (Android 및 Windows 모두) 용 Visual Studio 및 Xamarin 도구를 설치 하 고 Xamarin 라이선스를 사용 하 여 구성 합니다. TFS 빌드 에이전트는 서버에서 공유 위치에 Android SDK를 찾을 수를 이동 하는 데 필요한 이기도 합니다. 자세한 내용은 참조 하세요 [TFVC 구성](https://docs.microsoft.com/azure/devops/repos/tfvc/overview)합니다.
- **iOS 및 Xamarin:** 적절 한 라이선스를 사용 하 여 Windows 서버에 Visual Studio 및 Xamarin 도구를 설치 합니다. 다음 빌드 호스트로 하 고 최종 앱 패키지 (iOS, OS X 용 앱에 대 한 IPA)를 만들 네트워크에서 액세스할 수 있는 Mac OS X 컴퓨터에서 Mac 용 Visual Studio를 설치 합니다.

다음 다이어그램은이 토폴로지를 보여 줍니다.

[![](intro-to-ci-images/intro03-small.png "이 다이어그램은이 토폴로지를 보여 줍니다.")](intro-to-ci-images/intro03.png#lightbox)

로컬 서버에 위임 하 여 Azure DevOps 빌드 프로젝트를 Visual Studio Team Services를 로컬 TFS 서버에 연결할 수 이기도 합니다. 자세한 내용은 참조 하세요 [에이전트 빌드 및 릴리스](https://docs.microsoft.com/azure/devops/pipelines/agents/agents/)합니다.

### <a name="visual-studio-team-services-and-jenkins"></a>Visual Studio Team Services 및 Jenkins

앱을 빌드하도록 Jenkins를 사용 하는 경우 Visual Studio Team Services 또는 Team Foundation Server에서 코드를 저장할 수 있으며 계속 Jenkins CI 빌드에 사용할 수 있습니다. 팀 프로젝트의 Git 리포지토리 또는 tfvc 코드를 확인 하는 경우 코드를 푸시할 때 Jenkins 빌드를 트리거할 수 있습니다. 자세한 내용은 참조 하세요 [Azure DevOps를 사용 하 여 Jenkins](https://docs.microsoft.com/azure/devops/service-hooks/services/jenkins)합니다.

[![](intro-to-ci-images/intro04-small.png "Visual Studio Team Services 또는 Team Foundation Server에서 코드를 저장할 수 있으며 계속 Jenkins CI 빌드에 사용할 앱을 빌드하도록 Jenkins를 사용 하는 경우")](intro-to-ci-images/intro04.png#lightbox)

### <a name="git-and-jenkins"></a>Git 및 Jenkins

다른 일반적인 CI 환경을 완전히 OS X 기반 수 있습니다. 이 시나리오에서는 빌드 서버에 대 한 소스 코드 제어 및 Jenkins에 대 한 Git을 사용 합니다. 설치 하는 Mac 용 Visual Studio를 사용 하 여 단일 Mac OS X 컴퓨터에서 실행 중인 둘 다. Visual Studio Team Services + 이전 섹션에서 설명 하는 Jenkins 환경을 매우 비슷합니다.

[![](intro-to-ci-images/intro05-small.png "Visual Studio Team Services + 이전 섹션에서 설명 하는 Jenkins 환경을 매우 비슷합니다.")](intro-to-ci-images/intro05.png#lightbox)

> [!IMPORTANT]
> **Jenkins는 [Microsoft에서 지원 되지 않습니다](~/cross-platform/troubleshooting/questions/xamarin-jenkins.md)합니다.**
