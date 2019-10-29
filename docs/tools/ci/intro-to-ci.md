---
title: Xamarin을 사용한 연속 통합 소개
description: 이 문서에서는 Xamarin과의 연속 통합에 대해 설명 합니다. 버전 제어와 다양 한 연속 통합 환경에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: C034200E-2947-4309-9DDD-80DAC505C43F
author: davidortinau
ms.author: daortin
ms.date: 07/19/2017
ms.openlocfilehash: 2862f05f2d183c9345d2b92268ddf2101cc2492e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029810"
---
# <a name="introduction-to-continuous-integration-with-xamarin"></a>Xamarin을 사용한 연속 통합 소개

_연속 통합은 프로젝트의 버전 제어 리포지토리에서 개발자가 코드를 추가 하거나 변경할 때 자동화 된 빌드에서 앱을 컴파일하고 선택적으로 테스트 하는 소프트웨어 엔지니어링 방법입니다. 이 문서에서는 연속 통합의 일반적인 개념과 Xamarin 프로젝트와의 연속 통합에 사용할 수 있는 몇 가지 옵션에 대해 설명 합니다._

개발자가 동시에 작업을 수행 하는 데 일반적으로 소프트웨어 프로젝트를 사용 합니다. 특정 시점에이 작업의 모든 병렬 스트림을 최종 제품을 구성 하는 하나의 코드 베이스에 통합 해야 합니다. 소프트웨어 개발의 초반에이 통합은 프로젝트의 끝에서 수행 되었으며이는 어렵고 위험한 프로세스 였습니다.

CI (연속 통합)는 일반적으로 개발자가 프로젝트의 공유 코드 리포지토리에 대 한 변경 내용을 체크 인할 때마다 모든 개발자의 변경 내용을 일반적인 코드 베이스에 병합 하 여 이러한 복잡성을 방지 합니다. 각 체크 인은 자동화 된 빌드를 트리거하고 자동 테스트를 실행 하 여 새로 도입 된 코드가 기존 코드를 중단 하지 않았는지 확인 합니다.  이러한 방식으로 CI는 오류 및 문제를 즉시 표시 하 고 모든 팀 멤버가 서로의 작업을 최신 상태로 유지 하도록 합니다. 이로 인해 긴밀 하 고 안정적인 코드 베이스가 발생 합니다.

연속 통합 시스템은 두 가지 주요 부분으로 구성 됩니다.

- 원본 제어 또는 소스 코드 관리 라고도 하는 버전 제어 – VC ( **버전** 제어)는 모든 프로젝트의 코드를 단일 공유 리포지토리로 통합 하 고 모든 변경 내용의 전체 기록을 모든 파일에 보관 합니다. 주로 중요 또는 *마스터* 분기 라고도 *하는이* 리포지토리는 프로덕션 또는 릴리스 버전의 앱을 빌드하는 데 사용 되는 소스 코드를 포함 합니다. 이 작업에는 많은 오픈 소스 및 상용 제품이 있습니다. 일반적으로 팀 또는 개인이 코드의 복사본을 광범위 하 게 변경 하거나 마스터 분기에 대 한 위험 없이 실험을 수행할 수 있는 보조 분기로 분기할 수 있습니다. 보조 분기의 유효성을 검사 한 후에는 해당 변경 내용을 모두 마스터 분기로 다시 병합할 수 있습니다.
- **연속 통합 서버** – 연속 통합 서버는 프로젝트의 모든 아티팩트 (소스 코드, 이미지, 비디오, 데이터베이스, 자동화 된 테스트 등)를 수집 하 고, 앱을 컴파일하고, 자동화 된 테스트를 실행 하는 역할을 담당 합니다. 또한 많은 오픈 소스 및 상업적 CI 서버 도구가 있습니다.

개발자는 일반적으로 작업을 처음 수행 하는 워크스테이션에 하나 이상의 분기에 대 한 작업 복사본을 만듭니다. 적절 한 작업 집합이 완료 되 면 변경 내용이 적절 한 분기에 "체크 인" 또는 "커밋" 되어 다른 개발자의 작업 복사본으로 전파 됩니다. 이는 팀이 모두 동일한 코드에서 작동 하 고 있는지 확인 하는 방법입니다.

연속 통합을 사용 하 여 변경 내용을 커밋하는 작업을 수행 하면 CI 서버에서 프로젝트를 빌드하고 자동화 된 테스트를 실행 하 여 소스 코드가 올바른지 확인 합니다. 빌드 오류 또는 테스트 실패가 발생 한 경우 CI 서버는 담당자에 게 문제를 해결할 수 있도록 전자 메일, IM, Twitter, Growl 등을 통해 담당 개발자에 게 알립니다. (CI 서버는 "제어 된 체크 인" 이라고 하는 오류가 발생 하는 경우에도 커밋을 거절할 수 있습니다.)

