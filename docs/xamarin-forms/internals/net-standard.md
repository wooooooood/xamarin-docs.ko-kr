---
title: Xamarin.ios에서 .NET Standard 2.0 지원
description: 이 문서에서는 .NET Standard 2.0를 사용 하도록 Xamarin Forms 응용 프로그램을 변환 하는 방법을 설명 합니다. .NET Standard은 모든 .NET 구현에서 사용할 수 있는 .NET Api의 사양입니다.
ms.prod: xamarin
ms.assetid: 95805355-63a7-44e7-a3c6-6487a6276ab2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: eaef607e3e6095ccc7dfb1f5831d2384d11cab5f
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760058"
---
# <a name="net-standard-20-support-in-xamarinforms"></a>Xamarin.ios에서 .NET Standard 2.0 지원

_이 문서에서는 .NET Standard 2.0를 사용 하도록 Xamarin Forms 응용 프로그램을 변환 하는 방법을 설명 합니다._

.NET Standard은 모든 .NET 구현에서 사용할 수 있는 .NET Api의 사양입니다. 다른 플랫폼에 동일한 Api를 제공 하 여 데스크톱 응용 프로그램, 모바일 앱, 게임 및 클라우드 서비스에서 코드를 보다 쉽게 공유할 수 있습니다. .NET Standard에서 지 원하는 플랫폼에 대 한 자세한 내용은 [.net 구현 지원](/dotnet/standard/net-standard#net-implementation-support)을 참조 하세요.

.NET Standard 라이브러리는 PCL (이식 가능한 클래스 라이브러리)을 대체 합니다. 그러나 .NET Standard를 대상으로 하는 라이브러리는 여전히 PCL 이며 .NET Standard 기반 PCL 이라고 합니다. 특정 PCL 프로필은 .NET Standard 버전에 매핑되고 매핑이 있는 프로필의 경우 두 라이브러리 형식이 서로 참조할 수 있습니다. 자세한 내용은 [PCL 호환성](/dotnet/standard/net-standard#pcl-compatibility)을 참조 하세요.

Xamarin.ios 2.4은 PCL을 .NET Standard 2.0 라이브러리로 바꿔 .NET Standard 2.0를 대상으로 하는 Xamarin.ios 응용 프로그램을 허용 합니다. 이러한 작업은 다음과 같이 수행할 수 있습니다.

- [.Net Core 2.0](https://www.microsoft.com/net/download/core) 이 설치 되어 있는지 확인 합니다.
- Xamarin. 양식 2.4 이상을 사용 하도록 Xamarin.ios 솔루션을 업데이트 합니다.
- 2\.0 .NET Standard를 대상으로 하는 .NET Standard 라이브러리를 솔루션에 추가 합니다.
- .NET Standard 라이브러리에 추가 된 클래스를 삭제 합니다.
- .NET Standard 라이브러리에 Xamarin 2.4 이상 NuGet 패키지를 추가 합니다.
- 플랫폼 프로젝트에서 .NET Standard 라이브러리에 대 한 참조를 추가 하 고 Xamarin.ios 사용자 인터페이스 논리를 포함 하는 PCL 프로젝트에 대 한 참조를 제거 합니다.
- PCL 프로젝트의 파일을 .NET Standard 라이브러리로 복사 합니다.
- Xamarin.ios 사용자 인터페이스 논리를 포함 하는 PCL 프로젝트를 제거 합니다.

## <a name="related-links"></a>관련 링크

- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [코드 공유 옵션](~/cross-platform/app-fundamentals/code-sharing.md)
