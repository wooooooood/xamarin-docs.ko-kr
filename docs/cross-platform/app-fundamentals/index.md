---
title: "응용 프로그램 기본 사항"
description: "응용 프로그램의 핵심 개념"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: c5b823370e5b65fbcf9ba366cb89c05e003b1a89
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="application-fundamentals"></a>응용 프로그램 기본 사항

이 섹션에서 일반적인 작업 작업 또는 개발자가 모바일 응용 프로그램을 개발할 때 알고 있어야 하는 개념 중 일부는 지침을 제공 합니다.

##  <a name="building-cross-platform-applicationscross-platformapp-fundamentalsbuilding-cross-platform-applicationsindexmd"></a>[플랫폼 간 응용 프로그램 빌드](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)

Xamarin을 선택 하 고 디자인 하 고 모바일 응용 프로그램을 개발할 때는 몇 가지 사항에 유의 지속적으로 업데이트를 있습니다 수 모바일 플랫폼 간에 공유 되는 엄청난 코드 실현, 시장 출시 시간을 줄일, 기존 인력 활용 하 여, 모바일 액세스를 위한 고객 수요를 충족 하 고 플랫폼 간 복잡성을 줄입니다. &nbsp;이 문서에서는 유틸리티 및 생산성 응용 프로그램에 이러한 이점을 이해 하는 데 핵심 지침에 설명 합니다.

## <a name="code-sharing-optionscode-sharingmd"></a>[코드 공유 옵션](code-sharing.md)

Xamarin 프로젝트의 경우 이식 가능한 클래스 라이브러리 (PCLs), 공유 프로젝트 및.NET 표준 라이브러리를 포함 하 여 사용할 수 있는 옵션을 공유 하는 다른 코드에 대 한 알아봅니다.


## <a name="accessibilityaccessibilitymd"></a>[액세스 가능성](accessibility.md)

액세스할 수 있는 응용 프로그램을 구축 하기 위한 팁입니다.


## <a name="localizationlocalizationmd"></a>[지역화](localization.md)

로캘을 인식 하는 앱을 만들기 위한 지침은 여러 언어로 번역할 수 있습니다.


##  <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[이식 가능한 클래스 라이브러리](~/cross-platform/app-fundamentals/pcl.md)

이식 가능한 클래스 라이브러리 프로젝트를 빌드하고 여러 플랫폼에서 실행 하는 공유 코드를 포함 하는 어셈블리를 배포할 수 있습니다. 이식 가능한 클래스 라이브러리 (또는 "PCL")를 만들려면 먼저 플랫폼을 대상 다음 해당 플랫폼에 대해 정의 된 프로필에서 사용할 수 있는.NET Framework의 하위 집합에 대해 코드를 작성 하려면 선택 합니다. 이 문서에서는 만들고 PCLs Xamarin 함께 사용 하는 방법에 설명 합니다.

##  <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md)

공유 프로젝트의 여러 다른 응용 프로그램 프로젝트에서 참조 되는 일반적인 코드를 작성할 수 있습니다. 코드 참조 하는 각 프로젝트의 일부로 컴파일되고 공유 코드 베이스에서 플랫폼 관련 기능을 추가할 수 있도록 컴파일러 지시문을 포함할 수 있습니다. 이 문서에는 공유 프로젝트의 작동 방식 및 만들고 Xamarin 프로젝트와 함께 사용 하는 방법을 설명 합니다.

##  <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)

.NET 표준 플랫폼 간 코드 공유에 대 한 새로운 옵션입니다. 이식 가능한 클래스 라이브러리;에 유사한 방식으로 작동 코드는 특정 버전 (현재 1.0 1.6 ~)에 대해 만들어지고 해당 수준 지 원하는 다른 프로젝트에서 소비할 이상이 될 수 있습니다. 표준.NET 프로젝트는 Mac. Xamarin Studio 6.2, Visual Studio에 대 한 Windows 및 Visual Studio에서 지원

##  <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[코드 공유에 대 한 다중 플랫폼 라이브러리 NuGet 프로젝트의 경우:](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

표준 프로젝트; PCL 또는.NET에서에서 NuGet 패키지를 자동으로 생성 될 수 있습니다. 와 별도 NuGet 프로젝트 형식을 사용 하 여 "밥 and 스위치" NuGet 패키지로 공유 프로젝트를 패키지할 수 있습니다. 이 섹션에서는 각 코드 공유 시나리오에 대 한 NuGet 패키지를 만드는 방법에 설명 합니다.

##  <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[Xamarin에 대 한 NuGet 패키지를 수동으로 만들기](~/cross-platform/app-fundamentals/nuget-manual.md)

Xamarin 플랫폼을 사용 하는 NuGet 패키지를 만들기 위한 팁입니다.

##  <a name="cross-platform-data-accessxamarin-formsdata-cloudindexmd"></a>[크로스 플랫폼 데이터 액세스](~/xamarin-forms/data-cloud/index.md)

대부분의 응용 프로그램에는 로컬 장치에서 데이터를 저장 하려면 몇 가지 요구를 사항이 있습니다. 많으면 데이터의 양이 다르거나 일반적으로,이 일반적으로 필요 데이터베이스 및 데이터 계층 응용 프로그램에 데이터베이스 액세스를 관리 합니다. iOS 및 Android "내장" SQLite 데이터베이스 엔진 있고 데이터 저장 및 검색에 대 한 액세스 Xamarin 플랫폼을 통해 간소화 되었습니다. [Android 데이터 액세스](~/android/data-cloud/data-access/index.md), [iOS 데이터 액세스](~/ios/data-cloud/data/index.md), 및 [Xamarin.Forms 데이터 액세스](~/xamarin-forms/data-cloud/index.md) 가이드는 각 플랫폼에서 SQLite 액세스 하는 방법의 예를 제공 합니다.


##  <a name="transport-layer-securitytransport-layer-securitymd"></a>[전송 계층 보안](transport-layer-security.md)

앱의 네트워크 연결을 보호 하려면 selectingthe 올바른 SSL/TLS 구현에 대 한 정보입니다.


##  <a name="notificationsxamarin-formsdata-cloudpush-notificationsindexmd"></a>[알림](~/xamarin-forms/data-cloud/push-notifications/index.md)

모바일 응용 프로그램 비 가시적인 방법을 일부 응용 프로그램 특정 이벤트가 발생 하는 사용자에 게 알리는 알림을 사용 합니다. 알림을 백그라운드에서 실행 되는 응용 프로그램 프로세스의 상태를 알리기 위해 일반적으로 사용 됩니다. 이러한 예는 큰 파일을 다운로드 수 있습니다. 이 작업은 백그라운드에서 수행 해야 하므로이 파일에서를 다운로드 하는 데 시간이 오래 걸릴 수 있습니다. 다운로드가 완료 되 면 사용자가 정보를 받는 팩트의 알림으로 인해 합니다.
또한 알림 아레스 로컬 응용 프로그램에만 제한 되지 않습니다. 모바일 응용 프로그램에 알림을 게시할 서버 응용 프로그램에도 가능 합니다. 이 문서에서는 Android와 iOS에서 알림을 사용 하는 방법을 설명 합니다.