다음 다이어그램은이 프로세스를 보여 줍니다.

[![](intro-to-ci-images/intro01-small.png "This diagram illustrates this process")](intro-to-ci-images/intro01.png#lightbox)

Mobile apps는 연속 통합에 대 한 고유한 문제를 소개 합니다. 앱은 실제 장치 에서만 사용할 수 있는 GPS 또는 카메라와 같은 센서를 요구할 수 있습니다. 또한 시뮬레이터 또는 에뮬레이터는 하드웨어의 근사값 이며 문제를 숨기 거 나 숨길 수 있습니다. 결과적으로 실제 하드웨어에서 모바일 앱을 테스트 하 여 고객에 게는 진정한 준비가 되었다는 것을 확신 하는 것이 필요 합니다.

[App Center 테스트](https://docs.microsoft.com/appcenter/test-cloud) 는 수백 대의 물리적 장치에서 앱을 직접 테스트 하 여이 특정 문제를 해결 합니다. 개발자는 강력한 UI 테스트를 허용 하는 자동화 된 승인 테스트를 작성 합니다. 이러한 테스트를 App Center 업로드 하면 CI 서버는 다음 다이어그램에 표시 된 것 처럼 CI 프로세스의 일부로 자동으로 실행할 수 있습니다.

[![](intro-to-ci-images/intro02-small.png "Once these tests are uploaded to App Center, the CI server can run them automatically as part of a CI process as shown in this diagram")](intro-to-ci-images/intro02.png#lightbox)

## <a name="components-of-continuous-integration"></a>연속 통합 구성 요소

CI를 지원 하도록 설계 된 상용 및 오픈 소스 도구의 광범위 한 에코 시스템이 있습니다. 이 섹션에서는 가장 일반적인 몇 가지 방법에 대해 설명 합니다.

### <a name="version-control"></a>버전 제어

#### <a name="azure-devops-and-team-foundation-server"></a>Azure DevOps 및 Team Foundation Server

[Azure DevOps](https://azure.microsoft.com/services/devops/) 및 [Team Foundation Server](https://visualstudio.microsoft.com/tfs/) (TFS)는 지속적인 통합 빌드 서비스, 작업 추적, agile 계획 및 보고 도구, 버전 제어를 위한 Microsoft의 공동 작업 도구입니다. 버전 제어를 사용 하 여 Azure DevOps와 TFS는 자체 시스템 (Team Foundation 버전 제어 또는 TFVC) 또는 GitHub에서 호스트 되는 프로젝트와 함께 작동할 수 있습니다.

- Azure DevOps는 클라우드를 통해 서비스를 제공 합니다. 주요 이점은 전용 하드웨어 또는 인프라가 필요 하지 않으며, 웹 브라우저를 통해 어디서 나 Visual Studio와 같은 인기 있는 개발 도구를 통해 액세스 하 여 지리적으로 분산 된 팀에 게 매력적인 기능을 제공 하는 것입니다. . 5 명의 개발자 팀에 게는 무료 이며 이후 성장 하는 팀을 수용 하기 위해 추가 라이선스를 구매할 수 있습니다.
- TFS는 온-프레미스 Windows 서버를 위해 설계 되 고 로컬 네트워크 또는 해당 네트워크에 대 한 VPN 연결을 통해 액세스 됩니다. 기본 장점은 빌드 서버의 구성을 완전히 제어 하 고 필요한 추가 소프트웨어 또는 서비스를 설치할 수 있다는 것입니다. TFS에는 소규모 팀을 위한 무료 항목 수준 Express edition이 있습니다.

TFS와 Azure DevOps는 모두 Visual Studio와 긴밀 하 게 통합 되며 개발자는 단일 IDE에서 많은 버전 제어 및 CI 작업을 수행할 수 있습니다. Eclipse 용 Team Explorer Everywhere 플러그 인 (아래 참조)도 사용할 수 있습니다. Mac용 Visual Studio에는 [사용할 수 있는 TFVC의 미리 보기가](/visualstudio/mac/tf-version-control/)있습니다.

[Azure DevOps 파이프라인](https://docs.microsoft.com/azure/devops/pipelines/languages/xamarin/) 은 대상으로 하려는 각 플랫폼에 대 한 빌드 정의를 만드는 Xamarin 프로젝트를 직접 지원 합니다 (Android, IOS 및 Windows). 각 빌드 정의에 적절 한 Xamarin 라이선스가 필요 합니다. 이 목적을 위해 로컬 Xamarin 지원 TFS 빌드 서버를 Azure DevOps에 연결할 수도 있습니다. 이 설정을 사용 하면 Azure DevOps에 큐에 대기 중인 빌드가 로컬 서버에 위임 됩니다. 자세한 내용은 [빌드 및 릴리스 에이전트](https://docs.microsoft.com/azure/devops/pipelines/agents/agents)를 참조 하세요. 또는 Jenkins 또는 팀 도시와 같은 다른 빌드 도구를 사용할 수 있습니다.

Visual Studio, Azure DevOps 및 Team Foundation Server의 모든 ALM (응용 프로그램 수명 주기 관리) 기능에 대 한 전체 요약은 [Xamarin 앱을 사용 하는 Devops](https://docs.microsoft.com/visualstudio/cross-platform/application-lifecycle-management-alm-with-xamarin-apps)를 참조 하세요.

#### <a name="team-explorer-everywhere"></a>Team Explorer Everywhere

[Team Explorer Everywhere](https://docs.microsoft.com/azure/devops/java/download-eclipse-plug-in/) 는 Visual Studio 외부에서 개발 하는 팀에 Team Foundation Server 및 Azure devops의 기능을 제공 합니다. 이를 통해 개발자는 Eclipse 또는 OS X 및 Linux 용 플랫폼 간 명령줄 클라이언트에서 온-프레미스 또는 클라우드의 팀 프로젝트에 연결할 수 있습니다. Team Explorer Everywhere는 Windows 이외의 플랫폼에 대 한 버전 제어 (Git 포함), 작업 항목 및 빌드 기능에 대 한 모든 권한을 제공 합니다.

#### <a name="git"></a>Git

[Git](https://git-scm.com) 은 원래 Linux 커널의 소스 코드를 관리 하기 위해 개발 된 인기 있는 오픈 소스 버전 제어 솔루션입니다. 모든 규모의 소프트웨어 프로젝트에서 널리 사용 되는 매우 빠르고 유연한 시스템입니다. 인터넷에 연결 되지 않은 단일 개발자에서 전 세계에 걸친 규모가 많은 팀에 쉽게 확장할 수 있습니다. 또한 Git를 사용 하면 분기를 매우 쉽게 만들 수 있으며, 따라서 최소한의 위험으로 개발의 병렬 스트림을 권장 합니다.

Git은 웹 브라우저나 Linux, Mac OSX 및 Windows에서 실행 되는 [GUI 클라이언트](https://git-scm.com/downloads/guis) 를 통해 완전히 작동할 수 있습니다. 공용 리포지토리에 무료로 제공 됩니다. 개인 리포지토리에는 [유료 계획이](https://github.com/pricing)필요 합니다.

최신 버전의 Windows 및 Mac 용 Visual Studio는 Git에 대 한 기본 지원을 제공 합니다. Microsoft는 이전 버전의 Visual Studio에 대해 [Git 용으로 다운로드 가능한 확장](https://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c) 을 제공 합니다. 위에서 설명한 것 처럼 Azure DevOps 및 TFS는 TFVC 대신 버전 제어에 Git을 사용할 수 있습니다.

#### <a name="subversion"></a>손상을

[Subversion (Subversion](https://subversion.apache.org) )은 2000부터 사용 중인 인기 있는 오픈 소스 버전 제어 시스템입니다. SVN은 OS X, Windows, FreeBSD, Linux 및 Unix의 모든 최신 버전에서 실행 됩니다. Mac용 Visual Studio은 SVN을 기본적으로 지원 합니다. Visual Studio에 대 한 SVN 지원을 가져오는 타사 확장이 있습니다.

### <a name="continuous-integration-environments"></a>연속 통합 환경

연속 통합 환경을 설정 하는 것은 버전 제어 시스템을 빌드 서비스와 결합 하는 것을 의미 합니다.  후자의 경우 가장 일반적인 두 가지는 다음과 같습니다.

- [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/) 는 Azure devops 및 TFS의 빌드 시스템입니다. Visual Studio와 긴밀 하 게 통합 되므로 개발자가 빌드를 트리거하고 자동으로 테스트를 실행 하 고 결과를 볼 수 있습니다.
- Jenkins는 모든 종류의 소프트웨어 개발을 지원 하기 위한 다양 한 플러그인 에코 시스템이 있는 오픈 소스 CI 서버입니다. Windows 및 Mac OS X에서 실행 됩니다. Jenkins은 특정 IDE와 통합 되지 않습니다. 대신 웹 인터페이스를 통해 구성 및 관리 됩니다. 또한 Jenkins CI를 설치 하 고 구성 하 여 소규모 팀에 편리 하 게 사용할 수 있습니다.

TFS/Azure DevOps를 단독으로 사용 하거나, 다음 섹션에 설명 된 대로 TFS/Azure DevOps 또는 Git와 함께 Jenkins를 사용할 수 있습니다.

#### <a name="azure-devops-and-team-foundation-server"></a>Azure DevOps 및 Team Foundation Server

설명 된 대로 Azure DevOps 및 Team Foundation Server는 버전 제어와 빌드 서비스를 모두 제공 합니다. 빌드 서비스에는 각 대상 플랫폼에 대 한 Xamarin Business 또는 Enterprise 라이선스가 항상 필요 합니다.

Azure DevOps를 사용 하 여 각 대상 플랫폼에 대 한 별도의 빌드 정의를 만들고 여기에 적절 한 라이선스를 입력 합니다. 구성 되 면 Azure DevOps는 클라우드에서 빌드 및 테스트를 실행 합니다. 자세한 내용은 [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/) 를 참조 하세요.

Team Foundation Server를 사용 하 여 특정 대상 플랫폼에 대해 빌드 컴퓨터를 다음과 같이 구성 합니다.

- **Android 및 Windows:** Visual Studio 및 Xamarin 도구 (Android 및 Windows 모두)를 설치 하 고 Xamarin 라이선스를 사용 하 여 구성 합니다. 또한 Android SDK를 TFS 빌드 에이전트에서 찾을 수 있는 서버에서 공유 위치로 이동 해야 합니다. 자세한 내용은 [TFVC 구성](https://docs.microsoft.com/azure/devops/repos/tfvc/overview)을 참조 하세요.
- **iOS 및 Xamarin:** 적절 한 라이선스를 사용 하 여 Windows server에 Visual Studio 및 Xamarin 도구를 설치 합니다. 그런 다음, 네트워크에서 액세스할 수 있는 Mac OS X 컴퓨터에 Mac용 Visual Studio를 설치 합니다 .이 컴퓨터는 빌드 호스트로 사용 되며 최종 앱 패키지 (iOS 용 IPA, OS X 용 앱)를 만듭니다.

다음 다이어그램은이 토폴로지를 보여 줍니다.

[![](intro-to-ci-images/intro03-small.png "This diagram illustrates this topography")](intro-to-ci-images/intro03.png#lightbox)

Azure DevOps 빌드를 로컬 서버에 위임할 수 있도록 로컬 TFS 서버를 Azure DevOps 프로젝트에 연결할 수도 있습니다. 자세한 내용은 [빌드 및 릴리스 에이전트](https://docs.microsoft.com/azure/devops/pipelines/agents/agents/)를 참조 하세요.

#### <a name="azure-devops-and-jenkins"></a>Azure DevOps 및 Jenkins

Jenkins를 사용 하 여 앱을 빌드하는 경우 Azure DevOps 또는 Team Foundation Server에 코드를 저장 하 고 CI 빌드에 Jenkins을 계속 사용할 수 있습니다. 팀 프로젝트의 Git 리포지토리로 코드를 푸시 하거나 TFVC에 코드를 체크 인할 때 Jenkins 빌드를 트리거할 수 있습니다. 자세한 내용은 [Azure DevOps를 사용 하 여 Jenkins](https://docs.microsoft.com/azure/devops/service-hooks/services/jenkins)를 참조 하세요.

[![](intro-to-ci-images/intro04-small.png "If you use Jenkins to build your apps, you can store your code in Azure DevOps or Team Foundation Server and continue to use Jenkins for your CI builds")](intro-to-ci-images/intro04.png#lightbox)

#### <a name="git-and-jenkins"></a>Git 및 Jenkins

또 다른 일반적인 CI 환경은 완전히 OS X를 기반으로 할 수 있습니다. 이 시나리오는 소스 코드 제어에 Git를 사용 하 고 빌드 서버에 Jenkins를 사용 하는 것을 포함 합니다. 이러한 두 가지 모두 Mac용 Visual Studio 설치 된 단일 Mac OS X 컴퓨터에서 실행 됩니다. 이는 이전 섹션에서 설명한 Azure DevOps + Jenkins 환경과 매우 유사 합니다.

[![](intro-to-ci-images/intro05-small.png "This is very similar to the Azure DevOps + Jenkins environment discussed in the previous section")](intro-to-ci-images/intro05.png#lightbox)

> [!IMPORTANT]
> **Jenkins는 [Microsoft에서 지원 하지 않습니다](~/cross-platform/troubleshooting/questions/xamarin-jenkins.md).**
